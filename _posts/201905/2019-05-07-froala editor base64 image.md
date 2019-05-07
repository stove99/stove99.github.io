---
layout: post
title: "Froala Editor 이미지 업로드시 서버로 업로드 하지 않고 BASE64 인코딩으로 처리하기"
date: 2019-05-07
keywords: "javascript, Froala Editor"
categories: [Javascript]
image: https://images.unsplash.com/photo-1522426131985-b9b7019ec14d?ixlib=rb-1.2.1&q=80&fm=jpg&crop=entropy&cs=tinysrgb&w=2000&h=1200&fit=crop&ixid=eyJhcHBfaWQiOjF9
image-sm: https://images.unsplash.com/photo-1522426131985-b9b7019ec14d?ixlib=rb-1.2.1&q=80&fm=jpg&crop=entropy&cs=tinysrgb&w=500&h=300&fit=crop&ixid=eyJhcHBfaWQiOjF9
---

이미지 업로드 시 서버로 업로드 하지 않고 BASE64 문자열로 인코딩해서 처리하는 방법임.

요렇게 하면 서버쪽 파일 업로드 처리를 따로 하지 않아도 되는 장점이 있는데 에디터안 html 사이즈가 커져서 그런지 Code View 화면으로 전환할 때 딜레이가 꽤 발생한다.

## html

```html
<div id="froala-editor"></div>

<script>
    $(function() {
        $("div#froala-editor")
            .froalaEditor()
            .on("froalaEditor.image.beforeUpload", function(e, editor, files) {
                if (files.length) {
                    var reader = new FileReader();

                    reader.onload = function(e) {
                        var result = e.target.result;
                        editor.image.insert(result, null, null, editor.image.get());
                    };

                    // base64 로 인코딩 처리
                    reader.readAsDataURL(files[0]);
                }

                editor.popups.hideAll();

                return false;
            });
    });
</script>
```

요렇게 하면 image가 추가될 때 src 에 base64 인코딩 문자열이 들어간다.

```html
<img src="data:image/jpeg;base64,/9j/4AAQSkZJRgABAQEAYABgAAD/4RDaR...." style="width: 300px;" class="fr-fic fr-dib" />
```
