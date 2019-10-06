---
layout: post
title: 'JAVA : SpringBoot2 에 Ehcache3 적용하기'
keywords: 'SpringBoot, Ehcache3'
categories: java
tags: java springboot
image: https://images.unsplash.com/photo-1498623116890-37e912163d5d?crop=entropy&cs=tinysrgb&fit=crop&fm=jpg&h=1200&ixid=eyJhcHBfaWQiOjF9&ixlib=rb-1.2.1&q=80&w=2000
image-sm: https://images.unsplash.com/photo-1498623116890-37e912163d5d?crop=entropy&cs=tinysrgb&fit=crop&fm=jpg&h=1200&ixid=eyJhcHBfaWQiOjF9&ixlib=rb-1.2.1&q=80&w=2000
---

Srping 에 Cache 를 도입하면 메소드 레벨로 캐싱을 할 수 있다. 코드목록 가져오는 것 처럼 데이터가 자주 바뀌지 않지만 매우 자주 호출되는곳에 적용하면 좋을것 같다.

## maven dependency

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-cache</artifactId>
</dependency>

<dependency>
    <groupId>javax.cache</groupId>
    <artifactId>cache-api</artifactId>
    <version>1.1.1</version>
</dependency>
<dependency>
    <groupId>org.ehcache</groupId>
    <artifactId>ehcache</artifactId>
    <version>3.8.0</version>
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

## @EnableCaching 어노테이션 추가

요런식으로 적당한 곳에 추가해 주면 된다.

```java
@SpringBootApplication
@EnableCaching
public class Application {
    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}
```

## 캐시를 적용할 곳에 @Cacheable, @CacheEvict 어노테이션 추가

어노테이션이 몇개 더 있는데 대충 이것 두개만 써도 충분할것 같다. 샘플로 코드 가져오는 서비스 클래스에 적용해 보았다.

코드리스트 가져오는 메소드가 가장 많이 호출되기 때문에 굳이 다른 메소드는 캐쉬를 적용안해도 될 것 같다.

캐쉬를 적용할 메소드 위에 딸랑 @Cacheable 요 어노테이션만 살짝 설정해 주면 끝난다. 매우 간단하지 아니한가? 🤗

그리고 코드가 변경되거나 삭제됐을때는 캐쉬를 지워야 되기 때문에 해당 메소드에 @CacheEvict 를 적용하면 된다.

```java
package io.github.stove99.sample;

import java.util.List;
import java.util.Map;

import io.github.stove99.sample.module.code.mapper.CodeMapper;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.cache.annotation.CacheEvict;
import org.springframework.cache.annotation.Cacheable;
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;

/**
 * @author stove99
 */
@Service
@Transactional
public class CodeService {

    @Autowired
    private CodeMapper mapper;

    public Map<String, String> get(String code) {
        return mapper.select(code);
    }

    /**
    * value 는 캐쉬 뭉탱이의 이름이라고 생각하면 된다. (RDBMS 의 테이블)
    * key 는 RDBMS 의 테이블의 PK 라고 생각하면 됨
    * list("test") 를 호출하면 메소드가 한번 실행된 후에 리턴되는 List 가 codes 에 test 키에 캐쉬된다.
    * 다시 똑같은 파라메터로 list("test") 호출하면 메소드가 실행되지 않고 codes 캐쉬에 있는 test 키를 찾아서 저장된 값을 리턴한다.
    **/
    @Cacheable(value = "codes", key="#codeGrp")
    public List<Map<String, String>> list(String codeGrp) {
        return mapper.list(codeGrp);
    }

    /**
    * 데이터가 변경되면 캐시되 있던걸 지워야 됨
    * @CacheEvict(value = "codes", allEntries = true) 는 codes 캐시에 들어있는걸 싹 지운다는 뜻
    *
    * 특정 코드그룹 key 만 캐쉬에서 지우고 싶으면
    * @CacheEvict(value = "codes", key = "#data.get('codeGrp')") 요런식으로 쓰면 된다.
    *
    * getter 가 있는 일반 객체는 요렇게
    * @CacheEvict(value = "codes", key = "#data.codeGrp")
    **/
    @CacheEvict(value = "codes", allEntries = true)
    public Map<String, String> save(Map<String, String> data) {
        if (get(data.get("code")) == null) {
            mapper.insert(data);
        } else {
            mapper.update(data);
        }
        return data;
    }

    @CacheEvict(value = "codes", allEntries = true)
    public void remove(String code) {
        mapper.delete(code);
    }
}
```

## ehcache.xml 설정파일 추가

죠렇게 딸랑해주면 좋겠지만 ehcache 로 캐싱을 할려면 ehcahce 설정을 쫌 해줘야 된다.

/src/main/resources/ehcache.xml 에 파일을 추가해 준다.

```xml
<config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns="http://www.ehcache.org/v3"
    xmlns:jsr107="http://www.ehcache.org/v3/jsr107"
    xsi:schemaLocation="
            http://www.ehcache.org/v3 http://www.ehcache.org/schema/ehcache-core-3.0.xsd
            http://www.ehcache.org/v3/jsr107 http://www.ehcache.org/schema/ehcache-107-ext-3.0.xsd">

    <!--
        캐쉬가 하나만 있으면 굳이 템플릿을 안 맹글어도 되지만 여러군데 쓸 수 있기 때문에
        중복되는것을 피하기 위해 템플릿을 맹금
    -->
    <cache-template name="template">
        <!-- 캐시 만료기간 설정 -->
        <expiry>
            <!-- unit : nanos, micros, millis, seconds, minutes, hours, days -->
            <ttl unit="days">1</ttl>
        </expiry>

        <!--
        캐시가 생성되고 삭제되고 하는 이벤트를 모니터링 하고 싶으면
        org.ehcache.event.CacheEventListener 를 구현하는 클래스를 만들어서 요렇게 설정해 주면됨
        태그 순서가 중요해서 아무데나 listeners 태그를 추가하면 오류남
        <listeners>
            <listener>
                <class>io.github.stove99.sample.CacheEventLogger</class>
                <event-firing-mode>ASYNCHRONOUS</event-firing-mode>
                <event-ordering-mode>UNORDERED</event-ordering-mode>
                <events-to-fire-on>CREATED</events-to-fire-on>
                <events-to-fire-on>EVICTED</events-to-fire-on>
                <events-to-fire-on>REMOVED</events-to-fire-on>
                <events-to-fire-on>UPDATED</events-to-fire-on>
                <events-to-fire-on>EXPIRED</events-to-fire-on>
            </listener>
        </listeners>
        -->

        <resources>
            <!-- 캐시에 최대 몇개 까지 유지할지 -->
            <heap unit="entries">200</heap>
        </resources>
    </cache-template>

    <!--
        alias 는 @Cacheable(value = "codes", key="#codeGrp") 여기에서 썻던 value 값으로
    -->
    <cache alias="codes" uses-template="template"></cache>
    <cache alias="menus" uses-template="template" >
        <expiry>
            <ttl unit="seconds">60</ttl>
        </expiry>
    </cache>
    <cache alias="contents_pages" uses-template="template"></cache>
</config>
```

## application.properties 또는 application.yml 설정

```bash
spring.cache.jcache.config: classpath:ehcache.xml
```

```yml
spring:
    cache:
        jcache:
            config: classpath:ehcache.xml
```

끝~

<ins class="adsbygoogle"
     style="display:block; text-align:center;"
     data-ad-layout="in-article"
     data-ad-format="fluid"
     data-ad-client="ca-pub-7073298118440059"
     data-ad-slot="8400970402"></ins>

<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>