---
layout: post
title: '[Quasar & Capacitor] 길게 눌러서 삭제하기 구현하기 + 진동'
keywords: 'quasar, capacitor'
categories: javascript
tags: javascript vue quasar
image: https://images.unsplash.com/photo-1533376351882-bdcabed9b281?ixid=MXwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHw%3D&ixlib=rb-1.2.1&auto=format&fit=crop&w=1650&q=80
image-sm: https://images.unsplash.com/photo-1533376351882-bdcabed9b281?ixid=MXwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHw%3D&ixlib=rb-1.2.1&auto=format&fit=crop&w=1650&q=80
---

## 사전작업

진동기능과 quasar 에서 제공하는 컨펌 다이얼로그를 쓰기 위해 사전 작업이 필요하다.

```bash
# 진동을 위해
# 프로젝트 루트에 @capacitor/core 패키지 설치
npm i @capacitor/core

# src-capacitor\android\app\src\main\AndroidManifest.xml 에 진동관련 권한 추가
<uses-permission android:name="android.permission.VIBRATE" />

# 다이얼로그 플러그인 설정
# quasar.conf.js 파일 framework - plugins 배열에 "Dialog" 항목 추가
# ex
        framework: {
            iconSet: "material-icons",
            lang: "ko-kr",
            config: {},

            importStrategy: "auto",

            plugins: ["Dialog"]
        },

```

## 참고 소스

[quasar 문서](https://quasar.dev/vue-directives/touch-hold)에서는 길게 눌러서 실행하는 함수 호출시 파라메터를 같이 넘기는 예제가 없었다.

늘 하던데로 함수 호출시 파라메터를 넘기니 이상하게 동작했다. 찾아보니 대상 호출 함수를 참고 소스 처럼 함수를 리턴하게 작성하면 된다.

※ 리턴하는 함수 작성시 function() 으로 선언하지 말고 arrow function을 써야 컴포넌트 내부 data 를 접근해서 변경할 수 있다.

```html
<template>
    <q-page class="flex flex-center">
        <!-- 1.5 초 동안 눌렀을때 removeItem 호출 -->
        <q-btn v-for="item in items" :key="item.id" v-touch-hold:1500.mouse="removeItem(item.id)" color="primary" :label="item.name" />
    </q-page>
</template>

<script>
import { Plugins } from '@capacitor/core';

const { Haptics } = Plugins;

export default {
    name: 'PageIndex',
    data() {
        return {
            items: [
                {
                    id: 1,
                    name: '아이템1',
                },
                {
                    id: 2,
                    name: '아이템2',
                },
                {
                    id: 3,
                    name: '아이템3',
                },
                {
                    id: 4,
                    name: '아이템4',
                },
                {
                    id: 5,
                    name: '아이템5',
                },
            ],
        };
    },
    methods: {
        removeItem(id) {
            // arrow function 으로 만들어야 this 로 컴포넌트 데이터를 접근할 수 있다.
            return (evt) => {
                // 길게 누른 이벤트에 대한 정보는 evt 변수로 접근할 수 있다.

                // 진동시킨 다음 컨펌창 띄우기
                Haptics.vibrate();

                this.$q
                    .dialog({
                        title: '확인',
                        message: '지울래요?',
                        cancel: true,
                        persistent: true,
                    })
                    .onOk(() => {
                        this.items = this.items.filter((item) => item.id !== id);
                    });
            };
        },
    },
};
</script>
```
