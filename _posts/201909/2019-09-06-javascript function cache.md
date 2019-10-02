---
layout: post
title: '[JS] Javascript 로 함수 캐시하기'
keywords: 'javascript'
categories: javascript
image: https://images.unsplash.com/photo-1535406148013-5806f2127f58?crop=entropy&cs=tinysrgb&fit=crop&fm=jpg&h=1200&ixid=eyJhcHBfaWQiOjF9&ixlib=rb-1.2.1&q=80&w=2000
image-sm: https://images.unsplash.com/photo-1535406148013-5806f2127f58?crop=entropy&cs=tinysrgb&fit=crop&fm=jpg&h=1200&ixid=eyJhcHBfaWQiOjF9&ixlib=rb-1.2.1&q=80&w=2000
---

스프링에서 쓰던 캐시처럼 자바스크립트에서도 수행시간이 오래 걸리는 함수를 캐시해서 수행시간을 줄여보자.

## 캐시되는 함수 생성함수

파라메터로 캐싱할 함수를 넘겨줌

```js
/**
 * 캐시함수 생성함수
 * @param func 생성할 함수
 */
var cache = function(func) {
    var c = {};

    return function(...args) {
        var key = JSON.stringify(args);

        if (!c[key]) {
            console.log('캐시 X');
            c[key] = func(...args);
        } else {
            console.log('캐시 O');
        }

        return c[key];
    };
};
```

<ins class="adsbygoogle"
     style="display:block; text-align:center;"
     data-ad-layout="in-article"
     data-ad-format="fluid"
     data-ad-client="ca-pub-7073298118440059"
     data-ad-slot="8400970402"></ins>

<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

## 테스트

```js
var test = cache(function(a, b) {
    return a + b;
});

console.log(test(1, 5)); // 캐시 X
console.log(test(1, 5)); // 캐시 O
console.log(test(2, 5)); // 캐시 X
console.log(test(3, 5)); // 캐시 X
console.log(test(1, 5)); // 캐시 O
console.log(test(2, 5)); // 캐시 O
```
