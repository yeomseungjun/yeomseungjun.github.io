---
title:  "[SpringBoot] 설정파일(application.yml)" 
excerpt: ""
ategories:
  - SpringBoot
tags:
  - [SpringBoot, Intelij, application.properties, application.yml, config]
toc: true
toc_sticky: false
date: 2022-10-25
last_modified_at: 2022-11-08
---

## Spring Boot 설정파일 적용방법

Spring Boot 프로젝트를 생성하면 기본으로 resources 밑에 application.properties가 생성됩니다.  

하지만 properties는 가독성이 떨어지고 특정한 그룹에 원하는 설정을 선택적으로 설정할 수 없으므로  spring에서 지원하는 yaml 형식의 파일로 설정 하였습니다.  

추가로 logback에 대한 설정도 같이 진행하였습니다. 완료된 파일구조는 아래와 같습니다.  


```
ㄴresources
    ㄴ config
        ㄴapplication
            ㄴapplication-common.yml
            ㄴapplication-db.yml
            ㄴapplication-kakaopay.yml
        ㄴlogback
            ㄴlogback-dev.properties
            ㄴlogback-local.properties
            ㄴlogback-prod.properties
    ㄴapplication.yml
    ㄴlogback-spring.xml
```

## appication.yml 사용법

`application.yml`에 모든 설정을 작성 할 수 있지만 파일 형식으로 분기하여 원하는 설정만 `group`를 이용하여 지정 할 수 있습니다.  

따로 파일로 분기할 설정들은 application-*.yml 형식으로 작성해두고 `import`를 이용하여 해당 파일들을 불러옵니다.  

이후 `profiles`를 이용하여 그룹에 적용할 설정들을 지정해줍니다. 이러한 profiles 방식은 Spring Boot 2.4 이후부터 적용 할 수 있습니다.  

각각의 설정파일에는 `on-profile`을 이용하여 호출에 사용될 명칭을 지정합니다.  

모두 설정하였다면 `active`를 이용하여 사용할 profile group를 지정합니다. (intelliJ 실행 시 적용)  

빌드 이후 jar 파일에 적용하려면 `java -jar myapp.jar --spring.profiles.active=prod` 으로 원하는 profile group명을 입력하여 실행 할 수 있습니다.  


``` yml
# application.yml
spring:
  config:
    import:
      - classpath:/config/application/application-common.yml
      - classpath:/config/application/application-db.yml
      - classpath:/config/application/application-kakaopay.yml
  profiles:
    active: local
    group:
      "local": "common,kakaopay-token-dev"
      "dev": "common,kakaopay-token-dev"
      "prod": "common,kakaopay-token-prod"
logging:
  config: classpath:logback-spring.xml
```

``` yml
# application-common.yml
spring:
  config:
    activate:
      on-profile: "common"
server:
  port: 18080
  tomcat:
    uri-encoding: UTF-8
```

``` yml
# application-db.yml

## DEV
spring:
  config:
    activate:
      on-profile: "db-dev"
  datasource:
    url: "jdbc:log4jdbc:sqlserver://xxx.xxx.xx.xx;databaseName=test_dev;"
    username: "user_dev"
    password: "1234"
---

## PROD
spring:
  config:
    activate:
      on-profile: "db-prod"
  datasource:
    url: "jdbc:log4jdbc:sqlserver://xxx.xxx.xx.xx;databaseName=test_prod;"
    username: "user_prod"
    password: "1324"
```


## (번외) SpringBoot 2.4 이전 버전

만약 SpringBoot의 버전이 2.4 이전 이라면 `profiles`를 이용하여 설정을 적용해야합니다. (group 사용 불가)

하나의 application.yml 파일에 여러 환경의 설정 정보를 저장하려면 프로파일 구분자(---)를 이용하여 작성 할 수 있습니다.

```yml
# local, dev, prod 공통 설정
server:
  port: 8080
  tomcat:
    uri-encoding: UTF-8

---

spring:
  profiles: local
  datasource:
    url: "jdbc:mysql://test-server/test"
    username: "dbuser"
    password: "dbpass"

---

spring:
  profiles: dev
  datasource:
    url: "jdbc:mysql://test-server/test"
    username: "dbuser"
    password: "dbpass"


---
spring:
  profiles: prod
  datasource:
    url: "jdbc:mysql://service-server/web"
    username: "proddbuser"
    password: "proddbpass"
```


## 참고사이트
<https://1minute-before6pm.tistory.com/12>
<https://oingdaddy.tistory.com/464>