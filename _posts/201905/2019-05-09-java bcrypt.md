---
layout: post
title: 'Java : BCrypt 로 문자열 암호화 하기'
keywords: 'java,BCrypt'
categories: java
image: https://images.unsplash.com/photo-1547623542-de3ff5941ddb?ixlib=rb-1.2.1&q=80&fm=jpg&crop=entropy&cs=tinysrgb&w=2000&h=1200&fit=crop&ixid=eyJhcHBfaWQiOjF9
image-sm: https://images.unsplash.com/photo-1547623542-de3ff5941ddb?ixlib=rb-1.2.1&q=80&fm=jpg&crop=entropy&cs=tinysrgb&w=500&h=300&fit=crop&ixid=eyJhcHBfaWQiOjF9
---

비밀번호 디비 저장시 암호화 하는 용도로 쓰면 된다.

## Maven Dependency

```xml
<dependency>
    <groupId>org.springframework.security</groupId>
    <artifactId>spring-security-crypto</artifactId>
    <version>5.1.5.RELEASE</version>
</dependency>
```

## 샘플 소스

```java
import org.springframework.security.crypto.bcrypt.BCrypt;

public class BCryptTest {

    public static void main(String[] args) {
        String password = "암호화할 문자열";

        // 디비에 저장할 비밀번호 암호화
        String encrypted = BCrypt.hashpw(password, BCrypt.gensalt());

        System.out.println("encrypted : " + encrypted);

        // 로그인시 디비에 저장된 암호화된 문자열과 사용자가 입력한 비밀번호로 checkpw 검증
        System.out.println(BCrypt.checkpw(password, encrypted)); // true
        System.out.println(BCrypt.checkpw(password + "1", encrypted)); // false
    }
}
```
