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

## 이유

Github 페이지로 훌로구 활동을 하면서 뭐 하나 살짝 수정하고 쪽바로 바뀌는지 확인하기 위해 커밋하고 푸쉬하고 잠시 시간이 흐른 후에 훌로구를 리프레쉬 하는 흑우짖을 해왔었다.

그렇게 하다 보니 매우 빠른 속도로 증가하는 Commit 수도 문제였고 바로바로 확인이 안되서 이것저것 불편한 점이 많았다.

그러다 문득 docker 로 로컬서버 띄우면 아주 깔꼼하게 모든 문제가 해결될것 같다는 생각을 하게 되었다.

## 적용방법

1. 훌로구 최상위에 docker-compose.yml 파일을 아래와 같은 내용으로 생성
2. docker-compose up 으로 로컬서버 구동
3. <http://127.0.0.1:4000> 에 접속해서 수정한것들 바로바로 확인
4. 다 된것 같으면 commit 후 github 에 push

## docker-compose.yml

``` yaml
version: '3'
services:
  blog:
    container_name: my_blog
    command: jekyll serve --watch
    image: jekyll/jekyll
    volumes:
      - $PWD:/srv/jekyll
    ports:
      - 4000:4000
```

### 로컬서버 실행

``` bash
docker-compose up
```

### 로컬서버 종료

ctrl + c 로 종료하면 되는데 예외 상황이 발생해서 서버가 계속 떠 있다면 당황하지 말고 요렇게 종료 시키면 된다.

``` bash
docker rm my_blog
```

## 결론

docker 가 이미 설치되 있다면 docker-compose.yml 파일만 만들어서 docker-compose up 만 하면 순식간에 로컬에다 훌로구를 띄울 수 있다.

나만 몰랐던 참 편한 세상이다.