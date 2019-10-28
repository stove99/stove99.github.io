---
layout: post
title: 'ionic angular 환경에서 카카오맵 API 사용하기'
keywords: 'javascript'
categories: javascript
tags: javascript ionic
---

구글맵 사용시 지역설정을 잘 못하면 동해가 일본해로 표기되고 독도도 리앙쿠르 암초로 표기된다.

그외에도 서해로 표기하고 싶은데 황해로만 표기되는 점도 있다.

이런 문제를 해결하기 위해 구글맵 대신에 카카오맵을 사용해 보자.

<ins class="adsbygoogle"
     style="display:block; text-align:center;"
     data-ad-layout="in-article"
     data-ad-format="fluid"
     data-ad-client="ca-pub-7073298118440059"
     data-ad-slot="8400970402"></ins>

<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

## kakao 개발자 사이트에서 앱 생성 후 플랫폼 추가 및 설정

설정 - 일반 - 플랫폼 추가

1. 웹
2. 사이트 도메인 : file:// , http://localhost:8100, http://localhost 도메인 3개 추가
3. 추가

지도 API 사용시 사용할 키는 JavaScript키를 사용하면 된다.

## app/src/index.html 에 js 불러오는 부분 추가

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="utf-8" />
        <title>My APP</title>

        <base href="/" />

        <meta
            name="viewport"
            content="viewport-fit=cover, width=device-width, initial-scale=1.0,
                 minimum-scale=1.0, maximum-scale=1.0, user-scalable=no"
        />
        <meta name="format-detection" content="telephone=no" />
        <meta name="msapplication-tap-highlight" content="no" />

        <link rel="icon" type="image/png" href="assets/icon/favicon.png" />

        <meta name="apple-mobile-web-app-capable" content="yes" />
        <meta name="apple-mobile-web-app-status-bar-style" content="black" />

        <script type="text/javascript"
            src="https://dapi.kakao.com/v2/maps/sdk.js?appkey=javascript키"></script>
        <script type="text/javascript"
            src="https://dapi.kakao.com/v2/maps/sdk.js?appkey=javascript키&libraries=LIBRARY"></script>
    </head>

    <body>
        <app-root></app-root>
    </body>
</html>
```

## resources/android/xml/network_security_config.xml 파일편집

안드로이드에서 외부 도메인에 있는 자원을 불러오려면 network_security_config.xml 파일을 조금 편집해야 된다.

```html
<?xml version="1.0" encoding="utf-8"?>
<network-security-config>
    <domain-config cleartextTrafficPermitted="true">
        <domain includeSubdomains="true">localhost</domain>
        <!-- API -->
        <domain includeSubdomains="true">dapi.kakao.com</domain>
        <!-- map images -->
        <domain includeSubdomains="true">daumcdn.net</domain>
    </domain-config>
</network-security-config>
```

## map page 맨들기

```bash
ionic g page map
```

### app/map/map.page.html

```html
<ion-header>
    <ion-toolbar>
        <ion-title>다음맵</ion-title>
    </ion-toolbar>
</ion-header>

<ion-content>
    <div id="map" class="map"></div>
</ion-content>
```

### app/map/map.page.scss

```scss
.map {
    width: 100vw;
    height: 80vh;
    border: 1px solid #b8b8b8;
    background-size: cover;
}
```

### app/map/map.page.ts

```typescript
import { Component, OnInit } from '@angular/core';

// kakao map api 사용하기
declare var kakao;

@Component({
    selector: 'app-map',
    templateUrl: './map.page.html',
    styleUrls: ['./map.page.scss']
})
export class MapPage implements OnInit {
    map: any;
    infowindow: any;

    // 마커들
    places = [
        { name: '공대2호관 쪽문나가는길', lat: 36.363775, lon: 127.346709 },
        { name: '공대2호관 뒷길(초등학교사이)', lat: 36.364176, lon: 127.347213 },
        { name: '체육관 좌측', lat: 36.371124, lon: 127.341466 },
        { name: '박물관뒤 주차장', lat: 36.370845, lon: 127.345263 }
    ];

    constructor() {}

    ngOnInit() {
        // 살짝 딜레이 줘야 화면에 맵에 쪽바로 그려진다.
        setTimeout(() => {
            const options = {
                center: new kakao.maps.LatLng(36.370546, 127.345966),
                level: 3
            };

            this.map = new kakao.maps.Map(document.getElementById('map'), options);

            this.infowindow = new kakao.maps.InfoWindow({
                map: this.map,
                zIndex: 4,
                position: new kakao.maps.LatLng(36.370546, 127.345966),
                removable: true
            });

            this.addPlaces();
        }, 300);
    }

    addPlaces() {
        this.places.forEach(place => {
            const image = new kakao.maps.MarkerImage(
                'assets/img/marker.png',
                new kakao.maps.Size(64, 69),
                { offset: new kakao.maps.Point(27, 69) }
            );

            const marker = new kakao.maps.Marker({
                clickable: true,
                position: new kakao.maps.LatLng(place.lat, place.lon),
                image
            });

            marker.setMap(this.map);

            // 클릭했을때 마커위에 인포윈도우 그리기
            kakao.maps.event.addListener(marker, 'click', () => {
                this.infowindow.setContent(`<div style="padding:5px;width:250px;">${place.name}</div>`);
                this.infowindow.open(this.map, marker);
            });

            this.infowindow.setContent(`<div style="padding:5px;width:250px;">${place.name}</div>`);
            this.infowindow.open(this.map, marker);
        });
    }
}
```
