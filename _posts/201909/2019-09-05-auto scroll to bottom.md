---
layout: post
title: '[jQuery] html 채팅 div 메시지 수신시 자동으로 스크롤 아래로 움직이기'
keywords: 'html, javascript'
categories: javascript
image: https://images.unsplash.com/photo-1476722817135-8d33bf111a0c?crop=entropy&cs=tinysrgb&fit=crop&fm=jpg&h=1200&ixid=eyJhcHBfaWQiOjF9&ixlib=rb-1.2.1&q=80&w=2000
image-sm: https://images.unsplash.com/photo-1476722817135-8d33bf111a0c?crop=entropy&cs=tinysrgb&fit=crop&fm=jpg&h=1200&ixid=eyJhcHBfaWQiOjF9&ixlib=rb-1.2.1&q=80&w=2000
---

웹으로 채팅 기능 구현할때 메시지가 오면 메시지 출력하고 스크롤을 뽓 하단으로 움직이게 맹글어 보자.

## html

```html
<!-- 메시지가 추가되는 영역 -->
<div id="messages">
    <div>메시지1</div>
    <div>메시지2</div>
    .....
</div>
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

## css

```css
#messages {
    /* 채팅창 높이 */
    height: 300px;
    overflow-y: auto;
}
```

## script

```js

// messages 에 메시지 추가하는 부분
....
....

$('#messages')
        .stop()
        .animate({ scrollTop: $('#messages')[0].scrollHeight }, 1000);

```

## Socket.io 샘플

```html
<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.4.1/jquery.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/socket.io/2.2.0/socket.io.js"></script>

<script>
    var chat = io('http://localhost:3000/chat');

    ....
    ....

    // 메시지 수신시
    chat.on('message', function(data) {
        var message = $(`<div class="alert alert-success" role="alert">${data.name} : ${data.msg}</div>`);

        $('#messages')
            .append(message)
            .stop()
            .animate({ scrollTop: $('#messages')[0].scrollHeight }, 1000);
    });
</script>
```
