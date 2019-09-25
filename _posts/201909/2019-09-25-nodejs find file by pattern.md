---
layout: post
title: '[Node.js] 패턴과 일치하는 파일목록 찾기'
date: 2019-09-25
keywords: 'node, glob'
categories: [Node.js]
image: https://images.unsplash.com/photo-1444483911532-30de7b1b0aaf?crop=entropy&cs=tinysrgb&fit=crop&fm=jpg&h=1200&ixid=eyJhcHBfaWQiOjF9&ixlib=rb-1.2.1&q=80&w=2000
image-sm: https://images.unsplash.com/photo-1444483911532-30de7b1b0aaf?crop=entropy&cs=tinysrgb&fit=crop&fm=jpg&h=1200&ixid=eyJhcHBfaWQiOjF9&ixlib=rb-1.2.1&q=80&w=2000
---

glob 패키지를 사용하면 쉽게 문제를 해결할 수 있다.

## 설치

```bash
npm i glob

// 또는
yarn add glob
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

## 현재 프로젝트 안에서 \*.xml 파일 목록 찾기

```js
import * as glob from 'glob';

glob(
    '**/*.xml',
    {
        dot: true,
        // node_modules 은 검색대상에서 제외
        ignore: ['node_modules/**']
    },
    (er, files) => {
        console.log('xml files', files);
    }
);
```

기타 자세한 사용법은 <https://www.npmjs.com/package/glob> 요기 참고.
