---
layout: post
title: 'Froala editor 에디터 하단 Powered by Froala 메시지 제거하기'
keywords: 'Javascript, Froala'
categories: javascript
image: https://images.unsplash.com/photo-1444665283089-f1f68ad1d1dc?crop=entropy&cs=tinysrgb&fit=crop&fm=jpg&h=1200&ixid=eyJhcHBfaWQiOjF9&ixlib=rb-1.2.1&q=80&w=2000
image-sm: https://images.unsplash.com/photo-1444665283089-f1f68ad1d1dc?crop=entropy&cs=tinysrgb&fit=crop&fm=jpg&h=1200&ixid=eyJhcHBfaWQiOjF9&ixlib=rb-1.2.1&q=80&w=2000
---

Froala editor v3 버전을 보면 하단에 살짝 Powered by Froala 메시지가 출력된다.

이부분을 제거 하는 옵션은 attribution 인데 값을 false 로 설정해 주면 Powered by Froala 가 제거된다.

## 에디터 하단 Powered by Froala 메시지 제거하기

```js
new FroalaEditor('#editor', {
    attribution: false
});
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
