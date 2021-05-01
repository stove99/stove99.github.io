---
layout: post
title: '[VUE] Vue 에서 하단으로 스크롤 하기'
keywords: 'centos, mongodb'
categories: javascript
tags: javascript vue
image: https://images.unsplash.com/photo-1533376351882-bdcabed9b281?ixid=MXwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHw%3D&ixlib=rb-1.2.1&auto=format&fit=crop&w=1650&q=80
image-sm: https://images.unsplash.com/photo-1533376351882-bdcabed9b281?ixid=MXwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHw%3D&ixlib=rb-1.2.1&auto=format&fit=crop&w=1650&q=80
---

채팅앱에서 메시지가 수신 됐을때 맨 아래쪽에 메시지를 추가하고 아래로 스크롤 하는 예제

## Chat.vue

```html
<template>
    <div>
        <button @click="addMessage">메시지 추가</button>
        <div class="messages" ref="messages">
            <div class="container" v-for="(message, idx) in messages" :key="idx">
                <img src="https://cdn1.iconfinder.com/data/icons/website-internet/48/website_-_male_user-512.png" style="width:100%;" />
                <p>{{ message }}</p>
                <span class="time-right">11:00</span>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'Chat',
    data() {
        return {
            messages: [],
        };
    },
    methods: {
        addMessage() {
            this.messages.push(`메시지 - ${Math.random()}`);
        },
    },
    watch: {
        messages() {
            // 화면에 추가된 후 동작하도록
            this.$nextTick(() => {
                let messages = this.$refs.messages;

                messages.scrollTo({ top: messages.scrollHeight, behavior: 'smooth' });
            });
        },
    },
};
</script>

<style>
body {
    margin: 0 auto;
    max-width: 800px;
    padding: 0 20px;
}
.messages {
    height: 300px;
    overflow: auto;
}
.container {
    text-align: left;
    border: 2px solid #dedede;
    background-color: #f1f1f1;
    border-radius: 5px;
    padding: 10px;
    margin: 10px 0;
}

.darker {
    border-color: #ccc;
    background-color: #ddd;
}

.container::after {
    content: '';
    clear: both;
    display: table;
}

.container img {
    float: left;
    max-width: 60px;
    width: 100%;
    margin-right: 20px;
    border-radius: 50%;
}

.container img.right {
    float: right;
    margin-left: 20px;
    margin-right: 0;
}

.time-right {
    float: right;
    color: #aaa;
}

.time-left {
    float: left;
    color: #999;
}
</style>
```
