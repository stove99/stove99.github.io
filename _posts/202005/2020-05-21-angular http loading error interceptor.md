---
layout: post
title: '[Angular] Interceptor 로 API 호출할 때마다 로딩 에니메이션 보여주기 & 전역 에러 처리'
keywords: 'javascript'
categories: javascript
tags: javascript angular
---

로딩 에니메이션 있어 보이게 보여줄 라이브러리는 ngx-spinner 를 사용했고 에러 메시지를 있어 보이게 보여줄 라이브러리는 ngx-toastr 를 사용했다.

## 패키지 설치하기

```bash
npm i ngx-spinner ngx-toastr

# 또는
yarn add ngx-spinner ngx-toastr
```

## ngx-spinner 설정

[https://www.npmjs.com/package/ngx-spinner](https://www.npmjs.com/package/ngx-spinner) 여기에 적힌대로 따라하면 된다.

### AppModule

AppModule 에 NgxSpinnerModule 을 추가한다.

```typescript
import { NgxSpinnerModule } from 'ngx-spinner';

@NgModule({
    imports: [
        // ...
        NgxSpinnerModule,
    ],
})
export class AppModule {}
```

### src/app/app.component.html

app.component.html 맨 아래에다 추가

로딩 에니메이션 모양은 [https://github.danielcardoso.net/load-awesome/animations.html](https://github.danielcardoso.net/load-awesome/animations.html) 여기 가서 원하는 모양을 찾아보고 바꿔주면 된다.

la-ball-rotate 처럼 la-xxxx 요런식으로 클래스가 있는데 앞에있는 la- 를 지우고 type 속성에다 적어주면 된다.

```html
<ngx-spinner bdColor="rgba(255,255,255,0.8)" color="#f4696b" type="square-jelly-box"></ngx-spinner>
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

## ngx-toastr 설정

요것도 마찬가지로 [홈페이지](https://www.npmjs.com/package/ngx-toastr)에 친절하게 설명되 있으니 여기에 적힌대로 따라하면 된다.

### angular.json

angular.json css 에 추가

```json
    ...
    "build": {
        ...
        "styles": ["src/styles.sass", "node_modules/ngx-toastr/toastr.css"],
        "scripts": []
    },
```

### AppModule

AppModule 에 BrowserAnimationsModule, ToastrModule 추가

```typescript
import { BrowserAnimationsModule } from '@angular/platform-browser/animations';

import { ToastrModule } from 'ngx-toastr';

@NgModule({
    imports: [
        // ...
        BrowserAnimationsModule,
        ToastrModule.forRoot(),
    ],
    // ...
})
class AppModule {}
```

요기까지는 라이브러리 설정하는 것이고 지금부터 인터셉터를 맹글어 보자. ng g interceptor 명령으로 인터셉터를 편리하게 맨들 수 있다.

## loading, error interceptor 추가

```bash
ng g interceptor common/interceptor/loading
ng g interceptor common/interceptor/error
```

### AppModule 에 인터셉터 설정

```typescript
import { LoadingInterceptor } from './common/interceptor/loading.interceptor';
import { ErrorInterceptor } from './common/interceptor/error.interceptor';

@NgModule({
    // ...
    providers: [
        // 요기 순서대로 인터셉터 실행됨
        { provide: HTTP_INTERCEPTORS, useClass: LoadingInterceptor, multi: true },
        { provide: HTTP_INTERCEPTORS, useClass: ErrorInterceptor, multi: true },
    ],
    // ...
})
export class AppModule {}
```

### src/app/common/interceptor/loading.interceptor.ts (로딩 에니메이션 출력 인터셉터)

```typescript
import { Injectable } from '@angular/core';
import { HttpRequest, HttpHandler, HttpEvent, HttpInterceptor } from '@angular/common/http';
import { Observable } from 'rxjs';
import { finalize, delay } from 'rxjs/operators';
import { NgxSpinnerService } from 'ngx-spinner';

/**
 * API 호출할때 로딩 에니메이션 처리 인터셉터
 */
@Injectable()
export class LoadingInterceptor implements HttpInterceptor {
    activated = 0;

    constructor(private spinner: NgxSpinnerService) {}

    intercept(request: HttpRequest<unknown>, next: HttpHandler): Observable<HttpEvent<unknown>> {
        if (this.activated === 0) {
            console.log('로딩 똥그라미 출력');
            this.spinner.show();
        }

        this.activated++;

        return next.handle(request).pipe(
            delay(4000), // 테스트용 delay 5초
            finalize(() => {
                this.activated--;
                if (this.activated === 0) {
                    console.log('로딩 똥그라미 지우기');
                    this.spinner.hide();
                }
            })
        );
    }
}
```

### src/app/common/interceptor/error.interceptor.ts (전역 에러처리 인터셉터)

```typescript
import { Injectable } from '@angular/core';
import { HttpRequest, HttpHandler, HttpEvent, HttpInterceptor, HttpErrorResponse } from '@angular/common/http';
import { Observable, throwError } from 'rxjs';
import { catchError } from 'rxjs/operators';
import { ToastrService } from 'ngx-toastr';

/**
 * 에러 처리 인터셉터
 */
@Injectable()
export class ErrorInterceptor implements HttpInterceptor {
    constructor(private toastr: ToastrService) {}

    intercept(request: HttpRequest<unknown>, next: HttpHandler): Observable<HttpEvent<unknown>> {
        return next.handle(request).pipe(
            catchError((error: HttpErrorResponse) => {
                console.log(error);

                this.toastr.error('에러 났어염', 'Error');
                // this.toastr.error(error.message, 'Error');

                return throwError(error);
            })
        );
    }
}
```

전체 소스는 [깃허브](https://github.com/stove99/angular-http-loading-error-interceptor){:target="\_blank"}에 있어염~~

결과물 : [https://stove99.github.io/angular-http-loading-error-interceptor/](https://stove99.github.io/angular-http-loading-error-interceptor/){:target="\_blank"}
