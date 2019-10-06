---
layout: post
title: 'Spring Boot : Argument Resolver 로 로그인 유저 정보 쉽게 가져오기'
keywords: 'java,spring boot'
categories: java
tags: java springboot
image: https://picsum.photos/2000/1200?image=1050
image-sm: https://picsum.photos/500/300?image=1050
---

로그인한 사람이 들어와서 여러가지 동작을 하는 프로젝트는 이것저것 맹글다 보면 컨트롤러에서 로그인한 사람의 정보를 가져와야 되는 경우가 많다.

이럴때마다 요렇게 RequestMapping 한 메소드 파라메터에다 HttpSession session 을 추가하고 주절주절 로그인 정보를 가져온다.

```java
@GetMapping("/main")
private void main(HttpSession session){
    ...

    Map login = (Map)session.getAttribute("login_user");
    log.debug("login user id : {}", login.get("id"));

    ...
}
```

매번 요렇게 하면 귀찮기도 하고 멋스럽지도 않기 때문에 아래 소스처럼 왠지 모르지만 멋스럽게 가져오게 맹글어 보자.

```java
@GetMapping("/main")
private void main(@LoginUserId String id){
    ...

    log.debug("login user id : {}", id);

    ...
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

## Annotaion 맹글기

LoginUserId.java

```java
package io.github.stove99;

import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

@Target(ElementType.PARAMETER)
@Retention(RetentionPolicy.RUNTIME)
public @interface LoginUserId {
    String value() default "";
}
```

## 맹근 Annotaion 을 써먹는 Argument Resolver 추가

따로 LoginUserIdArgumentResolver 요딴 클래스를 맹글어도 되지만 굳이 맨들 필요성을 못느껴서 간단하게 처리함

```java
package io.github.stove99;

import java.util.List;
import java.util.Map;

import org.springframework.context.annotation.Configuration;
import org.springframework.core.MethodParameter;
import org.springframework.web.bind.support.WebDataBinderFactory;
import org.springframework.web.context.request.NativeWebRequest;
import org.springframework.web.context.request.WebRequest;
import org.springframework.web.method.support.HandlerMethodArgumentResolver;
import org.springframework.web.method.support.ModelAndViewContainer;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

@Configuration
public class WebMvcConfig implements WebMvcConfigurer {
    /**
     * controller 에서 쓰는 argument resolver 등록
     */
    @Override
    public void addArgumentResolvers(List<HandlerMethodArgumentResolver> resolvers) {
        resolvers.add(loginUserIdResolver());
    }

    public HandlerMethodArgumentResolver loginUserIdResolver() {
        return new HandlerMethodArgumentResolver() {

            @Override
            public boolean supportsParameter(MethodParameter parameter) {
                return parameter.hasParameterAnnotation(LoginUserId.class);
            }

            @SuppressWarnings("rawtypes")
            @Override
            public Object resolveArgument(MethodParameter parameter, ModelAndViewContainer mavContainer,
                    NativeWebRequest webRequest, WebDataBinderFactory binderFactory) throws Exception {
                Object resolved = null;

                if (String.class.isAssignableFrom(parameter.getParameterType())) {
                    Map user = (Map) webRequest.getAttribute("login_user", WebRequest.SCOPE_SESSION);

                    resolved = user.get("id");
                }

                return resolved;
            }
        };
    }
}
```

요렇게만 해 주면 로그인한 사람 아이디는 @LoginUserId String id 로 쉽게 가져올 수 있다.

## 추가적으로 로그인한 사람 전체 정보가 담긴 Map 을 가져올라면 우째해야 할까?

    @LoginUser Map user

요렇게 가져올려고 했는데 Argument Resolver 가 아예 호출이 안되서 잠깐 삽질의 시간을 가졌다.

구글님께 물어보니 Map 을 구현한 클래스가 파라메터면 Argument Resolver 를 거치지 않고 그냥 스킵된다고 한다. Spring 소스를 찾아 보면 왜 그런지 알 수 있겠지만 그렇게 깊게는 알고 싶지 않다.

아무튼 해결하기 Map 을 감싸는 껍데기 클래스를 살짝 맹글어본다. 클래스 이름을 뭘로 지을지 고민이 많았지만 그냥 Data 로...

## Data.java

있어보이게 Generic 으로 맹글어봄

```java
package io.github.stove99;

import java.util.HashMap;
import java.util.Map;

public class Data<K, V> {
    private Map<K, V> data = new HashMap<>();

    public Data(Map<K, V> data) {
        this.data.putAll(data);
    }

    public V get(K key) {
        return data.get(key);
    }

    public V put(K key, V value) {
        return this.data.put(key, value);
    }

    public String toString() {
        return this.data.toString();
    }
}
```

## LoginUser.java

Annotaion 맹글기

```java
package io.github.stove99;

import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

@Target(ElementType.PARAMETER)
@Retention(RetentionPolicy.RUNTIME)
public @interface LoginUser {
    String value() default "";
}
```

## Argument Resolver 에 추가

```java
package io.github.stove99;

import java.util.List;
import java.util.Map;

import org.springframework.context.annotation.Configuration;
import org.springframework.core.MethodParameter;
import org.springframework.web.bind.support.WebDataBinderFactory;
import org.springframework.web.context.request.NativeWebRequest;
import org.springframework.web.context.request.WebRequest;
import org.springframework.web.method.support.HandlerMethodArgumentResolver;
import org.springframework.web.method.support.ModelAndViewContainer;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

@Configuration
public class WebMvcConfig implements WebMvcConfigurer {

    @Override
    public void addArgumentResolvers(List<HandlerMethodArgumentResolver> resolvers) {
        resolvers.add(loginUserResolver());
        resolvers.add(loginUserIdResolver());
    }

    public HandlerMethodArgumentResolver loginUserResolver() {
        return new HandlerMethodArgumentResolver() {

            @Override
            public boolean supportsParameter(MethodParameter parameter) {
                return parameter.hasParameterAnnotation(LoginUser.class);
            }

            @SuppressWarnings({ "unchecked", "rawtypes" })
            @Override
            public Object resolveArgument(MethodParameter parameter, ModelAndViewContainer mavContainer,
                    NativeWebRequest webRequest, WebDataBinderFactory binderFactory) throws Exception {
                Object resolved = null;

                if (Data.class.isAssignableFrom(parameter.getParameterType())) {
                    Map session = (Map) webRequest.getAttribute("login_user", WebRequest.SCOPE_SESSION);

                    resolved = new Data<>(session);
                }

                return resolved;
            }
        };
    }

    public HandlerMethodArgumentResolver loginUserIdResolver() {
        return new HandlerMethodArgumentResolver() {

            @Override
            public boolean supportsParameter(MethodParameter parameter) {
                return parameter.hasParameterAnnotation(LoginUserId.class);
            }

            @SuppressWarnings("rawtypes")
            @Override
            public Object resolveArgument(MethodParameter parameter, ModelAndViewContainer mavContainer,
                    NativeWebRequest webRequest, WebDataBinderFactory binderFactory) throws Exception {
                Object resolved = null;

                if (String.class.isAssignableFrom(parameter.getParameterType())) {
                    Map session = (Map) webRequest.getAttribute("login_user", WebRequest.SCOPE_SESSION);

                    resolved = session.get("id");
                }

                return resolved;
            }
        };
    }
}
```

요렇게 다 맹글고 나서 보니

@LoginUser("id) String id, @LoginUser Data user 이렇게 쓰면 더 있어 보일것 같다는 생각이 뒤늦게 들었다.

과감하게 LoginUserId Annotaion 은 지워버리자. LoginUser Annotaion 은 그냥 냅두면 되고 ArgumentResolver 소스만 살짝 바꾸면 된다.

## 최종 WebMvcConfig.java

```java
package io.github.stove99;

import java.util.List;
import java.util.Map;

import org.springframework.context.annotation.Configuration;
import org.springframework.core.MethodParameter;
import org.springframework.web.bind.support.WebDataBinderFactory;
import org.springframework.web.context.request.NativeWebRequest;
import org.springframework.web.context.request.WebRequest;
import org.springframework.web.method.support.HandlerMethodArgumentResolver;
import org.springframework.web.method.support.ModelAndViewContainer;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

@Configuration
public class WebMvcConfig implements WebMvcConfigurer {

    @Override
    public void addArgumentResolvers(List<HandlerMethodArgumentResolver> resolvers) {
        resolvers.add(loginUserResolver());
    }

    public HandlerMethodArgumentResolver loginUserResolver() {
        return new HandlerMethodArgumentResolver() {

            @Override
            public boolean supportsParameter(MethodParameter parameter) {
                return parameter.hasParameterAnnotation(LoginUser.class);
            }

            @SuppressWarnings({ "unchecked", "rawtypes" })
            @Override
            public Object resolveArgument(MethodParameter parameter, ModelAndViewContainer mavContainer,
                    NativeWebRequest webRequest, WebDataBinderFactory binderFactory) throws Exception {
                Object resolved = null;

                Map user = (Map) webRequest.getAttribute("login_user", WebRequest.SCOPE_SESSION);

                if (user != null) {
                    if (Data.class.isAssignableFrom(parameter.getParameterType())) {
                        resolved = new Data<>(user);
                    } else if (String.class.isAssignableFrom(parameter.getParameterType())) {
                        LoginUser annotation = parameter.getParameterAnnotation(LoginUser.class);
                        resolved = annotation.value().isEmpty() ? null : user.get(annotation.value());
                    }
                }

                return resolved;
            }
        };
    }
}
```

## 컨트롤러에서는 요렇게~

```java
@GetMapping("/main")
private void main(@LoginUser Data<String, String> user, @LoginUser("id") String id) {
    log.debug("user info : {}", user);
    log.debug("user id : {}", id);
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
