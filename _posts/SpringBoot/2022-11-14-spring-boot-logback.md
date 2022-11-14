---
title:  "[SpringBoot] logback 설정" 
excerpt: ""
categories:
  - SpringBoot
tags:
  - [SpringBoot, Intelij, logback, application.yml, config]
toc: true
toc_sticky: false
date: 2022-11-14
last_modified_at: 2022-11-14
---

## logback 설정
logback 설정은 `*.xml` 포맷이지만 `*.yml` 포멧도 지원한다. 기존에는 xml 로 설정해서 작업했는데 이번에 아무리해도 pacakage 단위의 log.level 설정이 적용되지 않아서 엄청나게 삽질했다.😢 그냥 넘어갈 수 있는 아주 작은 설정이였는데 내 성격상 도저히 넘어 갈 수 없어서 4시간이나 찾아헤맸다. 결국은 찾긴했지만 로그에 찍히지가 않아서 찾는데 시간이 오래걸렸다.  

이것저것 해보다가 기존의 `*.xml` 포맷을 `*.yml` 포맷으로 바꿔보기도 헀는데 이게 더 깔끔해서 이참에 변경해버리고 어떻게 설정하는지 기록하기로 헀다.🤣

> 참고로 package 단위의 log.level 설정이 되지 않았던 이유는 Logger 생성자를 잘못 입력했기 때문이다.  
getSimpleName() 으로 생성해버리면 logger.name 설정의 패키지명으로 설정이 되지 않는다.

``` java
// 기존 Logger 생성자
private final Logger logger = LoggerFactory.getLogger(this.getClass().getSimpleName());

// 수정
> private final Logger logger = LoggerFactory.getLogger(this.getClass());
```

## 1. 기존 *.xml 설정
기존에는 local, dev, prod 설정파일을 각각 만들고 logback-spring.xml 설정에서 적용하는 형식이였다.


**logback-local.properties**
``` sh
## LOCAL PROPERTIES ##
# [ LOGBACK LEVEL ]
log.config.level.root=INFO
log.config.level.cxviewer=DEBUG
log.config.level.mybatis=DEBUG

# [ LOGBACK SETTING ]
log.config.path=./logs
log.config.filename=cxvapp
log.config.errfilename=err_cxvapp
log.config.history.max=30
log.config.filesize.max=10MB
```

**logback-spring.xml**
``` xml
<?xml version="1.0" encoding="UTF-8"?>

<!-- 60초마다 설정 파일의 변경을 확인 하여 변경시 갱신 -->
<configuration scan="true" scanPeriod="60 seconds">
	<conversionRule conversionWord="clr" converterClass="org.springframework.boot.logging.logback.ColorConverter" />
	    
    <!--LOG PROFILE -->
    <springProfile name="local">
        <property resource="logback-local.properties"/>
    </springProfile>
    <springProfile name="dev">
        <property resource="logback-dev.properties"/>
    </springProfile>
    <springProfile name="prod">
        <property resource="logback-prod.properties"/>
    </springProfile>
    

    <!-- LOG PATH, PATTERN -->
    <property name="LOG_PATH" value="${log.config.path}"/>
    <property name="LOG_FILE_NAME" value="${log.config.filename}"/>
    <property name="ERR_LOG_FILE_NAME" value="${log.config.errfilename}"/>
    <property name="LOG_PATTERN" value="%d{yy-MM-dd HH:mm:ss} %clr(%-5level) [%logger{0}:%line] - %msg%n"/>


    <!-- CONSOLE Appender -->
    <appender name="CONSOLE" class="ch.qos.logback.core.ConsoleAppender">
        <encoder class="ch.qos.logback.classic.encoder.PatternLayoutEncoder">
            <pattern>${LOG_PATTERN}</pattern>
        </encoder>
    </appender>


    <!-- FILE Appender -->
    <appender name="FILE" class="ch.qos.logback.core.rolling.RollingFileAppender">
        <file>${LOG_PATH}/${LOG_FILE_NAME}.log</file>
        <encoder class="ch.qos.logback.classic.encoder.PatternLayoutEncoder">
            <pattern>${LOG_PATTERN}</pattern>
        </encoder>

        <!-- Rolling 정책 -->
        <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
            <!-- .gz,.zip 등을 넣으면 자동 일자별 로그파일 압축 -->
            <fileNamePattern>${LOG_PATH}/${LOG_FILE_NAME}_%d{yyyy-MM-dd}_%i.log</fileNamePattern>
            <timeBasedFileNamingAndTriggeringPolicy class="ch.qos.logback.core.rolling.SizeAndTimeBasedFNATP">
                <!-- 파일당 최고 용량 kb, mb, gb -->
                <maxFileSize>${log.config.filesize.max}</maxFileSize>
            </timeBasedFileNamingAndTriggeringPolicy>
            <!-- 일자별 로그파일 최대 보관주기(~일), 해당 설정일 이상된 파일은 자동으로 제거-->
            <maxHistory>${log.config.history.max}</maxHistory>
        </rollingPolicy>
    </appender>


    <!-- ERROR FILE Appender -->
    <appender name="ERROR" class="ch.qos.logback.core.rolling.RollingFileAppender">
        <filter class="ch.qos.logback.classic.filter.LevelFilter">
            <level>error</level>
            <onMatch>ACCEPT</onMatch>
            <onMismatch>DENY</onMismatch>
        </filter>
        <file>${LOG_PATH}/${ERR_LOG_FILE_NAME}.log</file>
        <encoder class="ch.qos.logback.classic.encoder.PatternLayoutEncoder">
            <pattern>${LOG_PATTERN}</pattern>
        </encoder>
        <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
            <fileNamePattern>${LOG_PATH}/${ERR_LOG_FILE_NAME}_%d{yyyy-MM-dd}_%i.log</fileNamePattern>
            <timeBasedFileNamingAndTriggeringPolicy class="ch.qos.logback.core.rolling.SizeAndTimeBasedFNATP">
                <maxFileSize>${log.config.filesize.max}</maxFileSize>
            </timeBasedFileNamingAndTriggeringPolicy>
            <maxHistory>${log.config.history.max}</maxHistory>
        </rollingPolicy>
    </appender>


    <!-- ROOT LEVEL -->
    <root level="${log.config.level.root}">
        <appender-ref ref="CONSOLE"/>
        <appender-ref ref="FILE"/>
        <appender-ref ref="ERROR"/>
    </root>
    
     <!-- PACKAGE LEVEL (CXVIEWER) -->
    <logger name="trans.cosmos.korea.sol" level="${log.config.level.cxviewer}" additivity="false">
        <appender-ref ref="CONSOLE"/>
        <appender-ref ref="FILE"/>
        <appender-ref ref="ERROR"/>
    </logger>
    
    <!-- PACKAGE LEVEL (DB) -->
    <logger name="org.apache.ibatis" level="${log.config.level.mybatis}" additivity="false">
        <appender-ref ref="CONSOLE"/>
        <appender-ref ref="FILE"/>
        <appender-ref ref="ERROR"/>
    </logger>
    
    <!-- log4jdbc OPTION -->
	<logger name="jdbc" level="OFF"/>
	<!-- 커넥션 open close 이벤트를 로그로 남긴다. -->
	<logger name="jdbc.connection" level="OFF"/>
	<!-- SQL문만을 로그로 남기며, PreparedStatement일 경우 관련된 argument 값으로 대체된 SQL문이 보여진다. -->
	<logger name="jdbc.sqlonly" level="OFF"/>
	<!-- SQL문과 해당 SQL을 실행시키는데 수행된 시간 정보(milliseconds)를 포함한다. -->
	<logger name="jdbc.sqltiming" level="DEBUG"/>
	<!-- ResultSet을 제외한 모든 JDBC 호출 정보를 로그로 남긴다. 많은 양의 로그가 생성되므로 특별히 JDBC 문제를 추적해야 할 필요가 있는 경우를 제외하고는 사용을 권장하지 않는다. -->
	<logger name="jdbc.audit" level="OFF"/>
	<!-- ResultSet을 포함한 모든 JDBC 호출 정보를 로그로 남기므로 매우 방대한 양의 로그가 생성된다. -->
	<logger name="jdbc.resultset" level="OFF"/>
	<!-- SQL 결과 조회된 데이터의 table을 로그로 남긴다. -->
	<logger name="jdbc.resultsettable" level="OFF"/>
    
</configuration>
```

## 2. 신규 *.yml 설정
yml로 logback에서 제공하는 모든 설정을 적용 할 수는 없었다. 적어도 공식사이트에서는 찾아볼 수 없었다. 다행히 Spring Boot docs에서 설정 방법이 일부 적혀있었는데 기존의 설정에서 Appender를 추가해서 Error 로그만 따로 적재하는 부분을 제외하고는 다 설정할 수 있었다.

[SpringBoot Docs 참고](https://docs.spring.io/spring-boot/docs/current/reference/html/application-properties.html#appendix.application-properties.json)

**application.yml**
``` yml
# ** Logging **

## DEV
spring:
  config:
    activate:
      on-profile: "logging-loc"
logging:
  pattern:
    console: "%d{yy-MM-dd HH:mm:ss} %highlight([%-5level]) [%logger{0}:%line] - %msg%n"
  level:
    root: info
    tck.co.kr: debug
    org.hibernate.SQL: debug

---

## PROD
spring:
  config:
    activate:
      on-profile: "logging-prd"
logging:
  pattern:
    console: "%d{yy-MM-dd HH:mm:ss} %highlight([%-5level]) [%logger{0}:%line] - %msg%n"
    file: "%d{yy-MM-dd HH:mm:ss} %highlight([%-5level]) [%logger{0}:%line] - %msg%n"
  file:
    name: ./logs/crunkyapp.log
  logback:
    rollingpolicy:
      file-name-pattern: ./logs/crunkyapp_%d{yyyy-MM-dd}_%i.log
      max-file-size: 10MB
      max-history: 30
  level:
    root: info
    tck.co.kr: info
    org.hibernate.SQL: info

```