---
layout: post
title: 'anime.js 로 롤링배너 맹글기'
date: 2019-05-11
keywords: 'javascript'
categories: [Javascript]
image: https://images.unsplash.com/photo-1527062489662-19fc7250d875?ixlib=rb-1.2.1&q=80&fm=jpg&crop=entropy&cs=tinysrgb&w=2000&h=1200&fit=crop&ixid=eyJhcHBfaWQiOjF9
image-sm: https://images.unsplash.com/photo-1527062489662-19fc7250d875?ixlib=rb-1.2.1&q=80&fm=jpg&crop=entropy&cs=tinysrgb&w=500&h=300&fit=crop&ixid=eyJhcHBfaWQiOjF9
---

모든 슬라이더의 크기가 같다고 가정하고 anime.js 를 사용해서 맹글어봄

## 미리보기

<script async src="//jsfiddle.net/stove/wLxjby80/embed/result/dark/"></script>

## html

```html
<!DOCTYPE html>
<html lang="ko">
    <head>
        <meta charset="UTF-8" />
        <meta name="viewport" content="width=device-width, initial-scale=1.0" />
        <meta http-equiv="X-UA-Compatible" content="ie=edge" />
        <link rel="stylesheet" type="text/css" href="style.css" />
        <title>slider example</title>

        <script src="//code.jquery.com/jquery-3.4.0.min.js"></script>

        <!--IE10지원-->
        <script src="//cdnjs.cloudflare.com/ajax/libs/babel-polyfill/7.4.4/polyfill.min.js"></script>

        <script src="//cdnjs.cloudflare.com/ajax/libs/animejs/2.2.0/anime.js"></script>
    </head>

    <body>
        <!-- show : 한번에 몇개 보여줄지, duration : 움직이는 속도, delay : 움직인 후 대기시간-->
        <div class="sliderBox" data-show="3" data-duration="800" data-delay="4000">
            <div>
                <a href="#" title="뒤로 이동" class="nav_btn prev"><span>뒤로 이동</span></a>
            </div>
            <ul>
                <li>
                    <a href="#" target="_blank" title="바로가기 (새창)">배너의 내용이<br />들어가는 부분0</a>
                </li>
                <li><a href="#" target="_blank" title="바로가기 (새창)">배너 제목 한줄1</a></li>
                <li><a href="#" target="_blank" title="바로가기 (새창)">배너 제목 한줄2</a></li>
                <li><a href="#" target="_blank" title="바로가기 (새창)">배너 제목 한줄3</a></li>
                <li>
                    <a href="#" target="_blank" title="바로가기 (새창)">배너의 내용이<br />들어가는 부분4</a>
                </li>
                <li>
                    <a href="#" target="_blank" title="바로가기 (새창)">배너의 내용이<br />들어가는 부분5</a>
                </li>
                <li>
                    <a href="#" target="_blank" title="바로가기 (새창)">배너의 내용이<br />들어가는 부분6</a>
                </li>
                <li>
                    <a href="#" target="_blank" title="바로가기 (새창)">배너의 내용이<br />들어가는 부분7</a>
                </li>
                <li>
                    <a href="#" target="_blank" title="바로가기 (새창)">배너의 내용이<br />들어가는 부분8</a>
                </li>
                <li>
                    <a href="#" target="_blank" title="바로가기 (새창)">배너의 내용이<br />들어가는 부분9</a>
                </li>
            </ul>
            <div>
                <a href="#" title="앞으로 이동" class="nav_btn next"><span>앞으로 이동</span></a>
            </div>
        </div>
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

## style.scss

```scss
@import url('https://fonts.googleapis.com/css?family=Nanum+Gothic+Coding');

body {
    background: #20262e;
}

body,
ul,
li {
    font-family: 'Nanum Gothic Coding', monospace;
    margin: 0;
    padding: 0;
    list-style: none;
    font-size: 14px;
    color: #fff;
    line-height: 1.2;
}

.sliderBox {
    display: flex;
    align-items: center;
    justify-content: center;

    a {
        color: #fff;
        text-decoration: none;
    }

    span {
        display: block;
        overflow: hidden;
        position: relative;
        width: 100%;
        height: 100%;
        z-index: -1;
    }

    div {
        span {
            width: 9px;
            height: 18px;
        }

        a {
            display: block;
        }

        &:first-child a {
            background: #20262e url(https://user-images.githubusercontent.com/50475884/57508262-94762900-72f0-11e9-826c-b0b4997088e1.png) -100px -125px no-repeat;
            margin-right: 10px;
        }

        &:last-child a {
            background: #20262e url(https://user-images.githubusercontent.com/50475884/57508262-94762900-72f0-11e9-826c-b0b4997088e1.png) -125px -125px no-repeat;
            margin-left: 10px;
        }
    }

    ul {
        display: flex;

        li {
            flex: 0 0 150px;
            padding: 10px;
            margin: 5px;
            display: flex;
            text-align: center;
            background: #3c424a;
            border: solid 1px #4d5560;

            a {
                margin: auto;
            }
        }
    }
}
```

## javascript

```javascript
$('.sliderBox').each(function() {
    var idx = 1;
    var is_animating = false;
    var slider = $(this);
    var slide_cnt = slider.find('ul>li').length;
    var show_cnt = slider.data('show') || 1;
    var duration = slider.data('duration') || 800;
    var delay = slider.data('delay') || 4000;

    var slide_width = slider
        .find('ul>li')
        .eq(0)
        .outerWidth(true);

    // 보여지는 부분 넓이 설정
    slider
        .children('ul')
        .wrap('<div/>')
        .parent()
        .css({
            overflow: 'hidden',
            width: show_cnt * slide_width
        });

    $('.nav_btn').on('click', function(e) {
        e.preventDefault();

        if (is_animating) return;

        $(this).hasClass('next') ? idx++ : idx--;

        // 버튼 클릭했을때는 쫌 더 빨리 넘어가게
        go(idx, 300);
    });

    function go(to, dur) {
        if (to < 0) {
            idx = 0;
            return;
        }
        if (to + show_cnt > slide_cnt) {
            idx = 0;
            to = 0;
        }

        anime({
            targets: slider.find('ul')[0],
            translateX: -1 * slide_width * to,
            duration: dur || duration,
            easing: 'easeInOutQuad',
            begin: function(anim) {
                is_animating = true;
            },
            complete: function(anim) {
                is_animating = false;
            }
        });
    }

    setInterval(function() {
        if (is_animating) return;
        go(idx++);
    }, delay);
});
```
