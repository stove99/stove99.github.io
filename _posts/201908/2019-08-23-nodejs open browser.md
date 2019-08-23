---
layout: post
title: '[Node.js] express 서버가 실행될때 브라우저로 원하는 url 열기'
date: 2019-08-23
keywords: 'node, opn, express'
categories: [Node.js]
image: https://images.unsplash.com/photo-1560747144-493e3754fa12?crop=entropy&cs=tinysrgb&fit=crop&fm=jpg&h=1200&ixid=eyJhcHBfaWQiOjF9&ixlib=rb-1.2.1&q=80&w=2000
image-sm: https://images.unsplash.com/photo-1560747144-493e3754fa12?crop=entropy&cs=tinysrgb&fit=crop&fm=jpg&h=1200&ixid=eyJhcHBfaWQiOjF9&ixlib=rb-1.2.1&q=80&w=2000
---

서버가 실행된후 로컬서버 url을 브라우저로 띄어 보자.

## 필요한 package 추가

```bash
npm i opn
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

## 소스 샘플

```js
import express from 'express';
import opn from 'opn';

const PORT = process.env.PORT || 3000;
const app = express();

...
...

app.listen(PORT, () => {
    console.log(`서버가 시작됐어염. http://127.0.0.1:${PORT} 로 접속하세요.`);

    opn(`http://127.0.0.1:${PORT}`);
});

...
...
```
