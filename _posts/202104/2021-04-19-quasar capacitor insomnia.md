---
layout: post
title: '[Quasar & Capacitor] 화면켜짐 유지하기(Insomnia plugin)'
keywords: 'quasar, capacitor'
categories: javascript
tags: javascript vue quasar
image: https://images.unsplash.com/photo-1533376351882-bdcabed9b281?ixid=MXwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHw%3D&ixlib=rb-1.2.1&auto=format&fit=crop&w=1650&q=80
image-sm: https://images.unsplash.com/photo-1533376351882-bdcabed9b281?ixid=MXwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHw%3D&ixlib=rb-1.2.1&auto=format&fit=crop&w=1650&q=80
---

ionic native 에 있는 AudioManagement 를 사용하면 Native 볼륨을 컨트롤 할 수 있다. 하지만 지원되는 플랫폼은 안드로이드만 되는 것 같다.

## Quasar 에 capacitor 모드추가하기

```bash
quasar mode add capacitor
quasar dev -m capacitor -T android
```

## 프로젝트에 Insomnia 플러그인 추가하기

ionic native 에 있는 플러그인을 사용하기 위해서 @ionic-native/core 패키지도 추가적으로 설치가 필요하다.

```bash
cd src-capacitor
npm i @ionic-native/core cordova-plugin-insomnia @ionic-native/insomnia

npx cap sync

# quasar dev 로 web 모드로 실행할때 depencency 오류가 나는데 capacitor 프로젝트 상위 프로젝트에도 똑같이 설치해 주면 해결된다.
cd ..
npm i @ionic-native/core cordova-plugin-insomnia @ionic-native/insomnia
```

## vue 에서 플러그인 사용하기

```vue
<template>
    <q-page class="flex flex-center"> 화면 꺼짐 방지 </q-page>
</template>

<script>
import { Platform } from 'quasar';
import { Insomnia } from '@ionic-native/insomnia';

export default {
    name: 'PageIndex',
    created() {
        // 모바일 환경일때만 실행되도록
        if (this.$q.platform.is.capacitor) {
            Insomnia.keepAwake().then(
                () => console.log('Insomnia keepAwake success'),
                () => console.log('Insomnia keepAwake error')
            );
        }
    },

    destroyed() {
        // 종료시 다시 원래대로
        if (this.$q.platform.is.capacitor) {
            Insomnia.allowSleepAgain().then(
                () => console.log('Insomnia allowSleepAgain success'),
                () => console.log('Insomnia allowSleepAgain error')
            );
        }
    },
};
</script>
```
