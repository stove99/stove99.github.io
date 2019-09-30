---
layout: post
title: '[Node.js] 리눅스에서 yarn global 로 패키지 설치 후 명령어가 실행되지 않을때'
date: 2019-09-30
keywords: 'node, yarn, linux'
categories: [Node.js]
image: https://images.unsplash.com/photo-1506813713591-56fc5e539b80?crop=entropy&cs=tinysrgb&fit=crop&fm=jpg&h=1200&ixid=eyJhcHBfaWQiOjF9&ixlib=rb-1.2.1&q=80&w=2000
image-sm: https://images.unsplash.com/photo-1506813713591-56fc5e539b80?crop=entropy&cs=tinysrgb&fit=crop&fm=jpg&h=1200&ixid=eyJhcHBfaWQiOjF9&ixlib=rb-1.2.1&q=80&w=2000
---

yarn global 로 패키지 설치 후 명령어를 실행할때 command not found 라면서 실행이 않되는 경우

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
