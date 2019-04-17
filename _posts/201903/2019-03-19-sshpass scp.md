---
layout: post
title:  "scp로 파일 전송할때 비밀번호 입력창없이 전송하기"
date:   2019-03-19
keywords: "sshpass,scp"
categories: [Linux]
tags: [sshpass,scp]
icon: icon-ubuntu
image: https://picsum.photos/2000/1200?random
image-sm: https://picsum.photos/500/300?random
---

## 이미지 가져오기 및 실행하기

    sshpass -p비밀번호 scp -o StrictHostKeyChecking=no -P 원격지SSH Port 로컬파일경로/파일명 원격지계정@원격지주소:원격지디렉토리

## example

    sshpass -ptestpass! scp -o StrictHostKeyChecking=no -P 22 ./test.bak stove99@test.com:/home/stove99

## sshpass 없을경우 설치

``` bash
    # ubuntu
    sudo apt install sshpass

    # centos
    sudo yum install sshpass
```