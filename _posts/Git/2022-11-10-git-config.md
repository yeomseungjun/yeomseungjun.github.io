---
title: "[Git] Git Config"
excerpt: ""
categories:
  - Git
tags:
  - [Git, Github, config]
toc: false
toc_sticky: false
date: 2022-11-10
last_modified_at: 2022-11-10
---

## Git 설정
Git을 설치하고 나면 환경설정을 해야하는데 아무것도 모른채 하라는데로만 하면 나중에 여러아이디를 사용하게 될 경우 로그인 문제라던가 보안에 적합하지 않다거나 하는 문제가 발생한다. 😥 나는 회사에서 사용하는 `GitTea`와 개인 `Git`계정 두개를 사용하는데 설정이 꼬여서 굉장히 답답했다. 이것에 대해 명확하게 설명해주는 사이트도 딱히 없었다 😣

## 설정 우선순위
보통 설정한다고하면 사용자정보만 넣고 끝내는데 이 설정정보는 `system, global, local` 3개의 파일로 관리하며 역순으로 우선순위가 정해져 있다.
> 주로 명령어를 통해 `local, global` 파일을 설정한다.

```
system < global < local

⭐우선순위가 있으므로  `global`에 사용자정보를 넣어도 특정 `repository`에서 다른 사용자정보를 넣고싶다면 `local`에 설정하면 된다.
```

각각의 설정은 아래 명령어를 통해 확인할 수 있다.
``` sh
## 설정 확인
# All
$ git config --list

# Local
$ git config --local --list

# Global
$ git config --global --list

# System
$ git config --system --list
```

각각의 설정파일 실제 경로는 아래 위치에 있다. 파일위치는 `git bash` 기준으로 설명한다.
``` sh
## 설정 파일 경로
# Local
$ vi {Repository 경로}/.git/config

# Global
$ vi ~/.gitconfig

# System
$ vi /etc/gitconfig
```

## 사용자 정보 설정
Git에서는 책임성을 유지하기위해 `이름`과 `이메일`을 설정해야한다. 직접 설정파일에 접근하여 추가하여도 되지만 아래 명령어를 통해서 설정 할 수 있다.

``` sh
# Global
$ git config --global user.name "{이름}"
$ git config --global user.email "{이메일}"

# Local
$ git config --local user.name "{이름}"
$ git config --local user.email "{이메일}"
```

## Github 로그인정보
Git을 이용하여 Remote Repository에 접근 할 경우 `SSH-key`를 이용하여 인증 할 수도 있지만 그렇지 않은 경우 `접속정보`를 입력해야한다. 매번 접속정보를 입력하기 귀찮으니 GIT에서는 이런 과정을 생략하는 `Credential`을 제공한다.  

GIT에서 제공하는 `Credential`을 사용하는 방법 또한 `Cache, Store, Keychain` 3가지가 있다.

### 1. Credential `Cache`
잠시 동안 접속정보를 저장하여 사용 할 떄 유용한 방법이다. 한번 접속정보를 입력하면 설정한 `timeout` 시간동안 인증정보를 저장한다. `timeout`을 지정하지 않으면 `defaul로 15분`동안 저장한다.

``` sh
# Credential Cache 설정
$ git config --global credential.helper 'cache --timeout=300'

# 적용 확인
$ git config --global --list

credential.helper=cache

...

```

### 2. Credential `Store`
접속정보를 Disk에 저장하여 계속 유지할 때 사용한다. 저장된 파일은 `--file` 옵션을 통해 저장위치를 지정할 수 있으며 지정하지 않을 경우 `default로 ~/.git-credentials`에 저장된다.

``` sh
# Credential Store 설정
$ git config --global credential.helper 'store --file ~/.my-credentials'

# 적용 확인
$ git config --global --list

credential.helper=store

...
```

### 3. Credential `Keychain` (Mac)
Stor와 같이 접속정보를 계속 유지하지만 `MAC OS에서 지원하는 Keychain 시스템`을 이용하는 방법이다. `Store 방식보다 안전`하다는 장점이 있다.

``` sh
# MAC Keychain 설정
$ git config --global credential.helper osxkeychain
```

### 4. Credential `wincred` (Windows)
동일하게 `Windows OS에서 지원하는 Keychain 시스템`을 이용하는 방법이다. 마찬가지로 `Store 방식보다 안전`하다는 장점이 있다.

``` sh
# Windows wincred 설정
git config --global credential.helper wincred
```