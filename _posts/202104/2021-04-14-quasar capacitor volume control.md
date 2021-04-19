---
layout: post
title: '[Quasar & Capacitor] Native 볼륨 컨트롤 하기'
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

## 프로젝트에 AudioManagement 플러그인 추가하기

ionic native 에 있는 플러그인을 사용하기 위해서 @ionic-native/core 패키지도 추가적으로 설치가 필요하다.

```bash
cd src-capacitor
npm i @ionic-native/core clovelced-plugin-audiomanagement @ionic-native/audio-management

npx cap sync

# quasar dev 로 web 모드로 실행할때 depencency 오류가 나는데 capacitor 프로젝트 상위 프로젝트에도 똑같이 설치해 주면 해결된다.
cd ..
npm i @ionic-native/core clovelced-plugin-audiomanagement @ionic-native/audio-management
```

## vue 에서 플러그인 사용하기

```vue
<template>
    <q-page class="flex flex-center">
        {{ voloume }}

        <q-slider v-model="voloume" :min="0" :max="maxVolume" color="green" />
    </q-page>
</template>

<script>
import { AudioManagement } from '@ionic-native/audio-management';

export default {
    name: 'PageIndex',
    data() {
        return {
            maxVolume: 0,
            voloume: 0,
        };
    },
    watch: {
        voloume(value) {
            // MUSIC 볼륨 설정하기
            AudioManagement.setVolume(1, value);
        },
    },
    async mounted() {
        // Volume Type
        // AudioManagement.VolumeType.RING = 0
        // AudioManagement.VolumeType.MUSIC = 1
        // AudioManagement.VolumeType.NOTIFICATION = 2
        // AudioManagement.VolumeType.SYSTEM = 3

        // 현재 MUSIC 볼륨값 가져오기
        const curVolume = await AudioManagement.getVolume(1);

        // MUSIC 볼륨 최대값 가져오기
        const volume = await AudioManagement.getMaxVolume(1);

        this.voloume = curVolume.volume;
        this.maxVolume = volume.maxVolume;

        console.log('curVolume', curVolume.volume);
        console.log('maxVolumn', volume.maxVolume);
    },
};
</script>
```
