---
layout: post
title: 'JAVA : SpringBoot 어플리케이션 heroku 에 올리기'
date: 2019-08-06
keywords: 'SpringBoot, heroku'
categories: [SpringBoot]
image: https://images.unsplash.com/photo-1468276898059-765dd484c793?crop=entropy&cs=tinysrgb&fit=crop&fm=jpg&h=1200&ixid=eyJhcHBfaWQiOjF9&ixlib=rb-1.2.1&q=80&w=2000
image-sm: https://images.unsplash.com/photo-1468276898059-765dd484c793?crop=entropy&cs=tinysrgb&fit=crop&fm=jpg&h=1200&ixid=eyJhcHBfaWQiOjF9&ixlib=rb-1.2.1&q=80&w=2000
---

SpringBoot 로 개발한 웹어플리케이션을 heroku 에 올려서 인터넷으로 접속해 보자.

## heroku cli 설치

<https://devcenter.heroku.com/articles/heroku-cli> 요기로 가서 각자의 환경에 맞게 설치하면 된다. 윈도우인 경우 윈도우용 인스톨러를 다운로드 받아서 설치하면 된다.

또는 node.js 가 설치되 있는 경우 npm 으로도 간단히 설치가 가능하다.

```bash
npm i -g heroku
```

설치후 쪽바로 설치됐나 확인

```bash
heroku -v
```

heroku 로그인 명령을 입력하면 브라우저 창이 하나 뜨는데 가입한 heroku 아이디로 로그인 하면 된다.

```bash
heroku login
```

## heroku app 생성

git 으로 소스를 관리하고 있지 않고 있다면, heroku app 을 생성하기 전에 git 저장소를 init 하고 로컬 git 에 커밋까지 하고나서 진행한다.

이미 git 으로 관리하고 있다면 넘어가도 됨

```bash
cd 소스있는디렉토리

git init

git add .
git commit -m "최초 커밋함"
```

heroku app create

```bash
heroku create
```

요렇게 하면 heroku app 생성되고 remote 저장소로 heroku 가 추가된다.

추가된 리모트 저장소 확인

```bash
git remote -v
```

## heroku 에 push

heroku git 저장소에 push 를 하면 자동으로 빌드후 웹어플리케이션이 실행된다.

참고로 pom.xml 패키징 설정이 war 면 웹어플리케이션이 쪽바로 실행안된다. jar 로 해야됨.

```bash
git push heroku master

# 웹어플리케이션 확인 / 웹브라우저가 실행되면서 웹어플리케이션을 확인할 수 있다.
heroku open
```

## 수정했을때 다시 올리기

git 에 커밋하고 remote heroku 저장소에 push 하면 끝~

```bash
git add .
git commit -m "이것저것 수정함"

git push heroku master
```
