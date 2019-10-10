---
layout: post
title: 'typescript import 시 상대경로 대신 절대경로 사용하기'
keywords: 'javascript'
categories: javascript
tags: javascript
---

typescript 프로젝트 개발시 다른 모듈을 import 할때 상대경로를 사용하면 아래처럼 길어질 수 있다.

```javascript
import { AuthService } from '../../../../auth.service';
```

별로 보기에도 좋지 않고 불편한점 있다. 깔꼼하게 아래처럼 쓸 수 있도록 바꿔보자.

```javascript
import { AuthService } from '@services/auth.service';
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

## 프로젝트 구조

<img src="/assets/attach/201910/vscode-path.png" style="width:900px;">

## tsconfig.app.json

```json
{
    "extends": "./tsconfig.json",
    "compilerOptions": {
        "outDir": "./out-tsc/app",
        "types": []
    },
    "include": ["src/**/*.ts"],
    "exclude": ["src/test.ts", "src/**/*.spec.ts"]
}
```

## tsconfig.json

paths 에 필요한 것들을 추가해서 쓰면된다.

```json
{
    "compileOnSave": false,
    "compilerOptions": {
        "baseUrl": "src",
        "paths": {
            "@app/*": ["app/*"],
            "@views/*": ["app/views/*"],
            "@services/*": ["app/services/*"],
            "@env/*": ["environments/*"],
            "@assets/*": ["assets/*"]
        },
        "outDir": "./dist/out-tsc",
        "sourceMap": true,
        "declaration": false,
        "module": "esnext",
        "moduleResolution": "node",
        "emitDecoratorMetadata": true,
        "experimentalDecorators": true,
        "importHelpers": true,
        "target": "es2015",
        "typeRoots": ["node_modules/@types"],
        "lib": ["es2018", "dom"]
    },
    "angularCompilerOptions": {
        "fullTemplateTypeCheck": true,
        "strictInjectionParameters": true
    }
}
```
