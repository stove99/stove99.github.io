---
layout: post
title: 'npm 또는 yarn 으로 windows-build-tools 설치 시 멈춰서 진행이 안 될 때'
keywords: 'nestjs, windows-build-tools'
categories: nodejs
tags: nodejs
image: https://images.unsplash.com/photo-1533376351882-bdcabed9b281?ixid=MXwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHw%3D&ixlib=rb-1.2.1&auto=format&fit=crop&w=1650&q=80
image-sm: https://images.unsplash.com/photo-1533376351882-bdcabed9b281?ixid=MXwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHw%3D&ixlib=rb-1.2.1&auto=format&fit=crop&w=1650&q=80
---

나만 그런지 모르겠는데 최근에 windows-build-tools 를 설치할때 아무리 해도 설치가 진행이 안되는 상황이 발생했다. 혹시 네트웍이 느려서 그런가 해서 하루 이상 냅둬봐도 설치가 진행이 안됐다.

결론는 npm 이나 yarn 으로 설치는 포기하고 Chocolatey 라는 윈도우용 패키지 매니저로 설치하는 것으로 해결봤다.

## Chocolatey 설치

Powershell 을 관리자 권한으로 실행한다.

```bash
Set-ExecutionPolicy Bypass -Scope Process

Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))
```

## windows-build-tools 설치

npm 으로 windows-build-tools 설치시 python 도 같이 설치 되는데 choco 로 python 과 visualcpp-build-tools 를 설치한다.

```bash
choco install python visualcpp-build-tools

# npm 에서 설치된 build 툴 사용하도록 설정
npm config set msvs_version 2017

# yarn 에서 설치된 build 툴 사용하도록 설정
yarn config set msvs_version 2017
```
