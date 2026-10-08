# GitHub Actions · Docker · EC2 자동 빌드 및 배포 실습

실습일: 2026년 10월 7일 / 시간 기준: 한국 시간(KST)

전자정부프레임워크 프로젝트를 GitHub에 올리고, GitHub Actions에서 Maven으로 WAR를 생성한 뒤 Docker 이미지로 만들어 Docker Hub에 업로드했다. 이후 Actions가 SSH로 EC2에 접속하여 기존 컨테이너와 이미지를 제거하고 새 이미지로 애플리케이션을 실행하도록 구성했다. 배포 중 발생한 JDBC 드라이버 오류와 MySQL 데이터베이스 생성 오류를 해결하고, 브라우저에서 배포 결과를 확인했다.

이 문서는 최종 소스, 10개 Git 커밋, 원격 브랜치 reflog, 첨부한 PowerShell 기록 2,561줄을 기준으로 작성했다. 브라우저 접속 성공은 실습자의 확인 결과이며, Actions 실행 화면이나 최종 HTTP 응답 로그는 첨부 기록에 포함되어 있지 않다. 토큰과 DB 비밀번호의 실제 값은 생략했다.

## 1. 최종 실습 환경

| 항목 | 실제 사용한 구성 |
|---|---|
| GitHub 저장소 | https://github.com/cjoh0407/hello |
| 배포 브랜치 | `main` |
| 프로젝트 | 전자정부프레임워크 4.0.0 / Spring 5.3.6 / Maven / WAR |
| WAR 이름 | `target/hello-1.0.0.war` |
| 실제 동작하는 workflow | `.github/workflows/docker.yml` |
| Actions 빌드 서버 | GitHub가 제공하는 `ubuntu-latest` runner |
| 최종 실행 이미지 기반 | `tomcat:9.0-jdk11-temurin` |
| Docker Hub 로그인 계정 | `ooooocj` — 수동 로그인 기록 기준 |
| workflow의 이미지 이름 | `${DOCKER_USERNAME}/hello:latest` |
| 최종 컨테이너 이름 | `hello_app` |
| 배포 서버 | EC2 / Ubuntu 24.04.4 LTS / x86_64 |
| 최종 배포 서버 공인 IP | `43.203.249.65` |
| 서버 내부 IP | `172.31.15.145` |
| SSH 계정 | `ubuntu` |
| Docker 버전 | 29.8.2 — EC2 설치 출력 기준 |
| MySQL 버전 | 8.0.46 — EC2 접속 출력 기준 |
| 최종 JDBC 드라이버 | `com.mysql:mysql-connector-j:8.0.33` |
| 최종 DB 주소 | `jdbc:mysql://43.203.249.65:3306/kosa_db` |
| 최종 웹 포트 | EC2 `80` → 컨테이너 `8080` |
| 접속 주소 | `http://43.203.249.65/` |

처음 안내에서 사용했던 `hello-app`과 달리, 최종 workflow는 Docker Hub 저장소 이름을 `hello`로 사용한다. Docker Hub에 `hello-app` 저장소를 만들었더라도, 현재 코드가 빌드하고 업로드하는 대상은 `${DOCKER_USERNAME}/hello:latest`이다. GitHub Secret 값 자체는 로컬 파일로 확인할 수 없으므로, Actions의 실제 계정은 `DOCKER_USERNAME`에 등록된 값으로 결정된다.

이미지 이름, 컨테이너 이름, WAR 이름은 서로 다르다.

```text
hello-1.0.0.war        = Maven으로 생성하는 애플리케이션 파일
ooooocj/hello:latest   = Docker Hub에 저장하는 이미지 이름(위 계정 기준)
hello_app             = EC2에서 실행되는 컨테이너 이름
```

## 2. 전체 빌드·배포 흐름

```text
내 PC에서 파일 수정·커밋
    ↓ GitHub main에 push
GitHub Actions: 임시 Ubuntu runner
    ├─ checkout: 소스 다운로드
    ├─ mvn clean package: WAR 생성
    ├─ SSH 키 파일 생성·ssh-agent 설정·서버 지문 등록
    ├─ Docker Hub 로그인
    ├─ docker build -t .../hello:latest .
    └─ docker push .../hello:latest
    ↓ SSH 접속
EC2: Ubuntu 서버
    ├─ Docker Hub 로그인
    ├─ docker stop hello_app
    ├─ docker rm hello_app
    ├─ docker rmi .../hello:latest
    └─ docker run -dit --name hello_app -p 80:8080 .../hello:latest
          ↓
     Tomcat + Java + ROOT.war 실행
          ↓ JDBC
     EC2에 직접 설치한 MySQL / kosa_db
```

GitHub-hosted runner는 작업을 수행하는 빌드 환경이고, EC2는 배포된 서비스를 실행하는 서버다. 일반적인 GitHub-hosted 작업은 새 가상머신에서 실행된다. [GitHub runner 공식 설명](https://docs.github.com/en/actions/reference/runners/github-hosted-runners)

최종 방식에서는 Maven 빌드를 Dockerfile 안에서 하지 않는다. Actions가 먼저 WAR를 만들고, Dockerfile은 만들어진 WAR를 Tomcat 이미지 안으로 복사한다. EC2에는 애플리케이션 실행용 Java와 Tomcat을 따로 설치하지 않고, Docker와 DB용 MySQL을 설치했다.

## 3. 커밋·push·pull 내역

Git에 남아 있는 커밋은 총 10개다. 원격 브랜치 reflog에는 9개 커밋에 대한 push 갱신 기록과, `main.yml` 생성 커밋을 가져온 pull 기록이 있다.

| 커밋 시각 | 커밋 | 메시지 | 실제 변경 내용 | 원격 브랜치 갱신 |
|---|---|---|---|---|
| 09:38:42 | [`a6fcc80`](https://github.com/cjoh0407/hello/commit/a6fcc8042cdacd07e9ea2bd9441acceb6f35e14a) | first commit | 프로젝트 최초 등록. Dockerfile, `.dockerignore`, 소스, 설정, WAR 등 210개 파일 추가 | 09:39:00 push |
| 10:34:48 | [`3e708d1`](https://github.com/cjoh0407/hello/commit/3e708d116fc98ac32f0e67e49c588ae2c3d307a7) | Create main.yml | `.github/workflows/main.yml` 생성. 당시 내용은 `#` 한 줄 | 10:35:11 pull / fast-forward |
| 11:33:39 | [`be04213`](https://github.com/cjoh0407/hello/commit/be0421309004c328526fccd9c83982bbb4cf12c8) | docker 이미지 빌드 및 배포 | Docker workflow 추가, 기존 WAR 배포 workflow 구성, Dockerfile 단일 단계로 변경, DB 설정 변경, Eclipse 설정 및 생성 파일 변경 | 11:33:40 push |
| 11:36:02 | [`50bc825`](https://github.com/cjoh0407/hello/commit/50bc82540af61731ed7041cf3870a5f6880eedb2) | docker 빌드 파일 수정 | Dockerfile의 `COPY myapp.war`를 실제 생성 경로 `COPY target/hello-1.0.0.war`로 수정 | 11:36:03 push |
| 11:38:15 | [`0e44a42`](https://github.com/cjoh0407/hello/commit/0e44a42e7efca3d4c77392f4761bd4e97ef4100f) | docker hub 이미지 저장시 -t 삭제 | `docker push -t ...`에서 잘못된 `-t` 옵션 제거 | 11:38:16 push |
| 11:39:49 | [`19e4a1d`](https://github.com/cjoh0407/hello/commit/19e4a1d440a34fcf22cd80d34d4cea78c34fa84d) | docker 배포 기능 수정 | 별도 `deploy` job 선언을 주석 처리하여 SSH 배포 단계를 `build` job 안에 배치 | 11:39:50 push |
| 11:48:04 | [`2568259`](https://github.com/cjoh0407/hello/commit/25682598c14d206422d16cfb6db2736f0710b287) | 다시 한 번 검사 | workflow에 `#` 주석 한 줄 추가. 실행 동작을 바꾸는 수정은 없음 | 11:48:06 push |
| 12:08:06 | [`8fbc3ba`](https://github.com/cjoh0407/hello/commit/8fbc3ba) | hello.war + | `.dockerignore` 삭제, WAR와 WAR 전개 결과·클래스·리소스·구형 MySQL JAR 갱신 | 12:08:11 push |
| 12:14:50 | [`e8a24a1`](https://github.com/cjoh0407/hello/commit/e8a24a15534d5a2761d4ceb30beb7e397221640b) | ip 주소 다시 세팅 | 배포 대상 `SERVER_IP`를 `3.34.96.62`에서 `43.203.249.65`로 변경 | 12:14:51 push |
| 12:23:21 | [`423bd12`](https://github.com/cjoh0407/hello/commit/423bd12) | pom 에 mysql 버전 재 설정 | MySQL JDBC 의존성을 5.1.31에서 `mysql-connector-j:8.0.33`으로 교체. Maven/Eclipse 생성 메타데이터도 변경 | 12:23:22 push |

확인 시점의 로컬 `main`과 로컬에 기록된 `origin/main`은 모두 `423bd12`를 가리켰고, 정리 파일 생성 전 작업 폴더에는 미커밋 변경이 없었다. GitHub 서버의 현재 상태를 새로 fetch해서 확인한 것은 아니다.

첨부한 PowerShell 기록에는 `git add`, `git commit`, `git push` 명령 원문이 없다. 따라서 실제로 어떤 CLI 옵션이나 Eclipse 메뉴를 사용했는지는 단정하지 않았다. 위 표의 커밋은 Git log, push·pull 시각은 원격 브랜치 reflog에서 확인한 사실이다. 여러 push 항목의 reflog 메시지는 `push: forced-update`지만, 이 문구만으로 사용자가 `git push --force`를 입력했다고 단정할 수는 없다.

## 4. 변경한 파일과 변경 이유

최초 커밋에는 210개 파일이 등록되었으며, 그중 143개는 `target/` 빌드 결과물이다. 최초 커밋 이후 HEAD까지 순변경이 있는 파일은 30개다. 이 중 8개는 프로젝트 설정·배포 관련 파일이고, 22개는 `target/` 결과물이다. 전체 경로와 커밋별 목록은 별도 부록에 수록했다.

### 4-1. `.github/workflows/docker.yml`: 자동 빌드·Docker 배포

`main`에 push하면 아래 단계를 순서대로 실행한다.

| 순서 | 실제 설정 | 역할 |
|---|---|---|
| 1 | `actions/checkout@v3` | 저장소 소스를 runner에 가져옴 |
| 2 | `mvn clean package` | 이전 Maven 결과를 정리하고 WAR 생성 |
| 3 | `SERVER_SSH_KEY` → `~/.ssh/id_rsa`, `chmod 600` | EC2 접속용 개인키 파일 준비 |
| 4 | `webfactory/ssh-agent@v0.5.3` | 개인키를 SSH agent에 등록 |
| 5 | `ssh-keyscan ... >> ~/.ssh/known_hosts` | 배포 서버 SSH 호스트 키 등록 |
| 6 | `docker/login-action@v1` | runner에서 Docker Hub 로그인 |
| 7 | `docker build -t .../hello:latest .` | WAR가 들어 있는 이미지 생성 |
| 8 | `docker push .../hello:latest` | 이미지를 Docker Hub에 업로드 |
| 9 | SSH + 여러 Docker 명령 | EC2의 컨테이너 교체 |

현재 YAML에서 참조하는 GitHub Secret 이름은 다음 3개다. 이전 안내의 `DOCKERHUB_USERNAME`, `DOCKERHUB_TOKEN`이 아니라 아래 이름을 사용한다.

| Secret | 용도 |
|---|---|
| `SERVER_SSH_KEY` | EC2 SSH 개인키 내용 |
| `DOCKER_USERNAME` | Docker Hub 아이디 |
| `DOCKER_PASSWORD` | Docker Hub 로그인 비밀번호 자리에 사용하는 값. 실습에서는 Access Token 사용 |

현재 `pom.xml`에는 `<skipTests>true</skipTests>` 설정이 있다. 따라서 `mvn clean package` 성공을 자동 테스트 통과로 해석하지 않는다.

처음에는 `build`와 `deploy`가 서로 다른 job이었다. GitHub-hosted job은 별도 runner에서 실행되므로 앞 job의 `~/.ssh/id_rsa`가 다음 job에 자동으로 생기지 않는다. 수정 후에는 키를 준비한 runner 안에서 SSH 배포까지 이어서 실행한다. 이는 코드 변화에 따른 설명이며, 당시 Actions 실패 로그는 첨부되지 않았다.

### 4-2. `Dockerfile`: 생성된 WAR를 실행 이미지에 포함

최초 버전은 Maven 빌드 단계와 Tomcat 실행 단계가 있는 다단계 Dockerfile이었다. 최종 버전은 Actions에서 생성한 WAR를 복사하는 단일 단계다.

```dockerfile
FROM tomcat:9.0-jdk11-temurin

RUN rm -rf /usr/local/tomcat/webapps/*

COPY target/hello-1.0.0.war /usr/local/tomcat/webapps/ROOT.war

EXPOSE 8080

CMD ["catalina.sh", "run"]
```

| 항목 | 의미 |
|---|---|
| `FROM` | Java 11과 Tomcat 9이 포함된 기반 이미지 사용 |
| `RUN rm -rf ...` | 기반 이미지의 기존 웹앱 제거 |
| `COPY ... ROOT.war` | 내 WAR를 루트 웹앱으로 배치 |
| `EXPOSE 8080` | 컨테이너 웹 포트 명시. 이것만으로 EC2 포트를 공개하지는 않음 |
| `CMD` | 컨테이너 시작 시 Tomcat 실행 |

`myapp.war`는 실제 생성 파일명이 아니어서 `target/hello-1.0.0.war`로 수정했다. `ROOT.war`이므로 최종 접속 경로는 `/hello`가 아니라 `/`이다.

### 4-3. `.dockerignore`: WAR 복사 방식에 맞춰 삭제

최초 파일은 `target/`, `.git/`, `.github/`, IDE 설정과 환경 파일·키 파일을 빌드 컨텍스트에서 제외했다.

최종 Dockerfile은 `target/hello-1.0.0.war`를 직접 복사하므로 `target/`을 제외하면 필요한 WAR를 Docker가 읽을 수 없다. `8fbc3ba`에서 `.dockerignore`를 삭제한 것은 현재 WAR 복사 방식과 맞는다. 다만 삭제 후에는 Docker 빌드 컨텍스트에 불필요한 파일도 들어갈 수 있다. 이후 정리할 때는 `.dockerignore`를 다시 만들되 WAR를 빌드 컨텍스트에서 제외하지 않도록 구성할 수 있다.

`.dockerignore`는 Docker 빌드용 제외 규칙이다. Git 제외 규칙인 `.gitignore`와 다르며, 최초 커밋에 `target/`이 올라간 사실과도 모순되지 않는다.

### 4-4. `pom.xml`: JDBC 라이브러리 교체

기존 MySQL 의존성을 주석 처리하고 다음을 추가했다.

```xml
<dependency>
    <groupId>com.mysql</groupId>
    <artifactId>mysql-connector-j</artifactId>
    <version>8.0.33</version>
</dependency>
```

기존 라이브러리는 `mysql:mysql-connector-java:5.1.31`이었다. datasource가 `com.mysql.cj.jdbc.Driver`를 사용하도록 바뀌었지만 구형 JAR가 들어 있어 `Cannot load JDBC driver class`가 발생했다. 의존성 교체 후 다음 컨테이너 로그에는 `mysql-connector-j-8.0.33.jar`가 나타나며 오류가 `Unknown database 'kosa_db'`로 바뀌었다. 드라이버 로딩 다음 단계인 DB 연결까지 진행된 것을 확인했다.

### 4-5. `context-datasource.xml`: 실제 EC2 DB로 연결

경로: `src/main/resources/egovframework/spring/context-datasource.xml`

| 항목 | 초기 로컬 설정 | 최종 활성 설정 |
|---|---|---|
| 드라이버 | `com.mysql.jdbc.Driver` | `com.mysql.cj.jdbc.Driver` |
| DB 주소 | `127.0.0.1:13306/kosa_db` | `43.203.249.65:3306/kosa_db` |
| 계정 | `scott` | `root` |
| 설정 방식 | 로컬 값 / 한때 환경변수 방식 준비 | 실제 값을 XML에 직접 입력 |

최종 상태에서는 `${DB_URL}`, `${DB_USERNAME}`, `${DB_PASSWORD}`를 사용하는 bean이 주석 처리되어 있고, IP·계정·비밀번호가 직접 들어 있는 bean이 활성화되어 있다. `PropertySourcesPlaceholderConfigurer` 선언은 남아 있지만 현재 DB 값을 환경변수로 주입하는 구성은 아니다.

```xml
<bean id="dataSource"
      class="org.apache.commons.dbcp2.BasicDataSource"
      destroy-method="close">
    <property name="driverClassName" value="com.mysql.cj.jdbc.Driver"/>
    <property name="url" value="jdbc:mysql://43.203.249.65:3306/kosa_db"/>
    <property name="username" value="root"/>
    <property name="password" value="[실습 DB 비밀번호 생략]"/>
</bean>
```

컨테이너에서 `127.0.0.1`은 기본적으로 컨테이너 자신을 뜻한다. 최종 실습은 host 네트워크를 쓰지 않고 EC2의 실제 주소를 JDBC URL에 사용하여 연결했다.

### 4-6. `.github/workflows/main.yml`: 이전 WAR 직접 배포 실습

Maven 빌드 후 `scp`로 WAR를 서버에 전달하고, 서버의 `/opt/tomcat/webapps/`에 복사하는 이전 방식이 들어 있다. 해당 파일은 서버 `13.125.180.143`을 대상으로 한다.

현재 트리거 브랜치는 `empty`다. 주석에는 main이라고 쓰여 있지만 실제 조건은 `empty`이므로, `main` push로 동작하는 Docker 배포 workflow와 구별해야 한다. 최초 생성 커밋 당시에는 `#` 한 줄뿐이었고 실제 구성은 `be04213`에서 추가되었다.

### 4-7. Eclipse 설정과 빌드 결과물

| 파일·그룹 | 확인된 변화 |
|---|---|
| `.classpath` | `.github/workflows`를 소스 경로로 추가. JRE·테스트 경로 속성도 변경 |
| `.settings/org.eclipse.wst.common.component` | `.github/workflows`를 `/WEB-INF/classes` 배포 리소스에 추가 |
| `target/classes/docker.yml`, `main.yml` | workflow의 빌드 출력 복사본 생성·갱신 |
| `target/classes/.../context-datasource.xml` | DB 설정 복사본 갱신 |
| `target/hello-1.0.0.war` | 생성된 WAR 갱신 |
| `target/hello-1.0.0/WEB-INF/classes/` | 클래스 11개 재생성, DB 설정 갱신, workflow 복사본 추가 |
| `target/hello-1.0.0/WEB-INF/lib/mysql-connector-java-5.1.31.jar` | 12:08 커밋에서 구형 JDBC JAR 추가 |
| `target/m2e-wtp/.../pom.xml`, `pom.properties` 및 `target/maven-archiver/pom.properties` | Maven/Eclipse 생성 메타데이터 갱신 |

Java 소스와 JSP를 이후 커밋에서 수정한 기록은 없다. `.class` 변경은 소스 기능 변경을 뜻하지 않는다. 저장된 `target/` 결과물에는 구형 JDBC JAR가 남아 있지만, Actions는 최신 `pom.xml`로 `mvn clean package`를 실행하여 WAR를 다시 만든다. 컨테이너 로그에서도 신형 JAR 사용을 확인했다.

## 5. PowerShell·EC2 터미널에서 수행한 작업

첨부 기록의 시작 프롬프트는 Windows PowerShell이다. SSH 접속 후 `ubuntu@...:~$`에서 입력한 것은 EC2 Linux 명령이고, `mysql>`에서 입력한 것은 SQL이다.

### 5-1. 내 PC에서 EC2 SSH 접속

```powershell
ssh -i ~/.ssh/oti-key.pem ubuntu@43.203.249.65
```

SSH 개인키를 이용해 EC2에 접속했다. 첫 접속 시 서버 지문 확인에 `yes`를 입력하여 known_hosts에 등록했고, Ubuntu 24.04.4 LTS 로그인 화면을 확인했다. 이때 키 파일 경로는 내 PC의 경로다.

### 5-2. EC2 패키지 갱신

```bash
sudo apt update
sudo apt upgrade
```

패키지 목록을 갱신하고 설치된 패키지를 업그레이드했다. `apt upgrade`의 진행 질문에는 `y`를 입력했다. 실습 서버가 Ubuntu였으므로 이전 Amazon Linux 예시의 `yum`을 사용하지 않았다.

### 5-3. Docker 설치·상태·권한 확인

```bash
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh

sudo systemctl status docker
sudo usermod -aG docker $USER
newgrp docker
docker -v
```

설치 스크립트를 다운로드하여 실행했고, 스크립트가 Docker 서비스를 활성화·시작했다. `status`는 두 번 확인했으며 `active (running)`이 출력됐다. 현재 사용자 `ubuntu`를 docker 그룹에 추가한 뒤 `newgrp docker`로 그룹 설정을 현재 세션에 적용했다. `docker -v`에서 29.8.2를 확인했다.

### 5-4. MySQL 설치·시작·초기 설정

```bash
sudo apt update
sudo apt install mysql-server -y
sudo systemctl start mysql
sudo systemctl enable mysql
sudo mysql_secure_installation
```

초기 설정에서 비밀번호 검사 기능을 켜고 정책 수준은 `0(LOW)`를 선택했다. 익명 계정 제거, 원격 root 로그인 금지, test DB 제거, 권한 재적용 질문에 모두 `y`를 입력했다. 이후 실습 중 root의 접속 host를 `%`로 바꿨으므로 최종 root 접근 설정은 초기 보안 설정 이후에 다시 변경된 상태다.

### 5-5. MySQL root 인증 방식 변경

```bash
sudo mysql
```

MySQL 안에서 다음 SQL을 실행했다. 비밀번호는 정리본에서 생략했다.

```sql
ALTER USER 'root'@'localhost'
IDENTIFIED WITH mysql_native_password BY '[실습 DB 비밀번호]';
exit;
```

이후에는 아래 명령으로 비밀번호를 입력하여 접속했다.

```bash
sudo mysql -u root -p
```

나중에 `sudo mysql`만 실행했을 때 `using password: NO` 오류가 발생했고, `-u root -p`를 지정하여 접속에 성공했다.

### 5-6. 첫 DB·테이블 생성 시도와 실패

처음에는 아래처럼 입력했다.

```sql
CREATE DATABASE 'kosa_db'
CREATE DATABASE 'kosa_db';
```

첫 줄에 세미콜론이 없어 다음 입력이 같은 SQL에 이어졌고, DB 이름을 문자열용 작은따옴표로 감싼 문제도 있었다. SQL 문법 오류로 DB가 생성되지 않았다.

뒤이어 `USE kosa_db`는 Unknown database로 실패했고, 테이블 생성과 114개 샘플 INSERT 시도는 No database selected로 실패했다. 이 첫 번째 입력 묶음은 데이터를 넣은 성공 기록으로 계산하지 않는다.

### 5-7. Docker Hub 수동 로그인

```bash
docker login --help
docker login -u ooooocj -p [ACCESS_TOKEN]
```

실제 토큰 문자열 대신 `[ACCESS_TOKEN]`으로 표기했다. 결과는 `Login Succeeded`였다. 이는 EC2에서 로그인한 것이며, Actions runner의 `docker/login-action` 로그인과 별개다.

### 5-8. MySQL 설정 파일 편집·접속 권한 변경

```bash
sudo nano /etc/mysql/mysql.conf.d/mysqld.cnf
sudo vi /etc/mysql/mysql.conf.d/mysqld.cnf
sudo systemctl restart mysql
sudo mysql -u root -p
```

MySQL 설정 파일을 편집하고 서비스를 재시작했다. 편집 후 파일 내용 자체는 첨부 기록에 없으므로 bind-address 등의 최종 값은 확인할 수 없다.

MySQL 안에서 확인되는 권한 변경은 다음과 같다.

```sql
USE mysql;
UPDATE mysql.user SET host='%' WHERE user='root';
FLUSH PRIVILEGES;
exit;
```

`root` 계정의 접속 host를 `%`로 바꾸고 권한을 재적용했다. `%`는 특정 localhost 외의 host 접속도 허용하는 계정 범위를 뜻하지만, 방화벽과 MySQL 수신 설정까지 자동 변경하는 것은 아니다.

계정 조회 과정에서는 `selecet` 오타와 존재하지 않는 `name` 컬럼 때문에 실패했다. 올바른 확인 예시는 다음과 같으며, 이 올바른 문장을 실제로 실행한 기록은 없다.

```sql
SELECT host, user FROM mysql.user;
```

공인 IP 접속 확인도 세 번 시도했다.

```bash
sudo mysql -u root -p -h 43.203.249.65
```

각 시도는 `Ctrl+C`로 중단되어, 이 명령 자체의 접속 성공 출력은 없다.

### 5-9. Docker 상태 확인과 SSH 대상 오류

```bash
docker ps
docker images
```

해당 시점에는 컨테이너와 이미지 목록이 비어 있었다. 이것은 조회 시점의 상태이며 최종 배포 상태를 뜻하지 않는다.

EC2 안에서 다른 서버로 아래 SSH 접속도 시도했다.

```bash
ssh -i mykey.pem ubuntu@3.34.96.62
ssh -i oti-key.pem ubuntu@3.34.96.62
```

두 번 모두 해당 EC2 작업 경로에 키 파일이 없어 `Identity file ... not accessible` 경고와 `Permission denied (publickey)`가 발생했다. 내 PC에 있는 키가 EC2에도 자동으로 존재하는 것은 아니다. 이후 workflow의 배포 대상 IP도 실제 작업 중인 `43.203.249.65`로 수정했다.

### 5-10. 컨테이너 로그에서 애플리케이션 오류 확인

```bash
sudo docker logs hello --tail 100
sudo docker logs hello_app --tail 100
```

`hello`는 이미지 저장소 이름이고 실제 컨테이너 이름은 `hello_app`이어서 첫 명령은 No such container로 실패했다. `hello_app` 로그에서 아래 두 문제를 순서대로 확인했다.

1. `Cannot load JDBC driver class 'com.mysql.cj.jdbc.Driver'`
2. 의존성 변경 후 `Unknown database 'kosa_db'`

첫 오류는 `pom.xml`의 JDBC 라이브러리 교체로 해결했고, 두 번째 오류는 아래 DB 생성으로 해결했다.

### 5-11. 최종 DB 생성·테이블 생성·샘플 데이터 입력

```bash
sudo mysql -u root -p
```

```sql
SHOW DATABASES;

CREATE DATABASE kosa_db
DEFAULT CHARACTER SET utf8mb4
COLLATE utf8mb4_unicode_ci;

USE kosa_db;

CREATE TABLE SAMPLE (
    ID VARCHAR(16) NOT NULL PRIMARY KEY,
    NAME VARCHAR(50),
    DESCRIPTION VARCHAR(100),
    USE_YN CHAR(1),
    REG_USER VARCHAR(10)
);

CREATE TABLE IDS (
    TABLE_NAME VARCHAR(16) NOT NULL PRIMARY KEY,
    NEXT_ID DECIMAL(30) NOT NULL
);
```

`SHOW DATABASES`에는 당시 시스템 DB 4개만 보였고 `kosa_db`는 없었다. 올바른 CREATE DATABASE 후 `Query OK`, USE 후 `Database changed`를 확인했다.

그다음 `SAMPLE-00001`부터 `SAMPLE-00114`까지 114개 샘플 데이터를 입력했고, 각 INSERT의 `Query OK, 1 row affected`가 기록되어 있다. 아래는 전체 114개 중 첫 행의 예시다.

```sql
INSERT INTO SAMPLE VALUES (
    'SAMPLE-00001', 'Runtime Environment', 'Foundation Layer', 'Y', 'eGov'
);

INSERT INTO IDS VALUES ('SAMPLE', 115);
```

`SAMPLE`은 목록·등록 화면에서 사용하는 데이터 테이블이다. `IDS`는 전자정부프레임워크의 ID 생성 서비스가 사용하는 테이블이며, 코드의 `context-idgen.xml`도 `IDS`의 `SAMPLE` 행을 참조한다. 114번까지 샘플을 넣고 다음 ID 값으로 115를 등록했다.

프로젝트의 `src/main/resources/db/sampledb.sql`은 `CREATE MEMORY TABLE`, `SET SCHEMA PUBLIC` 등의 HSQL용 문법으로 작성되어 있다. 실습에서는 그 파일을 그대로 MySQL에 실행한 것이 아니라, MySQL용 `CREATE TABLE` 문장과 샘플 INSERT를 직접 입력했다. 해당 SQL 소스 파일을 변경한 커밋은 없다.

### 5-12. MySQL 종료 후 Linux 서비스 재시작

마지막에 `mysql>` 안에서 `sudo systemctl restart mysql`을 입력했다. 이 명령은 SQL이 아니므로 `Ctrl+C`로 취소하고 MySQL을 나왔다.

```sql
exit;
```

Linux 프롬프트에서 다시 실행했다.

```bash
sudo systemctl restart mysql
```

DB 생성 SQL이 적용되기 위해 매번 서비스를 재시작해야 하는 것은 아니다. 이 재시작은 실습 기록에 실제로 남아 있는 마지막 작업이다.

## 6. SSH 배포와 redirect 문법 이해

workflow의 실제 배포 명령은 다음과 같다.

```yaml
- name: 배포 실행
  run: |
    ssh -i ~/.ssh/id_rsa ubuntu@${{ env.SERVER_IP }} << 'EOF'
      docker login -u ${{ secrets.DOCKER_USERNAME }} -p ${{ secrets.DOCKER_PASSWORD }}
      docker stop hello_app
      docker rm hello_app
      docker rmi ${{ secrets.DOCKER_USERNAME }}/hello:latest
      docker run -dit --name hello_app -p 80:8080 ${{ secrets.DOCKER_USERNAME }}/hello:latest
    EOF
```

`<< 'EOF'`는 here-document라는 입력 리다이렉션이다. EOF 사이의 여러 줄을 SSH의 표준입력으로 전달하여 EC2의 원격 shell에서 실행하게 한다. 출력 내용을 파일로 저장하는 `>`나 `>>`와는 역할이 다르다. EOF는 끝을 표시하는 이름이다. [Bash 공식 설명](https://www.gnu.org/s/bash/manual/html_node/Redirections.html)

`'EOF'`의 따옴표는 runner shell이 본문 안의 shell 변수 등을 먼저 확장하지 않게 한다. `${{ secrets... }}`와 `${{ env... }}`는 GitHub Actions가 shell 실행 전에 처리하는 표현식이므로, 따옴표가 있어도 Actions의 값 치환은 수행된다.

| 문법 | 이 실습에서의 역할 |
|---|---|
| `run: \|` | YAML에서 여러 줄 shell 명령 작성 |
| `echo ... > ~/.ssh/id_rsa` | Secret의 개인키 내용을 파일에 기록 |
| `ssh-keyscan ... >> ~/.ssh/known_hosts` | 호스트 키를 기존 파일에 추가 |
| `ssh ... << 'EOF'` | 여러 원격 명령을 SSH 입력으로 전달 |

## 7. Docker 명령의 의미

```bash
docker build -t ooooocj/hello:latest .
docker push ooooocj/hello:latest
```

`docker build`의 `-t`는 이미지 이름·태그를 지정한다. 마지막 `.`은 현재 폴더를 빌드 컨텍스트로 사용한다는 뜻이다. `docker push`에는 `-t` 옵션을 붙이지 않고 이미지 이름을 바로 지정한다. [Docker push 공식 설명](https://docs.docker.com/reference/cli/docker/image/push/)

| 배포 명령 | 대상 | 의미 |
|---|---|---|
| `docker stop hello_app` | 컨테이너 | 실행 중인 앱 중지 |
| `docker rm hello_app` | 컨테이너 | 기존 컨테이너 삭제 |
| `docker rmi .../hello:latest` | 이미지 | EC2에 저장된 해당 이미지 태그 제거 |
| `docker run ...` | 새 컨테이너 | 이미지로 새 컨테이너 생성·실행 |

실제 workflow에는 별도의 `docker pull`이 없다. 기존 이미지 제거가 성공해 로컬에 이미지가 없으면 `docker run`이 필요한 이미지를 내려받는다. 기본 pull 정책은 `missing`이므로 이미지가 남아 있으면 항상 최신 이미지를 가져오는 것은 아니다. [Docker run 공식 설명](https://docs.docker.com/reference/cli/docker/container/run/)

최종 실행 명령:

```bash
docker run -dit --name hello_app -p 80:8080 ooooocj/hello:latest
```

| 옵션 | 의미 |
|---|---|
| `-d` | 백그라운드 실행 |
| `-i` | 표준입력 유지 |
| `-t` | 가상 터미널 할당. build의 `-t`와 의미가 다름 |
| `--name hello_app` | 컨테이너 이름 지정 |
| `-p 80:8080` | EC2 80번 → 컨테이너 8080번 연결 |

최종 명령에는 `--network host`, `--env-file`, `--restart unless-stopped`가 없다. 이전 예시와 달리 이번 실습에서 이 옵션들을 사용한 것으로 기록하지 않는다. 웹 접속 주소는 `http://43.203.249.65/`이며, 컨테이너 Tomcat은 내부에서 8080으로 실행된다.

## 8. 발생한 오류와 해결 과정

| 오류 또는 수정 대상 | 로그·코드에서 확인한 내용 | 해결·최종 상태 |
|---|---|---|
| Dockerfile의 `myapp.war` | 실제 Maven 산출물 이름과 불일치 | `target/hello-1.0.0.war`로 변경 |
| `docker push -t ...` | push에 build용 옵션 사용 | `-t` 삭제 |
| 배포 job 분리 | 키 파일이 build runner에만 준비되는 구성 | 배포 step을 같은 build job으로 통합. 당시 실패 출력은 없음 |
| WAR를 빌드 컨텍스트에서 제외 | `.dockerignore`의 `target/` 제외와 COPY 경로 충돌 | `.dockerignore` 삭제 |
| 배포 대상 IP 불일치 | workflow는 `3.34.96.62`, 실제 작업 서버는 `43.203.249.65` | `SERVER_IP` 수정 |
| SQL 문법 오류 1064 | CREATE DATABASE 작은따옴표·세미콜론 누락, SELECT 오타 | 올바른 CREATE DATABASE로 다시 생성. SELECT 수정 예시는 문서에 설명 |
| Unknown column `name` | `mysql.user`에 `name` 컬럼 없음 | 조회할 컬럼은 `user`. 올바른 조회의 실제 실행 기록은 없음 |
| No database selected 1046 | DB 생성·USE 실패 후 SQL 실행 | `kosa_db` 생성 → USE → 테이블·데이터 재입력 |
| Access denied 1045 / password: NO | 비밀번호 없이 root 접속 | `sudo mysql -u root -p`로 접속 |
| Permission denied (publickey) | EC2에 지정한 개인키 파일이 없음 | 올바른 서버 IP·키 위치 필요. 해당 접속 시도는 실패로 남음 |
| No such container: hello | 이미지 이름을 컨테이너 이름으로 사용 | 로그 대상 `hello_app` 사용 |
| Cannot load JDBC driver | 신형 드라이버 클래스와 구형 5.1.31 JAR 불일치 | Connector/J 8.0.33으로 교체·재빌드·재배포 |
| Unknown database `kosa_db` | MySQL 서버에 실제 DB 없음 | DB·SAMPLE·IDS 생성, 샘플 데이터 입력 |
| MySQL 안에서 systemctl 입력 | SQL 프롬프트에서 Linux 명령 입력 | Ctrl+C → exit → Linux 프롬프트에서 실행 |

## 9. 확인한 결과와 기록의 범위

| 항목 | 확인 근거 |
|---|---|
| Git 커밋과 원격 브랜치 갱신 | Git log 및 reflog |
| Docker 서비스 실행 | `systemctl status docker`의 active (running) |
| Docker Hub 수동 로그인 | Login Succeeded |
| 컨테이너·애플리케이션 실행 시도 | `hello_app`의 Tomcat·Spring 예외 로그 |
| 신형 JDBC JAR 사용 | 로그에 `mysql-connector-j-8.0.33.jar` 표시 |
| DB 생성 성공 | CREATE DATABASE의 Query OK / USE의 Database changed |
| 테이블·샘플 입력 성공 | SAMPLE·IDS 생성 및 INSERT의 Query OK |
| 최종 브라우저 접속 | 실습자가 정상 배포·접속을 확인했다고 설명 |

첨부 기록에는 최종 `docker ps` 결과, 최종 HTTP 200 응답, 브라우저 화면, 전체 Actions 실행 결과, EC2 보안 그룹 설정 화면이 없다. 따라서 그런 출력이나 설정 값을 새로 만들어 성공 증거처럼 넣지 않았다. MySQL 설정 파일의 실제 변경값과 로컬에서 사용한 Git 명령 원문도 확인 범위 밖이다.

## 10. 실습을 통해 이해한 점

1. GitHub Actions runner와 EC2는 역할이 다른 서버다. runner에서 빌드하고 EC2에서 실행한다.
2. 소스코드를 Maven으로 WAR로 만들고, 그 WAR를 Docker 이미지에 넣는다.
3. Docker Hub 로그인은 runner와 EC2에서 각각 필요하다. 공개 이미지의 pull은 로그인 없이도 가능하지만, 이번 workflow는 EC2에서도 로그인한다.
4. GitHub Secrets의 이름은 YAML이 참조하는 이름과 정확히 일치해야 한다.
5. 이미지와 컨테이너는 다르다. stop·rm은 컨테이너, rmi는 이미지를 대상으로 한다.
6. Java·Tomcat은 실행 이미지에 들어 있어 EC2에 직접 설치하지 않아도 된다. 이번 DB는 별도로 EC2에 설치했다.
7. 컨테이너 실행 성공과 웹앱 정상 동작은 다르다. JDBC 라이브러리, DB·테이블 준비까지 맞아야 한다.
8. 같은 `-t`라도 build에서는 태그, run에서는 터미널이라는 뜻이다.
9. `<< 'EOF'`로 여러 배포 명령을 SSH에 전달할 수 있다.
10. 오류 해결을 커밋·push하면 수정된 workflow와 소스로 다시 빌드·배포할 수 있다.

## 11. 다음 실습에서 개선할 부분

이 항목은 실제로 완료한 작업과 구별되는 개선 계획이다. 정리 과정에서 코드를 수정하지 않았다.

- 이미지 제거에 다운로드를 의존하기보다 먼저 `docker pull`하거나 `--pull always`를 사용하여 새 이미지 확보를 명확히 한다.
- 배포에 실패했을 때 runner에 실패를 제대로 전달하도록 원격 스크립트의 오류 처리를 추가한다. 현재 첫 배포의 stop·rm 실패 등 중간 오류가 그대로 다음 명령으로 이어질 수 있다.
- 재시작 정책과 HTTP 응답 확인을 추가한다. 현재 컨테이너 실행 후 자동 상태 검사는 없다.
- DB 값을 환경변수로 분리하고 애플리케이션 전용 DB 계정을 사용한다. 현재 실습은 root와 직접 입력한 비밀번호를 사용한다.
- workflow를 Eclipse 소스·WAR 배포 경로에서 제외한다. `.github/workflows`는 GitHub 설정이며 웹앱 리소스로 포함할 필요가 없다.
- `.dockerignore`를 WAR 복사를 허용하는 구성으로 복원하고, `.gitignore` 및 기존 추적 상태를 함께 정리하여 생성 파일을 소스 변경과 구분한다.

원본 터미널 기록에 Docker Hub 토큰 값이 직접 남아 있으므로 이 문서에는 복사하지 않았다. 원본 기록을 외부에 공유한다면 토큰을 제거하고, 이미 공유한 토큰은 교체하는 것이 좋다.

## 12. 노션에 사용할 실습 소개 문장

> 전자정부프레임워크 프로젝트의 Docker 기반 CI/CD를 실습했다. GitHub main 브랜치에 push하면 GitHub Actions가 Maven으로 WAR를 빌드하고, Tomcat·Java 기반 Docker 이미지를 생성하여 Docker Hub에 업로드한다. 이어서 SSH로 Ubuntu EC2에 접속해 기존 컨테이너와 이미지를 제거하고 새 컨테이너를 실행하도록 구성했다. 배포 중 Docker 명령 옵션, WAR 복사 경로, 서버 IP, JDBC 라이브러리, MySQL DB 생성 문제를 수정했고, 최종 웹 화면 접속을 확인했다.

전체 변경 파일, 커밋별 파일 내역, 터미널 명령 목록과 최종 workflow 원문은 함께 제공한 `2026-10-07-Docker-CICD-전체기록.md`에 있다.
