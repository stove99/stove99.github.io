---
layout: post
title: 'Node.js 운영/개발 환경별 프로퍼티 설정하기'
date: 2019-04-26
keywords: 'Node.js'
categories: [Node.js]
image: https://picsum.photos/2000/1200?image=1033
image-sm: https://picsum.photos/500/300?image=1033
---

어떻게 하면 운영환경과 개발환경 환경변수를 깔꼼하게 관리할 수 있을까? 하는 고민을 하면서 이것저것 찾아보다가 env-cmd 를 사용하는 것이 내가 바라는 방식에 가장 잘 맞을 것 같아서 한번 정리해 보았다.

## 설치

    npm i -D env-cmd

## 환경변수 설정

프로젝트 루트 경로에 .env-cmdrc 파일 생성하고 환경변수를 각각 개발/운영환경에 맞게 작성한다.

```json
{
    "common": {
        "ENV_PROP1": "값1",
        "ENV_PROP2": "값2"
    },
    "dev": {
        "db_url": "jdbc:mariadb://127.0.0.1:3306/dbname",
        "dp_user": "test",
        "dp_psssword": "test"
    },
    "prod": {
        "db_url": "jdbc:mariadb://xxx.xxx.xxx.xxx:3306/dbname",
        "dp_user": "test1",
        "dp_psssword": "test2"
    }
}
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

## package.json scripts 부분에 적용하기

요런식으로 node 실행전에 env-cmd 로드하고자 하는 환경변수를 추가해 주면 된다.

```json
"scripts": {
    "start": "env-cmd common,prod node index.js",
    "start:dev": "env-cmd common,dev nodemon index.js"
}
```

## 테스트 index.js

```javascript
console.log(`env ENV_PROP1 : ${process.env.ENV_PROP1}`); // 값1
console.log(`env db_url : ${process.env.db_url}`); // prod, dev 에 따라 값 변경됨
```
