---
layout: post
title: '[Quasar v2] q-input 한글입력 처리'
keywords: 'quasar, q-input'
categories: javascript
tags: javascript vue quasar
image: https://images.unsplash.com/photo-1533376351882-bdcabed9b281?ixid=MXwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHw%3D&ixlib=rb-1.2.1&auto=format&fit=crop&w=1650&q=80
image-sm: https://images.unsplash.com/photo-1533376351882-bdcabed9b281?ixid=MXwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHw%3D&ixlib=rb-1.2.1&auto=format&fit=crop&w=1650&q=80
---

q-input 컴포넌트에 한글을 입력하면 model 에 바로바로 반영 안되는 문제점이 있다.

v1 버전에서는 @input 이벤트가 있어서 @input.native="setValue($event.target.value)" 이렇게 해서 처리할 수 있었지만 (vue3 에는 native modifier 도 deprecate 됐음)

v2 로 바뀌면서 @input 가 없어지고 @update:model-value 로 변경되서 처리하기가 난감했다.

문서를 쭉 읽어보니 method 중에 getNativeElement() 가 있어서 저걸 활용하면 될 것 같아 한번 해보니 잘됐다.

## 소스

```html
<template>
    <q-page class="flex flex-center">
        <q-input ref="keywordRef" :model-value="keyword" label="검색어입력" />
        keyword : \{{ keyword }}
    </q-page>
</template>

<script>
    import { defineComponent, ref, onMounted } from 'vue';

    export default defineComponent({
        name: 'Home',

        setup() {
            const keyword = ref(null);
            const keywordRef = ref(null);

            onMounted(() => {
                // ref 로 native element 를 가져와서 input 이벤트가 발생할때 model 에 연결된 변수에 값을 저장한다.
                const el = keywordRef.value.getNativeElement();
                el.addEventListener('input', (e) => {
                    keyword.value = e.target.value;
                });
            });

            return { keywordRef, keyword };
        },
    });
</script>
```
