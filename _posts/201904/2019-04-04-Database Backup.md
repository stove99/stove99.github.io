---
layout: post
title:  "crontab 을 활용한 oracle, mysql 주기적으로 백업하기"
date:   2019-04-04
keywords: "mysql,oracle,backup,crontab"
categories: [Linux]
tags: [mysql,oracle,backup,crontab]
icon: icon-mysql
image: https://picsum.photos/2000/1200?random
image-sm: https://picsum.photos/500/300?random
---

## 백업을 수행할 shell script 작성

Mysql 용 백업 스크립트는 대충 요렇게 맹글어 본다.

``` bash
    mkdir ~/cron
    vi ~/cron/mysql_backup.sh

    # mysql_backup.sh 내용
    #######################################################################################
    # 요렇게 하면 /home/BACKUP/db_오늘날짜.sql 로 백업이된다.
    /usr/bin/mysqldump -uroot -p비밀번호 백업할db명 > /home/BACKUP/db_`date +%Y%m%d`.sql

    # 5일까지꺼만 백업파일 유지되게
    /usr/bin/find /home/BACKUP/* -mtime +5 -exec rm -rf {} \;
    #######################################################################################

    # script 실행권한 추가
    chmod u+x ~/mysql_backup.sh
```

oracle 용 백업스크립트는 요렇게.. exp 라는 걸 활용하면 되는데 오라클 설치할때 같이 설치됨

``` bash
    mkdir ~/cron
    vi ~/cron/oracle_backup.sh

    # oracle_backup.sh 내용
    # oracle 설치경로에 맞춰서 알아서 바꾸길..
    #######################################################################################
    /oracle/app/oracle/product/11.2.0/db/bin/exp 계정명/비밀번호 file=/home/BACKUP/db_`date +%Y%m%d`.dmp log=/home/BACKUP/backup.log

    # 5일까지꺼만 백업파일 유지되게
    /usr/bin/find /home/BACKUP/* -mtime +5 -exec rm -rf {} \;
    #######################################################################################

    # script 실행권한 추가
    chmod u+x ~/oracle_backup.sh
```

exp 를 실행해 보면 ORACLE_HOME 설정하라 그러면서 실행이 안되는 경우가 있는데 고럴땐 ORACLE_HOME, ORACLE_SID 를 환경변수에 등록해 주면 된다.

``` bash
    vi ~/.bash_profile

    # .bash_profile 에 환경변수 추가
    #######################################################################################
    export ORACLE_HOME=/oracle/app/oracle/product/11.2.0/db
    export ORACLE_SID=SID 명 또는 Service Name
    #######################################################################################

    # 적용
    source ~/.bash_profile
```

## 스크립트 테스트

``` bash
    ~/cron/mysql_backup.sh
```

실행해서 정상적으로 백업파일이 생성되는지 확인해 본다.

## crontab 에 등록해서 주기적으로 백업받기

``` bash
    crontab -e

    # 매일 새벽 3시에 백업돌리기
    0 3 * * * ~/cron/mysql_backup.sh
```
