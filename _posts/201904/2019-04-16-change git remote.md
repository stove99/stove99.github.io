---
layout: post
title:  "Git Remote URL 바꾸기"
date:   2019-04-15
keywords: "git"
categories: [ETC]
tags: [git,remote]
icon: icon-git
---

git 을 사용하다 새 원격 저장소 url 로 바꾸고 소스를 푸쉬하려고 할땐 요렇게

``` bash
    # 현재 설정된 원격 저장소 보기
    git remote -v

    # 신규 원격 저장소로 바꾸기
    git remote set-url origin "바꿀 url"

    # ex
    git remote set-url origin git@github.com:stove99/sample.git

    # push 하기
    git push -u origin --all
    git push -u origin --tags
```

<script async src="//jsfiddle.net/stove/kstq2xou/embed/js,html,css,result/dark/"></script>