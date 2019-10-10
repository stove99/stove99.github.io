---
layout: post
title: 'Java NIO File 스트림 맨들기'
keywords: 'java, nio, stream'
categories: java
image: https://images.unsplash.com/photo-1461638273344-c45a4eab94e1?crop=entropy&cs=tinysrgb&fit=crop&fm=jpg&h=1200&ixid=eyJhcHBfaWQiOjF9&ixlib=rb-1.2.1&q=80&w=2000
image-sm: https://images.unsplash.com/photo-1461638273344-c45a4eab94e1?crop=entropy&cs=tinysrgb&fit=crop&fm=jpg&h=1200&ixid=eyJhcHBfaWQiOjF9&ixlib=rb-1.2.1&q=80&w=2000
---

기존에 뻔질나게 써왔던 File 대신 있어보이게 NIO 로 스트림을 맨들어 보자.

## Java NIO(Path) 로 File 스트림 맨들기

```java
package io.github.stove99.sample;

import java.io.InputStream;
import java.io.OutputStream;
import java.nio.file.Files;
import java.nio.file.Path;
import java.nio.file.Paths;

public class FileSample {

    public static void main(String[] args) {
        Path file = Paths.get("d:/tmp/text.txt");

        // InputStream 만들기
        InputStream in = Files.newInputStream(file);

        // OutputStream 만들기
        OutputStream out = Files.newOutputStream(file);
    }
}
```
