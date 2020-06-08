---
layout: post
title: '[Java] Spring - url 에 따라 locale 설정해서 message 번들 가져오기'
keywords: 'java'
categories: java springboot
image: https://images.unsplash.com/photo-1489875347897-49f64b51c1f8?crop=entropy&cs=tinysrgb&fit=crop&fm=jpg&h=600&ixid=eyJhcHBfaWQiOjF9&ixlib=rb-1.2.1&q=80&w=1000
image-sm: https://images.unsplash.com/photo-1489875347897-49f64b51c1f8?crop=entropy&cs=tinysrgb&fit=crop&fm=jpg&h=600&ixid=eyJhcHBfaWQiOjF9&ixlib=rb-1.2.1&q=80&w=1000
---

http://127.0.0.1:8080/
http://127.0.0.1:8080/list

요딴 url 일 경우는 한글 메시지를 가져오고

http://127.0.0.1:8080/eng
http://127.0.0.1:8080/eng/list

처럼 eng 로 시작하는 url 은 영문 메시지를 가져오게끔 url 에 따라 Locale 을 적용해서 메시지를 가져오도록 해 보자.

## 컨피크를 위한 클래스 추가

대충 WebConfig 라고 이름짓고 클래스 하나 추가

```java

package io.github.stove99.localedemo;

import java.util.Locale;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.LocaleResolver;
import org.springframework.web.servlet.config.annotation.InterceptorRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;
import org.springframework.web.servlet.handler.HandlerInterceptorAdapter;
import org.springframework.web.servlet.i18n.SessionLocaleResolver;
import org.springframework.web.servlet.support.RequestContextUtils;

@Configuration
public class WebConfig implements WebMvcConfigurer {

    /**
     * 모든 url 에 대해 인터셉터 설정
     */
    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(new PathLocaleInterceptor()).addPathPatterns("/**");
    }

    /**
     * 디폴트 로케일 리졸버 등록 등록안하면 Cannot change HTTP accept header - use a different
     * locale resolution strategy 어쩌고 오류난다.
     *
     * @return
     */
    @Bean
    public LocaleResolver localeResolver() {
        SessionLocaleResolver slr = new SessionLocaleResolver();
        slr.setDefaultLocale(Locale.KOREA);

        return slr;
    }
}

class PathLocaleInterceptor extends HandlerInterceptorAdapter {
    @Override
    public boolean preHandle(HttpServletRequest req, HttpServletResponse res, Object handler) throws Exception {

        String uri = req.getRequestURI();

        // 서버에 올렸을때 영문 메시지를 쪽바로 못가져 오는 경우가 있는데 고럴때는
        // Locale.ENGLISH 를 Locale.US 로 바꾸고 메시지 번들 파일명을 messages_en_US.properties 로 바꿔보셈
        // /eng 로 시작하는 url 일 경우 로케일을 en으로 바꿈
        Locale locale = uri.startsWith("/eng") ? Locale.ENGLISH : Locale.KOREA;
        LocaleResolver localeResolver = RequestContextUtils.getLocaleResolver(req);
        localeResolver.setLocale(req, res, locale);

        return true;
    }
}
```

## thymeleaf 에서 메시지 땡겨다 쓰기

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8" />
        <meta name="viewport" content="width=device-width, initial-scale=1.0" />
        <title>Document</title>
    </head>
    <body>
        <span>[[#{message}]]</span>
    </body>
</html>
```

전체 소스는 [요기](https://github.com/stove99/spring-locale-by-uri)에 가보면 됩니다~
