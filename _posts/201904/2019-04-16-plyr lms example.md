---
layout: post
title: 'plyr 을 이용한 LMS example'
date: 2019-04-16
keywords: 'javascript,lms,plyr'
categories: [Javascript]
tags: [javascript]
icon: icon-javascript
image: https://picsum.photos/2000/1200?image=1062
image-sm: https://picsum.photos/500/300?image=1062
---

## LMS 란

LMS 가 뭔가 했더만 러닝 매니지먼트 시스템? 온라인강의시스템 뭐 요딴거라고 한다.

## 필요한 기능

-   현재 플레이 위치 확인
-   동영상을 앞으로 마음대로 못 넘기게
-   동영상 플레이 완료시 이벤트
-   특정 위치에서 부터 동영상 플레이 되도록 하기(동영상 플레이 도중 브라우저 종료 했다가 다시 그 위치에서 부터 플레이 되도록...)

대충 요정도 요구사항이 있을것 같다.
비디오 플레이어로 쓸 javascript 라이브러는 이것저것 찾아보다 [plyr](https://github.com/sampotts/plyr)로 선택했다.

가보면 여러가지 옵션들이 많으니까 이것저것 해보면 될듯하다.

간단한 샘플을 위해 유투브에 있는 동영상을 땡겨다 보는 방식으로 만들어 보았다.

동영상 플레이시 클릭해서 아무대나 이동시키는것을 막기위해 css 로 처리할수도 있고 javascript 로 처리할수도 있다.

## 만들게 될 것 미리보기

요런거 만들어볼 거임

<script async src="//jsfiddle.net/stove/kstq2xou/embed/result/dark/"></script>

<ins class="adsbygoogle"
     style="display:block; text-align:center;"
     data-ad-layout="in-article"
     data-ad-format="fluid"
     data-ad-client="ca-pub-7073298118440059"
     data-ad-slot="8400970402"></ins>

<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

## js, css 추가하기

```html
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/plyr/3.5.3/plyr.css" />

<script src="https://cdnjs.cloudflare.com/ajax/libs/plyr/3.5.3/plyr.polyfilled.min.js"></script>

<style>
    #container {
        padding-top: 50px;
        width: 700px;
        height: 600px;
        margin: 0 auto;
    }

    /*프로그레스바 클릭안되게 처리*/
    .plyr__progress {
        pointer-events: none;
    }
</style>
```

## html

```html
<div id="container">
    <!-- data-plyr-embed-id 에 재생할 유투브 동영상 아이디 설정 -->
    <div id="player" data-plyr-provider="youtube" data-plyr-embed-id="CNeNwplE_aw"></div>
    <p>현재 플레이 위치 : <span id="current"></span></p>
    <p><button id="go_btn" type="button" class="btn btn-primary">3분 55초 부터 시작하기</button></p>
    <p>
        참고사이트 :
        <a href="https://github.com/sampotts/plyr" target="_blank">https://github.com/sampotts/plyr</a>
    </p>
</div>
```

## javascript

```javascript
// 플레이어 만들기
var player = new Plyr('#player', {
    controls: ['progress', 'current-time', 'mute', 'volume', 'fullscreen'],
    invertTime: false,
    keyboard: {
        focused: false,
        global: false
    },
    listeners: {
        // 리스너로 seek 안되게 처리할려면 요렇게(javascript 적인 방법으로 플레이 위치 못바꾸게 할려면 요렇게)
        seek: function customSeekBehavior(e) {
            //e.preventDefault();
            //alert('변경 불가능');
            //return false;
        }
    }
});

// 비디오 플레이중 처리할 것들.. 현재 재생위치 저장?
player.on('timeupdate', function(e) {
    document.querySelector('#current').textContent = e.detail.plyr.currentTime + ' / ' + player.duration;
});

// 비디오 재생 종료시 처리할 것들
player.on('ended', function(e) {
    console.log(e.detail.plyr);
    alert('비디오 재생이 끝났음');
});

// 특정 위치에서 부터 동영상 플레이 되도록 하기
document.querySelector('#go_btn').addEventListener('click', function(e) {
    player.pause();
    // 3분 55초 부터 시작하기
    player.currentTime = 60 * 3 + 55;
    player.play();
});
```
