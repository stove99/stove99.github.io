---
layout: post
title: '[SpringBoot] RestTemplate 으로 Onesignal API 호출해서 푸쉬 메시지 보내기'
keywords: 'SpringBoot, onesignal, push'
categories: java
tags: java springboot
---

Java 에서 Onesignal Rest API 로 푸쉬 메시지를 보내 봅시다.

맵을 간단하게 만들기 위해 guava 의 ImmutableMap 을 활용했다. 라이브러리 추가하기 싫으면 그냥 Map 쓰면 됨

```xml
<dependency>
    <groupId>com.google.guava</groupId>
    <artifactId>guava</artifactId>
    <version>28.1-jre</version>
</dependency>
```

## config 추가

RestTemplate 을 Autowired 사용하기 위해 설정을 추가한다.

```java
@Bean
public RestTemplate restTemplate(RestTemplateBuilder builder) {
    return builder.build();
}
```

필요하면 공통적으로 추가할 인증정보를 요런식으로 인터셉터를 추가해서 처리할 수 있다.

```java
@Bean
public RestTemplate restTemplate(RestTemplateBuilder builder) {
    return builder.additionalInterceptors(new ClientHttpRequestInterceptor() {
        @Override
        public ClientHttpResponse intercept(HttpRequest request, byte[] body, ClientHttpRequestExecution execution)
                throws IOException {
            request.getHeaders().set("Authorization", "Basic xxxxxxxx(REST API KEY)");
            return execution.execute(request, body);
        }
    }).build();
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

## PUSH 전송 샘플소스

```java
package io.github.stove99.sample;

import java.util.Arrays;
import java.util.Map;

import com.google.common.collect.ImmutableMap;

import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.http.HttpEntity;
import org.springframework.http.HttpHeaders;
import org.springframework.http.ResponseEntity;
import org.springframework.test.context.junit4.SpringRunner;
import org.springframework.web.client.RestTemplate;

import lombok.extern.slf4j.Slf4j;

@Slf4j
@RunWith(SpringRunner.class)
@SpringBootTest
public class TestApp {
    @Autowired
    private RestTemplate rest;

    @Test
    public void sendPush() {
        HttpHeaders headers = new HttpHeaders();
        headers.set("Authorization", "Basic REST API KEY");

        /* request json body
            {
                "app_id": "ONESIGNAL APP ID",
                "included_segments": ["All"],
                "contents": {"en": "푸쉬 메시지 보내기 테스트"}
            }
        */

        Map<String, Object> body = ImmutableMap.of(
            "app_id", "ONESIGNAL APP ID",
            "included_segments", Arrays.asList("All"),
            "contents", ImmutableMap.of("en", "푸쉬 메시지 보내기 테스트")
        );

        HttpEntity<Map<String, Object>> request = new HttpEntity<>(body, headers);

        ResponseEntity<Map> sample = rest.postForEntity("https://onesignal.com/api/v1/notifications", request,
                Map.class);

        log.debug("result : {}", sample.getBody());
    }
}
```

특정 조건에 매칭되는 사람에게만 Push 메시지를 보낼려면 요렇게.

ex) 수신동의한 사람에게만 푸시 보내기

```java
/* request json body
    {
        "app_id": "ONESIGNAL APP ID",
        "contents": {"en": "푸쉬 메시지 보내기 테스트"},
        "filters": [
            {"field": "tag", "key": "recv_agree", "relation": "=", "value": "Y"}
        ]
    }
*/

Map<String, Object> body = ImmutableMap.of(
    "app_id", "ONESIGNAL APP ID",
    "contents", ImmutableMap.of("en", "푸쉬 메시지 보내기 테스트"),
    "filters", Arrays.asList(
                    ImmutableMap.of(
                        "field", "tag",
                        "key", "recv_agree",
                        "relation", "=",
                        "value", "Y"
                    )
                )
);
```

자세한 사용법은 요기 참고 <https://documentation.onesignal.com/reference#create-notification>
