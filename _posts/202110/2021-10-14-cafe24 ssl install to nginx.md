---
layout: post
title: "CAFE24 SSL 인증서 nginx 서버에 설치하기"
keywords: "sshpass,scp"
categories: linux
image: https://picsum.photos/2000/1200?image=874
image-sm: https://picsum.photos/500/300?image=874
---

## ssl 인증서 및 key 다운로드

[https://hosting.cafe24.com/?controller=myservice_ssl_manage&method=down](https://hosting.cafe24.com/?controller=myservice_ssl_manage&method=down)

<img src="/assets/attach/202110/ssl.png">

개인키(ssl.key), 인증서(ssl.crt), 체인인증서(chain_all_ssl.crt)를 다운로드 받는다.

## 인증서 파일 하나로 합치기

nginx 에 설치하게 편하기 위해서 인증서 파일 2개를 하나의 파일(cert.crt)로 합쳐준다. 텍스트 편집기로 합쳐도 되고 cmd 창에서 아래 명령으로 합쳐도 된다.

```bash
FOR %f IN (ssl.crt,chain_all_ssl.crt) DO type %f >> .\cert.crt & echo. >> .\cert.crt
```

## ftp 나 sftp 로 서버에 접속해 ssl.key, cert.crt 파일을 원하는 위치에 업로드 한다.

업로드 후 nginx 설치를 위해 key 파일을 openssl 명령어로 변환해 준다.

```bash
    openssl rsa -in ssl.key -out cert.key
```

## nginx 에 설정

1. ubuntu

```bash
    vi /etc/nginx/site-sites-available/default 혹은 site에 연결된 설정파일

    ### 파일내용
    server {
        server_name www.test.com;  # 사이트 도메인

        location / {
            root /home/ubuntu/test;     # 사이트 루트 디렉토리
            index index.html index.htm;
        }

        listen 443 ssl;

        ssl_certificate /home/ubuntu/ssl/cert.crt;   # 업로드한 cert.crt 파일경로
        ssl_certificate_key /home/ubuntu/ssl/cert.key;   # openssl 명령어로 생성한 cert.key 파일경로

    }
    server {
        # https 로 redirect 처리
        if ($host = www.test.com) {
            return 301 https://$host$request_uri;
        }

        server_name www.test.com;  # 사이트 도메인

        listen 80;

        return 404;
    }
```

2. centos

```bash
    vi /etc/nginx/conf.d/default.conf

    ### 파일내용은 ubuntu 와 동일함
```

## nginx restart

```bash
sudo systemctl restart nginx.service
```
