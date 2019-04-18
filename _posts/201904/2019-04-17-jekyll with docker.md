---
layout: post
title:  "내 PC에서 Github Page 편집하고 바로바로 확인하기(Docker 를 활용한 jekyll 로컬 서버 맹글기)"
date:   2019-04-17
keywords: "docker,jekyll,github"
categories: [Docker]
tags: [docker]
icon: icon-docker
image: https://picsum.photos/2000/1200?image=2
image-sm: https://picsum.photos/500/300?image=2
---

Github 페이지로 훌로구 활동을 하면서 뭐 하나 살짝 수정하고 쪽바로 바뀌는지 확인하기 위해 커밋하고 푸쉬하고 잠시 시간이 흐른 후 훌로구를 리프레쉬 하는 흑우짖을 해왔었다.

그렇게 하다 보니 매우 빠른 속도로 증가하는 Commit 수도 문제였고 바로바로 확인이 안 되서 이것저것 불편한 점이 많았다.

그러다 문득 docker 로 로컬서버를 구동하면 아주 깔꼼하게 모든 문제가 해결될 것 같다는 생각을 하게 되었다.

## 적용방법

1. 훌로구 최상위에 docker-compose.yml 파일을 아래와 같은 내용으로 생성
2. docker-compose up 으로 로컬서버 구동
3. <http://127.0.0.1:4000> 에 접속해서 수정한것들 바로바로 확인
4. 다 된것 같으면 commit 후 github 에 push

## docker-compose.yml

``` yaml
version: '3'
services: 
  site:
    container_name: my_blog
    command: jekyll serve --watch --destination /home/jekyll/dist
    image: jekyll/jekyll
    volumes: 
      - .:/srv/jekyll
    ports: 
      - 4000:4000
```

## 로컬서버 실행

``` bash
docker-compose up
```

## 로컬서버 종료

ctrl + c 로 종료하면 되는데 예외 상황이 발생해서 서버가 계속 구동되 있다면 당황하지 말고 요렇게 종료 시키면 된다.

``` bash
docker rm my_blog
```

## Docker 컨테이너에 접속하기

컨테이너에 접속해서 추가 적으로 작업할 것이 있다면

``` bash
docker exec -it my_blog /bin/sh
```

## Windows Docker 에서 작업하기

Winodws Docker 는 Shared Drive 설정을 해 줘야 한다. 그리고 파일 변경시 이벤트가 발생하지 않아서 docker jekyll 에서 재빌드 되지 않는 현상이 발생한다.

파일 변경하면 docker jekyll 을 껏다 켯다 하는 불편함이 있다. 뭐 이렇게 껏다 켰다 하면서 해도 되고 불편하면 [docker-windows-volume-watcher](https://github.com/hnakamur/docker-windows-volume-watcher/releases) 라는 후로그램을 실행시켜 놓고 써도 된다.

1. Docker Windows Volume Watcher
 - [다운로드](https://github.com/hnakamur/docker-windows-volume-watcher/releases)
 - 다운로드 받은 docker-windows-volume-watcher.exe 파일을 PATH 가 설정된 디렉토리(ex)c:\windows)에 복사한다.
 - 현재 프로젝트 최상위 디렉토리에서 docker-windows-volume-watcher 를 실행한다.
 - docker-windows-volume-watcher -ignoredir .git
2. Shared Drive 설정

<img src="/assets/attach/201904/windows-docker-setting.png" alt="drawing" style="max-width:700px;"/>

## 결론

docker 가 이미 설치되 있다면 docker-compose.yml 파일만 만들어서 docker-compose up 만 하면 순식간에 로컬에다 훌로구를 띄울 수 있다.

나만 몰랐던 참 편한 세상이다.
