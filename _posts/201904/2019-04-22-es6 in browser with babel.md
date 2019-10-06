---
layout: post
title: 'IE 에서 ES6 구문 사용하기'
keywords: 'javascript,babel'
categories: javascript
image: https://picsum.photos/2000/1200?image=20
image-sm: https://picsum.photos/500/300?image=20
---

일반적인 웹환경에서 자신도 모르게 ES6 구문을 사용해서 IE에서 쪽바로 작동안되는 경우가 종종 발생한다. 이런 오류를 방지하기 위해서 babel-standalone, babel-polyfill 을 사용하면 된다.

## 설정

babel-standalone, babel-polyfill js 두개만 불러오면 끝.

```html
<script src="//cdnjs.cloudflare.com/ajax/libs/babel-standalone/6.26.0/babel.min.js"></script>
<script src="//cdnjs.cloudflare.com/ajax/libs/babel-polyfill/7.4.3/polyfill.js"></script>
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

## es6 사용하기

### script type 에 text/babel 을 설정하면 es6 구문을 사용할 수 있다.

```html
<script type="text/babel" data-presets="es2015, stage-3">
    // 요기다 코딩하면 됨
</script>
```

### 다른 js 파일을 불러올려면 요렇게 하면 된다.

```html
<script src="./main.js" type="text/babel" data-presets="es2015, stage-3"></script>
```

### main.js

```javascript
var users = { stove: { name: '홍길동' }, test: { name: '김철수' } };

// async, await 테스트
process();

async function process() {
    var id = await getId();
    console.log('User ID: ' + id);

    var name = await getUserName(id);
    console.log('User Name: ' + name);
}

function getId() {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            console.log('calling');
            resolve('stove');
        }, 2000);
    });
}

function getUserName(id) {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            console.log('requesting user name with id: ' + id);
            resolve(users[id].name);
        }, 3000);
    });
}

// forEach, arrow function
const arr = [1, 2, 3, 4, 5, 6];

arr.forEach(i => console.log(i));

// Destructuring
const { stove, test } = users;
console.log(stove); // {name : '홍길돌'}
console.log(test); // {name : '김철수'}

const a = 1;
const b = 'sample';

const obj = { a, b };
console.log(obj); // {a: 1, b: "sample"}

// string
console.log(`my name is ${stove.name}`); // my name is 홍길동
```

IE에서 테스트 해 본 결과 IE9까지는 깔꼼하게 잘되는 것 같다. 끝.

<ins class="adsbygoogle"
     style="display:block; text-align:center;"
     data-ad-layout="in-article"
     data-ad-format="fluid"
     data-ad-client="ca-pub-7073298118440059"
     data-ad-slot="8400970402"></ins>

<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>
