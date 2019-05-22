---
layout: post
title: 'JAVA : 두 날짜 사이의 차이 구하기'
date: 2019-05-22
keywords: 'java'
categories: [Java]
image: https://images.unsplash.com/photo-1552978730-f820c227a2f1?ixlib=rb-1.2.1&q=80&fm=jpg&crop=entropy&cs=tinysrgb&w=2000&h=1200&fit=crop&ixid=eyJhcHBfaWQiOjF9
image-sm: https://images.unsplash.com/photo-1552978730-f820c227a2f1?ixlib=rb-1.2.1&q=80&fm=jpg&crop=entropy&cs=tinysrgb&w=2000&h=1200&fit=crop&ixid=eyJhcHBfaWQiOjF9
---

Java 8 에 추가된 java.time 패키지를 이용해서 계산하기

ChronoUnit 에 있는 여러가지 단위로 between 을 계산할 수 있다.

## 예제

```java
import java.time.LocalDate;
import java.time.temporal.ChronoUnit;

public class Test {

    public static void main(String[] args) {
        LocalDate today = LocalDate.now();
        LocalDate compare_dt = today.minusDays(2);

        long diff = ChronoUnit.DAYS.between(today, compare_dt);

        // 차이 : -2
        System.out.println("차이 : " + diff);
    }
}
```
