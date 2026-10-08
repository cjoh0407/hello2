# Docker CI/CD 실습 전체 기록 부록

기준: 2026-10-07, HEAD 423bd12. 본문은 2026-10-07-Docker-CICD-실습정리.md. 경로·커밋 목록은 Git에서 추출했으며 토큰·비밀번호는 생략했다.

## 1. 최초 커밋에 등록한 전체 파일 — 210개

a6fcc80의 전체 파일이다. 기존 프로젝트와 생성 파일을 처음 Git에 등록한 내역이며, 모두 이번 실습에서 새로 코딩한 파일이라는 뜻은 아니다. target/ 결과물은 143개다.

```text
A	.classpath
A	.dockerignore
A	.project
A	.settings/.jsdtscope
A	.settings/org.eclipse.core.resources.prefs
A	.settings/org.eclipse.jdt.core.prefs
A	.settings/org.eclipse.wst.common.component
A	.settings/org.eclipse.wst.common.project.facet.core.xml
A	.settings/org.eclipse.wst.jsdt.ui.superType.container
A	.settings/org.eclipse.wst.jsdt.ui.superType.name
A	.settings/org.eclipse.wst.validation.prefs
A	Dockerfile
A	pom.xml
A	src/main/java/egovframework/example/cmmn/EgovSampleExcepHndlr.java
A	src/main/java/egovframework/example/cmmn/EgovSampleOthersExcepHndlr.java
A	src/main/java/egovframework/example/cmmn/web/EgovBindingInitializer.java
A	src/main/java/egovframework/example/cmmn/web/EgovImgPaginationRenderer.java
A	src/main/java/egovframework/example/sample/service/EgovSampleService.java
A	src/main/java/egovframework/example/sample/service/SampleDefaultVO.java
A	src/main/java/egovframework/example/sample/service/SampleVO.java
A	src/main/java/egovframework/example/sample/service/impl/EgovSampleServiceImpl.java
A	src/main/java/egovframework/example/sample/service/impl/SampleDAO.java
A	src/main/java/egovframework/example/sample/service/impl/SampleMapper.java
A	src/main/java/egovframework/example/sample/web/EgovSampleController.java
A	src/main/resources/db/sampledb.sql
A	src/main/resources/egovframework/message/message-common.properties
A	src/main/resources/egovframework/message/message-common_en.properties
A	src/main/resources/egovframework/message/message-common_ko.properties
A	src/main/resources/egovframework/spring/context-aspect.xml
A	src/main/resources/egovframework/spring/context-common.xml
A	src/main/resources/egovframework/spring/context-datasource.xml
A	src/main/resources/egovframework/spring/context-idgen.xml
A	src/main/resources/egovframework/spring/context-mapper.xml
A	src/main/resources/egovframework/spring/context-properties.xml
A	src/main/resources/egovframework/spring/context-sqlMap.xml
A	src/main/resources/egovframework/spring/context-transaction.xml
A	src/main/resources/egovframework/spring/context-validator.xml
A	src/main/resources/egovframework/sqlmap/example/mappers/EgovSample_Sample_SQL.xml
A	src/main/resources/egovframework/sqlmap/example/sample/EgovSample_Sample_SQL.xml
A	src/main/resources/egovframework/sqlmap/example/sql-map-config.xml
A	src/main/resources/egovframework/sqlmap/example/sql-mapper-config.xml
A	src/main/resources/log4j2.xml
A	src/main/webapp/META-INF/MANIFEST.MF
A	src/main/webapp/WEB-INF/config/egovframework/springmvc/dispatcher-servlet.xml
A	src/main/webapp/WEB-INF/config/egovframework/validator/validator-rules.xml
A	src/main/webapp/WEB-INF/config/egovframework/validator/validator.xml
A	src/main/webapp/WEB-INF/jsp/egovframework/example/cmmn/dataAccessFailure.jsp
A	src/main/webapp/WEB-INF/jsp/egovframework/example/cmmn/egovBizException.jsp
A	src/main/webapp/WEB-INF/jsp/egovframework/example/cmmn/egovError.jsp
A	src/main/webapp/WEB-INF/jsp/egovframework/example/cmmn/transactionFailure.jsp
A	src/main/webapp/WEB-INF/jsp/egovframework/example/cmmn/validator.jsp
A	src/main/webapp/WEB-INF/jsp/egovframework/example/sample/egovSampleList.jsp
A	src/main/webapp/WEB-INF/jsp/egovframework/example/sample/egovSampleRegister.jsp
A	src/main/webapp/WEB-INF/web.xml
A	src/main/webapp/common/error.jsp
A	src/main/webapp/css/egovframework/sample.css
A	src/main/webapp/images/egovframework/cmmn/btn_page_next1.gif
A	src/main/webapp/images/egovframework/cmmn/btn_page_next10.gif
A	src/main/webapp/images/egovframework/cmmn/btn_page_pre1.gif
A	src/main/webapp/images/egovframework/cmmn/btn_page_pre10.gif
A	src/main/webapp/images/egovframework/example/btn_bg_l.gif
A	src/main/webapp/images/egovframework/example/btn_bg_r.gif
A	src/main/webapp/images/egovframework/example/civilappeal_topmn_bg.jpg
A	src/main/webapp/images/egovframework/example/paging_line.gif
A	src/main/webapp/images/egovframework/example/th_bg.gif
A	src/main/webapp/images/egovframework/example/title_dot.gif
A	src/main/webapp/index.jsp
A	target/classes/db/sampledb.sql
A	target/classes/egovframework/example/cmmn/EgovSampleExcepHndlr.class
A	target/classes/egovframework/example/cmmn/EgovSampleOthersExcepHndlr.class
A	target/classes/egovframework/example/cmmn/web/EgovBindingInitializer.class
A	target/classes/egovframework/example/cmmn/web/EgovImgPaginationRenderer.class
A	target/classes/egovframework/example/sample/service/EgovSampleService.class
A	target/classes/egovframework/example/sample/service/SampleDefaultVO.class
A	target/classes/egovframework/example/sample/service/SampleVO.class
A	target/classes/egovframework/example/sample/service/impl/EgovSampleServiceImpl.class
A	target/classes/egovframework/example/sample/service/impl/SampleDAO.class
A	target/classes/egovframework/example/sample/service/impl/SampleMapper.class
A	target/classes/egovframework/example/sample/web/EgovSampleController.class
A	target/classes/egovframework/message/message-common.properties
A	target/classes/egovframework/message/message-common_en.properties
A	target/classes/egovframework/message/message-common_ko.properties
A	target/classes/egovframework/spring/context-aspect.xml
A	target/classes/egovframework/spring/context-common.xml
A	target/classes/egovframework/spring/context-datasource.xml
A	target/classes/egovframework/spring/context-idgen.xml
A	target/classes/egovframework/spring/context-mapper.xml
A	target/classes/egovframework/spring/context-properties.xml
A	target/classes/egovframework/spring/context-sqlMap.xml
A	target/classes/egovframework/spring/context-transaction.xml
A	target/classes/egovframework/spring/context-validator.xml
A	target/classes/egovframework/sqlmap/example/mappers/EgovSample_Sample_SQL.xml
A	target/classes/egovframework/sqlmap/example/sample/EgovSample_Sample_SQL.xml
A	target/classes/egovframework/sqlmap/example/sql-map-config.xml
A	target/classes/egovframework/sqlmap/example/sql-mapper-config.xml
A	target/classes/log4j2.xml
A	target/hello-1.0.0.war
A	target/hello-1.0.0/META-INF/MANIFEST.MF
A	target/hello-1.0.0/WEB-INF/classes/db/sampledb.sql
A	target/hello-1.0.0/WEB-INF/classes/egovframework/example/cmmn/EgovSampleExcepHndlr.class
A	target/hello-1.0.0/WEB-INF/classes/egovframework/example/cmmn/EgovSampleOthersExcepHndlr.class
A	target/hello-1.0.0/WEB-INF/classes/egovframework/example/cmmn/web/EgovBindingInitializer.class
A	target/hello-1.0.0/WEB-INF/classes/egovframework/example/cmmn/web/EgovImgPaginationRenderer.class
A	target/hello-1.0.0/WEB-INF/classes/egovframework/example/sample/service/EgovSampleService.class
A	target/hello-1.0.0/WEB-INF/classes/egovframework/example/sample/service/SampleDefaultVO.class
A	target/hello-1.0.0/WEB-INF/classes/egovframework/example/sample/service/SampleVO.class
A	target/hello-1.0.0/WEB-INF/classes/egovframework/example/sample/service/impl/EgovSampleServiceImpl.class
A	target/hello-1.0.0/WEB-INF/classes/egovframework/example/sample/service/impl/SampleDAO.class
A	target/hello-1.0.0/WEB-INF/classes/egovframework/example/sample/service/impl/SampleMapper.class
A	target/hello-1.0.0/WEB-INF/classes/egovframework/example/sample/web/EgovSampleController.class
A	target/hello-1.0.0/WEB-INF/classes/egovframework/message/message-common.properties
A	target/hello-1.0.0/WEB-INF/classes/egovframework/message/message-common_en.properties
A	target/hello-1.0.0/WEB-INF/classes/egovframework/message/message-common_ko.properties
A	target/hello-1.0.0/WEB-INF/classes/egovframework/spring/context-aspect.xml
A	target/hello-1.0.0/WEB-INF/classes/egovframework/spring/context-common.xml
A	target/hello-1.0.0/WEB-INF/classes/egovframework/spring/context-datasource.xml
A	target/hello-1.0.0/WEB-INF/classes/egovframework/spring/context-idgen.xml
A	target/hello-1.0.0/WEB-INF/classes/egovframework/spring/context-mapper.xml
A	target/hello-1.0.0/WEB-INF/classes/egovframework/spring/context-properties.xml
A	target/hello-1.0.0/WEB-INF/classes/egovframework/spring/context-sqlMap.xml
A	target/hello-1.0.0/WEB-INF/classes/egovframework/spring/context-transaction.xml
A	target/hello-1.0.0/WEB-INF/classes/egovframework/spring/context-validator.xml
A	target/hello-1.0.0/WEB-INF/classes/egovframework/sqlmap/example/mappers/EgovSample_Sample_SQL.xml
A	target/hello-1.0.0/WEB-INF/classes/egovframework/sqlmap/example/sample/EgovSample_Sample_SQL.xml
A	target/hello-1.0.0/WEB-INF/classes/egovframework/sqlmap/example/sql-map-config.xml
A	target/hello-1.0.0/WEB-INF/classes/egovframework/sqlmap/example/sql-mapper-config.xml
A	target/hello-1.0.0/WEB-INF/classes/log4j2.xml
A	target/hello-1.0.0/WEB-INF/config/egovframework/springmvc/dispatcher-servlet.xml
A	target/hello-1.0.0/WEB-INF/config/egovframework/validator/validator-rules.xml
A	target/hello-1.0.0/WEB-INF/config/egovframework/validator/validator.xml
A	target/hello-1.0.0/WEB-INF/jsp/egovframework/example/cmmn/dataAccessFailure.jsp
A	target/hello-1.0.0/WEB-INF/jsp/egovframework/example/cmmn/egovBizException.jsp
A	target/hello-1.0.0/WEB-INF/jsp/egovframework/example/cmmn/egovError.jsp
A	target/hello-1.0.0/WEB-INF/jsp/egovframework/example/cmmn/transactionFailure.jsp
A	target/hello-1.0.0/WEB-INF/jsp/egovframework/example/cmmn/validator.jsp
A	target/hello-1.0.0/WEB-INF/jsp/egovframework/example/sample/egovSampleList.jsp
A	target/hello-1.0.0/WEB-INF/jsp/egovframework/example/sample/egovSampleRegister.jsp
A	target/hello-1.0.0/WEB-INF/lib/ST4-4.0.7.jar
A	target/hello-1.0.0/WEB-INF/lib/activation-1.1.jar
A	target/hello-1.0.0/WEB-INF/lib/antlr-2.7.7.jar
A	target/hello-1.0.0/WEB-INF/lib/antlr-3.5.jar
A	target/hello-1.0.0/WEB-INF/lib/antlr-runtime-3.5.jar
A	target/hello-1.0.0/WEB-INF/lib/aspectjrt-1.9.5.jar
A	target/hello-1.0.0/WEB-INF/lib/aspectjweaver-1.9.5.jar
A	target/hello-1.0.0/WEB-INF/lib/commons-beanutils-1.9.4.jar
A	target/hello-1.0.0/WEB-INF/lib/commons-collections-3.2.2.jar
A	target/hello-1.0.0/WEB-INF/lib/commons-dbcp2-2.4.0.jar
A	target/hello-1.0.0/WEB-INF/lib/commons-digester-2.1.jar
A	target/hello-1.0.0/WEB-INF/lib/commons-lang3-3.10.jar
A	target/hello-1.0.0/WEB-INF/lib/commons-logging-1.2.jar
A	target/hello-1.0.0/WEB-INF/lib/commons-pool2-2.5.0.jar
A	target/hello-1.0.0/WEB-INF/lib/commons-validator-1.7.jar
A	target/hello-1.0.0/WEB-INF/lib/hsqldb-2.5.0.jar
A	target/hello-1.0.0/WEB-INF/lib/ibatis-sqlmap-2.3.4.726.jar
A	target/hello-1.0.0/WEB-INF/lib/javaee-api-7.0.jar
A	target/hello-1.0.0/WEB-INF/lib/javax.annotation-api-1.3.2.jar
A	target/hello-1.0.0/WEB-INF/lib/javax.mail-1.5.0.jar
A	target/hello-1.0.0/WEB-INF/lib/jcl-over-slf4j-1.7.30.jar
A	target/hello-1.0.0/WEB-INF/lib/jstl-1.2.jar
A	target/hello-1.0.0/WEB-INF/lib/log4j-api-2.17.1.jar
A	target/hello-1.0.0/WEB-INF/lib/log4j-core-2.17.1.jar
A	target/hello-1.0.0/WEB-INF/lib/log4j-over-slf4j-1.7.30.jar
A	target/hello-1.0.0/WEB-INF/lib/log4j-slf4j-impl-2.17.1.jar
A	target/hello-1.0.0/WEB-INF/lib/log4jdbc-1.2.jar
A	target/hello-1.0.0/WEB-INF/lib/mariadb-java-client-3.5.10.jar
A	target/hello-1.0.0/WEB-INF/lib/mybatis-3.5.3.jar
A	target/hello-1.0.0/WEB-INF/lib/mybatis-spring-2.0.3.jar
A	target/hello-1.0.0/WEB-INF/lib/org.egovframe.rte.fdl.cmmn-4.0.0.jar
A	target/hello-1.0.0/WEB-INF/lib/org.egovframe.rte.fdl.idgnr-4.0.0.jar
A	target/hello-1.0.0/WEB-INF/lib/org.egovframe.rte.fdl.logging-4.0.0.jar
A	target/hello-1.0.0/WEB-INF/lib/org.egovframe.rte.fdl.property-4.0.0.jar
A	target/hello-1.0.0/WEB-INF/lib/org.egovframe.rte.psl.dataaccess-4.0.0.jar
A	target/hello-1.0.0/WEB-INF/lib/org.egovframe.rte.ptl.mvc-4.0.0.jar
A	target/hello-1.0.0/WEB-INF/lib/slf4j-api-1.7.25.jar
A	target/hello-1.0.0/WEB-INF/lib/spring-aop-5.3.6.jar
A	target/hello-1.0.0/WEB-INF/lib/spring-beans-5.3.6.jar
A	target/hello-1.0.0/WEB-INF/lib/spring-context-5.3.6.jar
A	target/hello-1.0.0/WEB-INF/lib/spring-context-support-5.3.6.jar
A	target/hello-1.0.0/WEB-INF/lib/spring-core-5.3.6.jar
A	target/hello-1.0.0/WEB-INF/lib/spring-expression-5.3.6.jar
A	target/hello-1.0.0/WEB-INF/lib/spring-jcl-5.3.6.jar
A	target/hello-1.0.0/WEB-INF/lib/spring-jdbc-5.3.6.jar
A	target/hello-1.0.0/WEB-INF/lib/spring-modules-validation-0.9.jar
A	target/hello-1.0.0/WEB-INF/lib/spring-orm-5.3.6.jar
A	target/hello-1.0.0/WEB-INF/lib/spring-tx-5.3.6.jar
A	target/hello-1.0.0/WEB-INF/lib/spring-web-5.3.6.jar
A	target/hello-1.0.0/WEB-INF/lib/spring-webmvc-5.3.6.jar
A	target/hello-1.0.0/WEB-INF/lib/standard-1.1.2.jar
A	target/hello-1.0.0/WEB-INF/lib/stringtemplate-3.2.1.jar
A	target/hello-1.0.0/WEB-INF/web.xml
A	target/hello-1.0.0/common/error.jsp
A	target/hello-1.0.0/css/egovframework/sample.css
A	target/hello-1.0.0/images/egovframework/cmmn/btn_page_next1.gif
A	target/hello-1.0.0/images/egovframework/cmmn/btn_page_next10.gif
A	target/hello-1.0.0/images/egovframework/cmmn/btn_page_pre1.gif
A	target/hello-1.0.0/images/egovframework/cmmn/btn_page_pre10.gif
A	target/hello-1.0.0/images/egovframework/example/btn_bg_l.gif
A	target/hello-1.0.0/images/egovframework/example/btn_bg_r.gif
A	target/hello-1.0.0/images/egovframework/example/civilappeal_topmn_bg.jpg
A	target/hello-1.0.0/images/egovframework/example/paging_line.gif
A	target/hello-1.0.0/images/egovframework/example/th_bg.gif
A	target/hello-1.0.0/images/egovframework/example/title_dot.gif
A	target/hello-1.0.0/index.jsp
A	target/m2e-wtp/web-resources/META-INF/MANIFEST.MF
A	target/m2e-wtp/web-resources/META-INF/maven/kr.co.oti/hello/pom.properties
A	target/m2e-wtp/web-resources/META-INF/maven/kr.co.oti/hello/pom.xml
A	target/maven-archiver/pom.properties
A	target/maven-status/maven-compiler-plugin/compile/default-compile/createdFiles.lst
A	target/maven-status/maven-compiler-plugin/compile/default-compile/inputFiles.lst
A	target/maven-status/maven-compiler-plugin/testCompile/default-testCompile/inputFiles.lst
```

## 2. 최초 커밋 이후 최종 상태까지 변경한 전체 파일 — 30개

A = 추가 / M = 수정 / D = 삭제. target/ 밖 8개, target/ 안 22개. 최초 커밋과 HEAD의 순변경이다.

```text
M	.classpath
D	.dockerignore
A	.github/workflows/docker.yml
A	.github/workflows/main.yml
M	.settings/org.eclipse.wst.common.component
M	Dockerfile
M	pom.xml
M	src/main/resources/egovframework/spring/context-datasource.xml
A	target/classes/docker.yml
M	target/classes/egovframework/spring/context-datasource.xml
A	target/classes/main.yml
M	target/hello-1.0.0.war
A	target/hello-1.0.0/WEB-INF/classes/docker.yml
M	target/hello-1.0.0/WEB-INF/classes/egovframework/example/cmmn/EgovSampleExcepHndlr.class
M	target/hello-1.0.0/WEB-INF/classes/egovframework/example/cmmn/EgovSampleOthersExcepHndlr.class
M	target/hello-1.0.0/WEB-INF/classes/egovframework/example/cmmn/web/EgovBindingInitializer.class
M	target/hello-1.0.0/WEB-INF/classes/egovframework/example/cmmn/web/EgovImgPaginationRenderer.class
M	target/hello-1.0.0/WEB-INF/classes/egovframework/example/sample/service/EgovSampleService.class
M	target/hello-1.0.0/WEB-INF/classes/egovframework/example/sample/service/SampleDefaultVO.class
M	target/hello-1.0.0/WEB-INF/classes/egovframework/example/sample/service/SampleVO.class
M	target/hello-1.0.0/WEB-INF/classes/egovframework/example/sample/service/impl/EgovSampleServiceImpl.class
M	target/hello-1.0.0/WEB-INF/classes/egovframework/example/sample/service/impl/SampleDAO.class
M	target/hello-1.0.0/WEB-INF/classes/egovframework/example/sample/service/impl/SampleMapper.class
M	target/hello-1.0.0/WEB-INF/classes/egovframework/example/sample/web/EgovSampleController.class
M	target/hello-1.0.0/WEB-INF/classes/egovframework/spring/context-datasource.xml
A	target/hello-1.0.0/WEB-INF/classes/main.yml
A	target/hello-1.0.0/WEB-INF/lib/mysql-connector-java-5.1.31.jar
M	target/m2e-wtp/web-resources/META-INF/maven/kr.co.oti/hello/pom.properties
M	target/m2e-wtp/web-resources/META-INF/maven/kr.co.oti/hello/pom.xml
M	target/maven-archiver/pom.properties
```

## 3. 커밋별 파일 목록

### 3e708d1 — Create main.yml

커밋 시간: 2026-10-07T10:34:48+09:00

```text
A	.github/workflows/main.yml
```

### be04213 — docker 이미지 빌드 및 배포

커밋 시간: 2026-10-07T11:33:39+09:00

```text
M	.classpath
A	.github/workflows/docker.yml
M	.github/workflows/main.yml
M	.settings/org.eclipse.wst.common.component
M	Dockerfile
M	src/main/resources/egovframework/spring/context-datasource.xml
A	target/classes/docker.yml
M	target/classes/egovframework/spring/context-datasource.xml
A	target/classes/main.yml
```

### 50bc825 — docker 빌드 파일 수정

커밋 시간: 2026-10-07T11:36:02+09:00

```text
M	Dockerfile
```

### 0e44a42 — docker hub 이미지 저장시 -t 삭제

커밋 시간: 2026-10-07T11:38:15+09:00

```text
M	.github/workflows/docker.yml
M	target/classes/docker.yml
```

### 19e4a1d — docker 배포 기능 수정

커밋 시간: 2026-10-07T11:39:49+09:00

```text
M	.github/workflows/docker.yml
M	target/classes/docker.yml
```

### 2568259 — 다시 한 번 검사

커밋 시간: 2026-10-07T11:48:04+09:00

```text
M	.github/workflows/docker.yml
M	target/classes/docker.yml
```

### 8fbc3ba — hello.war +

커밋 시간: 2026-10-07T12:08:06+09:00

```text
D	.dockerignore
M	target/hello-1.0.0.war
A	target/hello-1.0.0/WEB-INF/classes/docker.yml
M	target/hello-1.0.0/WEB-INF/classes/egovframework/example/cmmn/EgovSampleExcepHndlr.class
M	target/hello-1.0.0/WEB-INF/classes/egovframework/example/cmmn/EgovSampleOthersExcepHndlr.class
M	target/hello-1.0.0/WEB-INF/classes/egovframework/example/cmmn/web/EgovBindingInitializer.class
M	target/hello-1.0.0/WEB-INF/classes/egovframework/example/cmmn/web/EgovImgPaginationRenderer.class
M	target/hello-1.0.0/WEB-INF/classes/egovframework/example/sample/service/EgovSampleService.class
M	target/hello-1.0.0/WEB-INF/classes/egovframework/example/sample/service/SampleDefaultVO.class
M	target/hello-1.0.0/WEB-INF/classes/egovframework/example/sample/service/SampleVO.class
M	target/hello-1.0.0/WEB-INF/classes/egovframework/example/sample/service/impl/EgovSampleServiceImpl.class
M	target/hello-1.0.0/WEB-INF/classes/egovframework/example/sample/service/impl/SampleDAO.class
M	target/hello-1.0.0/WEB-INF/classes/egovframework/example/sample/service/impl/SampleMapper.class
M	target/hello-1.0.0/WEB-INF/classes/egovframework/example/sample/web/EgovSampleController.class
M	target/hello-1.0.0/WEB-INF/classes/egovframework/spring/context-datasource.xml
A	target/hello-1.0.0/WEB-INF/classes/main.yml
A	target/hello-1.0.0/WEB-INF/lib/mysql-connector-java-5.1.31.jar
M	target/maven-archiver/pom.properties
```

### e8a24a1 — ip 주소 다시 세팅

커밋 시간: 2026-10-07T12:14:50+09:00

```text
M	.github/workflows/docker.yml
M	target/classes/docker.yml
```

### 423bd12 — pom 에 mysql 버전 재 설정

커밋 시간: 2026-10-07T12:23:21+09:00

```text
M	pom.xml
M	target/m2e-wtp/web-resources/META-INF/maven/kr.co.oti/hello/pom.properties
M	target/m2e-wtp/web-resources/META-INF/maven/kr.co.oti/hello/pom.xml
```

## 4. 원격 브랜치 push·pull 기록 원문

로컬 원격 추적 브랜치 reflog 기준이다. 명령 원문이나 force 옵션을 단정하는 자료가 아니라 브랜치 갱신 기록이다.

```text
423bd12 refs/remotes/origin/main@{2026-10-07T12:23:22+09:00}: push: forced-update
e8a24a1 refs/remotes/origin/main@{2026-10-07T12:14:51+09:00}: push: forced-update
8fbc3ba refs/remotes/origin/main@{2026-10-07T12:08:11+09:00}: push: forced-update
2568259 refs/remotes/origin/main@{2026-10-07T11:48:06+09:00}: push: forced-update
19e4a1d refs/remotes/origin/main@{2026-10-07T11:39:50+09:00}: push: forced-update
0e44a42 refs/remotes/origin/main@{2026-10-07T11:38:16+09:00}: push: forced-update
50bc825 refs/remotes/origin/main@{2026-10-07T11:36:03+09:00}: push: forced-update
be04213 refs/remotes/origin/main@{2026-10-07T11:33:40+09:00}: push: forced-update
3e708d1 refs/remotes/origin/main@{2026-10-07T10:35:11+09:00}: pull: fast-forward
a6fcc80 refs/remotes/origin/main@{2026-10-07T09:39:00+09:00}: update by push
```

## 5. 터미널 명령 목록 — 원본 줄 번호 포함

원본 첨부 기록의 프롬프트를 추출했다. 실패·중단·재시도도 포함한다. SAMPLE INSERT 114개를 반복해서 붙여 넣은 부분은 제외했고 본문에서 성공·실패를 설명했다. 여러 줄 SQL의 연속 입력도 포함한다. 줄 번호는 원본 텍스트 기준이다.

```text
4: PS C:\Users\KOSA_L3> ssh -i ~/.ssh/oti-key.pem ubuntu@43.203.249.65
45: ubuntu@ip-172-31-15-145:~$ sudo apt update
105: ubuntu@ip-172-31-15-145:~$ sudo apt upgrade
1014: ubuntu@ip-172-31-15-145:~$ curl -fsSL https://get.docker.com -o get-docker.sh
1015: ubuntu@ip-172-31-15-145:~$ sudo sh get-docker.sh
1082: ubuntu@ip-172-31-15-145:~$ sudo systemctl status docker
1106: ubuntu@ip-172-31-15-145:~$ sudo systemctl status docker
1130: ubuntu@ip-172-31-15-145:~$ sudo usermod -aG docker $USER
1131: ubuntu@ip-172-31-15-145:~$ newgrp docker
1132: ubuntu@ip-172-31-15-145:~$ docker -v
1134: ubuntu@ip-172-31-15-145:~$ sudo apt update
1144: ubuntu@ip-172-31-15-145:~$ sudo apt install mysql-server -y
1419: ubuntu@ip-172-31-15-145:~$ sudo systemctl start mysql
1420: ubuntu@ip-172-31-15-145:~$ sudo systemctl enable mysql
1423: ubuntu@ip-172-31-15-145:~$ sudo mysql_secure_installation
1486: ubuntu@ip-172-31-15-145:~$ sudo mysql
1499: mysql> ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY '[DB_PASSWORD]';
1502: mysql> exit
1504: ubuntu@ip-172-31-15-145:~$ sudo mysql -u root -p
1518: mysql> CREATE DATABASE 'kosa_db'
1519:     -> CREATE DATABASE 'kosa_db';
1522: mysql> use kosa_db;
1524: mysql>
1525: mysql> CREATE TABLE SAMPLE(ID VARCHAR(16) NOT NULL PRIMARY KEY,NAME VARCHAR(50),DESCRIPTION VARCHAR(100),USE_YN CHAR(1),REG_USER VARCHAR(10));
1527: mysql> CREATE TABLE IDS(TABLE_NAME VARCHAR(16) NOT NULL PRIMARY KEY,NEXT_ID DECIMAL(30) NOT NULL);
1529: mysql>
1806: mysql> INSERT INTO IDS VALUES('SAMPLE',115);
1808: mysql> ^C
1809: mysql> exit
1811: ubuntu@ip-172-31-15-145:~$ docker login --help
1821: ubuntu@ip-172-31-15-145:~$ docker login -u ooooocj -p [DOCKER_HUB_TOKEN]
1829: ubuntu@ip-172-31-15-145:~$ sudo nano /etc/mysql/mysql.conf.d/mysqld.cnf
1830: ubuntu@ip-172-31-15-145:~$ sudo vi /etc/mysql/mysql.conf.d/mysqld.cnf
1831: ubuntu@ip-172-31-15-145:~$ sudo systemctl restart mysql
1832: ubuntu@ip-172-31-15-145:~$ sudo mysql -u root -p
1846: mysql> use mysql;
1851: mysql> selecet host, name from user;
1853: mysql> select host, name from user;
1855: mysql> select host, name from user;
1857: mysql> UPDATE mysql.user SET host='%' WHERE user='root';
1861: mysql> FLUSH PRIVILEGES;
1864: mysql> exit
1866: ubuntu@ip-172-31-15-145:~$ sudo mysql -u root -p -h 43.203.249.65
1869: ubuntu@ip-172-31-15-145:~$ sudo mysql -u root -p -h 43.203.249.65
1877: ubuntu@ip-172-31-15-145:~$ sudo mysql -u root -p -h 43.203.249.65
1889: ubuntu@ip-172-31-15-145:~$ sudo mysql
1891: ubuntu@ip-172-31-15-145:~$ docker ps
1893: ubuntu@ip-172-31-15-145:~$ docker images
1896: ubuntu@ip-172-31-15-145:~$ ssh -i mykey.pem ubuntu@3.34.96.62
1903: ubuntu@3.34.96.62: Permission denied (publickey).
1904: ubuntu@ip-172-31-15-145:~$ ssh -i oti-key.pem ubuntu@3.34.96.62
1906: ubuntu@3.34.96.62: Permission denied (publickey).
1907: ubuntu@ip-172-31-15-145:~$ sudo docker logs hello --tail 100
1909: ubuntu@ip-172-31-15-145:~$ sudo docker logs hello --tail 100
1911: ubuntu@ip-172-31-15-145:~$ ^C
1912: ubuntu@ip-172-31-15-145:~$ sudo docker logs hello_app --tail 100
2013: ubuntu@ip-172-31-15-145:~$ ^C
2014: ubuntu@ip-172-31-15-145:~$ sudo docker logs hello_app --tail 100
2115: ubuntu@ip-172-31-15-145:~$ sudo mysql
2117: ubuntu@ip-172-31-15-145:~$ sudo mysql -u root -p
2131: mysql> SHOW DATABASES;
2142: mysql> CREATE DATABASE kosa_db
2143:     -> DEFAULT CHARACTER SET utf8mb4
2144:     -> COLLATE utf8mb4_unicode_ci;
2147: mysql> USE kosa_db;
2149: mysql> use kosa_db;
2151: mysql>
2152: mysql> CREATE TABLE SAMPLE(ID VARCHAR(16) NOT NULL PRIMARY KEY,NAME VARCHAR(50),DESCRIPTION VARCHAR(100),USE_YN CHAR(1),REG_USER VARCHAR(10));
2198: mysql> CREATE TABLE IDS(TABLE_NAME VARCHAR(16) NOT NULL PRIMARY KEY,NEXT_ID DECIMAL(30) NOT NULL);
2201: mysql>
2552: mysql> INSERT INTO IDS VALUES('SAMPLE',115);
2555: mysql> sudo systemctl restart mysql
2556:     -> ^C
2557: mysql> ^C
2558: mysql> exit
2560: ubuntu@ip-172-31-15-145:~$ sudo systemctl restart mysql
2561: ubuntu@ip-172-31-15-145:~$
```

## 6. 최종 docker.yml 원문

코드에 저장된 Action 버전, 이미지명, 옵션, 주석을 그대로 수록했다. 개선 제안과 구별되는 실제 실습 구성이다. secrets 표현식은 실제 비밀값이 아니다.

```yaml
name: docker 이미지 빌드 및 배포

on:
  push:
    branches:
      - main      # 주석 : main 브랜치에 push가 되면 actions가 동작함

#
#환경변수
env:
  SERVER_IP: 43.203.249.65

jobs:
  build:
    runs-on: ubuntu-latest  # 어떤 운영체제 버전에서 실행하는지 설정 cancel-timeout-minutes: cancel-timeout-minutes: 

    steps:
    - name: 소스 check out 
      uses: actions/checkout@v3

    - name: 메이븐 빌드
      run: mvn clean package

    - name: 보안환경변수를 이용하여 개인키 파일생성
      run: |
        mkdir -p ~/.ssh
        echo "${{ secrets.SERVER_SSH_KEY }}" > ~/.ssh/id_rsa
        chmod 600 ~/.ssh/id_rsa
        
    - name: actions을 사용하여 보안환경변수를 이용하여 개인키 파일생성
      uses: webfactory/ssh-agent@v0.5.3
      with:
        ssh-private-key: ${{ secrets.SERVER_SSH_KEY }}
        
    - name: 서버연결시 지문등록
      run: ssh-keyscan -t ed25519 ${{ env.SERVER_IP }} >> ~/.ssh/known_hosts
    
    
    - name: Docker 로그인
      uses: docker/login-action@v1
      with:
        username: ${{ secrets.DOCKER_USERNAME}}
        password: ${{ secrets.DOCKER_PASSWORD}}
        
    
    - name: docker 이미지 빌드 
      run: |
        docker build -t ${{secrets.DOCKER_USERNAME}}/hello:latest .
        
    - name: docker hub 이미지 저장 
      run: |
        docker push ${{secrets.DOCKER_USERNAME}}/hello:latest
        
#    - name: 개인키파일확인 
#      run: |
#        cat ~/.ssh/id_rsa
#        cat ~/.ssh/known_hosts
 
#  deploy:
#    needs: build
#    name: 운영서버에 배포 진행
#    runs-on: ubuntu-latest  # 어떤 운영체제 버전에서 실행하는지 설정 cancel-timeout-minutes: cancel-timeout-minutes:
#    steps: 

    - name: 배포 실행      
      run: |
        ssh -i ~/.ssh/id_rsa ubuntu@${{ env.SERVER_IP }} << 'EOF'
           #docker 로그인 
           docker login -u ${{ secrets.DOCKER_USERNAME}} -p ${{ secrets.DOCKER_PASSWORD}}
           
           #docker 관련 실행명령어 기록
           docker stop hello_app
           docker rm hello_app
           docker rmi ${{secrets.DOCKER_USERNAME}}/hello:latest
           docker run -dit --name hello_app -p 80:8080 ${{secrets.DOCKER_USERNAME}}/hello:latest
        EOF
      
```

## 7. 최종 main.yml 원문 — 이전 WAR 직접 배포 방식

트리거는 empty 브랜치다. main push의 현재 Docker 배포는 위 docker.yml이 담당한다.

```yaml
name: 전자정부프레엠웍을 사용하여 빌드 및 배포

on:
  push:
    branches:
      - empty      # 주석 : main 브랜치에 push가 되면 actions가 동작함

jobs:
  build:
    runs-on: ubuntu-latest  # 어떤 운영체제 버전에서 실행하는지 설정 cancel-timeout-minutes: cancel-timeout-minutes: 

    steps:
    - name: git 저장소 소스 복사전의 폴더 구조
      run: ls -la
    
    - name: 소스 check out 
      uses: actions/checkout@v3

    - name: git 저장소 소스 복사후의 폴더 구조
      run: ls -la

    - name: git 저장소 소스 복사후의 hello 폴더 구조 # 리눅스 셀 명령어 2줄 이상 실행시
      run: |   
        ls -la
      
    - name: 메이븐 빌드
      run: mvn clean package

    - name: 보안환경변수를 이용하여 개인키 파일생성
      run: |
        mkdir -p ~/.ssh
        echo "${{ secrets.SERVER_SSH_KEY }}" > ~/.ssh/id_rsa
        chmod 600 ~/.ssh/id_rsa
        
    - name: actions을 사용하여 보안환경변수를 이용하여 개인키 파일생성
      uses: webfactory/ssh-agent@v0.5.3
      with:
        ssh-private-key: ${{ secrets.SERVER_SSH_KEY }}
        
    - name: 서버연결시 지문등록
      run: ssh-keyscan -t ed25519 13.125.180.143 >> ~/.ssh/known_hosts
       
#    - name: 개인키파일확인 
#      run: |
#        cat ~/.ssh/id_rsa
#        cat ~/.ssh/known_hosts
 
    - name: 운영서버에 배포 진행 
      run: |
        scp target/hello-1.0.0.war ubuntu@13.125.180.143:~/ROOT.war
        ssh -i ~/.ssh/id_rsa ubuntu@13.125.180.143 << 'EOF'
          cp ~/ROOT.war /opt/tomcat/webapps/
        EOF
      
    
```

## 8. Dockerfile 변경 전후

### 최초 커밋의 다단계 Dockerfile

```dockerfile
# Maven으로 WAR 만들기
FROM maven:3.9-eclipse-temurin-11 AS build

WORKDIR /app
COPY pom.xml .
COPY src ./src

RUN mvn -B clean package -DskipTests

# Tomcat에 WAR 넣기
FROM tomcat:9.0-jdk11-temurin

RUN rm -rf /usr/local/tomcat/webapps/*

COPY --from=build /app/target/hello-1.0.0.war \
    /usr/local/tomcat/webapps/ROOT.war

EXPOSE 8080
CMD ["catalina.sh", "run"]
```

### 최종 단일 단계 Dockerfile

```dockerfile
FROM   tomcat:9.0-jdk11-temurin

RUN rm -rf /usr/local/tomcat/webapps/*

COPY target/hello-1.0.0.war /usr/local/tomcat/webapps/ROOT.war

EXPOSE 8080

CMD ["catalina.sh", "run"]
```

## 9. 삭제된 .dockerignore의 최초 내용

최종 HEAD에는 이 파일이 없다. target/ 제외가 최종 WAR COPY 방식과 충돌하는 이유는 본문에서 설명했다.

```text
target/
.git/
.github/
.idea/
.settings/
.project
.classpath
.env
*.env
*.pem
```

## 10. pom.xml 및 DB 설정의 실제 변경 diff

비밀번호는 가렸다. 첫 커밋부터 최종 커밋까지의 차이다.

```diff
diff --git a/pom.xml b/pom.xml
index 31a1889..8ace463 100644
--- a/pom.xml
+++ b/pom.xml
@@ -122,12 +122,18 @@
 			<version>2.4.0</version>
 		</dependency>
      
-        <dependency>
+<!--         <dependency>
             <groupId>mysql</groupId>
             <artifactId>mysql-connector-java</artifactId>
             <version>5.1.31</version>
-        </dependency>
+        </dependency> -->
 
+<!-- MySQL JDBC Driver -->
+<dependency>
+    <groupId>com.mysql</groupId>
+    <artifactId>mysql-connector-j</artifactId>
+    <version>8.0.33</version>
+</dependency>
  
 <!-- 		 <dependency>
 		    <groupId>org.mariadb.jdbc</groupId>
diff --git a/src/main/resources/egovframework/spring/context-datasource.xml b/src/main/resources/egovframework/spring/context-datasource.xml
index b29a771..e3df6f4 100644
--- a/src/main/resources/egovframework/spring/context-datasource.xml
+++ b/src/main/resources/egovframework/spring/context-datasource.xml
@@ -22,12 +22,12 @@
     -->  
     
     <!-- Mysql (POM에서 commons-dbcp, mysql-connector-java 관련 라이브러리 설정 ) --> 
-<!--    <bean id="dataSource" class="org.apache.commons.dbcp2.BasicDataSource" destroy-method="close">
-        <property name="driverClassName" value="com.mysql.jdbc.Driver"/>
-        <property name="url" value="jdbc:mysql://127.0.0.1:13306/kosa_db" />
-        <property name="username" value="scott"/>
+    <bean id="dataSource" class="org.apache.commons.dbcp2.BasicDataSource" destroy-method="close">
+        <property name="driverClassName" value="com.mysql.cj.jdbc.Driver"/>
+        <property name="url" value="jdbc:mysql://43.203.249.65:3306/kosa_db" />
+        <property name="username" value="root"/>
         <property name="password" value="[DB_PASSWORD]"/>
-    </bean>  -->
+    </bean> 
     
     <!-- mariadb (POM에서 commons-dbcp, mysql-connector-java 관련 라이브러리 설정 ) --> 
 <!--     <bean id="dataSource" class="org.apache.commons.dbcp2.BasicDataSource" destroy-method="close">
@@ -54,7 +54,7 @@
     </bean> -->
     
     <!-- Mysql -->
-<bean id="dataSource"
+<!-- <bean id="dataSource"
       class="org.apache.commons.dbcp2.BasicDataSource"
       destroy-method="close">
 
@@ -63,7 +63,7 @@
     <property name="url" value="${DB_URL}"/>
     <property name="username" value="${DB_USERNAME}"/>
     <property name="password" value="${DB_PASSWORD}"/>
-</bean>
+</bean> -->
 
    
     
```

## 11. Eclipse 설정 변경 diff

workflow가 target/classes와 WAR 안에 복사된 원인을 보여 주는 설정이다.

```diff
diff --git a/.classpath b/.classpath
index 54347b7..6327e21 100644
--- a/.classpath
+++ b/.classpath
@@ -6,6 +6,7 @@
 			<attribute name="maven.pomderived" value="true"/>
 		</attributes>
 	</classpathentry>
+	<classpathentry kind="src" path=".github/workflows"/>
 	<classpathentry excluding="**" kind="src" output="target/classes" path="src/main/resources">
 		<attributes>
 			<attribute name="maven.pomderived" value="true"/>
@@ -13,19 +14,20 @@
 	</classpathentry>
 	<classpathentry kind="src" output="target/test-classes" path="src/test/java">
 		<attributes>
+			<attribute name="test" value="true"/>
 			<attribute name="optional" value="true"/>
 			<attribute name="maven.pomderived" value="true"/>
-			<attribute name="test" value="true"/>
 		</attributes>
 	</classpathentry>
 	<classpathentry excluding="**" kind="src" output="target/test-classes" path="src/test/resources">
 		<attributes>
-			<attribute name="maven.pomderived" value="true"/>
 			<attribute name="test" value="true"/>
+			<attribute name="maven.pomderived" value="true"/>
 		</attributes>
 	</classpathentry>
 	<classpathentry kind="con" path="org.eclipse.jdt.launching.JRE_CONTAINER/org.eclipse.jdt.internal.debug.ui.launcher.StandardVMType/JavaSE-1.8">
 		<attributes>
+			<attribute name="module" value="true"/>
 			<attribute name="maven.pomderived" value="true"/>
 		</attributes>
 	</classpathentry>
diff --git a/.settings/org.eclipse.wst.common.component b/.settings/org.eclipse.wst.common.component
index 89360c0..25d38c4 100644
--- a/.settings/org.eclipse.wst.common.component
+++ b/.settings/org.eclipse.wst.common.component
@@ -1,9 +1,18 @@
 <?xml version="1.0" encoding="UTF-8"?><project-modules id="moduleCoreId" project-version="1.5.0">
+        
     <wb-module deploy-name="hello-1.0.0">
+                
         <wb-resource deploy-path="/" source-path="/target/m2e-wtp/web-resources"/>
+                
         <wb-resource deploy-path="/" source-path="/src/main/webapp" tag="defaultRootSource"/>
+                
         <wb-resource deploy-path="/WEB-INF/classes" source-path="/src/main/java"/>
+        <wb-resource deploy-path="/WEB-INF/classes" source-path="/.github/workflows"/>
+                
         <property name="context-root" value="hello"/>
+                
         <property name="java-output-path" value="/hello/build/classes"/>
+            
     </wb-module>
+    
 </project-modules>
```

## 12. 원본과 기록 범위

원본: 사용자가 첨부한 Windows PowerShell 텍스트 2,561줄, 프로젝트 Git 이력과 현재 파일. 서버에 재접속하거나 GitHub에 새 push를 하지 않고 읽기 작업으로 정리했다. 최종 브라우저 확인은 사용자의 보고이며 첨부에는 해당 화면이 없다.
