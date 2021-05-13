---
layout: post
title: '노드로 간단하게 웹 스크랩핑 하기'
keywords: 'nestjs, cheerio, scraping'
categories: nodejs
tags: nodejs
image: https://images.unsplash.com/photo-1533376351882-bdcabed9b281?ixid=MXwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHw%3D&ixlib=rb-1.2.1&auto=format&fit=crop&w=1650&q=80
image-sm: https://images.unsplash.com/photo-1533376351882-bdcabed9b281?ixid=MXwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHw%3D&ixlib=rb-1.2.1&auto=format&fit=crop&w=1650&q=80
---

간단하게 뽐뿌게시판 핫게시글 스크랩핑 예제

## 필요한 모듈 설치

```bash
yarn add axios cheerio iconv-lite
```

## 소스

```javascript
const axios = require('axios');
const cheerio = require('cheerio');
const iconv = require('iconv-lite');

async function scrape() {
    // 뽐뿌게시판 핫게시글 1페이지 url
    const response = await axios.get('http://www.ppomppu.co.kr/zboard/zboard.php?id=ppomppu&page=1&hotlist_flag=999&divpage=66', 
        { responseEncoding: 'binary', responseType: 'arraybuffer' });

    // 대상 페이지 인코딩이 요즘 거의 볼 수 없는 EUC-KR 이라서 별도 처리가 필요하다. UTF-8 이면 필요없음
    const html = iconv.decode(response.data, 'euc-kr').toString();

    const $ = cheerio.load(html);

    // 핫게시글 추출 #revolution_main_table 테이블 중 class 가 list1, list0 인 tr
    const hot = $('#revolution_main_table .list1,.list0')
        .map((_, element) => {
            const tr = $(element);

            const a = tr.find('td').eq(3).find('a');

            return {
                link: `http://www.ppomppu.co.kr/zboard/${a.attr('href')}`,
                title: a.text(),
            };
        })
        .get();

    return hot;
}

scrape().then((result) => {
    console.log(result);
});

```