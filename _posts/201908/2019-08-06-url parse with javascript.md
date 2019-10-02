---
layout: post
title: 'Javascript 로 파라메터 가져오기(URI.js)'
keywords: 'javascript, url, parameter'
categories: javascript
image: https://images.unsplash.com/photo-1501436513145-30f24e19fcc8?crop=entropy&cs=tinysrgb&fit=crop&fm=jpg&h=1200&ixid=eyJhcHBfaWQiOjF9&ixlib=rb-1.2.1&q=80&w=2000
image-sm: https://images.unsplash.com/photo-1501436513145-30f24e19fcc8?crop=entropy&cs=tinysrgb&fit=crop&fm=jpg&h=1200&ixid=eyJhcHBfaWQiOjF9&ixlib=rb-1.2.1&q=80&w=2000
---

javascript 로 url 에 있는 정보 가져오는거랑, 기타 유용한 함수가 있는 라이브러리가([URI.js](http://medialize.github.io/URI.js/)) 있어서 가져다 써 보았다.

## 브라우저에서 땡겨다 쓰기

```html
<script src="https://cdnjs.cloudflare.com/ajax/libs/URI.js/1.19.1/URI.min.js"></script>
```

## npm

```bash
npm install urijs
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

## 사용법

URL : http://127.0.0.1:5500/test.html?param1=%ED%85%8C%EC%8A%A4%ED%8A%B8&param2=1&param2=2 라고 했을때

```javascript
var uri = URI(window.location.href);

console.log(uri.path()); // /test.html
console.log(uri.filename()); // test.html
console.log(uri.suffix()); // html
console.log(uri.search()); // ?param1=%ED%85%8C%EC%8A%A4%ED%8A%B8&param2=1&param2=2
console.log(uri.query()); // param1=%ED%85%8C%EC%8A%A4%ED%8A%B8&param2=1&param2=2
console.log(URI.parseQuery(uri.query())); // {param1=테스트, param2=[1,2]}
console.log(URI.parseQuery(uri.query()).param1); // 테스트
console.log(URI.parseQuery(uri.query()).param2); // [1,2]
```

자세한 기타등등의 사용법은 [API Documentation](http://medialize.github.io/URI.js/docs.html) 참고
