---
layout: post
title:  "특정 날짜 까지만 팝업창 뜨게 하기"
date:   2019-04-17
keywords: "javascript"
categories: [Javascript]
image: https://picsum.photos/2000/1200?image=15
image-sm: https://picsum.photos/500/300?image=15
---

특정 시점이 지나서 절대로 노출되지 말아야 할 중요한 팝업이라면 **반드시** 서버쪽에서 처리해야 하겠지만, 그럴필요 없는 간단한 팝업이라면 요렇게 Javascript 로 간단하게 처리하면 될 것 같다.

## 전체소스

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Javascript 특정일 까지만 팝업창 뜨도록</title>

    <style>
        .popup {
            position: absolute;
            border: 1px solid #222222;
            width: 400px;
            height: 500px;
            top: 30px;
            left: 30px;
            display: none;
        }
    </style>
</head>
<body>
    <div class="popup" data-expired="2019-04-18 18:20">
        2019-04-18일 오후 6시 20분 까지 뜨는 팝업창이에염
    </div>

    <div class="popup" data-expired="2020-12-31" style="left:440px;">
        2020-12-31일 까지 뜨는 팝업창이에염
    </div>

    <script>
        var today = new Date();

        // html 에서 data-expired 가 설정된 팝업 div 들 찾기
        var popup = document.querySelectorAll('[data-expired]');
        for( var i=0, size=popup.length; i<size; i++ ){
            // 문자열을 공백, -, : 으로 나눠서 배열로 저장
            var d = popup[i].getAttribute('data-expired').split(/[\s,\-:]+/);
            var expired_date = new Date(d[0], d[1]-1, d[2], d[3]||24, d[4]||0);

            // 오늘이 설정한 expired date 전이면 팝업창 보여지게
            if( today < expired_date ){
                popup[i].style.display = 'block';
            }
            // 아니면 html 에서 삭제 시킴
            else{
                popup[i].parentElement.removeChild(popup[i]);
            }
        }
    </script>
</body>
</html>
```

## 차근차근 맹글어 보기

- 팝업창 div 를 display:none 으로 설정해서 팝업창을 처음부터 안보이는 상태로 맹글어 둔다.

``` html
<style>
    .popup {
        position: absolute;
        border: 1px solid #222222;
        width: 400px;
        height: 500px;
        top: 30px;
        left: 30px;
        display: none;
    }
</style>

<div class="popup" data-expired="2019-04-18 18:20">
    2019-04-18일 오후 6시 20분 까지 뜨는 팝업창이에염
</div>

<div class="popup" data-expired="2020-12-31" style="left:440px;">
    2020-12-31일 까지 뜨는 팝업창이에염
</div>
```

- html 에서 data-expired 가 설정된 팝업 div 들을 찾아서 오늘 날짜와 비교 후 보이게 하거나 html 에서 삭제 처리한다.

```javascript
var today = new Date();

// html 에서 data-expired 가 설정된 팝업 div 들을 찾기
var popup = document.querySelectorAll('[data-expired]');
for( var i=0, size=popup.length; i<size; i++ ){
    // 문자열을 공백, -, : 으로 나눠서 배열로 저장
    var d = popup[i].getAttribute('data-expired').split(/[\s,\-:]+/);
    var expired_date = new Date(d[0], d[1]-1, d[2], d[3]||24, d[4]||0);

    // 오늘이 설정한 expired date 전이면 팝업창 보여지게
    if( today < expired_date ){
        popup[i].style.display = 'block';
    }
    // 아니면 html 에서 삭제 시킴
    else{
        popup[i].parentElement.removeChild(popup[i]);
    }
}
```

- jQuery 도 맨들어봄

```javascript
var today = new Date();

// html 에서 data-expired 가 설정된 팝업 div 들을 찾기
$('[data-expired]').each(function(){
    // 문자열을 공백, -, : 으로 나눠서 배열로 저장
    var d = $(this).data('expired').split(/[\s,\-:]+/);

    var expired_date = new Date(d[0], d[1]-1, d[2], d[3]||24, d[4]||0);

    // 오늘이 설정한 expired date 전이면 팝업창 보여지게
    if( today < expired_date ){
        $(this).show();
    }
    // 아니면 html 에서 삭제 시킴
    else{
        $(this).remove();
    }
});
```

## 실행결과

<script async src="//jsfiddle.net/stove/1bwsav0m/embed/result/dark/"></script>