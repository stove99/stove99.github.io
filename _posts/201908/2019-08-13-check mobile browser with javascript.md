---
layout: post
title: 'Javascript 로 모바일 접속여부 판단하기'
keywords: 'javascript, bowser'
categories: javascript
image: https://images.unsplash.com/photo-1466995937966-2e6f29c6ed60?crop=entropy&cs=tinysrgb&fit=crop&fm=jpg&h=1200&ixid=eyJhcHBfaWQiOjF9&ixlib=rb-1.2.1&q=80&w=2000
image-sm: https://images.unsplash.com/photo-1466995937966-2e6f29c6ed60?crop=entropy&cs=tinysrgb&fit=crop&fm=jpg&h=1200&ixid=eyJhcHBfaWQiOjF9&ixlib=rb-1.2.1&q=80&w=2000
---

Javascript 로 mobile 접속여부를 판단해 보자.

착하고 헌신적인 사람이 나같은 사람이 쉽게 쓸 수 있도록 만들어준 라이브러리([bowser](https://github.com/lancedikson/bowser))를 가져다 써 보았다.

요걸쓰면 단순 mobile 접속여부 판단뿐 아니라 User Agent(UA) 분석도 할 수 있다.

script 태그로 불러다 쓸 js 파일이 없어서 직접 살짝 빌드해서 써야 한다.

```bash
git clone https://github.com/lancedikson/bowser.git
cd bowser
npm i
npm run build
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

요렇게 빌드하면 bundled.js 파일이 생성되는데 요 파일을 가져다 쓰면 된다.

빌드하기 귀찮은 사람들을 위해 생성한 [bundled.js](/assets/attach/201908/bundled.js)

## 사용법

js 파일 불러오기

```html
<script src="bundled.js"></script>
```

```javascript
var info = bowser.parse(window.navigator.userAgent);

console.log(JSON.stringify(info));
// {
//      "browser":{
//          "name":"Chrome",
//          "version":"76.0.3809.100"
//      },
//      "os":{
//          "name":"macOS",
//          "version":"10.14.6",
//          "versionName":"Mojave"
//      },
//      "platform":{
//          "type":"desktop",
//          "vendor":"Apple"
//      },
//      "engine":{"name":"Blink"}
//  }

console.log(info.platform.type); // desktop
console.log(info.browser.name); // Chrome

var parser = bowser.getParser(window.navigator.userAgent);
console.log(parser.getPlatformType()); // desktop
console.log(parser.getBrowserName()); // Chrome

console.log(parser.some(['desktop'])); // true
console.log(parser.some(['mobile'])); // false

if (parser.some(['mobile'])) {
    console.log('모바일 브라우저일 경우');
}
```

가타등등 사용법은 <https://lancedikson.github.io/bowser/docs/> 요기로 가서 보아요~
