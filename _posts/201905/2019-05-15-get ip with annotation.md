---
layout: post
title: 'Spring Boot : Argument Resolver 로 IP 가져오기'
keywords: 'IP, Annotation, Spring-boot'
categories: java
tags: java springboot
image: https://images.unsplash.com/photo-1553602932-f93f674a9aaa?ixlib=rb-1.2.1&q=80&fm=jpg&crop=entropy&cs=tinysrgb&w=2000&h=1200&fit=crop&ixid=eyJhcHBfaWQiOjF9
image-sm: https://images.unsplash.com/photo-1553602932-f93f674a9aaa?ixlib=rb-1.2.1&q=80&fm=jpg&crop=entropy&cs=tinysrgb&w=500&h=300&fit=crop&ixid=eyJhcHBfaWQiOjF9
---

컨트롤러 메소드에서 어노테이션으로 클라이언트 IP 가져오는 방법

## ClientIp.java

먼저 간단한 어노테이션을 하나 맹근다.

```java
package io.github.stove99.sample.resolver;

import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

@Target(ElementType.PARAMETER)
@Retention(RetentionPolicy.RUNTIME)
public @interface ClientIp {
}
```

## ClientIpResolver.java

맨든 어노테이션을 인식하도록 resolver 를 작성하고, 작성한 resolver 를 mvc config 에 설정한다.

```java
package io.github.stove99.sample.resolver;

import javax.servlet.http.HttpServletRequest;

import org.springframework.core.MethodParameter;
import org.springframework.stereotype.Component;
import org.springframework.web.bind.support.WebDataBinderFactory;
import org.springframework.web.context.request.NativeWebRequest;
import org.springframework.web.method.support.HandlerMethodArgumentResolver;
import org.springframework.web.method.support.ModelAndViewContainer;

@Component
public class ClientIpResolver implements HandlerMethodArgumentResolver {
    private static final String[] IP_HEADER_CANDIDATES = {
            "X-Forwarded-For", "Proxy-Client-IP", "WL-Proxy-Client-IP",
            "HTTP_X_FORWARDED_FOR", "HTTP_X_FORWARDED", "HTTP_X_CLUSTER_CLIENT_IP",
            "HTTP_CLIENT_IP", "HTTP_FORWARDED_FOR", "HTTP_FORWARDED", "HTTP_VIA",
            "REMOTE_ADDR"
        };

    @Override
    public boolean supportsParameter(MethodParameter parameter) {
        return parameter.hasParameterAnnotation(ClientIp.class);
    }

    @Override
    public Object resolveArgument(MethodParameter param, ModelAndViewContainer mavc, NativeWebRequest req,
            WebDataBinderFactory wbf) throws Exception {

        HttpServletRequest request = (HttpServletRequest) req.getNativeRequest();

        for (String header : IP_HEADER_CANDIDATES) {
            String ip = request.getHeader(header);

            if (ip != null && ip.length() != 0 && !"unknown".equalsIgnoreCase(ip)) {
                return ip;
            }
        }

        return request.getRemoteAddr();
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

## mvc 설정 클래스(MvcConfig.java)

```java
package io.github.stove99.sample;

import java.util.List;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.method.support.HandlerMethodArgumentResolver;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

import io.github.stove99.sample.resolver.ClientIpResolver;

@Configuration
public class MvcConfig implements WebMvcConfigurer {

    @Autowired
    private ClientIpResolver clientIpResolver;

    @Override
    public void addArgumentResolvers(List<HandlerMethodArgumentResolver> resolvers) {
        resolvers.add(clientIpResolver);
    }
}
```

컨트롤러에서는 요렇게 하면 간편하게 아이피를 가져올 수 있다.

```java
@GetMapping("/ip")
@ResponseBody
public String iptest(@ClientIp String ip) {
    System.out.println("ip : " + ip);

    return ip;
}
```
