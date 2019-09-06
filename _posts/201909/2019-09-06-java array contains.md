---
layout: post
title: '[Java] string array 에 특정 문자열이 포함됐는지 체크'
date: 2019-09-06
keywords: 'java'
categories: [Java]
image: https://images.unsplash.com/photo-1476283721796-dd935b062838?crop=entropy&cs=tinysrgb&fit=crop&fm=jpg&h=1200&ixid=eyJhcHBfaWQiOjF9&ixlib=rb-1.2.1&q=80&w=2000
image-sm: https://images.unsplash.com/photo-1476283721796-dd935b062838?crop=entropy&cs=tinysrgb&fit=crop&fm=jpg&h=1200&ixid=eyJhcHBfaWQiOjF9&ixlib=rb-1.2.1&q=80&w=2000
---

## 소스

```java
package io.github.stove99.sample;

import java.util.Arrays;

public class ContainsTest {

    public static void main(String[] args) {
        Arrays.asList("gif", "png", "jpg", "bmp").contains("exe"); // false
        Arrays.asList("gif", "png", "jpg", "bmp").contains("png"); // true
        Arrays.asList("gif", "png", "jpg", "bmp").contains("bat"); // false
    }
}
```
