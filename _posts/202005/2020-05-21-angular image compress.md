---
layout: post
title: '[Angular] 클라이언트 사이드에서 이미지 사이즈 줄이고 업로드 하기'
keywords: 'javascript'
categories: javascript
tags: javascript angular
---

ngx-image-compress 를 사용해 이미지 사이즈를 조절할 수 있다.

## 패키지 설치하기

```bash
npm i ngx-image-compress

# 또는
yarn add ngx-image-compress
```

## src/app/app.module.ts

app.module.ts 프로바이더에 NgxImageCompressService 추가하기

Reactive Forms 으로 화면 맨들거라 FormsModule, ReactiveFormsModule 도 imports 해 줌

```typescript
import { FormsModule, ReactiveFormsModule } from '@angular/forms';
import { NgxImageCompressService } from 'ngx-image-compress';

@NgModule({
    declarations: [AppComponent],
    imports: [BrowserModule, FormsModule, ReactiveFormsModule],
    providers: [NgxImageCompressService],
    bootstrap: [AppComponent],
})
export class AppModule {}
```

## src/app/app.component.html (UI)

bootstrap 으로 화면을 구성해 봄

```html
<div class="container">
    <form class="my-3" [formGroup]="form">
        <div class="form-row">
            <div class="col">
                <label>비율 {{ form.value.ratio }}%</label>
                <input type="range" class="custom-range" min="0" max="200" step="1" formControlName="ratio" />
            </div>
            <div class="col">
                <label>퀄리티 {{ form.value.quality }}%</label>
                <input type="range" class="custom-range" min="0" max="200" step="1" formControlName="quality" />
            </div>
        </div>

        <div class="form-row">
            <div class="col">
                <label>이미지 파일</label>
                <div class="custom-file">
                    <input type="file" class="custom-file-input" (change)="selectFile($event)" />
                    <label class="custom-file-label" accept=".jpg,.png,.jpeg">Choose file</label>
                </div>
            </div>
        </div>
    </form>

    <div *ngIf="img" class="card" style="width: 18rem;">
        <img [src]="img" class="card-img-top" alt="원본" />
        <div class="card-body">
            <h5 class="card-title">원본 이미지</h5>
            <p class="card-text">File Size : {{ imgSize | number: "1.2-2" }}MB</p>
        </div>
    </div>

    <div *ngIf="convertImg" class="my-3 p-3 border shadow">
        <p>File Size : {{ convertImgSize | number: "1.2-2" }}MB</p>
        <img [src]="convertImg" alt="압축" />
    </div>
</div>
```

※ angular 프로젝트에 bootstrap 적용 할려면

```bash
npm i bootstrap

# 또는
yarn add bootstrap
```

요렇게 설치한 다음 angular.json architect/build/options/styles 에다 bootstrap.min.css 추가해 주면 된다.

```json
...
"styles": [
    "src/styles.sass",
    "node_modules/bootstrap/dist/css/bootstrap.min.css"
],
...

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

## src/app/app.component.ts (로직)

constructor 로 NgxImageCompressService 를 주입

```typescript
    constructor(
        private compressor: NgxImageCompressService,
        private fb: FormBuilder
    ) {}
```

파일 선택시 이미지 사이즈 조절하기

```typescript
    form = this.fb.group({
        ratio: [100],
        quality: [50]
    });

    img: string;

    imgSize: number;

    convertImg: string;

    convertImgSize: number;

    selectFile(e: any) {
        if (e.target.files) {
            const file = e.target.files[0];
            const reader = new FileReader();

            reader.onload = async (event: any) => {
                // base64 image
                this.img = event.target.result;
                this.imgSize = this.compressor.byteCount(this.img) / (1024 * 1024);

                const compressed = await this.compress({ data: this.img, ...this.form.value });
                this.convertImg = compressed.img;
                this.convertImgSize = compressed.size;
            };

            reader.readAsDataURL(file);
        }
    }

    async compress({ data, ratio, quality }: { data: string, ratio?: number, quality?: number }) {
        console.log('ratio : ', ratio, ' quality : ', quality);
        const result = await this.compressor.compressFile(data, -1, ratio, quality);

        return { img: result, size: this.compressor.byteCount(result) / (1024 * 1024) };
    }
```

전체 소스는 [깃허브](https://github.com/stove99/angular-image-compress-example){:target="\_blank"}에 있어염~~

결과물 : [https://stove99.github.io/angular-image-compress-example/](https://stove99.github.io/angular-image-compress-example/){:target="\_blank"}
