---
layout: post
title: 'jQuery 에서 arrow function 쓸 때 this 처리'
date: 2019-04-23
keywords: 'javascript,jquery'
categories: [Javascript,jQuery]
image: https://picsum.photos/2000/1200?image=27
image-sm: https://picsum.photos/500/300?image=27
---

arrow function 을 사용하지 않는다면 굳이 이렇게 할 필요는 없겠지만 arrow function 을 쓰면 왠지 뭔가 더 깔끔한거 같고 멋짐이 느껴져서 굳이 이렇게 쓰고 있다.

```javascript
$('#temp_btn').on('click', e => {
    var el = $(this);
    console.log(el);  // Window {postMessage: ƒ, parent: Window, …}
});
```

요렇게 로그를 찍어서 확인해 볼 수 있는데 arrow function 내에서 this 는 Window 객체를 참조한다. 따라서 $(this).attr('href') 요런 코드를 쓰면 원하는 값을 쪽바로 가져오지 못하고 undefined 만 리턴된다.

## 수정

쪽바로 작동하기 위해서는 이벤트 객체의 currentTarget 을 가져다 쓰면 된다.

```javascript
$('#temp_btn').on('click', e => {
    var el = $(e.currentTarget);
    console.log(el);
    console.log(el.attr('href'));
});
```

## each 에서 this 처리

이벤트가 발생하는 경우는 죠렇게 이벤트 객체의 currentTarget 을 가져다 쓰면 되고 이벤트가 발생하지 않는 each() 같은 경우는 요렇게 쓰면 된다.

```javascript
$('a').each((idx, elm) => {
    var el = $(elm);
    console.log(el);
    console.log(el.attr('href'));
});
```