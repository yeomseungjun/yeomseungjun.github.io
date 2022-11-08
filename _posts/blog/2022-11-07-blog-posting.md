---
title: "[Blog] 포스팅 하는 방법"
excerpt: ""
categories:
  - Blog
tags:
  - [Blog, jekyll, Github, minimal-mistake]
toc: true
toc_sticky: false
date: 2022-11-07
last_modified_at: 2022-11-07
---

## 1. 에디터
여러가지 에디터가 있지만 `Visual Studio Code`에디터를 사용했다. Angular로 개발 할 때 사용했기 때문에 거부감 없이 사용 할 수 있다.  

```
Visual Studio Code
```
[**Visual Studio Code 다운로드**](https://code.visualstudio.com/download)

## 2. 포스트 경로
나중에 폴더 구조를 포스팅 할 예정이지만 깃블로그는 `_posts`경로에 *.md 파일을 생성하여 포스팅 할 수 있다. 내 경우 경로에 태그 별로 폴더를 나누어 저장하였다. 파일명의 형식은 `yyyy-mm-dd-*.md` 형식으로 작성한다.

```
_posts
    ㄴ blog
        ㄴ 2022-11-07-blog-posting.md
```
## 3. 머릿말
최상단의 `---`를 사용하여 머릿말 영역을 구분한다.

```
---
title: "[Github 블로그] 포스팅 하는 방법"
excerpt: ""

categories:
  - Blog
tags:
  - [Blog, jekyll, Github, Git]

toc: true
toc_sticky: true
 
date: 2022-11-07
last_modified_at: 2022-11-07
---
```

- title : 포스트 제목
- excerpt : 포스트 소개 글
- categories : 카테고리  
`(내 경우 _posts 밑의 폴더명으로 구분하였다.)`
- tags : [] 대활호 안에서 콤마로 구분하여 여러개의 태그를 지정한다.
- toc : 목차 사용 여부
- toc_sticky : 목차가 스크롤을 따라오는 기능 활성화여부  
`(내 경우 layout을 수정하여 옵션과 상관없이 항상 최상단에 고정되어 있다.)`
- date : 포스팅 생성 일자
- last_modified_at : 마지막 수정일자

## 4. 미리보기
vs code 에서는 놀랍게도 마크다운을 미리보는 기능을 제공합니다. 상단의 아이콘중 `측면에서 미리보기`버튼을 클릭해도 되지만 단축키인 `Ctrl + k v`를 눌러도 됩니다.

![아이콘](/assets/images/2022-11-07-blog-postring/previewe.png)  

## 5. 미리보기 단축키가 안될 경우
단축키를 눌렀을때 반응하지 않을 경우 환경설정 변경을 통해서 단축키를 변경 할 수 있습니다.  
```
EN : Code -> Preferences -> Keyboard Shortcurs
KO : 파일 -> 기본 설정 -> 바로 가기 키
```
![아이콘](/assets/images/2022-11-07-blog-postring/key-setting.PNG)


## 5. 참고
[공부하는 식빵맘](https://ansohxxn.github.io/blog/posting/)