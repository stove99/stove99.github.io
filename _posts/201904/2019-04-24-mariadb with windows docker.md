---
layout: post
title: 'Windows Docker 에서 MariaDB 설치하기'
date: 2019-04-24
keywords: 'docker,MariaDB'
categories: [Docker]
image: https://picsum.photos/2000/1200?image=1025
image-sm: https://picsum.photos/500/300?image=1025
---

MariaDB 공식 Docker 이미지로 -v 옵션을 줘서 data 가 저장될 폴더를 윈도우에 있는 폴더로 지정할때 오류가 나면서 컨테이너가 실행되지 않는다.

쫌 찾아봤는데 공식 이미지로는 오류가 나서 bitnami/mariadb 요 이미지를 사용하라고 했다.

## 컨테이너 실행하기

요렇게 하면 D:\docker\maria 폴더에 데이터가 저장된다. 만약 실행이 안되면 해당 경로에 폴더를 만들어 주면 된다.

컨테이너를 삭제 했다가 다시 만들어도 데이터 볼륨만 -v 옵션으로 다시 지정해 주면 데이터가 고대로 유지된다.

    docker run -d -p 3306:3306 -v d:\docker\maria:/bitnami/mariadb -e MARIADB_ROOT_PASSWORD=비밀번호 --name maria bitnami/mariadb

<ins class="adsbygoogle"
     style="display:block; text-align:center;"
     data-ad-layout="in-article"
     data-ad-format="fluid"
     data-ad-client="ca-pub-7073298118440059"
     data-ad-slot="8400970402"></ins>

<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

## 종료

    docker stop maria

## 다시 시작하기

    docker start maria

## 쉘접속

    docker exec -it maria /bin/bash
