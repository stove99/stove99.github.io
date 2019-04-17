---
layout: post
title:  "Mysql InnoDB database drop 했을때 복구하기"
date:   2019-03-19
keywords: "mysql,recover,drop"
categories: [Linux]
tags: [mysql,recover]
icon: icon-mysql
image: https://picsum.photos/2000/1200?random
image-sm: https://picsum.photos/500/300?random
---

요렇게 해서 복구를 하긴 했는데 완벽한것 같지는 않다.

데이터 복구과정에서 컬럼에 들어가는 데이터가 뒤죽박죽 엉키는 현상이 있음.

## TwinDB data recovery toolkit 설치

``` bash
    git clone https://github.com/twindb/undrop-for-innodb.git

    cd undrop-for-innodb

    make
```

## mysql data 디렉토리에 있는 ibdata1 처리

``` bash
    ./stream_parser -f /var/lib/mysql/ibdata1
```

요렇게 하면 pages-ibdata1 디렉토리에 파일이 여러가지 생성된다.

## 복구가능한 테이블 보기

``` bash
    ./c_parser -4Df pages-ibdata1/FIL_PAGE_INDEX/0000000000000001.page -t dictionary/SYS_TABLES.sql | grep db명

    # db 명이 sample 이면 요렇게
    ./c_parser -4Df pages-ibdata1/FIL_PAGE_INDEX/0000000000000001.page -t dictionary/SYS_TABLES.sql | grep sample
```

## 복구할 테이블 데이터가 있는 인덱스 파일 찾기

``` bash
    ./c_parser -4Df pages-ibdata1/FIL_PAGE_INDEX/0000000000000003.page -t dictionary/SYS_INDEXES.sql | grep 테이블명

    # 테이블 명이 tb_test 면 요렇게
    ./c_parser -4Df pages-ibdata1/FIL_PAGE_INDEX/0000000000000003.page -t dictionary/SYS_INDEXES.sql | grep tb_test

    # 실행하면 요런식으로 나오는데 인덱스 파일 번호는 381 이다.
    00000000CA51    1F000002AF1F53  SYS_INDEXES   379   381   "tb\_test"  1  3  0       4294967295     
    00000000CA51    1F000002AF1F53  SYS_INDEXES   379   381   "tb\_test"  1  3  0       4294967295
```

## 복구용 sql 데이터 및 스크립트 생성하기

tb_test.sql 파일을 생성해서 tb_test 테이블의 CREATE DDL 을 작성한다.

``` bash
    ./c_parser -6f pages-ibdata1/FIL_PAGE_INDEX/인덱스파일번호.page -t "create table 스카마파일" > "생성될 데이터 파일" 2> "데이터 파일 로드용 sql 파일"

    # 위에서 찾은 인덱스 파일 번호가 381 이기 때문에 요렇게
    ./c_parser -6f pages-ibdata1/FIL_PAGE_INDEX/0000000000000381.page -t tb_test.sql > tb_test.tsv 2> tb_test_load.sql
```

## mysql 접속 후 복구하기

``` bash
    mysql --local-infile -uroot -p

    # 디비가 drop 됐기 때문에 디비 생성
    create database sample

    use sample;

    # 데이터를 복구할 테이블을 미리 생성해 둬야 됨, GUI 툴에서 하든 여기서 하든 상관없음


    # 데이터 로드
    source tb_test_load.sql
```