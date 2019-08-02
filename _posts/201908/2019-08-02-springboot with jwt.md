---
layout: post
title: 'JAVA : SpringBoot2 & Thymeleaf 에서 JWT 이용해서 로그인 하기'
date: 2019-08-02
keywords: 'SpringBoot, jwt'
categories: [SpringBoot]
image: https://images.unsplash.com/photo-1450340171252-2766ed65a4ec?crop=entropy&cs=tinysrgb&fit=crop&fm=jpg&h=1200&ixid=eyJhcHBfaWQiOjF9&ixlib=rb-1.2.1&q=80&w=2000
image-sm: https://images.unsplash.com/photo-1450340171252-2766ed65a4ec?crop=entropy&cs=tinysrgb&fit=crop&fm=jpg&h=1200&ixid=eyJhcHBfaWQiOjF9&ixlib=rb-1.2.1&q=80&w=2000
---

SpringBoot2 & Thymeleaf 로 구성된 전통적인(?) 환경에서 JWT 를 이용한 로그인 처리 샘플

JWT 방식으로 하면 SSO 처리를 쉽게 할 수 있을 것 같아서 나중에 써먹을려고 샘플을 일단 만들어 보았다.

전체소스는 여기로 <https://github.com/stove99/springboot-thymeleaf-jwt>

## jwt maven dependency

```xml
<dependency>
    <groupId>io.jsonwebtoken</groupId>
    <artifactId>jjwt</artifactId>
    <version>0.9.1</version>
</dependency>
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

## JWTService.java

jwt 토큰을 맨들고 토큰이 쪽바른지 아닌지 체크하는 서비스

```java
package io.github.stove99.jwt_sample.service;

import java.security.Key;
import java.sql.Timestamp;
import java.time.LocalDateTime;
import java.util.HashMap;
import java.util.Map;
import java.util.Optional;

import javax.crypto.spec.SecretKeySpec;
import javax.xml.bind.DatatypeConverter;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Service;

import io.jsonwebtoken.Claims;
import io.jsonwebtoken.JwtBuilder;
import io.jsonwebtoken.Jwts;
import io.jsonwebtoken.SignatureAlgorithm;

@Service
public class JWTService {
    // application.properties 에 secret 설정, 대충 원하는 문자열 10~20글자정도?
    @Value("${site.jwt.secret}")
    private String secret;

    /**
     * body 가 들어간 토큰 생성
     *
     * @param body
     * @param expired 토근 만료 시간
     * @return
     */
    public String token(Map<String, Object> body, Optional<LocalDateTime> expired) {
        byte[] apiKeySecretBytes = DatatypeConverter.parseBase64Binary(secret);

        Key key = new SecretKeySpec(apiKeySecretBytes, SignatureAlgorithm.HS512.getJcaName());

        JwtBuilder builder = Jwts.builder()
                                .setClaims(body)
                                .setExpiration(Timestamp.valueOf(LocalDateTime.now().plusDays(1)))
                                .signWith(SignatureAlgorithm.HS512, key);

        // 만료시간을 설정할 경우 expir 설정
        expired.ifPresent(exp -> {
            builder.setExpiration(Timestamp.valueOf(exp));
        });

        return builder.compact();
    }

    /**
     * 기본 만료시간 : 하루 30분 : LocalDateTime.now().plusMinutes(30) 1시간 :
     * LocalDateTime.now().plusHours(1)
     *
     * @param body
     * @return
     */
    public String token(Map<String, Object> body) {
        return token(body, Optional.of(LocalDateTime.now().plusDays(1)));
    }

    /**
     * 토큰 검증후 저장된 값 복원
     */
    public Map<String, Object> verify(String token) {
        Claims claims = Jwts.parser()
                            .setSigningKey(DatatypeConverter.parseBase64Binary(secret))
                            .parseClaimsJws(token)
                            .getBody();

        return new HashMap<>(claims);
    }
}
```

## LoginCheck.java

JWTService 를 써먹도록 인터셉터를 하나 살짝 추가해 준다.

jwt 토큰 체크 후 쪽바른 토큰이 아니라면 로그인 화면으로 이동시킴

```java
package io.github.stove99.jwt_sample.login;

import java.util.Arrays;
import java.util.Map;

import javax.servlet.http.Cookie;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Component;
import org.springframework.web.servlet.HandlerInterceptor;
import org.springframework.web.servlet.ModelAndView;
import org.springframework.web.servlet.ModelAndViewDefiningException;

import io.github.stove99.jwt_sample.service.JWTService;
import io.jsonwebtoken.ExpiredJwtException;
import io.jsonwebtoken.JwtException;
import lombok.extern.slf4j.Slf4j;

/**
 * 로그인 여부 체크 인터셉터
 */
@Component
@Slf4j
public class LoginCheck implements HandlerInterceptor {
    public static final String COOKIE_NAME = "login_token";

    @Autowired
    private JWTService jwtService;

    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler)
            throws ModelAndViewDefiningException {

        String token = Arrays.stream(request.getCookies())
                            .filter(cookie -> cookie.getName().equals(LoginCheck.COOKIE_NAME))
                            .findFirst().map(Cookie::getValue)
                            .orElse("dummy");

        log.info("token : {}", token);

        try {
            Map<String, Object> info = jwtService.verify(token);

            // View 에서 session.id 처럼 로그인 정보 쉽게 가져다 쓸수 있도록 request 에 verify 한 사용자 정보 설정
            User user = User.builder()
                                .id((String) info.get("id"))
                                .name((String) info.get("name"))
                                .build();

            // view 에서 login.id 로 접근가능
            request.setAttribute("login", user);
        } catch (ExpiredJwtException ex) {
            log.error("토근이 만료됨");

            ModelAndView mav = new ModelAndView("login");
            mav.addObject("return_url", request.getRequestURI());

            throw new ModelAndViewDefiningException(mav);
        } catch (JwtException ex) {
            log.error("비정상 토큰");

            ModelAndView mav = new ModelAndView("login");

            throw new ModelAndViewDefiningException(mav);
        }

        return true;
    }
}
```

## 로그인 정보를 위한 아규먼트 리졸버

로그인 정보를 가져올려면 Cookie 에서 토큰을 뒤져서 verify 하는 귀찮은 짖을 계속해야 된다.

@LoginUser String id 요딴식으로 어노테이션을 써서 쉽게 로그인한 사용자의 정보를 가져올 수 있도록 아규먼트 리졸버를 하나 맹들어 본다.

LoginUserResolver.java

```java
package io.github.stove99.jwt_sample.login;

import java.util.Arrays;
import java.util.HashMap;
import java.util.Map;

import javax.servlet.http.Cookie;
import javax.servlet.http.HttpServletRequest;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.core.MethodParameter;
import org.springframework.stereotype.Component;
import org.springframework.web.bind.support.WebDataBinderFactory;
import org.springframework.web.context.request.NativeWebRequest;
import org.springframework.web.method.support.HandlerMethodArgumentResolver;
import org.springframework.web.method.support.ModelAndViewContainer;

import io.github.stove99.jwt_sample.service.JWTService;

/**
 * 로그인 유저 정보 쉽게 가져오기 위한 Argument Resolver
 */
@Component
public class LoginUserResolver implements HandlerMethodArgumentResolver {
    @Autowired
    private JWTService jwtService;

    @Override
    public boolean supportsParameter(MethodParameter param) {
        return param.hasParameterAnnotation(LoginUser.class);
    }

    @Override
    public Object resolveArgument(MethodParameter param, ModelAndViewContainer mvc, NativeWebRequest nreq,
            WebDataBinderFactory dbf) throws Exception {
        final Map<String, Object> resolved = new HashMap<>();

        HttpServletRequest req = (HttpServletRequest) nreq.getNativeRequest();

        // 쿠키에 토큰이 있는 경우 꺼내서 verify 후 로그인 정보 리턴
        Arrays.stream(req.getCookies())
                .filter(cookie -> cookie.getName().equals(LoginCheck.COOKIE_NAME))
                .map(Cookie::getValue).findFirst().ifPresent(token -> {
                    Map<String, Object> info = jwtService.verify(token);

                    // @LoginUser String id, @LoginUser String name
                    if (param.getParameterType().isAssignableFrom(String.class)) {
                        resolved.put("resolved", info.get(param.getParameterName()));
                    }
                    // @LoginUser User user
                    else if (param.getParameterType().isAssignableFrom(User.class)) {
                        User user = User.builder()
                                        .id((String) info.get("id"))
                                        .name((String) info.get("name"))
                                        .build();

                        resolved.put("resolved", user);
                    }
                });

        return resolved.get("resolved");
    }
}
```

LoginUser.java

```java
package io.github.stove99.jwt_sample.login;

import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

@Target(ElementType.PARAMETER)
@Retention(RetentionPolicy.RUNTIME)
public @interface LoginUser {
}
```

User.java

```java
package io.github.stove99.jwt_sample.login;

import lombok.Builder;
import lombok.Data;

@Builder
@Data
public class User {

    private String id;
    private String name;
}
```

## 맨든 인터셉터와 아규먼트 리졸버 등록하기

적당히 Config 를 위한 클래스를 하나 맹글어서 설정해 준다.

DemoConfig.java

```java
package io.github.stove99.jwt_sample;

import java.util.List;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.method.support.HandlerMethodArgumentResolver;
import org.springframework.web.servlet.config.annotation.InterceptorRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

import io.github.stove99.jwt_sample.login.LoginCheck;
import io.github.stove99.jwt_sample.login.LoginUserResolver;

@Configuration
public class DemoConfig implements WebMvcConfigurer {

    @Autowired
    private LoginUserResolver loginUserResolver;

    @Autowired
    LoginCheck loginCheck;

    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(loginCheck).addPathPatterns("", "/**")
                    .excludePathPatterns("/login", "/resources/**");
    }

    @Override
    public void addArgumentResolvers(List<HandlerMethodArgumentResolver> resolvers) {
        resolvers.add(loginUserResolver);
    }
}
```

## 컨트롤러

```java
package io.github.stove99.jwt_sample.controller;

import java.time.LocalDateTime;
import java.util.HashMap;
import java.util.Map;
import java.util.Optional;

import javax.servlet.http.Cookie;
import javax.servlet.http.HttpServletResponse;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestParam;

import io.github.stove99.jwt_sample.login.LoginCheck;
import io.github.stove99.jwt_sample.login.LoginUser;
import io.github.stove99.jwt_sample.service.JWTService;
import lombok.extern.slf4j.Slf4j;

@Slf4j
@Controller
public class SampleController {
    @Autowired
    private JWTService jwtService;

    @GetMapping("/")
    public String rootPage() {
        return "redirect:/main";
    }

    @GetMapping("login")
    public void loginPage() {
    }

    @PostMapping("login")
    public String login(@RequestParam String id, @RequestParam String pwd, HttpServletResponse res) {
        // 로그인 로직

        // 로그인 성공시 쿠키에 token 저장
        Map<String, Object> user = new HashMap<>();
        user.put("id", id);
        user.put("name", "홍길동");

        // 30분후 만료되는 jwt 만들어서 쿠키에 저장
        Cookie cookie = new Cookie(
                            LoginCheck.COOKIE_NAME,
                            jwtService.token(user, Optional.of(LocalDateTime.now().plusMinutes(30)))
                        );

        cookie.setPath("/");
        cookie.setMaxAge(Integer.MAX_VALUE);

        res.addCookie(cookie);

        return "redirect:/main";
    }

    /**
     * 로그아웃 처리 : 쿠키에서 jwt 삭제
     */
    @GetMapping
    public String logout(HttpServletResponse res) {
        Cookie cookie = new Cookie(LoginCheck.COOKIE_NAME, "");
        cookie.setPath("/");
        cookie.setMaxAge(0);

        res.addCookie(cookie);

        return "redirect:/login";
    }

    @GetMapping("main")
    public void mainPage(Model model, @LoginUser String id) {
        log.info("로그인 아이디 : {}", id);
    }
}
```

## View

main.html

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8" />
        <meta name="viewport" content="width=device-width, initial-scale=1.0" />
        <meta http-equiv="X-UA-Compatible" content="ie=edge" />
        <title>Document</title>
    </head>
    <body>
        <p>로그인 후 접속가능한 페이지</p>

        ID : <span th:text="${login.id}">로그인 아이디</span> NAME : <span th:text="${login.name}">로그인 사용자</span>

        <p><a href="/logout">로그아웃</a></p>
    </body>
</html>
```
