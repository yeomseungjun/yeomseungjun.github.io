---
title:  "[Spring] 환경설정" 
toc: false
toc_sticky: false
date: 2022-10-25
categories:
  - Spring
tags:
  - [Intelij, Spring, Spring Boot]
---


### Spring 환경설정

## 1. 사전 준비물
- Java 설치 (무료버전 Java SE 8 : v1.8.0.202)  
- InteliJ 또는 Eclipse (InteliJ 추천)

## 2. 프로젝트 생성
요즘에는 Spring 프로젝트라고 하면 Spring Boot를 기본으로 생각합니다.   

Spring Boot 프로젝트 생성방법 로컬 생성 방식과 Spring에서 제공하는 인터넷방식의 생성방법이 있습니다.

아래 URL을 통해서 pring Boot 버전 외 기본 라이브러리 설정 등을 설정한 뒤 Generrate 버튼을 통해 프로젝트를 다운받습니다.

  <https://start.spring.io>

![프로젝트 생성](/assets/images/2022-10-25-springboot/spring-starter.png)

- 설정  

|속성|선택|비고|
|:--:|:--:|--|
|Project|Gradle Project|Gradle문법이 더 쉽고 간편|
|Spring Boot|2.7.5|릴리즈 최신 버전|
|Packaging|Jar|서버를 내장하므로 Jar 사용|
|Java|17|LTS 버전 (2021.09 출시)|
|Dependencies|Spring Web, Thymeleaf|-|

## 3. 프로젝트 실행
- 다운받은 압축파일을 해제한 뒤 InteliJ에서 해당 폴더를 Import 합니다.  
- 처음 실행됬을 경우 필요한 라이브러리를 자동으로 다운받습니다.

![프로젝트 Import](/assets/images/2022-10-25-springboot/import.png)

- 프로젝트 Run 이후 <http://localhost:8080> 을 통해서웹페이지를 확인합니다.

![프로젝트 Run](/assets/images/2022-10-25-springboot/run.png)

## 4. (번외) Run Option 변경
프로젝트를 처음 Run할 경우 Gradle로 실행 되어있습니다.  
이럴경우 Build시 시간이 느려지는 현상이 있기 때문에 아래와 같이 InteliJ 실행이 될 수 있도록 설정을 변경합니다.

File -> Setting -> Gradle 검색 -> Run 부분을 InteliJ 로 변경

![InteliJ Run](/assets/images/2022-10-25-springboot/run-using.png)