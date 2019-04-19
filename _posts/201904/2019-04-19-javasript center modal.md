---
layout: post
title:  "Javascript 로 페이지 딱 중앙에다 모달창을 띄어보자"
date:   2019-04-19
keywords: "javascript"
categories: [Javascript]
image: https://picsum.photos/2000/1200?image=16
image-sm: https://picsum.photos/500/300?image=16
---

Javascript 와 CSS 를 이용해서 깨작깨작 한번 맹글어 보았다. 무조건 한번은 읽어봐라 하는 공지사항 띄울 때 살짝 디자인 바꿔서 써먹으면 될것같다.

## 전체소스

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Center Modal</title>

    <style>
        #my_modal {
            display: none;
            width: 300px;
            padding: 20px 60px;
            background-color: #fefefe;
            border: 1px solid #888;
            border-radius: 3px;
        }

        #my_modal .modal_close_btn {
            position: absolute;
            top: 10px;
            right: 10px;
        }
    </style>
</head>

<body>
    <div id="my_modal">
        Lorem ipsum, dolor sit amet consectetur adipisicing elit. Expedita dolore eveniet laborum repellat sit
        distinctio, ipsa rem dicta alias velit? Repellat doloribus mollitia dolorem voluptatum ex reiciendis aut in
        incidunt?
        <a class="modal_close_btn">닫기</a>
    </div>

    <script>
        function modal(id) {
            var zIndex = 9999;
            var modal = document.getElementById(id);

            // 모달 div 뒤에 희끄무레한 레이어
            var bg = document.createElement("div");
            bg.setStyle({
                position: 'fixed',
                zIndex: zIndex,
                left: '0px',
                top: '0px',
                width: '100%',
                height: '100%',
                overflow: 'auto',
                // 레이어 색갈은 여기서 바꾸면 됨
                backgroundColor: 'rgba(0,0,0,0.4)'
            });
            document.body.append(bg);

            // 닫기 버튼 처리, 시꺼먼 레이어와 모달 div 지우기
            modal.querySelector('.modal_close_btn').addEventListener('click', function () {
                bg.remove();
                modal.remove();
            });

            modal.setStyle({
                position: 'fixed',
                display: 'block',
                boxShadow: '0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19)',

                // 시꺼먼 레이어 보다 한칸 위에 보이기
                zIndex: zIndex + 1,

                // div center 정렬
                top: '50%',
                left: '50%',
                transform: 'translate(-50%, -50%)',
                msTransform: 'translate(-50%, -50%)',
                webkitTransform: 'translate(-50%, -50%)'
            });
        }

        // Element 에 style 한번에 오브젝트로 설정하는 함수 추가
        Element.prototype.setStyle = function (styles) {
            for (var k in styles)
                this.style[k] = styles[k];
            return this;
        };

        // 모달창 띄우기
        modal('my_modal');
    </script>
</body>
</html>
```

## 차근차근 맹글어 보기

- 팝업창 div 모양을 대충 잡고 display:none 으로 설정해서 팝업창을 처음부터 안보이는 상태로 맹글어 둔다.

``` html
<style>
    #my_modal {
        display: none;
        width: 300px;
        padding: 20px 60px;
        background-color: #fefefe;
        border: 1px solid #888;
        border-radius: 3px;
    }

    #my_modal .modal_close_btn {
        position: absolute;
        top: 10px;
        right: 10px;
    }
</style>

<div id="my_modal">
    Lorem ipsum, dolor sit amet consectetur adipisicing elit. Expedita dolore eveniet laborum repellat sit
    distinctio, ipsa rem dicta alias velit? Repellat doloribus mollitia dolorem voluptatum ex reiciendis aut in
    incidunt?
    <a class="modal_close_btn">닫기</a>
</div>
```

- 모달창 맨들어 주는 함수

```javascript
function modal(id) {
    var zIndex = 9999;
    var modal = document.getElementById(id);

    // 모달 div 뒤에 희끄무레한 레이어
    var bg = document.createElement("div");
    bg.setStyle({
        position: 'fixed',
        zIndex: zIndex,
        left: '0px',
        top: '0px',
        width: '100%',
        height: '100%',
        overflow: 'auto',
        // 레이어 색갈은 여기서 바꾸면 됨
        backgroundColor: 'rgba(0,0,0,0.4)'
    });
    document.body.append(bg);

    // 닫기 버튼 처리, 시꺼먼 레이어와 모달 div 지우기
    modal.querySelector('.modal_close_btn').addEventListener('click', function () {
        bg.remove();
        modal.remove();
    });

    modal.setStyle({
        position: 'fixed',
        display: 'block',
        boxShadow: '0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19)',

        // 시꺼먼 레이어 보다 한칸 위에 보이기
        zIndex: zIndex + 1,

        // div center 정렬
        top: '50%',
        left: '50%',
        transform: 'translate(-50%, -50%)',
        msTransform: 'translate(-50%, -50%)',
        webkitTransform: 'translate(-50%, -50%)'
    });
}

// Element 에 style 한번에 오브젝트로 설정하는 함수 추가
Element.prototype.setStyle = function (styles) {
    for (var k in styles)
        this.style[k] = styles[k];
    return this;
};

// 모달창 띄우기
modal('my_modal');
```

- jQuery 버전으로도 맨들어봄

```javascript
function modal(id) {
    var zIndex = 9999;
    var modal = $('#'+id);

    // 모달 div 뒤에 희끄무레한 레이어
    var bg = $("<div>").css({
        position: 'fixed',
        zIndex: zIndex,
        left: '0px',
        top: '0px',
        width: '100%',
        height: '100%',
        overflow: 'auto',
        // 레이어 색갈은 여기서 바꾸면 됨
        backgroundColor: 'rgba(0,0,0,0.4)'
    }).appendTo('body');

    modal
        .css({
            position: 'fixed',
            boxShadow: '0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19)',

            // 시꺼먼 레이어 보다 한칸 위에 보이기
            zIndex: zIndex + 1,

            // div center 정렬
            top: '50%',
            left: '50%',
            transform: 'translate(-50%, -50%)',
            msTransform: 'translate(-50%, -50%)',
            webkitTransform: 'translate(-50%, -50%)'
        })
        .show()
        // 닫기 버튼 처리, 시꺼먼 레이어와 모달 div 지우기
        .find('.modal_close_btn').on('click', function () {
            bg.remove();
            modal.remove();
        });
}

// 모달창 띄우기
modal('my_modal');
```

## 실행결과

<script async src="//jsfiddle.net/stove/oe6gxa0j/embed/result/dark/"></script>