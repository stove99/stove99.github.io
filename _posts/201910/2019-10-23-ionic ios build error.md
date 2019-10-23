---
layout: post
title: 'ionic ios build 시 Cannot read property 'toLowerCase' of undefined 오류'
keywords: 'javascript'
categories: javascript
tags: javascript ionic
---

Mac OS 랑 XCODE 를 최신버전으로 업데이트 하고 기존에 빌드하던 앱을 빌드해보니 역시 에러가 났다.

업데이트만 했다 하면 등에서 땀나게 오류가 난다.

오류가 나는 이유는 내 생각이지만 cordova-ios 버전이 낮아서 그런것 같다. 그래서 cordova-ios 만 최신버전으로 업데이트 할려니까 또 등에서 땀나게 오류가 나면서 쪽바로 안됐다.

살짝 등에서 땀을 조금더 쥐어짜낸 다음에 찾아낸 해결방법은 플랫폼을 지웠다가 다시 추가하면 된다는 것이다.

<ins class="adsbygoogle"
     style="display:block; text-align:center;"
     data-ad-layout="in-article"
     data-ad-format="fluid"
     data-ad-client="ca-pub-7073298118440059"
     data-ad-slot="8400970402"></ins>

<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

```bash
# platform 제거
ionic cordova platform remove ios

# platform 다시 추가
ionic cordova platform add ios@latest

# 빌드
ionic cordova build ios
```

매번 OS 혹은 XCODE 를 업데이트 할때마다 느끼는 거지만 업데이트는 등에 땀이 나게 만든다. 바들바들
