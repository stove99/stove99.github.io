---
layout: post
title: "CSS & Javascript 로 맹글어본 메뉴"
date: 2019-04-20
keywords: "javascript,css,jquery"
categories: [Javascript, jQuery]
image: https://picsum.photos/2000/1200?image=1010
image-sm: https://picsum.photos/500/300?image=1010
---

마우스로 클릭하거나 탭키로 포커스를 이동했을때 아래쪽으로 메뉴가 폿 열리는걸 맨들어 보았다.

## 맨들어본 결과물 미리보기

<script async src="//jsfiddle.net/stove/ersc65u0/embed/result/dark/"></script>

## 전체소스

소스에다 대충 주석을 달아보았다.

<img src="/assets/img/description.jpg"  style="max-width:512px;">

```html
<!DOCTYPE html>
<html lang="ko">
    <head>
        <meta charset="UTF-8" />
        <meta name="viewport" content="width=device-width, initial-scale=1.0" />
        <meta http-equiv="X-UA-Compatible" content="ie=edge" />
        <title>메뉴</title>

        <style>
            .menuBox {
                width: 350px;
                position: relative;
            }

            .menuBox ul {
                margin: 0;
                padding: 0;
                list-style: none;
            }

            .menuBox li {
                float: left;
                width: 150px;
                margin: 0 1px 1px 0;
            }

            .menuBox li > a {
                display: block;
                position: relative;
                color: #000;
                text-decoration: none;
                padding: 10px 0px 10px 20px;
                background: #ccc;
            }

            /* a 태그 옆에 화살표 이미지 추가 */
            .menuBox li > a:after {
                content: "";
                display: inline-block;
                position: absolute;
                width: 16px;
                height: 16px;
                top: 35%;
                right: 10px;
                background-image: url(https://github.com/stove99/stove99.github.io/blob/master/assets/img/arrow.png?raw=true);

                /* 이미지 돌아갈때 보드랍게 에니메이션 처리 */
                -webkit-transition: all 0.3s ease-out;
                -moz-transition: all 0.3s ease-out;
                -ms-transition: all 0.3s ease-out;
                -o-transition: all 0.3s ease-out;
                transition: all 0.3s ease-out;
            }

            .menuBox li > a:hover {
                color: #000;
                font-weight: bold;
            }

            /* 마우스 오버나 on 클래스가 있을때 이미지 90도로 돌리기 */
            .menuBox li > a:hover::after,
            .menuBox li > a.on::after {
                -webkit-transform: rotateZ(90deg);
                -moz-transform: rotateZ(90deg);
                -ms-transform: rotateZ(90deg);
                -o-transform: rotateZ(90deg);
                transform: rotateZ(90deg);
            }

            .menuBox li > a.on {
                color: #000;
                font-weight: bold;
                background-color: #fff;
            }

            .menuBox li div {
                position: absolute;
                left: 0;
                background: #fff;
                width: 301px;
                z-index: 1;
                display: none;
            }

            .menuBox li div p {
                margin: 20px;
            }
        </style>
    </head>

    <body>
        <div class="menuBox">
            <ul>
                <li>
                    <a href="#">메뉴1</a>
                    <div>
                        <p>
                            메뉴1 ipsum dolor, sit amet <a href="#">11111</a> adipisicing elit. Officia animi ad laborum
                            dolorem, totam, repellendus, minus laboriosam quas temporibus magnam quod aperiam illum
                            blanditiis ratione ipsa dignissimos explicabo quisquam iusto!
                        </p>
                    </div>
                </li>
                <li>
                    <a href="#">메뉴2</a>
                    <div>
                        <p>
                            메뉴2 ipsum dolor, sit amet consectetur adipisicing elit. Officia animi ad laborum dolorem,
                            totam, repellendus, minus laboriosam quas temporibus magnam quod aperiam illum blanditiis
                            ratione ipsa dignissimos explicabo quisquam iusto!
                        </p>
                    </div>
                </li>
                <li>
                    <a href="#">메뉴3</a>
                    <div>
                        <p>
                            메뉴3 ipsum dolor, sit amet consectetur adipisicing elit. Officia animi ad laborum dolorem,
                            totam, repellendus, minus laboriosam quas temporibus magnam quod aperiam illum blanditiis
                            ratione ipsa dignissimos explicabo quisquam iusto!
                        </p>
                    </div>
                </li>
                <li>
                    <a href="#">메뉴4</a>
                    <div>
                        <p>
                            메뉴4 ipsum dolor, sit amet consectetur adipisicing elit. Officia animi ad laborum dolorem,
                            totam, repellendus, minus laboriosam quas temporibus magnam quod aperiam illum blanditiis
                            ratione ipsa dignissimos explicabo quisquam iusto!
                        </p>
                    </div>
                </li>
                <li>
                    <a href="#">메뉴5</a>
                    <div>
                        <p>
                            메뉴5 ipsum dolor, sit amet consectetur adipisicing elit. Officia animi ad laborum dolorem,
                            totam, repellendus, minus laboriosam quas temporibus magnam quod aperiam illum blanditiis
                            ratione ipsa dignissimos explicabo quisquam iusto!
                        </p>
                    </div>
                </li>
                <li>
                    <a href="#">메뉴6</a>
                    <div>
                        <p>
                            메뉴6 ipsum dolor, sit amet consectetur adipisicing elit. Officia animi ad laborum dolorem,
                            totam, repellendus, minus laboriosam quas temporibus magnam quod aperiam illum blanditiis
                            ratione ipsa dignissimos explicabo quisquam iusto!
                        </p>
                    </div>
                </li>
            </ul>
        </div>

        <a href="#">메뉴 밖 링크</a>

        <script>
            // 메뉴버튼 6개 가져오기
            var btn = document.querySelectorAll(".menuBox li > a");

            for (var i = 0, size = btn.length; i < size; i++) {
                // 메뉴 버튼 클릭했을때 처리
                btn[i].addEventListener("click", function(e) {
                    e.stopImmediatePropagation();

                    // 클릭한 메뉴가 이미 펼쳐진 상태면 현재 메뉴 닫기
                    if (this.className.indexOf("on") > -1) {
                        this.classList.remove("on");

                        // 메뉴 레이어 안보이게 처리
                        this.nextElementSibling.style.display = "none";
                    }
                    // 다른 열려 있는 메뉴들 닫고 현재 클릭한 메뉴만 펼치기
                    else {
                        // 다른 메뉴 버튼에 설정된 on 클래스 삭제 후 클릭한 버튼에 on 클래스 추가
                        btn.removeClass("on");
                        this.classList.add("on");

                        // 다른 열려 있는 메뉴 레이어 안보이게 처리 후 클릭한 메뉴 보이게
                        for (var j = 0, size = btn.length; j < size; j++) {
                            btn[j].nextElementSibling.style.display = "none";
                        }
                        this.nextElementSibling.style.display = "block";
                    }
                });

                // 탭키로 포커스 왔을때 펼치기 (클릭이벤트랑 포커스 이벤트가 겹쳐서 쫌 꾸리하게 처리함)
                btn[i].addEventListener("focusin", function(e) {
                    e.stopImmediatePropagation();

                    var el = this;
                    setTimeout(function() {
                        el.className.indexOf("on") < 0 && el.dispatchEvent(new MouseEvent("click"));
                    }, 150);
                });
            }

            // 메뉴에서 포커스 벗어나면 메뉴 닫기
            document.body.addEventListener("focusin", function(e) {
                // 펼쳐진 메뉴에 포커스가 갔을때 메뉴가 닫히지 않게 처리
                if (!e.target.parents(".menuBox").length) {
                    btn.removeClass("on");
                    for (var i = 0, size = btn.length; i < size; i++) {
                        btn[i].nextElementSibling.style.display = "none";
                    }
                }
            });

            // NodeList 에 removeClass 함수 추가
            NodeList.prototype.removeClass = function(cls) {
                for (var i = 0, size = this.length; i < size; i++) {
                    this[i].classList.remove(cls);
                }
                return this;
            };

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
        </script>
    </body>
</html>
```

## jQuery 버전으로도 맨들어봄

```javascript
// 메뉴버튼 6개 가져오기
var btn = $(".menuBox li > a");

// 메뉴버튼 클릭했을때 처리
btn.on("click", function(e) {
    e.stopImmediatePropagation();

    var el = $(this);

    // 클릭한 메뉴가 이미 펼쳐진 상태면 현재 메뉴 닫기
    // $(this) = 클릭한 <a> 태그
    if (el.hasClass("on")) {
        el.removeClass("on");

        // <a> 태그의 바로옆 태그 = div
        el.next().hide();
    }
    // 다른 열려 있는 메뉴들 닫고 현재 클릭한 메뉴만 펼치기
    else {
        // 다른 메뉴 버튼에 설정된 on 클래스 삭제 후 클릭한 버튼에 on 클래스 추가
        btn.removeClass("on");
        el.addClass("on");

        // 다른 열려 있는 메뉴 레이어 안보이게 처리 후 클릭한 메뉴 보이게
        // 6개 <a> 태그들의 이웃에 있는 애들중에 <div> 태그들 = 메뉴 레이어 div
        btn.siblings("div").hide();
        el.next().show();
    }
});

// 탭키로 포커스 왔을때 펼치기 (클릭이벤트랑 포커스 이벤트가 겹쳐서 쫌 꾸리하게 처리함)
btn.on("focusin", function(e) {
    e.stopImmediatePropagation();

    var el = $(this);

    setTimeout(function() {
        !el.hasClass("on") && el.trigger("click");
    }, 150);
});

// 메뉴에서 포커스 벗어나면 메뉴 닫기
$(document).on("focusin", function(e) {
    // 펼쳐진 메뉴에 포커스가 갔을때 메뉴가 닫히지 않게 처리
    if (!$(e.target).parents(".menuBox").length) {
        btn.removeClass("on");
        btn.siblings("div").hide();
    }
});
```