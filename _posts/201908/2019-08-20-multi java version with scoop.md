---
layout: post
title: "scoop 로 jdk 버전 바꿔가며 사용하기"
date: 2019-08-20
keywords: "scoop,java"
categories: [JAVA]
image: https://images.unsplash.com/photo-1541397734122-b770d5235576?crop=entropy&cs=tinysrgb&fit=crop&fm=jpg&h=1200&ixid=eyJhcHBfaWQiOjF9&ixlib=rb-1.2.1&q=80&w=2000
image-sm: https://images.unsplash.com/photo-1541397734122-b770d5235576?crop=entropy&cs=tinysrgb&fit=crop&fm=jpg&h=1200&ixid=eyJhcHBfaWQiOjF9&ixlib=rb-1.2.1&q=80&w=2000
---

scoop 는 windows 커맨드 라인에서 여러가지 오만 후로그램을 설치할 수 있도록 해주는 프로그램이다.

scoop 를 이용하면 여러가지 jdk 버전을 설치해서, 어떤 jdk 를 쓸 지 쉽게 변경할 수 있다.

## scoop 설치

윈도우 찾기 버튼을 클릭해서 PowerShell 을 검색해서 실행

<img src="/assets/attach/201908/powershell.png" style="width:900px;">

```bash
Set-ExecutionPolicy RemoteSigned -scope CurrentUser

iex (new-object net.webclient).downloadstring('https://get.scoop.sh')
```

<ins class="adsbygoogle"
     style="display:block; text-align:center;"
     data-ad-layout="in-article"
     data-ad-format="fluid"
     data-ad-client="ca-pub-7073298118440059"
     data-ad-slot="8400970402"></ins>

<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

## scoop 로 jdk 설치

jdk8 과 jdk1 버전을 설치해 보자

```bash
# git 이 설치안되 있으면 git 설치
scoop install git

# java bucket 추가
scoop bucket add java

# jdk8 설치
scoop install adopt8-hotspot

java -version
# openjdk version "1.8.0_xxx"

# jdk 최신버전 설치
scoop install adoptopenjdk-hotspot

java -version
# openjdk version "12.0.2"


##############################################
############## jdk 버전 변경하기 ##############

# jdk8 로 변경하기
scoop reset adopt8-hotspot

java -version
# openjdk version "1.8.0_xxx"


# jdk 최신버전으로 변경하기
scoop reset adoptopenjdk-hotspot

java -version
# openjdk version "12.0.2"

```

## 설치가능한 jdk 목록

<https://github.com/ScoopInstaller/Java/tree/master/bucket>
