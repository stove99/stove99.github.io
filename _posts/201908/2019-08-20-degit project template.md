---
layout: post
title: 'degit 으로 git 에 있는 템플릿 프로젝트 받아오기'
date: 2019-08-20
keywords: 'degit'
categories: [ETC]
image: https://images.unsplash.com/photo-1440778303588-435521a205bc?crop=entropy&cs=tinysrgb&fit=crop&fm=jpg&h=1200&ixid=eyJhcHBfaWQiOjF9&ixlib=rb-1.2.1&q=80&w=2000
image-sm: https://images.unsplash.com/photo-1440778303588-435521a205bc?crop=entropy&cs=tinysrgb&fit=crop&fm=jpg&h=1200&ixid=eyJhcHBfaWQiOjF9&ixlib=rb-1.2.1&q=80&w=2000
---

git 으로 clone 하면 되지만 clone 했을경우 remote 저장소 정보가 고대로 남아있고 history 도 고대로 남아 있어서 고런것들 지울려면 이것저것 해야 되기 때문에 조금 귀찮다.

degit 으로 git 에 있는 프로젝트를 클론(?) 하면 저장소 정보가 싹 지워진 깔꼼한 프로젝트가 생성된다.

## 설치

설치를 위해서 node 가 먼저 설치되 있어야 한다.

```bash
npm i -g degit
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

## 사용법

git clone 처럼 똑같이 쓰면 된다.

```bash
degit [클론받을 프로젝트 url] [다운받을 디렉토리명]
```

예를 들어 sveltejs 템플릿 프로젝트를 받을려면 요렇게 하면 된다.

```bash
degit https://github.com/sveltejs/template.git my-app

cd my-app

# git repo init
git init
git add .
git commit -m "처음 커밋"
```
