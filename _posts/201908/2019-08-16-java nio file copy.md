---
layout: post
title: "Java NIO (Files & Path) 로 파일 복사하기"
date: 2019-08-16
keywords: "java, nio, copy"
categories: [Java]
image: https://images.unsplash.com/photo-1564407343863-4c93a68ebe53?crop=entropy&cs=tinysrgb&fit=crop&fm=jpg&h=1200&ixid=eyJhcHBfaWQiOjF9&ixlib=rb-1.2.1&q=80&w=2000
image-sm: https://images.unsplash.com/photo-1564407343863-4c93a68ebe53?crop=entropy&cs=tinysrgb&fit=crop&fm=jpg&h=1200&ixid=eyJhcHBfaWQiOjF9&ixlib=rb-1.2.1&q=80&w=2000
---

Java NIO 패키지를 사용한 파일복사 예제

```java
package io.github.stove99.sample;

import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.Path;
import java.nio.file.Paths;
import java.nio.file.StandardCopyOption;

public class FileSample {

    public static void main(String[] args) throws IOException {
        Path source = Paths.get("d:/tmp/text.txt");
        Path target = Paths.get("d:/tmp1/sample/text.txt");

        // 복사할 대상 폴더가 있는지 체크해서 없으면 생성
        if (!Files.exists(target.getParent())) {
            Files.createDirectories(target.getParent());
        }

        // StandardCopyOption.REPLACE_EXISTING : 파일이 이미 존재할 경우 덮어쓰기
        Files.copy(source, target, StandardCopyOption.REPLACE_EXISTING);

        // 파일 삭제
        // Files.deleteIfExists(target);
    }
}
```
