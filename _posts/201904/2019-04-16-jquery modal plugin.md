---
layout: post
title: 'jQuery Modal Plugin 팝업창 띄우기 예제'
keywords: 'javascript,jquery,modal'
categories: javascript
tags: javascript jquery
image: https://picsum.photos/2000/1200?image=1011
image-sm: https://picsum.photos/500/300?image=1011
---

jQuery Modal Plugin 팝업창 띄우기 예제

## 소개

-   [jquery-modal](https://github.com/kylefox/jquery-modal) 플러그인 사용
-   ajax 로 다른 파일을 모달로 여러개 띄우기 가능
-   팝업창 닫을 때 부모창으로 값 전달 가능 ([jquery-modal](https://github.com/kylefox/jquery-modal) 플러그인 소스 살짝 수정했음)
-   창뜰때 애니메이션을 [Animate.css](https://github.com/daneden/animate.css) 여기에 있는것중 마음에 드는걸로 바꿀수 있음(animation 옵션)

### modal.js

```javascript
(function($) {
    $.popup = function(opt) {
        $.ajax({
            url: opt.url,
            data: opt.data,
            dataType: 'html',
            success: function(html) {
                $("<div class='modal_popup'>" + html + '</div>')
                    .appendTo('body')
                    .modal({
                        escapeClose: false,
                        clickClose: false,
                        closeExisting: false
                    })
                    // 창뜰때 애니메이션 : jackInTheBox
                    .addClass('animated faster')
                    .addClass(opt.animation || 'jackInTheBox')
                    .on($.modal.CLOSE, function(event, modal) {
                        if (opt.close) opt.close(modal.result);

                        modal.elm.remove();
                    });
            },
            error: function(res) {
                if (opt.error) opt.error(res.responseText);
                else {
                    alert('error');
                }
                throw 'popup_error';
            }
        });
    };
})(jQuery);
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

사용법은 대충 요렇게.

```javascript
$.popup({
    url: '모달창 띄울 url',
    // 모달창 호출할때 넘길 파라메터들
    data: {
        param_name1: value_1,
        param_name2: value_2
    },
    //animation : "bounce",
    close: function(result) {
        // 모달창에서 넘어온 값
        console.log(result);
    }
});
```

## 사용하기

사용하기 위해서 jquery, jquery.modal.js, jquery.modal.css, animate.min.css 이 필요하다.

```html
<link rel="stylesheet" href="//cdn.jsdelivr.net/gh/stove99/jquery-modal-sample@v1.4/css/animate.min.css" />
<link rel="stylesheet" href="//cdn.jsdelivr.net/gh/stove99/jquery-modal-sample@v1.4/css/jquery.modal.css" />

<script src="//cdn.jsdelivr.net/gh/stove99/jquery-modal-sample@v1.4/js/jquery.modal.js"></script>

<script src="//cdn.jsdelivr.net/gh/stove99/jquery-modal-sample@v1.4/js/modal.js"></script>
```

## 샘플

### sample.html

```html
<button id="btn" type="button" class="btn btn-primary btn-lg">팝업</button>

<script>
    $('#btn').click(function(event) {
        event.preventDefault();
        this.blur();

        $.popup({
            url: './modal1.html',
            close: function(result) {
                console.log(result);
            }
        });
    });
</script>
```

### modal1.html

```html
<div id="test_page">
    <style>
        #test_page p {
            width: 900px;
        }
    </style>

    <p>
        Lorem, ipsum dolor sit amet consectetur adipisicing elit. Sint nisi voluptatem iusto beatae sequi ex quam tempora laboriosam facere, facilis nulla nostrum impedit ducimus, porro quasi quos, itaque optio corporis.
    </p>

    <div class="text-center">
        <button id="close_btn" type="button" class="btn btn-danger btn-lg">닫기</button>
    </div>

    <script>
        var base = $('#test_page').parents('.modal_popup');

        $('#close_btn', base).on('click', function() {
            // 창 닫을때 부모창으로 값 넘기기
            $.modal.getCurrent().close({ x: '333333' });
        });
    </script>
</div>
```

### 결과물

<script async src="//jsfiddle.net/stove/okcbywm0/embed/result/dark/"></script>

## 사이트

-   [github](https://github.com/stove99/jquery-modal-sample)
-   git clone https://github.com/stove99/jquery-modal-sample.git

<ins class="adsbygoogle"
     style="display:block; text-align:center;"
     data-ad-layout="in-article"
     data-ad-format="fluid"
     data-ad-client="ca-pub-7073298118440059"
     data-ad-slot="8400970402"></ins>

<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>
