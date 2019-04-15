---
layout: post
title:  "Javascript 팁2"
date:   2019-04-16
keywords: "javascript"
categories: [Javascript]
tags: [javascript]
icon: icon-javascript
---

# Javascript 여러가지 유용한 문구들

## 배열 복사하기

``` javascript
    const arr = ['사과', '딸기', '오렌지'];

    // 기존
    const copyed1 = arr.slice();

    // es6 로
    const copyed2 = [...arr];
```

## 객체 복사하기

``` javascript
    const obj = {
        a : 1,
        b : "2",
        c : {
            d : 3,
            e : "good",
        },
    };

    // es6
    const copyed = {...obj};

    console.log(copyed);  // { a: 1, b: '2', c: { d: 3, e: 'good' } }
```

## !! 연산자로 boolean 형변환

``` javascript
    let val = 1;

    console.log(!!val); // true

    // "", null, 0, undefined 는 false
    val = 0;
    console.log(!!val); // false
```

## + 연산자를 이용해서 number 로 형변환

``` javascript
    let val = "1234";

    console.log(+val); // 1234

    val = "1234.12";
    console.log(+val); // 1234.12

    val = "1234a";
    console.log(+val); // NaN

    console.log(+new Date());  // 1555303468747
```

## if( 조건 ) 실행 간결하게 표현하기

``` javascript
    // user 가 있으면 login() 함수 실행하기
    if( user ) {
        user.login();
    }

    // 요렇게 간결하게 표현할 수 있다.
    user && user.login();
```

## || 연산자로 디폴트값 설정하기

``` javascript
    const fun = (param1) => {
        param1 || '디폴트값';
        return 'param1 : ' + param1;
    };

    console.log(fun());   // param1 : 디폴트값
    console.log(fun(1));  // param1 : 1
```

## ~~ 연산자로 소수점 이하값 버리기

``` javascript
    const val = 1234.5678;

    // Math.floor 로 하는 방법
    console.log(Math.floor(val));    // 1234

    // ~~ 연산자로 하는 방법(속도가 빠름)
    console.log(~~(val));    // 1234
```

## 배열 합치기

``` javascript
    const arr1 = [1, 2, 3];
    const arr2 = [4, 5, 6];

    // 요렇게 하면 메모리 적게 씀
    Array.push.apply(arr1, arr2);   // [1,2,3,4,5,6]

    // es6
    const merged = [...arr1, ...arr2];  // [1,2,3,4,5,6]
```