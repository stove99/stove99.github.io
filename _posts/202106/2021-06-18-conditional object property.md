---
layout: post
title: '[Javascript] 조건에 따라 Object 에 프로퍼티 추가하기'
keywords: 'javascript'
categories: javascript
image: https://picsum.photos/2000/1200?image=10
image-sm: https://picsum.photos/500/300?image=10
---

어떤 조건이 true 일때 data Object 에 age 프로퍼티를 추가하려면, 조금 없어 보이게 코딩하면 요렇게 할 수 있다.

```javacript
let data = {
  post: '111-222',
  name: 'stove'
};

let condition = true;

if( condition ){
  data['age'] = 18;
}
```

요것을 조금더 있어보이게 할려면 요렇게 하면 된다.

```javacript
let data = {
  post: '111-222',
  name: 'stove',
  ...(condition && { age: 18 })
};
```