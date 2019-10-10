---
layout: post
title: '[jQuery] div 팝업창 처리하기'
keywords: 'javascript'
categories: javascript
tags: javascript jquery
---

jQuery를 사용해 div 팝업창을 특정일자 까지 오픈되도록 처리해 보자.

그리고 쿠키로 하루동안 이창을 열지 않음 기능 구현하기

## 소스 & 설명

```html
<!DOCTYPE html>
<html lang="ko">

<head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <title>div 팝업</title>

    <script src='//cdnjs.cloudflare.com/ajax/libs/jquery/3.4.1/jquery.min.js'></script>
    <script src="//cdn.jsdelivr.net/npm/js-cookie@2.2.0/src/js.cookie.min.js"></script>

    <style>
        .popup_layer {
            /* 초기 로딩시 팝업창은 닫힌 상태로.. 쿠키에 없는 경우만 다시 보이게 처리할거임 */
            display: none;
            border: 1px solid #222222;
            font-size: 11px;
        }

        .popup_close_btn {
            position: absolute;
            top: 5px;
            right: 5px;
            border: 0;
            background-color: transparent;
            font-size: 1.5rem;
        }

        .popup_layer p,
        .popup_layer div,
        .popup_layer img {
            margin: 0;
            padding: 0;
        }

        .popup_footer {
            position: absolute;
            bottom: 0;
            left: 0;
            width: 100%;
            background: #333;
            text-align: right;
        }

        .popup_footer button {
            border: 0;
            background: #333;
            color: #ccc;
            padding: 10px;
            letter-spacing: -1px;
            cursor: pointer;
        }

        .popup_footer span {
            color: #c00;
            display: inline-block;
            margin-right: 7px;
        }
    </style>
</head>

<body>
    <!-- 2019년 11월 1일 20시 33분 까지만 뜨는 팝업창, 위치 : (10, 10), 크기 : (450, 300) -->
    <div data-popup_id="popup_test1" data-open_to="2019-11-01 20:33"
         data-t="10px" data-l="10px" data-w="450px" data-h="300px"
         class="popup_layer" style="background:#c4d8f1">
        <button class="popup_close_btn">&times;</button>
        <img src="https://www.kipo.go.kr/wcom/ipt/image/ipt_gs_img.jpg" />
        <div style="padding:10px 0 10px 40px;">
            * 하이염!
        </div>
        <div class="popup_footer">
            <button class="close_with_cookie_btn" data-expired="1"><span>ⓧ</span>하루동안 이창을 열지 않음</button>
        </div>
    </div>

    <!-- 2020년 12월 31일 까지만 뜨는 팝업창, 위치 : (10, 470), 크기 : (450, 300) -->
    <div data-popup_id="popup_test2" data-open_to="2020-12-31"
         data-t="10px" data-l="470px" data-w="450px" data-h="300px"
         class="popup_layer">
        <button class="popup_close_btn">&times;</button>
        <img src="https://www.kipo.go.kr/wcom/ipt/image/20190418_popup.gif" />

        <div class="popup_footer">
            <button class="close_with_cookie_btn" data-expired="1"><span>ⓧ</span>하루동안 이창을 열지 않음</button>
        </div>
    </div>

    <script>
        $(document).ready(function () {
            // 팝업창을 찾아서 띄울지 말지 판단하고 팝업창을 보이게 함
            $('[data-popup_id]').each(function () {
                var el = $(this);
                var data = el.data();

                // 쿠키에 저장된 값이 없으면 팝업창을 띄움
                if (!Cookies.get(data.popup_id)) {
                    var today = new Date();
                    var d = (el.data('open_to') || '').split(/[\s,\-:]+/);
                    var open_date = new Date(+d[0], +d[1] - 1, +d[2], +d[3] || 23, +d[4] || 59, +d[5] || 59);
                    if (today < open_date) {
                        el.css({
                            position: 'absolute',
                            top: data.t,
                            left: data.l,
                            width: data.w,
                            height: data.h
                        }).show();
                    }
                }
            });

            // 오늘하루 열지 않음 처리
            $('.close_with_cookie_btn').on('click', function (e) {
                e.preventDefault();

                var el = $(this);
                var popup = el.parents('[data-popup_id]');
                var id = popup.data('popup_id');
                var expired = +(el.data('expired') || '1');

                // 쿠키가 언제까지 유지될지 설정한 다음에 팝업창을 닫는다.
                Cookies.set(id, 'popup_closed', { expires: expired });
                popup.hide();
            });

            // 팝업창 닫기 버튼 처리
            $('.popup_close').on('click', function (e) {
                e.preventDefault();

                $(this).parents('[data-popup_id]').hide();
            });
        });
    </script>
</body>

</html>
```
