---
layout: post
title: 'Windows WSL 과 연동된 Docker 에서 Jekyll 컨테이너 실행하기'
keywords: 'wsl,docker,Jekyll'
categories: docker
image: https://images.unsplash.com/photo-1497275898956-8166fcc6721d?crop=entropy&cs=tinysrgb&fit=crop&fm=jpg&h=1200&ixid=eyJhcHBfaWQiOjF9&ixlib=rb-1.2.1&q=80&w=2000
image-sm: https://images.unsplash.com/photo-1497275898956-8166fcc6721d?crop=entropy&cs=tinysrgb&fit=crop&fm=jpg&h=1200&ixid=eyJhcHBfaWQiOjF9&ixlib=rb-1.2.1&q=80&w=2000
---

[Windows WSL 에다 docker 를 연동](https://stove99.github.io/docker/2021/04/23/windows-wsl-docker/)하려고 했던 이유 중 하나는 

github 블로그 게시글을 작성하다보면 수정할 일이 많은데, github에 커밋한 다음 변경된 내용을 확인하려니 번거롭고 시간도 많이 걸렸다. 

그래서 로컬서버로 브라우저를 접속해 수정된 내용을 바로바로 확인해 보고 싶었다.

그런데 막상 docker 로 Jekyll 컨테이너를 띄워서 접속해 보니 스타일이 적용이 안되고 이것저것 오류가 많았다.

그냥 글내용만 대충보면서 확인하면 되지 하고 그냥 쓸려다가 아무리 봐도 꼴뵈기 싫어서 고쳐 보기로 했다.

문제점을 파악해 보니 css 를 http://0.0.0.0 에서 가져오도록 html 이 생성되서 그랬다.

이것저것 검색을 해서 해결법을 찾아 보았다.

먼저 루트 폴더에 _config_docker.yml 파일을 추가로 생성하고 내용을 아래와 같이 작성한다.

## _config_docker.yml

```yml
--- 
url: ""
```

## docker-compose.yml

그런 다음 docker-compose.yml 파일을 로컬용 Jekyll 컨피크 파일인 _config_docker.yml 사용하도록 요렇게 변경한다.

```yml
version: '3'
services: 
  site:
    container_name: my_blog
    command: jekyll serve --watch --host 0.0.0.0 --config _config.yml,_config_docker.yml
    image: jekyll/jekyll:3.8
    volumes: 
      - .:/srv/jekyll
    ports: 
      - 4000:4000
    environment:
      - JEKYLL_ENV=docker
```

컨테이너를 삭제하고 다시 시작한 다음 브라우저에서 http://localhost:4000 으로 접속해 확인하면 블로그가 쪽바로 잘 나온다.

