---
layout: post
title: 'HTML 특정 영역 이미지로 다운로드 하기'
keywords: 'html, screenshot, html2canvas'
categories: javascript
tags: javascript
image: https://images.unsplash.com/photo-1533376351882-bdcabed9b281?ixid=MXwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHw%3D&ixlib=rb-1.2.1&auto=format&fit=crop&w=1650&q=80
image-sm: https://images.unsplash.com/photo-1533376351882-bdcabed9b281?ixid=MXwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHw%3D&ixlib=rb-1.2.1&auto=format&fit=crop&w=1650&q=80
---

html2canvas 라이브러리로 html 특정 영역을 canvas로 그린 다음 canvas 이미지를 png 파일로 다운로드 하는 예제

## 소스코드

```html
<!DOCTYPE html>
<html lang="ko">
    <head>
        <meta charset="UTF-8" />
        <meta http-equiv="X-UA-Compatible" content="IE=edge" />
        <meta name="viewport" content="width=device-width, initial-scale=1.0" />
        <title>화면 png 파일로 다운로드 하기</title>

        <script src="https://html2canvas.hertzen.com/dist/html2canvas.min.js"></script>

        <style>
            #capture {
                padding: 10px;
                background: #f5da55;
            }

            #download-btn {
                margin-top: 1rem;
            }
        </style>
    </head>
    <body>
        <!-- 이미지로 만들영역 -->
        <div id="capture">
            <h4>캡쳐 테스트</h4>
            <p>
                Lorem, ipsum dolor sit amet consectetur adipisicing elit. Blanditiis ratione, animi iste, nam laborum praesentium repellat necessitatibus iusto voluptatum hic vitae possimus, ad dolore
                deserunt ea laboriosam quibusdam. Suscipit facilis adipisci cum earum nam iure dolorem doloremque rem provident? Corrupti, nemo necessitatibus. Molestias vitae aliquam repellendus
                dicta minima doloribus excepturi praesentium odio, mollitia quod rem nisi, sequi ducimus ab pariatur quos! Pariatur quisquam fugiat repellat itaque praesentium recusandae quis cumque
                quasi quaerat, error culpa distinctio illum molestias aliquid nobis fugit et aperiam corporis soluta! Omnis aliquam quibusdam, expedita porro animi adipisci odit dignissimos ipsa
                veritatis. Deserunt tenetur debitis eveniet excepturi!
            </p>
        </div>

        <button id="download-btn">다운로드</button>

        <script>
            const getScreenshotOfElement = async (element) => {
                const canvas = await html2canvas(element);

                return canvas;
            };

            const download = (canvas, filename) => {
                const link = document.createElement('a');

                link.download = filename;
                link.href = canvas.toDataURL();
                link.click();
            };

            document.querySelector('#download-btn').addEventListener('click', async () => {
                const target = document.querySelector('#capture');
                const canvas = await getScreenshotOfElement(target);

                download(canvas, 'screenshot.png');
            });
        </script>
    </body>
</html>
```
