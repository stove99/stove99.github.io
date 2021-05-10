---
layout: post
title: 'Firebase Function 리전을 서울(asia-northeast3)로 설정하기'
keywords: 'node, firebase, functions'
categories: nodejs
tags: nodejs nodejs_etc
image: https://images.unsplash.com/photo-1506813713591-56fc5e539b80?crop=entropy&cs=tinysrgb&fit=crop&fm=jpg&h=1200&ixid=eyJhcHBfaWQiOjF9&ixlib=rb-1.2.1&q=80&w=2000
image-sm: https://images.unsplash.com/photo-1506813713591-56fc5e539b80?crop=entropy&cs=tinysrgb&fit=crop&fm=jpg&h=1200&ixid=eyJhcHBfaWQiOjF9&ixlib=rb-1.2.1&q=80&w=2000
---

firebase function 을 서울 리전(asia-northeast3)에 생성하고 클라이언트에서 호출하기

## function 코드

```javascript
const functions = require("firebase-functions");

exports.test = functions
    .region("asia-northeast3")
    .https.onCall((data, context)=>{
        const { param1, param2 } = data;

        // 함수내용

        return {
            result: "ok";
        }
    });

```

## function 배포 또는 로컬 에뮬레이터로 테스트

```bash
# 배포
firebase deploy --only functions:test

# 로컬 에뮬레이터 실행
firebase emulators:start
```

## 클라이언트에서 호출하기

```javascript
import firebase from "firebase/app";
import "firebase/functions";

const firebaseConfig = {
    apiKey: "",
    authDomain: "",
    projectId: "",
    storageBucket: "",
    messagingSenderId: "",
    appId: ""
};

firebase.initializeApp(firebaseConfig);

const functions = firebase.app().functions("asia-northeast3");

// 로컬 에뮬레이터로 테스트 하기 위한 코드
if (location.hostname === "localhost") {
    functions.useEmulator("localhost", 5001);
}

const test = functions.httpsCallable("test");

test({param1 : '파라메터1', param2 : '파라메터2'}).then(result=>{
    console.log(result);    // "ok"
});

```