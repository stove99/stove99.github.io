---
layout: post
title: '브라우저 현재 페이지 강제 리로딩 하기(Hard Reload)'
date: 2019-04-29
keywords: 'etc'
categories: [ETC]
image: https://picsum.photos/2000/1200?image=1040
image-sm: https://picsum.photos/500/300?image=1040
---

개발을 하다 보면 리프레쉬를 해도 캐시에 남아있는 css 나 js 파일 image 등등이 로딩되어 변경된 내용이 바로 반영되지 않는 경우가 있다.
요럴때는 각 브라우져 설정 메뉴로 들어가서 브라우저 전체 캐시를 지운 다음 다시 리프레쉬 했었다.
요렇게 귀찮게 설정에 들어가서 캐시를 지우지 않아도, 지금 현재 접속한 페이지만 캐시를 무시하고 강제 리로딩(Hard Reload) 하는 기능이 있다.

## Windows 일 경우 단축키

IE, Chrome, Firefox 에서 작동함

<kbd>Ctrl</kbd>+<kbd>F5</kbd> 혹은 컨트롤키 누른 상태에서 리프레쉬 버튼 클릭

<ins class="adsbygoogle"
     style="display:block; text-align:center;"
     data-ad-layout="in-article"
     data-ad-format="fluid"
     data-ad-client="ca-pub-7073298118440059"
     data-ad-slot="8400970402"></ins>

<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

## Mac 일 경우 단축키

<kbd>⇧</kbd>+<kbd>⌘</kbd>+<kbd>R</kbd>

## 기타

추가적으로 크롬에서는 개발자 도구(<kbd>F12</kbd>)를 띄어 놓은 상태에서 리프레쉬 아이콘을 마우스 우클릭 하면 요런 메뉴가 추가로 나타난다.

<img src="/assets/attach/201904/hard_reload.png" style="width:900px;">
