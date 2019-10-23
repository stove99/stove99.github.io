---
layout: post
title: '[javascript] object, string, yaml, buffer 변환하기'
keywords: 'javascript'
categories: javascript
tags: javascript
---

Object 를 string, yaml, buffer 로 변환하기

## Object <-> String

```javascript
const obj = {
    id: 'stove99',
    rel: {
        friends: ['hd', 'er']
    }
};

// object <-> string
const json_str = JSON.stringify(obj);
console.log(json_str); // {"id":"stove99","rel":{"friends":["hd","er"]}}
console.log(JSON.parse(json_str)); // { id: 'stove99', rel: { friends: [ 'hd', 'er' ] } }
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

## Object <-> Buffer

```bash
npm i bson

# 또는
yarn add bson
```

```javascript
import * as bson from 'bson';

const obj = {
    id: 'stove99',
    rel: {
        friends: ['hd', 'er']
    }
};

const buf = bson.serialize(obj);
console.log(buf); // <Buffer 41 00 00 00 02 69 64 00 08 ....>
console.log(bson.deserialize(buf)); // { id: 'stove99', rel: { friends: [ 'hd', 'er' ] } }
```

## Object <-> yaml

```bash
npm i yaml

# 또는
yarn add yaml
```

```javascript
import * as yaml from 'yaml';

const obj = {
    id: 'stove99',
    rel: {
        friends: ['hd', 'er']
    }
};

const yaml_str = yaml.stringify(obj);
console.log(yaml_str);
/*
id: stove99
rel:
  friends:
    - hd
    - er
*/
console.log(yaml.parse(yaml_str)); // { id: 'stove99', rel: { friends: [ 'hd', 'er' ] } }
```
