---
layout: post
title: '[Quasar & Capacitor] 화면켜짐 유지하기(Capacitor Keep Awake plugin)'
keywords: 'quasar, capacitor'
categories: javascript
tags: javascript vue quasar
image: https://images.unsplash.com/photo-1533376351882-bdcabed9b281?ixid=MXwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHw%3D&ixlib=rb-1.2.1&auto=format&fit=crop&w=1650&q=80
image-sm: https://images.unsplash.com/photo-1533376351882-bdcabed9b281?ixid=MXwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHw%3D&ixlib=rb-1.2.1&auto=format&fit=crop&w=1650&q=80
---

[[Quasar & Capacitor] 화면켜짐 유지하기(Cordova Insomnia plugin)](https://stove99.github.io/javascript/2021/04/19/quasar-capacitor-insomnia/) 에서 Cordova 플러그인 사용법을 알아봤는데

이것저것 보다보니 Insomnia 를 Capacitor 플러그인 형태로 제공하는 Keep Awake 라는게 있었다.

그냥 Cordova 플러그인 써도 되지만 Capacitor 프로젝트니깐 Capacitor 플러그인을 써 보기로 했다.

## Quasar 에 capacitor 모드추가하기

```bash
quasar mode add capacitor
quasar dev -m capacitor -T android
```

## 프로젝트에 Keep Awake 플러그인 추가하기

src-capacitor/package.json 을 확인해서 @capacitor/core 버전이 2.0 일 경우는 @capacitor-community/keep-awake@1.0.0 으로 설치하고,

@capacitor/core 버전이 3.0 일 경우는 그냥 최신버전으로 설치하면 된다.

```bash
cd src-capacitor

# capacitor 2.0 일때
npm i @capacitor-community/keep-awake@1.0.0

# capacitor 3.0 일때
npm i @capacitor-community/keep-awake

npx cap sync



# quasar dev 로 web 모드로 실행할때 depencency 오류가 나는데 capacitor 프로젝트 상위 프로젝트에도 똑같이 설치해 주면 해결된다.
cd ..
# capacitor 2.0 일때
npm i @capacitor-community/keep-awake@1.0.0

# capacitor 3.0 일때
npm i @capacitor-community/keep-awake
```

## vue 에서 플러그인 사용하기

```html
<template>
    <q-page class="flex flex-center"> 화면 꺼짐 방지 </q-page>
</template>

<script>
    import { KeepAwake } from '@capacitor-community/keep-awake';

    export default {
        name: 'PageIndex',
        created() {
            // 모바일 환경일때만 실행되도록
            if (this.$q.platform.is.capacitor) {
                KeepAwake.keepAwake();
            }
        },

        destroyed() {
            // 종료시 다시 원래대로
            if (this.$q.platform.is.capacitor) {
                KeepAwake.allowSleep();
            }
        },
    };
</script>
```
