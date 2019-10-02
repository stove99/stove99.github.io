---
layout: post
title: 'Java : User-Agent 분석하기'
keywords: 'java'
categories: java
image: https://images.unsplash.com/photo-1489321336462-efe12c02d099?ixlib=rb-1.2.1&q=80&fm=jpg&crop=entropy&cs=tinysrgb&w=2000&h=1200&fit=crop&ixid=eyJhcHBfaWQiOjF9
image-sm: https://images.unsplash.com/photo-1489321336462-efe12c02d099?ixlib=rb-1.2.1&q=80&fm=jpg&crop=entropy&cs=tinysrgb&w=500&h=300&fit=crop&ixid=eyJhcHBfaWQiOjF9
---

Java 로 User-Agent 분석하는 간단한 라이브러리가 있어서 정리해 봄

## Maven Repository 및 dependency 추가

```xml
<repositories>
    <repository>
        <id>clojars</id>
        <url>http://clojars.org/repo/</url>
    </repository>
</repositories>

<dependencies>
    <dependency>
        <groupId>org.clojure</groupId>
        <artifactId>clojure</artifactId>
        <version>1.8.0</version>
    </dependency><dependency>
        <groupId>uap-clj</groupId>
        <artifactId>uap-clj</artifactId>
        <version>1.3.3</version>
    </dependency>
</dependencies>
```

## Spring boot sample Controller

```java
package io.github.stove99.sample.web;

import java.util.HashMap;
import java.util.Map;

import javax.servlet.http.HttpServletRequest;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.ResponseBody;

import uap_clj.java.api.Browser;
import uap_clj.java.api.Device;
import uap_clj.java.api.OS;

@Controller
public class AccessLogController {

    @SuppressWarnings("unchecked")
    @GetMapping("/analysis")
    @ResponseBody
    public Map<String, Object> ua(HttpServletRequest req) {
        String ua = req.getHeader("User-Agent");

        Map<String, String> browser = (Map<String, String>) Browser.lookup(ua);
        Map<String, String> os = (Map<String, String>) OS.lookup(ua);
        Map<String, String> device = (Map<String, String>) Device.lookup(ua);

        Map<String, Object> result = new HashMap<>();
        result.put("browser", browser);
        result.put("os", os);
        result.put("device", device);

        return result;
    }
}
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

## PC Chrome Result

```json
{
    "os": { "patch": "", "patch_minor": "", "family": "Windows 10", "major": "", "minor": "" },
    "browser": { "patch": "3729", "family": "Chrome", "major": "74", "minor": "0" },
    "device": { "model": null, "family": "Other", "brand": null }
}
```

## PC IE Result

```json
{
    "os": { "patch": "", "patch_minor": "", "family": "Windows 10", "major": "", "minor": "" },
    "browser": { "patch": "", "family": "IE", "major": "11", "minor": "0" },
    "device": { "model": null, "family": "Other", "brand": null }
}
```

## 모바일 브라우저 Result

```json
{
    "os": { "patch": "0", "patch_minor": "", "family": "Android", "major": "8", "minor": "1" },
    "browser": { "patch": "3729", "family": "Chrome Mobile", "major": "74", "minor": "0" },
    "device": { "model": "Redmi Note 5", "family": "XiaoMi Redmi Note 5", "brand": "XiaoMi" }
}
```
