---
layout: post
title: '팝업창 오늘하루 그만보기 만들기'
date: 2019-04-18
keywords: 'javascript,cookie,jquery'
categories: [Javascript, jQuery]
image: https://picsum.photos/2000/1200?image=11
image-sm: https://picsum.photos/500/300?image=11
---

오늘 하루 그만보기 기능을 만들려면 쿠키를 써야 한다. 쌩으로 써도 되지만 분명히 누군가 쉽게 쓸 수 있도록 만들어 놓은것이 있을거라서 구글에서 javascript cookie 로 검색해 본다.

검색 결과 제일 상단에 나오는 js-cookie 라는 것을 가져다가 쓰면 될 것 같다.

## 라이브러리 설치

```javascript
<script src="//cdn.jsdelivr.net/npm/js-cookie@2.2.0/src/js.cookie.min.js" />
```

jQuery 로 하면 더 쉽고 짧게 만들수도 있겠지만 이상하게 요즘 jQuery 를 잘 안쓰는 추세가 있기 때문에 유행에 발맞춰 순수 자바스크립트로 작성해 보았다.

jQuery 로 맨든 버전은 죠아래 스크롤 내려보면 있음

일단 전체소스는 아래와 같다.

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8" />
        <meta name="viewport" content="width=device-width, initial-scale=1.0" />
        <meta http-equiv="X-UA-Compatible" content="ie=edge" />
        <title>Document</title>

        <script src="//cdn.jsdelivr.net/npm/js-cookie@2.2.0/src/js.cookie.min.js"></script>

        <style>
            .popup_layer {
                /* 초기 로딩시 팝업창은 닫힌 상태로.. 쿠키에 없는 경우만 다시 보이게 처리할거임 */
                display: none;
                border: 1px solid #222222;
            }

            .popup_close_btn {
                position: absolute;
                bottom: 0px;
            }
        </style>
    </head>

    <body>
        <div data-popup-id="popup_test1" class="popup_layer">
            팝업창1

            <div class="popup_close_btn">
                <button class="close_with_cookie" data-expired="1">하루동안 안볼래염</button>
                <button class="close_with_cookie" data-expired="365">계속 안볼래염</button>
            </div>
        </div>

        <div data-popup-id="popup_test2" class="popup_layer">
            팝업창2

            <div class="popup_close_btn">
                <button class="close_with_cookie" data-expired="1">하루동안 안볼래염</button>
                <button class="close_with_cookie" data-expired="365">계속 안볼래염</button>
            </div>
        </div>

        <script>
            // html 이 다 로딩된 후에 실행함
            document.addEventListener('DOMContentLoaded', function() {
                // popup_test1 레이어 팝업을 300 x 500 크기로 위에서 10 왼쪽에서 10 위치에 띄운다.
                layerPopup('popup_test1', '10px', '10px', '300px', '500px');

                // popup_test1 레이어 팝업을 600 x 300 크기로 위에서 10 왼쪽에서 320 위치에 띄운다.
                layerPopup('popup_test2', '10px', '320px', '600px', '300px');

                // 안볼래염 버튼 클릭했을때
                var btns = document.querySelectorAll('.close_with_cookie');
                for (var i = 0, length = btns.length; i < length; i++) {
                    btns[i].addEventListener('click', function() {
                        // html 에 설정된 팝업창 아이디와 언제까지 닫을지(expired) 값을 가져온다.
                        var popup = this.parents('[data-popup-id]')[0];
                        var id = popup.getAttribute('data-popup-id');
                        var expired = parseInt(this.getAttribute('data-expired') || '1');

                        // 쿠키가 언제까지 유지될지 설정한 다음에 팝업창을 닫는다.
                        Cookies.set(id, 'popup_closed', { expires: expired });
                        popup.style.display = 'none';
                    });
                }

                function layerPopup(id, top, left, w, h) {
                    // 팝업창 id로 저장된 쿠키가 없으면 팝업창 보여주기
                    if (!Cookies.get(id)) {
                        var popup = document.querySelector('[data-popup-id="' + id + '"]');

                        popup.style.position = 'absolute';
                        popup.style.top = top;
                        popup.style.left = left;
                        popup.style.width = w;
                        popup.style.height = h;
                        popup.style.display = 'block';
                    }
                }

                Element.prototype.parents = function(selector) {
                    var elements = [];
                    var elem = this;
                    var ishaveselector = selector !== undefined;

                    while ((elem = elem.parentElement) !== null) {
                        if (elem.nodeType !== Node.ELEMENT_NODE) {
                            continue;
                        }

                        if (!ishaveselector || elem.matches(selector)) {
                            elements.push(elem);
                        }
                    }

                    return elements;
                };
            });
        </script>
    </body>
</html>
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

## 차근차근 맹글어 보기

-   팝업창 div 를 display:none 으로 설정해서 팝업창을 처음부터 안보이는 상태로 맹글어 둔다.

```html
<style>
    .popup_layer {
        /* 초기 로딩시 팝업창은 닫힌 상태로.. 쿠키에 없는 경우(안볼래염 버튼 클릭전 팝업)만 다시 보이게 처리할거임 */
        display: none;
        border: 1px solid #222222;
    }

    .popup_close_btn {
        position: absolute;
        bottom: 0px;
    }
</style>

<!-- data-popup-id 에 겹치지 않게 원하는 팝업창 아이디 설정 -->
<div data-popup-id="popup_test1" class="popup_layer">
    팝업창1

    <div class="popup_close_btn">
        <button class="close_with_cookie" data-expired="1">하루동안 안볼래염</button>
        <button class="close_with_cookie" data-expired="365">계속 안볼래염</button>
    </div>
</div>

<div data-popup-id="popup_test2" class="popup_layer">
    팝업창2

    <div class="popup_close_btn">
        <button class="close_with_cookie" data-expired="1">하루동안 안볼래염</button>
        <button class="close_with_cookie" data-expired="365">계속 안볼래염</button>
    </div>
</div>
```

-   팝업창 아이디로 저장된 쿠키가 없으면 팝업창이 보이게 하는 팝업창 띄우는 함수를 만든다.

```javascript
function layerPopup(id, top, left, w, h) {
    // 팝업창 id로 저장된 쿠키가 없으면 팝업창 보여주기
    if (!Cookies.get(id)) {
        var popup = document.querySelector('[data-popup-id="' + id + '"]');

        popup.style.position = 'absolute';
        popup.style.top = top;
        popup.style.left = left;
        popup.style.width = w;
        popup.style.height = h;
        popup.style.display = 'block';
    }
}
```

-   하루동안 안볼래염 버튼을 클릭했을때 해당 팝업창 아이디로 쿠키에 대충 아무 값이나 저장한다.

```javascript
// 안볼래염 버튼 클릭했을때 처리하기
// 클래스가 close_with_cookie 인 엘리먼트들 가져옴
var btns = document.querySelectorAll('.close_with_cookie');

// 하나씩 반복하면서 클릭 이벤트가 발생했을때 할 일을 정의한다.
for (var i = 0, length = btns.length; i < length; i++) {
    btns[i].addEventListener('click', function() {
        // html 에 설정된 팝업창 아이디와 언제까지 닫아 둘지(expired) 설정한 값을 가져온다.
        var popup = this.parents('[data-popup-id]')[0];
        var id = popup.getAttribute('data-popup-id');

        // data-expired 설정한 값이 없다면 디폴트값으로 하루를 설정한다.
        var expired = parseInt(this.getAttribute('data-expired') || '1');

        // 쿠키가 언제까지 유지될지 설정한 다음에 팝업창을 닫는다.
        Cookies.set(id, 'popup_closed', { expires: expired });
        popup.style.display = 'none';
    });
}

// jQuery parents 함수구현
Element.prototype.parents = function(selector) {
    var elements = [];
    var elem = this;
    var ishaveselector = selector !== undefined;

    while ((elem = elem.parentElement) !== null) {
        if (elem.nodeType !== Node.ELEMENT_NODE) {
            continue;
        }

        if (!ishaveselector || elem.matches(selector)) {
            elements.push(elem);
        }
    }

    return elements;
};
```

-   팝업창 띄우기

```javascript
// popup_test1 레이어 팝업을 300 x 500 크기로 위에서 10 왼쪽에서 10 위치에 띄운다.
layerPopup('popup_test1', '10px', '10px', '300px', '500px');

// popup_test1 레이어 팝업을 600 x 300 크기로 위에서 10 왼쪽에서 320 위치에 띄운다.
layerPopup('popup_test2', '10px', '320px', '600px', '300px');
```

## jQuery 버전으로 맨들때는 요렇게 맨들 수 있다

```javascript
$(document).ready(function() {
    // popup_test1 레이어 팝업을 300 x 500 크기로 위에서 10 왼쪽에서 10 위치에 띄운다.
    layerPopup('popup_test1', '10px', '10px', '300px', '500px');

    // popup_test1 레이어 팝업을 600 x 300 크기로 위에서 10 왼쪽에서 320 위치에 띄운다.
    layerPopup('popup_test2', '10px', '320px', '600px', '300px');

    // 안볼래염 버튼 클릭했을때
    $('.close_with_cookie').on('click', function() {
        var popup = $(this).parents('[data-popup-id]');
        var id = popup.data('popup-id');
        var expired = parseInt($(this).data('expired') || '1');

        Cookies.set(id, 'popup_closed', { expires: expired });
        popup.hide();
    });

    function layerPopup(id, top, left, w, h) {
        // 팝업창 id로 저장된 쿠키가 없으면 팝업창 보여주기
        if (!Cookies.get(id)) {
            $('[data-popup-id="' + id + '"]')
                .css({
                    position: 'absolute',
                    top: top,
                    left: left,
                    width: w,
                    height: h
                })
                .show();
        }
    }
});
```

## 실행결과

<script async src="//jsfiddle.net/stove/s5ec1wqa/embed/result/dark/"></script>
