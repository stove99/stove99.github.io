---
layout: post
title: '[SCSS] SCSS Spacing(margin&padding) 믹스인'
date: 2019-09-11
keywords: 'scss'
categories: [ETC]
image: https://images.unsplash.com/photo-1480365720180-945d31112159?crop=entropy&cs=tinysrgb&fit=crop&fm=jpg&h=1200&ixid=eyJhcHBfaWQiOjF9&ixlib=rb-1.2.1&q=80&w=2000
image-sm: https://images.unsplash.com/photo-1480365720180-945d31112159?crop=entropy&cs=tinysrgb&fit=crop&fm=jpg&h=1200&ixid=eyJhcHBfaWQiOjF9&ixlib=rb-1.2.1&q=80&w=2000
---

## _mixins.scss

```css
$spacer: 1rem !default;
$spacers: () !default;
// stylelint-disable-next-line scss/dollar-variable-default
$spacers: map-merge(
    (
        0: 0,
        1: (
            $spacer * 0.25
        ),
        2: (
            $spacer * 0.5
        ),
        3: $spacer,
        4: (
            $spacer * 1.5
        ),
        5: (
            $spacer * 3
        )
    ),
    $spacers
);

@function map-get-or-key($map, $key) {
    @if map-has-key($map, $key) or map-has-key($map, -$key) {
        @if $key != 'auto' and type-of($key) == 'number' and $key < 0 {
            @return 0 - map-get($map, -$key);
        } @else {
            @return map-get($map, $key);
        }
    } @else if type-of($key) == 'string' {
        @return unquote($key);
    } @else {
        @return $key;
    }
}

@function bsize($key) {
    @return map-get-or-key($spacers, $key);
}

@mixin m($space) {
    margin: bsize($space);
}

@mixin mt($space) {
    margin-top: bsize($space);
}

@mixin mb($space) {
    margin-bottom: bsize($space);
}

@mixin ml($space) {
    margin-left: bsize($space);
}

@mixin mr($space) {
    margin-right: bsize($space);
}

@mixin p($space) {
    padding: bsize($space);
}

@mixin pt($space) {
    padding-top: bsize($space);
}

@mixin pb($space) {
    padding-bottom: bsize($space);
}

@mixin pl($space) {
    padding-left: bsize($space);
}

@mixin pr($space) {
    padding-right: bsize($space);
}

@mixin mx($space) {
    @include ml($space);
    @include mr($space);
}

@mixin my($space) {
    @include mt($space);
    @include mb($space);
}

@mixin px($space) {
    @include pl($space);
    @include pr($space);
}

@mixin py($space) {
    @include pt($space);
    @include pb($space);
}

.test {
    @include my(1);
}
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

## 사용

### 마이너스

```scss
@include mr(-2);
````

```css
margin-right: -0.5rem;
```

### 패딩 & 마진주기

```scss
@include px(3);
```

```css
padding-left: 1rem;
padding-right: 1rem;
```

```scss
@include m(3);
```

```css
margin: 1rem;
```

### 값지정

```scss
@include my(2.3em);
@include p(3vh);
```

```css
margin-top: 2.3em;
margin-bottom: 2.3em;
padding: 3vh;
```

### Auto

```scss
@include ml(auto);
```

```css
margin-left: auto;
```
