---
layout: post
title: 'CentOS7 + nginx 에 Lets Encrypt 무료인증서 설치 & 설정'
keywords: 'centos,ssl,nginx'
categories: linux
image: https://images.unsplash.com/photo-1561399250-9d86b9ee228c?crop=entropy&cs=tinysrgb&fit=crop&fm=jpg&h=1200&ixid=eyJhcHBfaWQiOjF9&ixlib=rb-1.2.1&q=80&w=2000
image-sm: https://images.unsplash.com/photo-1561399250-9d86b9ee228c?crop=entropy&cs=tinysrgb&fit=crop&fm=jpg&h=1200&ixid=eyJhcHBfaWQiOjF9&ixlib=rb-1.2.1&q=80&w=2000
---

## YUM epel 저장소 추가

```bash
yum install epel-release
```

## certbot 설치

CLI 로 인증서를 생성 및 관리할 수 있도록 해주는 프로그램이다.

```bash
yum install certbot
```

## 443 포트 방화벽 오픈

```bash
firewall-cmd --permanent --add-service=https
firewall-cmd --reload
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

## nginx stop

--standalone 옵션으로 인증서 생성시 오류가 나기 때문에 잠깐 nginx 를 끈다.

```bash
service nginx stop
```

## 인증서 생성

```bash
certbot certonly --standalone -d 생성할 도메인

# ex
certbot certonly --standalone -d stove99.gihub.io
```

이메일 입력하라 그라고 이것저것 몇 번 묻는데 다 y 로 하면 된다.

작업이 성공적으로 끝나면 인증서는 /etc/letsencrypt/live/도메인명 에 생성된다.

## nginx 설정

예를들어 stove99.github.io 로 인증서를 발급받았다고 가정함.

```nginx
#http 로 접속할 경우 https 로 리다이렉트 시킴
server {
    listen       80;
    server_name  stove99.gihub.io;
    return 301 https://$host$request_uri;
}

server {
    listen 443 ssl;
    server_name stove99.gihub.io;

    ssl_certificate /etc/letsencrypt/live/stove99.gihub.io/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/stove99.gihub.io/privkey.pem;

    location / {
        ...
    }
}
```

## nginx start

```bash
service nginx start
```

## 인증서 갱신 처리

인증서 유효기간이 3개월이기 때문에 인증서를 한 달에 한번 정도 갱신하도록 crontab 에 등록시킨다.

crontab 에 등록하기전에 먼저 인증서 갱신시 nginx 가 리스타트 되도록 설정이 필요하다.

```bash
vi /etc/letsencrypt/renewal/stove99.gihub.io.conf

## [renewalparams] 섹션에 pre_hook, post_hook 설정 추가
[renewalparams]
...
...

pre_hook = systemctl stop nginx
post_hook = systemctl start nginx

...
...
```

crontab 에 갱신작업 등록

```bash
crontab -e

## 한달에 한번씩 인증서가 갱신되도록 작업 추가
0 0 1 * * /bin/bash -l -c 'certbot renew --quiet'
```
