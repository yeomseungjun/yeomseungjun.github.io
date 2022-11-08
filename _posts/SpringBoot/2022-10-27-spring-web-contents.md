---
title:  "[SpringBoot] 정적 컨텐츠 / MVC / API" 
excerpt: ""
ategories:
  - SpringBoot
tags:
  - [SpringBoot, Intelij, 정적컨텐츠, MVC, API]
toc: true
toc_sticky: false
date: 2022-10-25
last_modified_at: 2022-11-08
---

# Spring Boot에서 Web Contents 종류
1. 정적컨텐츠
2. MVC (템플릿엔진)
3. API

## 1. 정적컨텐츠
정적컨텐츠는 서버에서 작업없이 **파일을 있는 그대로 웹브라우저에 내려주는 방식**을 말합니다.  

ex) static/index.html 등

![Static Contents](/assets/images/2022-10-27-spring-web-contents/static.PNG)

## 2. MVC (템플릿엔진)
Model/View/Controller의 약자로 **서버에서 프로그래밍을 해서 html을 동적으로 변경하여 내리는 방식**을 말합니다.  

ex) thymeleaf, jsp, php 등

![MVC](/assets/images/2022-10-27-spring-web-contents/mvc.PNG)

## 3. API
필요한 데이터를 xml, json 등과 같은 **데이터 구조 포멧으로 클라이언트에게 전달하는 방식**을 말합니다.

![API](/assets/images/2022-10-27-spring-web-contents/api.PNG)

## 4. (번외) PDF뷰어(cunky) 프로젝트
PDF뷰어(crunky) 프로젝트는 React + Spring Boot 로 설계되었습니다.  

요즘 트랜드는 Frontend와 Backend를 분리하여 각각 배포하는 형태가 많이 보이지만, Crunky 프로젝트는 Spring Boot에 React Build 파일을 넣어 하나의 서버를 배포하는 형식을 채택하였습니다.  

이유는 개발기간이 짧고, 분리배포 시의 보안측면 등의 리스크를 아직 전부 파악하지 못했기 때문입니다.  

따라서 Crunky의 뷰어 부분은 정적컨텐츠와 처럼 React에서 Build된 html을 그대로 웹브라우저에 내려주고, 필요한 데이터를 API 형태로 서버에서 값을 받아서 처리하는 방식으로 설계하였습니다.  

또한 뷰어가 인터넷익스플로러(IE10)을 지원해야하므로 뷰어 핵심 라이브러리인 pdf.js가 IE를 지원하는 아주 낮은 버전의 뷰어도 개발 및 추가 할 예정입니다.  

IE 버전의 뷰어는 템플릿엔진을 사용하지 않은 HTML입니다.