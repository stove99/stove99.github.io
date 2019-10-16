---
layout: post
title: 'Mac 안드로이드 에뮬레이터에서 인터넷이 안될때'
keywords: 'android emulator'
categories: etc
tags: etc
---

앱을 개발하다 에뮬레이터에서 한번 돌려 봐야지~ 하고 에뮬레이터로 실행시켰는데 동작이 전혀 되질 않았다.

네트워크 때문일줄은 꿈에도 몰랐기 때문에 원인을 파악하기 위해 한참을 찾았는데

허무하게도 에뮬레이터에서 인터넷 접속이 안돼서 나는 오류였다.

곤칠려면 시스템 설정 - 네트워크 - 활성화된 네트워크 선택 - 고급 - DNS 탭 - 8.8.8.8 추가

끝.

<img src="/assets/attach/201910/capture1.png" style="width:500px;">

<img src="/assets/attach/201910/capture2.png" style="width:500px;">
