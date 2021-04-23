---
layout: post
title: 'Windows WSL 과 Docker 연동하기'
keywords: 'wsl,docker'
categories: docker
image: https://images.unsplash.com/photo-1497275898956-8166fcc6721d?crop=entropy&cs=tinysrgb&fit=crop&fm=jpg&h=1200&ixid=eyJhcHBfaWQiOjF9&ixlib=rb-1.2.1&q=80&w=2000
image-sm: https://images.unsplash.com/photo-1497275898956-8166fcc6721d?crop=entropy&cs=tinysrgb&fit=crop&fm=jpg&h=1200&ixid=eyJhcHBfaWQiOjF9&ixlib=rb-1.2.1&q=80&w=2000
---

개인적인 생각으로 windows 용 docker 를 쌩으로 쓰는것 보다 wsl 을 통해서 리눅스 환경에서 docker 를 쓰는게 더 좋은것 같다.

## 체크사항

WSL 설치를 위해 먼저 바이오스에서 가상화 옵션을 켜줘야 되고, Windows 기능 중 Linux 용 Windows 하위 시스템을 체크해 줘야 된다.

<img src="/assets/attach/202104/wsl2_01.png">

<img src="/assets/attach/202104/wsl2_02.png">

## Ubuntu 설치

Microsoft Store 에 가서 Ubuntu 를 검색하면 여러개 나오는데 최신 버전으로 보이는 20.04 LTS 버전을 설치한다.

<img src="/assets/attach/202104/wsl2_03.png">

## WSL2  활성화

powershell 을 검색해서 관리자 권한으로 실행한다.

<img src="/assets/attach/202104/wsl2_04.png">

만약 명령어가 실행이 안된다면 재부팅이 한번하면 됨.

```bash
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart

# 설치된 ubuntu 확인
wsl -l

# 명령어로 나온 목록 Name 확인 => Ubuntu-20.04
# ubuntu wsl 버전을 2 설정
wsl --set-version Ubuntu-20.04 2

# 확인
wsl -l -v

#  NAME                   STATE           VERSION
#  Ubuntu-20.04           Running         2

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

## 윈도우용 Docker 설치 및 설정

[https://hub.docker.com/editions/community/docker-ce-desktop-windows](https://hub.docker.com/editions/community/docker-ce-desktop-windows) 에 가서 windows 용 docker 를 다운받아 설치한다.

설치 후 docker 설정화면을 열어서 다음과 같이 설정되 있는지 확인한다.

<img src="/assets/attach/202104/wsl2_05.png">
<img src="/assets/attach/202104/wsl2_06.png">

## ubuntu 에 접속해서 docker 명령어를 실행해 본다.

```bash
# 커맨드 창에서 wsl 이라고 입력하거나 검색란에 ubuntu 로 검색해서 나오는걸 클릭하면 ubuntu 에 접속할 수 있다.

docker --version

# nginx 설치해 보고 쪽바로 동작하나 확인해 보기
docker run -it -p 80:80 nginx

# 브라우저로 127.0.0.1 로 접속해 본다.
```