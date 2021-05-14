---
layout: post
title: 'Vue 다이나믹 컴포넌트로 탭별 컴포넌트 불러오기'
keywords: 'vue, dynamic component'
categories: javascript
tags: javascript vue
image: https://images.unsplash.com/photo-1533376351882-bdcabed9b281?ixid=MXwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHw%3D&ixlib=rb-1.2.1&auto=format&fit=crop&w=1650&q=80
image-sm: https://images.unsplash.com/photo-1533376351882-bdcabed9b281?ixid=MXwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHw%3D&ixlib=rb-1.2.1&auto=format&fit=crop&w=1650&q=80
---

각 탭별로 다른 컴포넌트를 보여줘야 할 때 아래처럼 v-if 로 각 탭별 컴포넌트를 보였다 안보였다 처리를 할 수 있다.

## Before

```html
<template>
    <div>
        <div>
            <button @click="changeTab('Tab1')">tab1</button>
            <button @click="changeTab('Tab2')">tab2</button>
            <button @click="changeTab('Tab3')">tab3</button>
        </div>

        <div>
            <Tab1 v-if="tab==='Tab1'"/>
            <Tab2 v-if="tab==='Tab2'"/>
            <Tab3 v-if="tab==='Tab3'"/>
        </div>
    </div>
</template>

<script>
import Tab1 from '`../components/Tab';
import Tab2 from '`../components/Tab';
import Tab3 from '`../components/Tab';

export default {
    name: 'Index',

    components : {
        Tab1, Tab2, Tab3
    },

    data() {
        return {
            tab: "Tab1",
        };
    },

    methods: {
        changeTab(tab) {
            this.tab = tab;
        }
    }
    
};

</script>
```

하지만 최대 문제는 없어 보인다는 것이다. 좀 더 있어 보이게 할려면 다이내믹 컴포넌트를 활용하면 된다.

## After

```html
<template>
    <div>
        <div>
            <button @click="changeTab('Tab1')">tab1</button>
            <button @click="changeTab('Tab2')">tab2</button>
            <button @click="changeTab('Tab3')">tab3</button>
        </div>

        <div>
            <keep-alive>
                <component :is="component"></component>
            </keep-alive>
        </div>
    </div>
</template>

<script>
export default {
    name: 'Index',

    data() {
        return {
            tab: "Tab1",
        };
    },

    computed: {
        component() {
            const tab = this.tab;
            return () => import(`../components/${tab}`);
        }
    },

    methods: {
        changeTab(tab) {
            this.tab = tab;
        }
    }
    
};
</script>
```

이렇게 다이내믹 컴포넌트를 활용하면 일일히 각 탭별 컴포넌트들을 import 해서 components 에 등록 안해도 되는데다 있어 보이기 까지 하니 매우 흡족스럽다.