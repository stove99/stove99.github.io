---
layout: post
title: '[Node.js] 리눅스에서 yarn global 로 패키지 설치 후 명령어가 실행되지 않을때'
keywords: 'node, yarn, linux'
categories: nodejs
tags: nodejs node_etc
image: https://images.unsplash.com/photo-1506813713591-56fc5e539b80?crop=entropy&cs=tinysrgb&fit=crop&fm=jpg&h=1200&ixid=eyJhcHBfaWQiOjF9&ixlib=rb-1.2.1&q=80&w=2000
image-sm: https://images.unsplash.com/photo-1506813713591-56fc5e539b80?crop=entropy&cs=tinysrgb&fit=crop&fm=jpg&h=1200&ixid=eyJhcHBfaWQiOjF9&ixlib=rb-1.2.1&q=80&w=2000
---

yarn 을 사용해 global 로 패키지 설치 후 설치한 패키지를 커맨드창에서 실행했을때 명령어를 찾을 수 없다면서 command not found 에러가 나는 경우가 있다.

npm i -g 와는 다르게 yarn 을 추가적으로 설정이 살짝 필요하다.

## .bashrc 수정

.bashrc 파일 맨 끝에 PATH 추가

```bash
vi ~/.bashrc

# 맨 끝에 PATH 추가
export PATH="$PATH:`yarn global bin`"

# 저장후 나오기

# 변경내용 적용
source ~/.bashrc
```
