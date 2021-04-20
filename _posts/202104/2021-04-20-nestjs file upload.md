---
layout: post
title: '[NestJS] 파일업로드 & 다운로드 처리(프로젝트 외부 디렉토리에 파일 저장하기)'
keywords: 'nestjs, Multer, 파일업로드'
categories: javascript
tags: javascript nestjs
image: https://images.unsplash.com/photo-1533376351882-bdcabed9b281?ixid=MXwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHw%3D&ixlib=rb-1.2.1&auto=format&fit=crop&w=1650&q=80
image-sm: https://images.unsplash.com/photo-1533376351882-bdcabed9b281?ixid=MXwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHw%3D&ixlib=rb-1.2.1&auto=format&fit=crop&w=1650&q=80
---

## module 및 controller 생성

```bash
nest g mo common/file
nett g co common/file --no-spec
```

## common/file.moudle.ts

기본 설정으로 했을때 파일 업로드시 프로젝트 내부 폴더에 파일이 업로드가 되버려서 파일 관리가 어렵다.

그래서 이왕 하는김에 파일업로드 위치를 설정파일에서 설정하도록 하고,

NestJs ConfigService 로 설정값을 가져다 MulterModule 을 설정할 수 있도록 해 보자.

.env 파일에 요렇게 설정하고

```bash
ATTACH_SAVE_PATH=c:\file\cms
```

NestJs config 설정을 해준다. [NestJs config 설정방법](https://stove99.github.io/javascript/2021/04/15/nestjs-mongo-setting-with-config-service/)은 요글 참고.

※ c:\file\cms 경로는 미리 폴더가 생성되 있어야 된다.

common/file.moudle.ts은 다음과 같다.

```typescript
import { Module } from '@nestjs/common';
import { FileController } from './file.controller';
import { MulterModule } from '@nestjs/platform-express';
import { ConfigModule, ConfigService } from '@nestjs/config';

import { diskStorage } from 'multer';
// 오늘날짜 포맷팅 쉽게 할려고 가볍게 쓸 수 있는것 하나 설치해봄
// npm i light-date
import { format } from 'light-date';
import { extname } from 'path';

import * as fs from 'fs';

@Module({
    imports: [
        // ConfigService 를 inject 하기 위해
        MulterModule.registerAsync({
            imports: [ConfigModule],
            useFactory: async (config: ConfigService) => ({
                storage: diskStorage({
                    destination: function (req, file, cb) {
                        // 파일저장위치 + 년월 에다 업로드 파일을 저장한다.
                        // 요 부분을 원하는 데로 바꾸면 된다.
                        const dest = `${config.get('ATTACH_SAVE_PATH')}/${format(new Date(), '{yyyy}{MM}')}/`;

                        if (!fs.existsSync(dest)) {
                            fs.mkdirSync(dest);
                        }

                        cb(null, dest);
                    },
                    filename: (req, file, cb) => {
                        // 업로드 후 저장되는 파일명을 랜덤하게 업로드 한다.(동일한 파일명을 업로드 됐을경우 오류방지)
                        const randomName = Array(32)
                            .fill(null)
                            .map(() => Math.round(Math.random() * 16).toString(16))
                            .join('');
                        return cb(null, `${randomName}${extname(file.originalname)}`);
                    },
                }),
            }),
            inject: [ConfigService],
        }),
    ],
    controllers: [FileController],
})
export class FileModule {}
```

## common/file.controller.ts

```typescript
import { Controller, Get, Post, Param, UseInterceptors, UploadedFile, Res, Query } from '@nestjs/common';
import { FileInterceptor } from '@nestjs/platform-express';
import { Response } from 'express';

import { ConfigService } from '@nestjs/config';

@Controller('file')
export class FileController {
    constructor(private config: ConfigService) {}

    // 파일 업로드
    // http://localhost:3000/file
    @Post()
    @UseInterceptors(FileInterceptor('file'))
    create(@UploadedFile() file) {
        const path = file.path.replace(this.config.get('ATTACH_SAVE_PATH'), '');

        // 원본파일명과 저장된 파일명을 리턴해서 다운로드 받을때 씀
        return {
            fileName: file.originalname,
            savedPath: path.replace(/\\/gi, '/'),
            size: file.size,
        };
    }

    // 파일 다운로드
    // http://localhost:3000/file/[savedPath]?fn=[fileName]
    // http://localhost:3000/file/202104/12312541515151.xlsx?fn=다운받을원본파일명.xlsx
    @Get(':path/:name')
    async download(@Res() res: Response, @Param('path') path: string, @Param('name') name: string, @Query('fn') fileName) {
        res.download(`${this.config.get('ATTACH_SAVE_PATH')}/${path}/${name}`, fileName);
    }
}
```
