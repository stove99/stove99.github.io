---
layout: post
title: 'heroku 에 일반 html 사이트 올리기'
keywords: 'static html, heroku'
categories: etc
image: https://images.unsplash.com/photo-1553683982-bf97c72e4a1c?crop=entropy&cs=tinysrgb&fit=crop&fm=jpg&h=1200&ixid=eyJhcHBfaWQiOjF9&ixlib=rb-1.2.1&q=80&w=2000
image-sm: https://images.unsplash.com/photo-1553683982-bf97c72e4a1c?crop=entropy&cs=tinysrgb&fit=crop&fm=jpg&h=1200&ixid=eyJhcHBfaWQiOjF9&ixlib=rb-1.2.1&q=80&w=2000
---

정적인 html 웹 사이트를 heroku 에 올려보자.

## 필요한 것들

[heroku](https://www.heroku.com) 계정, heroku cli, git 이 필요하다.

heroku cli 는 <https://devcenter.heroku.com/articles/heroku-cli> 요기로 가서 각자의 환경에 맞게 설치하면 된다. 윈도우인 경우 윈도우용 인스톨러를 다운로드 받아서 설치하면 된다.

또는 node.js 가 설치되 있는 경우 npm 으로도 간단히 설치가 가능하다.

```bash
npm i -g heroku
```

설치후 쪽바로 설치됐나 확인

```bash
heroku -v
```

설치완료 후 heroku login 명령을 입력하면 브라우저 창이 하나 뜨는데 가입한 heroku 아이디로 로그인 하면 완료된다.

```bash
heroku login
```

git 은 <https://git-scm.com/> 요기로 가서 다운로드 받은 다음에 계속 next next 하면 됨.

<ins class="adsbygoogle"
     style="display:block; text-align:center;"
     data-ad-layout="in-article"
     data-ad-format="fluid"
     data-ad-client="ca-pub-7073298118440059"
     data-ad-slot="8400970402"></ins>

<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

## 정적인 html 웹 사이트를 heroku 에 올리기

그냥 냅다 올리면 heroku 가 어떤 언어로 개발된 프로젝트인지 몰라서 쪽바로 구동되지 않는다.

그래서 heroku 에서 지원하는 언어중 하나인 php 프로젝트로 인식되게 처리해 줘야 하는데 방법은 매우 간단하다.

제일 상위 디렉토리에 html 파일로 리다이렉트 시켜주는 index.php 파일만 하나 살짝 맨들어주면 된다.

index.php

```php
<?
    header('Location: /index.html');
?>
```

요러면 모든 준비는 끝나고 heroku cli 와 git 을 이용해서 heroku 에 웹사이트를 올려보자.

```bash
# git 저장소 맨들고 커밋
git init

git add .
git commit -m "최초등록"

# heroku app 생성
heroku create

# heroku 에 내 웹사이트 올리기
git push heroku master

# heroku에 올라간 내 웹사이트 브라우저로 확인하기
heroku open
```

요렇게 하면 간단히 인터넷에 내 웹사이트를 올릴수 있다.

## 수정하고 다시 올릴 때

```bash
git add .
git commit -m "살짝 수정함"

git push heroku master
```

잠시 후 확인해 보면 수정사항이 반영된다.

<ins class="adsbygoogle"
     style="display:block; text-align:center;"
     data-ad-layout="in-article"
     data-ad-format="fluid"
     data-ad-client="ca-pub-7073298118440059"
     data-ad-slot="8400970402"></ins>

<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>