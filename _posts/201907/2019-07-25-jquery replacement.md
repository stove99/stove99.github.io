---
layout: post
title: 'jQuery 대체 라이브러리 : cash'
date: 2019-07-25
keywords: 'javascript'
categories: [Javascript]
image: https://images.unsplash.com/photo-1542282910-7337bcfea235?crop=entropy&cs=tinysrgb&fit=crop&fm=jpg&h=1200&ixid=eyJhcHBfaWQiOjF9&ixlib=rb-1.2.1&q=80&w=2000
image-sm: https://images.unsplash.com/photo-1542282910-7337bcfea235?crop=entropy&cs=tinysrgb&fit=crop&fm=jpg&h=1200&ixid=eyJhcHBfaWQiOjF9&ixlib=rb-1.2.1&q=80&w=2000
---

jQuery 를 대체할 수 있는 사이즈 작은 라이브러리. 비슷한 라이브러리로 Zepto 가 있는데 Zepto 보다 size 가 더 작다.

순수 자바스크립트로 엘리먼트를 select 해서 조작하기가 코드 길이도 길어지고 은근히 어려운데, jQuery 쓰기는 쫌 그렇고 할때 쓰면 좋을것 같다. \$.ajax 는 없어서 axios 같은걸 써야됨.

사이트 : <https://github.com/kenwheeler/cash>

## js 파일 사이즈 비교

| Size               | Cash        | Zepto 1.2.0 | jQuery 3.3.1 |
| ------------------ | ----------- | ----------- | ------------ |
| Uncompressed       | **32 KB**   | 58.7 KB     | 271 KB       |
| Minified           | **14.5 KB** | 26 KB       | 87 KB        |
| Minified & Gzipped | **5 KB**    | 9.8 KB      | 30.3 KB      |

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

그동안 사용하던 jQuery 처럼 그냥 사용하면 된다.

```html
<script src="https://cdnjs.cloudflare.com/ajax/libs/cash/4.1.3/cash.min.js"></script>
<script>
    $(function() {
        $('html').addClass('dom-loaded');
        $('<footer>Appended with Cash</footer>').appendTo(document.body);
    });
</script>
```

## 그 외에 왠지 유용할것 같은 함수

```js
$.camelCase('border-width'); // 'borderWidth'
```

```js
$.parseHTML(htmlString); // collection
```
