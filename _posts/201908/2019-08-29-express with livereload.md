---
layout: post
title: '[Node.js] express 에 live reload 적용하기'
keywords: 'node, express, livereload'
categories: nodejs
tags: nodejs nodejs_etc
image: https://images.unsplash.com/photo-1507546530-14a03f8d180a?crop=entropy&cs=tinysrgb&fit=crop&fm=jpg&h=1200&ixid=eyJhcHBfaWQiOjF9&ixlib=rb-1.2.1&q=80&w=2000
image-sm: https://images.unsplash.com/photo-1507546530-14a03f8d180a?crop=entropy&cs=tinysrgb&fit=crop&fm=jpg&h=1200&ixid=eyJhcHBfaWQiOjF9&ixlib=rb-1.2.1&q=80&w=2000
---

express로 개발할때 html, css 가 변경되면 브라우저에 바로바로 반영되게 맹글어 보자.

## 필요한 package 추가

livereload 패키지와 connect-livereload 패키지 설치

```bash
npm i livereload connect-livereload

## 또는
yarn add livereload connect-livereload
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

## 적용

```js
import express from 'express';
import livereload from 'livereload';
import livereloadMiddleware from 'connect-livereload';

// 라이브 서버 설정
const liveServer = livereload.createServer({
    // 변경시 다시 로드할 파일 확장자들 설정
    exts: ['html', 'css', 'ejs'],
    debug: true
});

liveServer.watch(__dirname);

...
...

const app = express();

...
...

// 미들웨어 설정
app.use(livereloadMiddleware());

...
...
```

## 크롬 확장프로그램 설치

<https://chrome.google.com/webstore/detail/livereload/jnihajbhpnppcggbcgedagnkighmdlei>
