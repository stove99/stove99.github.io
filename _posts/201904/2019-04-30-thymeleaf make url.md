---
layout: post
title: 'Thymeleaf 로 URL 맹글기 정리'
date: 2019-04-30
keywords: 'java,thymeleaf'
categories: [Java, Thymeleaf]
image: https://picsum.photos/2000/1200?image=1042
image-sm: https://picsum.photos/500/300?image=1042
---

## sample 용 model 에 있는 board object

```json
{
    "no": 123,
    "title": "테스트"
}
```

## PathVariable 맹글기

```html
<!-- 코드 -->
<a th:href="@{|/board/${board.no}|}">링크1</a>
<a th:href="@{|/board/${board.no}/${board.title}|}">링크2</a>

<!-- 생성된 html -->
<a href="/board/123">링크1</a>
<a href="/board/123/테스트">링크2</a>
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

## Parameter(@RequestParam) 맹글기

```html
<!-- 코드 -->
<a th:href="@{/board(no=${board.no})}">링크1</a>
<a th:href="@{/board(no=${board.no}, title=${board.title})}">링크2</a>

<!-- 생성된 html -->
<a href="/board?no=123">링크1</a>
<a href="/board?no=123&title=테스트">링크2</a>
```

## 짬뽕

```html
<!-- 코드 -->
<a th:href="@{/board/{no}/details(no=${board.no},title=${board.title})}">링크1</a>

<!-- 생성된 html -->
<a href="/board/123/details?title=테스트">링크1</a>
```

## 꾸질꾸질 하지만 혹시 모르니 써둠

```html
<!-- 코드 -->
<a th:href="${baseUrl + '/' + board.no}" th:with="baseUrl=@{/board}">링크1</a>

<!-- 생성된 html -->
<a href="/board/123">링크1</a>
```
