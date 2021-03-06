---
layout: post
title: 'Visual Studio Code 에서 async/await 구문 에러표시 될때'
keywords: 'ETC'
categories: etc
image: https://images.unsplash.com/photo-1502604459884-2842c8f3624a?crop=entropy&cs=tinysrgb&fit=crop&fm=jpg&h=1200&ixid=eyJhcHBfaWQiOjF9&ixlib=rb-1.2.1&q=80&w=2000
image-sm: https://images.unsplash.com/photo-1502604459884-2842c8f3624a?crop=entropy&cs=tinysrgb&fit=crop&fm=jpg&h=1200&ixid=eyJhcHBfaWQiOjF9&ixlib=rb-1.2.1&q=80&w=2000
---

Visual Studio Code 에서 async/await 구문을 썼을때

"Expected ')' to match '{' from line 35 and instead saw '.'. (E020)" 요런 에러표시가 날때가 있는데 원인은 jshint, eslint 확장이 동시에 처리되어 그렇다.

<img src="/assets/attach/201907/jshint.png" style="width:900px;">

<ins class="adsbygoogle"
     style="display:block; text-align:center;"
     data-ad-layout="in-article"
     data-ad-format="fluid"
     data-ad-client="ca-pub-7073298118440059"
     data-ad-slot="8400970402"></ins>

<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

에러나는 것을 제거할려면 jshint 를 삭제하거나 아니면 사용자/작업영역 설정에서 jshint enable을 체크해제 하면 된다.

해당 프로젝트에서만 적용할려면 작업영역 설정에서 살짝 jshint 사용을 꺼주면 된다.

<img src="/assets/attach/201907/vs_setting.png" style="width:900px;">
