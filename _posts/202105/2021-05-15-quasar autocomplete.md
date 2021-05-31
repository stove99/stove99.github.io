---
layout: post
title: '[Quasar] Autocomplete 구현하기(한글입력)'
keywords: 'quasar, autocomplete'
categories: javascript
tags: javascript vue quasar
image: https://images.unsplash.com/photo-1533376351882-bdcabed9b281?ixid=MXwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHw%3D&ixlib=rb-1.2.1&auto=format&fit=crop&w=1650&q=80
image-sm: https://images.unsplash.com/photo-1533376351882-bdcabed9b281?ixid=MXwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHw%3D&ixlib=rb-1.2.1&auto=format&fit=crop&w=1650&q=80
---

q-select 컴포넌트로 autocomplete 기능을 간단하게 만들수 있지만 문제는 한글입력이었다. 영어로 autocomplete 할 경우는 거의 없으니까 -.-

vue 에서 한글 입력시 model 에 바로바로 반영이 안된다. 검색을 해 보면 @input 으로 처리하라 그러는데

또 문제는 q-select 의 @input 이나 @input-value 는 내부적으로 다른 처리를 하는 것 같아서 원하는 이벤트가 발생하지 않았다.

아무튼 이래저래 삽질하면서 이것저것 조합해서 문제를 해결했다.

1. v-model 대신 :value 로 변경
2. @input.native 이벤트
3. ref 를 활용해서 @input.native 이벤트 발생시 q-select filter 메소드 호출로 autocomplete 처리

## 소스

```html
<template>
    <div>
        <div class="q-gutter-md row">
            <q-select
                ref="sample"
                filled
                :value="model"
                use-input
                hide-selected
                fill-input
                :options="options"
                @input.native="filter($event.target.value)"
                @filter="filterFn"
                @input-value="setValue"
                hide-dropdown-icon
                style="width: 250px; padding-bottom: 32px"
                new-value-mode="add-unique"
            >
            </q-select>
        </div>
    </div>
</template>

<script>
    import { debounce } from 'quasar';

    let suggestList = ['한글', '세탁기', '티비', '아령', '아세톤', '글램핑'];

    export default {
        name: 'Index',

        created() {
            // filter 함수 debounce 처리(입력할때 마다 필터링 하지 않고 살짝 딜레이 줘서 필터링 되게)
            // 자세한건 debounce 검색~
            this.filter = debounce(this.filter, 500);
        },

        data() {
            return {
                model: null,
                options: suggestList,
            };
        },

        methods: {
            setValue(value){
                this.model = value;
            },

            filter(keyword) {
                this.$refs.sample.filter(keyword);
                this.model = keyword;
            },
            filterFn(val, update, abort) {
                // 1글자이상 입력시
                if (val.length < 1) {
                    abort();
                    return;
                }

                update(() => {
                    const keyword = val.toLowerCase();
                    this.options = suggestList.filter((v) => v.toLowerCase().indexOf(keyword) > -1);
                });
            },
        },
    };
</script>
```
