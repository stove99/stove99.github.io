---
layout: post
title: '[NestJS] @nestjs/config ConfigService 로 MongDB 설정하기'
keywords: 'nestjs, mongo'
categories: nodejs
tags: nodejs nestjs
image: https://images.unsplash.com/photo-1533376351882-bdcabed9b281?ixid=MXwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHw%3D&ixlib=rb-1.2.1&auto=format&fit=crop&w=1650&q=80
image-sm: https://images.unsplash.com/photo-1533376351882-bdcabed9b281?ixid=MXwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHw%3D&ixlib=rb-1.2.1&auto=format&fit=crop&w=1650&q=80
---

## @nestjs/config package 설치

```bash
npm i @nestjs/config

또는

yarn add @nestjs/config
```

## .env , .env.production 파일 만들기

프로젝트 루트에 .env(개발용), .env.production(운영용) 파일을 만들어서 필요한 환경변수를 작성한다.
굳이 루트에 만들지 않고 따로 config 같은 폴더를 만들어서 그 안에 파일을 생성해도
아래 AppModule 설정 중 envFilePath 를 살짝 바꿔서 할 수 있다.

```bash
DB_URL=mongodb+srv://아이디:암호@도메인/디비명
```

디폴트는 .env 를 사용하고 운영서버에는 NODE_ENV 환경변수에 production 으로 설정해 둔다.

```bash
# 윈도우 PowerSehll 에서 환경변수 설정하기
$env:NODE_ENV="production"

# 윈도우 커맨드창에서 환경변수 설정하기
set NODE_ENV=production

# 리눅스에서 환경변수 설정하기
export NODE_ENV=production
```

## app.moudle.ts 파일 설정하기

```typescript
import { Module } from '@nestjs/common';
import { ConfigModule, ConfigService } from '@nestjs/config';
import { MongooseModule } from '@nestjs/mongoose';
import { AppController } from './app.controller';
import { AppService } from './app.service';

@Module({
    imports: [
        ConfigModule.forRoot({
            // 다른 모듈에서 imports 에 등록 안해도 constructor(config: ConfigService){} 이렇게 가져다 쓸 수 있도록 한다.
            isGlobal: true,
            // NODE_ENV 값이 production 일때 .env.production 파일을 읽도록 설정
            envFilePath: `.env${process.env.NODE_ENV ? `.${process.env.NODE_ENV}` : ''}`,
        }),
        // ConfigService 를 사용하기 위해 일반적으로 사용하는 MongooseModule.forRoot 대신 MongooseModule.forRootAsync 로 모듈을 가져온다.
        MongooseModule.forRootAsync({
            imports: [ConfigModule],
            useFactory: async (config: ConfigService) => ({
                uri: config.get('DB_URL'),
                useNewUrlParser: true,
                useUnifiedTopology: true,
            }),
            inject: [ConfigService],
        }),
    ],
    controllers: [AppController],
    providers: [AppService],
})
export class AppModule {}
```
