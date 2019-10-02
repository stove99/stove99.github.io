---
layout: post
title: 'Gmail javax.mail.AuthenticationFailedException: 535-5.7.8 Username and Password not accepted. 에러'
keywords: 'java,gmail'
categories: java
image: https://picsum.photos/2000/1200?image=1043
image-sm: https://picsum.photos/500/300?image=1043
---

Javamail 을 사용해 gmail 로 메일을 보낼 때 아이디 암호를 쪽바로 셋팅했는데도 AuthenticationFailedException 발생하는 경우가 있다.

원인은 보안 수준이 낮은 앱의 액세스를 차단했기 때문이다.

[구글계정 - 보안](https://myaccount.google.com/security) 에 접속해서 보안 수준이 낮은 앱의 액세스를 허용해 주면 쪽바로 메일이 잘 가진다.

<img src="/assets/attach/201904/gmail1.png" style="max-width:1000px;">

<img src="/assets/attach/201904/gmail2.png" style="max-width:900px;">
