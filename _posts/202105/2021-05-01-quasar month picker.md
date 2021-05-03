---
layout: post
title: '[Quasar] 년/월 선택 (quasar month picker)'
keywords: 'quasar'
categories: javascript
tags: javascript vue quasar
image: https://images.unsplash.com/photo-1533376351882-bdcabed9b281?ixid=MXwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHw%3D&ixlib=rb-1.2.1&auto=format&fit=crop&w=1650&q=80
image-sm: https://images.unsplash.com/photo-1533376351882-bdcabed9b281?ixid=MXwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHw%3D&ixlib=rb-1.2.1&auto=format&fit=crop&w=1650&q=80
---

## Month Picker

년/월만 선택하고 싶을때 q-date 컴포넌트에 옵션이 있을것 같지만 의외로 그런 옵션이 없다.

그래서 살짝 작업을 해 줘야 한다.

q-date 에서 뭔가를 선택하는 이벤트가 input 이고 input 이벤트가 발생했을때 무엇을 선택했는지 판단해서 캘린더 팝업을 닫도록 한다.

```html
<template>
    <q-page class="flex flex-center">

        <q-input
            readonly
            input-class="cursor-pointer"
            :value="selectedMonth"
            @click="$refs.monthPicker.show()"
        >
            <template v-slot:append>
                <q-icon name="event" class="cursor-pointer">
                    <q-popup-proxy
                        ref="monthPicker"
                        transition-show="scale"
                        transition-hide="scale"
                    >
                        <q-date
                            ref="calendar"
                            emit-immediately
                            years-in-month-view
                            default-view="Months"
                            v-model="selectedMonth"
                            @input="checkSelected"
                            mask="YYYY/MM"
                            subtitle="년/월 선택"
                            :title="`${selectedMonth}`"
                        />
                    </q-popup-proxy>
                </q-icon>
            </template>
        </q-input>
    </q-page>
</template>

<script>
export default {
    name: "Home",

    data() {
        return {
            selectedMonth: null
        };
    },

    methods: {
        checkSelected(_, reason, __) {
            if (reason === "month") {
                this.$refs.monthPicker.hide();
            } 
            // 년도 선택시 달 선택 화면으로 이동
            else if (reason === "year") {
                this.$refs.calendar.setView("Months");
            }
        }
    },
};
</script>
```