---
layout: post
title: 'Quasar v1 에서 vueuse 사용하기'
keywords: 'quasar, q-input'
categories: javascript
tags: javascript vue quasar
image: https://images.unsplash.com/photo-1533376351882-bdcabed9b281?ixid=MXwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHw%3D&ixlib=rb-1.2.1&auto=format&fit=crop&w=1650&q=80
image-sm: https://images.unsplash.com/photo-1533376351882-bdcabed9b281?ixid=MXwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHw%3D&ixlib=rb-1.2.1&auto=format&fit=crop&w=1650&q=80
---

사이트를 돌아 다니다 [vueuse](https://vueuse.org/) 라는걸 봤는데 꽤 쓸만한게 많아 보였다.

그래서 지금 하고 있는 프로젝트에 가져다 써보려고 했는데 시키는데로 했지만 잘 안 됐다.

지금 하고 있는 프로젝트가 quasar v2 라면 vue3 기반이라 그냥 가져다 쓰면 될것 같은데

vue2 기반인 quasar v1 은 쫌 설정이 필요하다.

(v2를 쓰면 되는데 왜 v1을 쓰고 있는가? IE11 을 지원해야 되기 때문이다 -\_-)

## library install

```bash
yarn add @vue/composition-api @vueuse/core
```

## boot/composition-api.js 생성

정확히는 모르지만 vueuse 가 vue-demi 라는 툴로 vue2/vue3 를 동시에 지원하도록 돼 있는데 vue-demi 가 내부적으로 vue2 인 경우 VueCompositionApi를 인스톨 하는것 같다.
dummy 로 아무거나 vueuse 에서 import 한번 해 줘야 쪽바로 작동한다. 정확인 이유는 잘 모름 'ㅗ'

```js
import VueCompositionApi from '@vue/composition-api';
import { boot } from 'quasar/wrappers';

// dummy 로 필요함
import { useMouse } from '@vueuse/core';

export default boot(({ Vue }) => {
    Vue.use(VueCompositionApi);
});
```

## quasar.conf.js boot 설정

```
...
    boot: ['기존에 있던 boot설정', 'composition-api']
...

```

요렇게 해주면 사이트에 있는 예제처럼 했을 때 잘 작동한다.
