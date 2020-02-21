---
layout: post
title: '[Node.js] 알리고 SMS 메시지 보내기 예제'
keywords: 'node, aligo'
categories: node
tags: node
image: https://images.unsplash.com/photo-1529436916121-28a9a43d4fc9?crop=entropy&cs=tinysrgb&fit=crop&fm=jpg&h=1200&ixid=eyJhcHBfaWQiOjF9&ixlib=rb-1.2.1&q=80&w=2000
image-sm: https://images.unsplash.com/photo-1529436916121-28a9a43d4fc9?crop=entropy&cs=tinysrgb&fit=crop&fm=jpg&&ixid=eyJhcHBfaWQiOjF9&ixlib=rb-1.2.1&q=80&w=400
---

http 요청 보내는 라이브러리 중에 개인적으로 axios 가 제일 간결하고 좋아서 고것을 설치한다.

axios 패키지 설치

```bash
npm i axios

# 또는

yarn add axios
```

## 샘플소스

```js
const axios = require('axios');

const sendSms = ({ receivers, message }) => {
    return axios.post('https://apis.aligo.in/send/', null, {
        params: {
            key: 'API Key',
            user_id: '알리고 id',
            sender: '보내는사람 전화번호',
            receiver: receivers.join(','),
            msg: message,
            // 테스트모드
            testmode_yn: 'Y'
        },
    }).then((res) => res.data).catch(err => {
        console.log('err', err);
    });
}

// 메시지 보내기
sendSms({ receivers: ['01012341234', '010-4321-4321'], message: '메시지 테스트' }).then((result) => {
    console.log('전송결과', result);
});
```
