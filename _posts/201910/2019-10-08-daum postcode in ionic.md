---
layout: post
title: 'ionic(angular) 에서 다음 우편번호 서비스 땡겨다 쓰기'
keywords: 'javascript'
categories: javascript
tags: javascript angular ionic
---

angular 기반 ionic 에서 다음 우편번호 서비스를 땡겨다 써 보자

사용한 ionic, angular 버전은 다음과 같다.

-   ionic : 5.4.2
-   angular : 8.1.2

## postcode.js

아래 postcode.js 파일을 적당한 위치에 추가한다. 나는 src/assets/js/ 에다 추가했다.

```javascript
var script = document.createElement('script');
script.type = 'text/javascript';
script.src = '//t1.daumcdn.net/mapjsapi/bundle/postcode/prod/postcode.v2.js';
script.onload = () => console.log('daum postcode loaded');

/**
 * 스크립트 삽입
 */
var before = document.getElementsByTagName('script')[0];
before.parentNode.insertBefore(script, before);

/**
 *
 * @param {@angular/core/Renderer2} renderer
 * @param {@angular/core/ElementRef.nativeElement} elem
 * @param {주소선택완료시 콜백} callback
 */
export function postcode(renderer, elem, callback) {
    new daum.Postcode({
        oncomplete: data => {
            callback(data);
            elem.style.display = 'none';
        },
        width: '100%',
        height: '100%',
        maxSuggestItems: 5
    }).embed(elem);

    /**
     * 창크기 조정, 팝업창 센터로
     */
    var width = 380;
    var height = 480;
    var borderWidth = 1;

    renderer.setStyle(elem, 'display', 'block');
    renderer.setStyle(elem, 'width', width + 'px');
    renderer.setStyle(elem, 'height', height + 'px');
    renderer.setStyle(elem, 'border', borderWidth + 'px solid');
    renderer.setStyle(elem, 'left', ((window.innerWidth || document.documentElement.clientWidth) - width) / 2 - borderWidth + 'px');
    renderer.setStyle(elem, 'top', ((window.innerHeight || document.documentElement.clientHeight) - height) / 2 - borderWidth + 'px');
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

## 화면 html

다음 우편번호 찾기 UI가 들어갈 div 를 하나 추가한다.

ts 파일에서 접근할 수 있게 #daum_popup 추가

```html
<ion-header>
    <ion-toolbar>
        <ion-title>다음 우편번호 테스트</ion-title>
    </ion-toolbar>
</ion-header>

<ion-content>
    <form [formGroup]="frm">
        <ion-item>
            <ion-label position="floating">자택주소</ion-label>
            <ion-input name="addr1" formControlName="addr1" (click)="openDaumPopup()"></ion-input>
        </ion-item>
        <ion-item>
            <ion-label position="floating">상세주소</ion-label>
            <ion-input name="addr2" formControlName="addr2"></ion-input>
        </ion-item>
    </form>
</ion-content>

<div #daum_popup style="display:none;position:fixed;overflow:hidden;z-index:1;-webkit-overflow-scrolling:touch;">
    <img (click)="closeDaumPopup()" src="//t1.daumcdn.net/postcode/resource/images/close.png" style="cursor:pointer;position:absolute;right:-3px;top:-3px;z-index:1" />
</div>
```

## 로직처리 ts 파일

위에서 추가한 postcode.js 파일 import 후 사용하면 된다.

최근 업데이트된 angular 에서는 ElementRef 객체로 참조하는 element 를 깨작깨작 바꾸지 못해서 @angular/core/Renderer2 로 바꿔야 된다.

```typescript
import { Component, OnInit, ElementRef, ViewChild, Renderer2 } from '@angular/core';
import { FormGroup, FormBuilder } from '@angular/forms';
import { postcode } from 'src/assets/js/postcode.js';

@Component({
    selector: 'daum-sample',
    templateUrl: './daum.sample.page.html',
    styleUrls: ['./daum.sample.page.scss']
})
export class DaumSamplePage implements OnInit {
    frm: FormGroup;

    @ViewChild('daum_popup', { read: ElementRef, static: true }) popup: ElementRef;

    constructor(private formBuilder: FormBuilder, private renderer: Renderer2) {}

    ngOnInit() {
        this.frm = this.formBuilder.group({
            addr1: [''],
            addr2: ['']
        });
    }

    openDaumPopup() {
        postcode(this.renderer, this.popup.nativeElement, data => {
            this.frm.controls.addr1.setValue(data.address);
            console.log(data);
            /*
                {
                    address: "대전 서구 둔산로 100"
                    addressEnglish: "100, Dunsan-ro, Seo-gu, Daejeon, Korea"
                    addressType: "R"
                    apartment: "N"
                    autoJibunAddress: ""
                    autoJibunAddressEnglish: ""
                    autoRoadAddress: ""
                    autoRoadAddressEnglish: ""
                    bcode: "3017011200"
                    bname: "둔산동"
                    bname1: ""
                    bname2: "둔산동"
                    buildingCode: "3017011200114200000021946"
                    buildingName: "대전광역시청"
                    hname: ""
                    jibunAddress: "대전 서구 둔산동 1420"
                    jibunAddressEnglish: "1420, Dunsan-dong, Seo-gu, Daejeon, Korea"
                    noSelected: "N"
                    postcode: "302-789"
                    postcode1: "302"
                    postcode2: "789"
                    postcodeSeq: "007"
                    query: "대전시청"
                    roadAddress: "대전 서구 둔산로 100"
                    roadAddressEnglish: "100, Dunsan-ro, Seo-gu, Daejeon, Korea"
                    roadname: "둔산로"
                    roadnameCode: "3166019"
                    sido: "대전"
                    sigungu: "서구"
                    sigunguCode: "30170"
                    userLanguageType: "K"
                    userSelectedType: "R"
                    zonecode: "35242"
                }
            */
        });
    }

    closeDaumPopup() {
        this.renderer.setStyle(this.popup.nativeElement, 'display', 'none');
    }
}
```
