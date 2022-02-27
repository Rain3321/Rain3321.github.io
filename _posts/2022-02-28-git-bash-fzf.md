---
layout: post
title: window git bash에서 fzf 사용하기
comments: true 
categories: etc
---

회사 맥에서 사용하는 터미널 툴중에 파일을 찾을 때 굉장히 강력한 툴이 바로 fzf 이다.

링크: [https://github.com/junegunn/fzf](https://github.com/junegunn/fzf)

git 작업을 할때도 굉장히 편리하다.

```
[alias]
    ch = "!git checkout $(git branch | fzf)"
```
예를 들어 이런식으로 해두면 브랜치 이동이 굉장히 편리하다.

근데 집에서 윈도우의 git bash 를 통해서 git 작업을 할 때 fzf 가 적용이 안되서 굉장히 불편하다.
그래서 간단하게 git bash 에 적용해보는 방법을 알아보자

---

# 1. fzf.exe 설치
[링크](https://github.com/junegunn/fzf/releases) 에서 windows 버전에 맞게 fzf.exe를 설치한다.

# 2. git 설치 경로로 파일 이동
다운로드된 fzf.exe 파일을 git 이 설치된 경로로 아래 `usr/bin` 으로 이동시킨다.

# 3. 행복하게 사용하기
현재 켜져있는 git bash 를 재시작하고 fzf 를 입력하면 행복하게 사용할 수 있다.
이제 다양한 git alias 를 적용하여 사용해보자.

더 많은 활용을 위해서는 이를 이용한 프로젝트를 사용해보면 되지 않을까?

[https://github.com/junegunn/fzf/wiki/Related-projects](https://github.com/junegunn/fzf/wiki/Related-projects)