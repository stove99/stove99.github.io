---
layout: post
title: '롤링 배너 샘플'
keywords: 'javascript'
categories: javascript
image: https://picsum.photos/2000/1200?image=1026
image-sm: https://picsum.photos/500/300?image=1026
---

뽓뽓뽓 배너가 돌아가는것을 맨들어 봅시다.

## 결과물

<script async src="//jsfiddle.net/stove/4ztyfgpe/embed/result/dark/"></script>

<ins class="adsbygoogle"
     style="display:block; text-align:center;"
     data-ad-layout="in-article"
     data-ad-format="fluid"
     data-ad-client="ca-pub-7073298118440059"
     data-ad-slot="8400970402"></ins>

<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

## 소스

```html
<!DOCTYPE html>
<html lang="ko">
    <head>
        <meta charset="UTF-8" />
        <meta name="viewport" content="width=device-width, initial-scale=1.0" />
        <meta http-equiv="X-UA-Compatible" content="ie=edge" />
        <title>돌아가는 배너</title>

        <script src="//code.jquery.com/jquery-3.4.0.min.js"></script>

        <style>
            body {
                background: #20262e;
            }
            .banner {
                background-color: #fff;
                position: relative;
                overflow: hidden;
                display: -webkit-box;
                display: -ms-flexbox;
                display: flex;
                -webkit-box-align: center;
                -ms-flex-align: center;
                align-items: center;
                /* 페이지 로딩할때 안보이게 */
                visibility: hidden;
            }

            .banner > button {
                float: left;
                border: none;
                width: 21px;
                height: 21px;
                color: transparent;
            }

            .banner > button.prev {
                background-image: url('https://www.kipo.go.kr/wcom/new/images/main/btn_banner_prev.gif');
                margin: 0px 10px;
            }

            .banner > button.next {
                background-image: url('https://www.kipo.go.kr/wcom/new/images/main/btn_banner_next.gif');
                margin: 0px 10px;
            }

            .banner > .scroll_area {
                position: relative;
                overflow: hidden;
                float: left;
            }

            .banner > div > ul {
                position: relative;
                list-style: none;
                margin: 0px;
                padding: 0px;
            }

            .banner > div > ul li {
                float: left;
                padding: 20px 5px;
            }
        </style>
    </head>

    <body>
        <div class="banner" data-show="5">
            <button class="banner_nav_btn prev" data-direction="prev">뒤</button>
            <div class="scroll_area">
                <ul>
                    <li>
                        <a href="#"><img src="https://www.kipo.go.kr/upload/main/banner/20170818141201753035_1.jpg"/></a>
                    </li>
                    <li>
                        <a href="#"><img src="https://www.kipo.go.kr/upload/main/banner/20170906142030163420_1.jpg"/></a>
                    </li>
                    <li>
                        <a href="#"><img src="https://www.kipo.go.kr/upload/main/banner/20170911172721038024_1.jpg"/></a>
                    </li>
                    <li>
                        <a href="#"><img src="https://www.kipo.go.kr/upload/main/banner/20180725163246119162_1.jpg"/></a>
                    </li>
                    <li>
                        <a href="#"><img src="https://www.kipo.go.kr/upload/main/banner/20160628141812963678_1.jpg"/></a>
                    </li>
                    <li>
                        <a href="#"><img src="https://www.kipo.go.kr/upload/main/banner/20160420162654302295_1.gif"/></a>
                    </li>
                    <li>
                        <a href="#"><img src="https://www.kipo.go.kr/upload/main/banner/20160923132607937030_1.gif"/></a>
                    </li>
                    <li>
                        <a href="#"><img src="https://www.kipo.go.kr/upload/main/banner/20180208171141526163_1.gif"/></a>
                    </li>
                    <li>
                        <a href="#"><img src="https://www.kipo.go.kr/upload/main/banner/20190325094141522105_1.gif"/></a>
                    </li>
                    <li>
                        <a href="#"><img src="https://www.kipo.go.kr/upload/main/banner/20170103152552844157_1.gif"/></a>
                    </li>
                    <li>
                        <a href="#"><img src="https://www.kipo.go.kr/upload/main/banner/20190222112136379016_1.gif"/></a>
                    </li>
                    <li>
                        <a href="#"><img src="https://www.kipo.go.kr/upload/main/banner/20190412092828089256_1.gif"/></a>
                    </li>
                </ul>
            </div>
            <button class="banner_nav_btn next" data-direction="next">앞</button>
        </div>

        <script>
            var banners = $('.banner');

            banners.each(function(idx, el) {
                var banner = $(el);

                var btn = banner.find('.banner_nav_btn');
                var scroll_area = banner.find('.scroll_area > ul');
                var banner_items = scroll_area.find('li');
                var show = parseInt(banner.data('show'));

                // 딱 보이는 영역 넓이
                var banner_width = 0;
                // 안보이는 영역까지 합한 넓이
                var scroll_area_width = 0;
                // 버튼 영역 넓이
                var btn_width = 0;

                // 넓이 계산하기 & 배너 갯수
                var banner_count = banner_items.each(function(idx, li) {
                    scroll_area_width += $(li).outerWidth();
                    if (idx < show) {
                        banner_width = scroll_area_width;
                    }
                }).length;

                // 버튼영역 넓이 계산
                btn.each(function(idx, btn) {
                    btn_width += $(btn).outerWidth(true);
                });

                // 배너 스크롤
                var cur_pos = 0;

                // 보여줄 배너랑 실제 배너 갯수가 다르면 스크롤 시작
                if (banner_count != show) {
                    setInterval(function() {
                        _move();
                    }, 3000);
                }

                function _move(duration) {
                    var w = banner_items.eq(cur_pos).outerWidth();

                    cur_pos++;

                    scroll_area.stop().animate(
                        {
                            left: -w * cur_pos + 'px'
                        },
                        {
                            duration: duration || 500,
                            complete: function() {
                                // 끝까지 갔을때 맨 앞으로 땡김
                                if (cur_pos > banner_count - show - 1) {
                                    setTimeout(function() {
                                        cur_pos = 0;
                                        scroll_area.animate({ left: '0px' }, 100);
                                    }, 1000);
                                }
                            }
                        }
                    );
                }

                // 포커스 이동 처리
                // 포커스가 왔을때 제일 앞쪽으로 땡김
                banner_items.find('a').on('focus', function(e) {
                    var el = $(e.currentTarget);
                    var width = 0;

                    cur_pos = banner_items.index(el.parents('li'));
                    banner_items.filter(':lt(' + cur_pos + ')').each(function(idx, item) {
                        width += $(item).outerWidth();
                    });
                    scroll_area.stop().css('left', -1 * width + 'px');
                });

                // 네이게이션 버튼 이벤트 처리
                btn.on('click', function(e) {
                    e.preventDefault();

                    var el = $(e.currentTarget);
                    var direction = el.data('direction');

                    // 뒤로 가기 버튼 클릭했을때
                    if (direction == 'prev') {
                        if (cur_pos == 0) return;
                        cur_pos = cur_pos - 2;
                        _move(100);
                    }
                    // 앞으로 가기 버튼 클릭했을때
                    else if (direction == 'next') {
                        if (cur_pos > banner_count - show - 1) return;
                        cur_pos == 0 && cur_pos++;
                        _move(100);
                    }
                });

                scroll_area_width += 4;

                // 넓이 설정
                scroll_area.parent().css('width', banner_width);
                scroll_area.css('width', scroll_area_width);
                banner.css({
                    width: banner_width + btn_width,
                    visibility: 'visible'
                });
            });
        </script>
    </body>
</html>
```
