---
layout: post
title: '[CentOS] CentOS 에 Mongdb 설치하고 설정하기'
keywords: 'centos, mongodb'
categories: linux database
image: https://images.unsplash.com/photo-1544383835-bda2bc66a55d?ixid=MXwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHw%3D&ixlib=rb-1.2.1&auto=format&fit=crop&w=1021&q=80
image-sm: https://images.unsplash.com/photo-1544383835-bda2bc66a55d?ixid=MXwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHw%3D&ixlib=rb-1.2.1&auto=format&fit=crop&w=1021&q=80
---

## yum repository 추가

```bash
sudo vi /etc/yum.repos.d/mongodb-org-4.4.repo
```

파일 내용은 다음과 같이 현재 최신 버전인 4.4 버전으로 작성한다.

```bash
[mongodb-org-4.4]
name=MongoDB Repository
baseurl=https://repo.mongodb.org/yum/redhat/$releasever/mongodb-org/4.4/x86_64/
gpgcheck=1
enabled=1
gpgkey=https://www.mongodb.org/static/pgp/server-4.4.asc
```

## yum 으로 설치

```bash
sudo yum install -y mongodb-org
```

## mongo 서버스 실행

```bash
sudo systemctl start mongod.service
```

## 방화벽 오픈하기

외부에서 접속할 수 있도록 방화벽을 오픈한다.

```bash
sudo firewall-cmd --permanent --zone=public --add-service=mongodb

sudo firewall-cmd --reload
```

## mongo 에 접속해서 사용자 추가하기

슈퍼 관리자 느낌의 계정을 하나 추가한다.

```bash
mongo

# 접속 후
use admin

db.createUser(
    {
        user : '계정명',
        pwd : '비밀번호',
        roles: [
            { "role" : "userAdminAnyDatabase", "db" : "admin" },
            { "role" : "dbAdminAnyDatabase", "db" : "admin" },
            { "role" : "readWriteAnyDatabase", "db" : "admin" }
        ]
    }
)

# test
db.auth("계정명", "비밀번호")

exit
```

## mongod.conf 편집

```bash
sudo vi /etc/mongod.conf

# 파일 내용중
net:
  port: 27017
  bindIp: 127.0.0.1

bindIp 를 0.0.0.0 으로 바꾼다. 혹은 특정 아이피만 허용하도록 하려면 허용하고자 하는 ip 들을 , 로 구분해서 적어준다.
ex) bindIp 127.0.0.1,x.x.x.x

# 파일내용중 주석처리된 security: 를 주석해제 하고 아래에 authorization: enabled 을 추가해 준다.
security:
  authorization: enabled

대충 최종적으로
net:
  port: 27017
  bindIp: 127.0.0.1

security:
  authorization: enabled

요런 내용이 들어가면 된다.
```

## mongo service restart

```bash
sudo systemctl restart mongod.service
```

## mongo 에 접속해서 일반 계정과 db 를 만들어 본다.

```bash
mongo

# 접속후
db.createUser({
    user:'계정명',
    pwd:'비밀번호',
    roles: [
        { "role" : "dbAdmin", "db" : "디비명" },
        { "role" : "readWrite", "db" : "디비명" }
    ]
})

# test
db.auth("계정명", "비밀번호")
```

## 접속테스트

compass 같은 접속툴로 접속 테스트를 해 본다.

접속시 connection url

```
mongodb://계정명:비밀번호@193.123.250.200:27017/디비명
```
