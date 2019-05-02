---
layout: post
title: "Spring Boot : Interceptor 에서 forward 처리하기"
date: 2019-05-02
keywords: "java,spring boot"
categories: [Java, SpringBoot]
image: https://picsum.photos/2000/1200?image=1047
image-sm: https://picsum.photos/500/300?image=1047
---

Spring Boot / Thymeleaf 로 개발하고 있는데 Interceptor 에서 늘하던데로

    request.getRequestDispatcher("xxxx").forward(request, response);

요렇게 forward 를 할려니까 잘 않되서 이것저것 찾아보았다. 쫌 꾸질꾸질 한 것 같기도 하지만 처음 보던 방식이라 한번 정리해 보았다.

## 소스

```java
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import org.springframework.stereotype.Component;
import org.springframework.web.servlet.HandlerInterceptor;
import org.springframework.web.servlet.ModelAndView;
import org.springframework.web.servlet.ModelAndViewDefiningException;

@Component
public class AuthCheckInterceptor implements HandlerInterceptor {
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler)
            throws ModelAndViewDefiningException {
        if (request.getSession().getAttribute("login_user") == null) {
            ModelAndView mav = new ModelAndView("user/login");
            mav.addObject("message", "추가 에트리뷰트");

            throw new ModelAndViewDefiningException(mav);
        }

        return true;
    }
}
```
