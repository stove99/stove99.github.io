---
layout: post
title: 'ionic(angular) 채팅창 하단으로 스크롤 하기'
keywords: 'javascript'
categories: javascript
tags: javascript angular ionic
---

채팅창에서 메시지가 입력됐을때 화면 제일 하단으로 스크롤 하기

-   ionic : 5.4.2
-   angular : 8.1.2

## 화면 html

```html
<ion-header>
    <ion-toolbar>
        <ion-title>채팅방</ion-title>
        <ion-buttons slot="start">
            <ion-button>
                <ion-icon slot="icon-only" name="arrow-back"></ion-icon>
            </ion-button>
        </ion-buttons>
    </ion-toolbar>
</ion-header>

<ion-content>
    <ion-list>
        <ion-item *ngFor="let message of messages">
            <ion-label>{{message.name}}</ion-label>
            <ion-label>{{message.msg}}</ion-label>
        </ion-item>
    </ion-list>
</ion-content>

<ion-footer>
    <ion-toolbar>
        <ion-input placeholder="message..." [(ngModel)]="send_msg"></ion-input>
        <ion-buttons slot="end">
            <ion-button (click)="send_message()" color="primary">
                <ion-icon slot="start" name="send"></ion-icon>
            </ion-button>
        </ion-buttons>
    </ion-toolbar>
</ion-footer>
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

## 로직처리 ts 파일

@ViewChild 로 ion-content를 참조해서 scrollToBottom() 메소드를 사용하면 된다.

```typescript
import { Component, OnInit, ViewChild, AfterViewInit } from '@angular/core';
import { ActivatedRoute } from '@angular/router';
import { env } from '@env/environment';
import { IonContent } from '@ionic/angular';
import io from 'socket.io-client';

@Component({
    selector: 'chat-view',
    templateUrl: './chat.view.page.html',
    styleUrls: ['./chat.view.page.scss']
})
export class ChatsViewPage implements OnInit, AfterViewInit {
    private socket: any;
    messages: any[] = [];
    send_msg: string;

    // ion-content 참조
    @ViewChild(IonContent, { read: IonContent, static: true }) conent: IonContent;

    constructor(private route: ActivatedRoute) {}

    ngOnInit() {
        this.socket = io(`${env.socket_url}/chat`, {
            query: {
                // 소켓 연결할때 인증처리를 위해 요렇게 jwt token 추가로 넘길수 있음.
                access_token: localStorage.getItem('access_token')
            }
        });

        // 소켓 연결성공
        this.socket.on('connect', () => {
            console.log('Connected');
            this.socket.emit('join', {
                to: 'stove99'
            });
        });

        // 메시지 수신시 화면에 반영하고 스크롤 제일 하단으로...
        this.socket.on('chat message', data => {
            this.messages.push(data);
            this.scrollToBottom();
        });
    }

    // 화면이 다 초기화 된 후 스크롤 제일 하단으로
    ngAfterViewInit() {
        this.scrollToBottom();
    }

    scrollToBottom() {
        setTimeout(() => {
            this.conent.scrollToBottom(300);
        }, 100);
    }

    send_message() {
        this.socket.emit('chat message', {
            name: '테스트',
            to: 'stove99',
            msg: this.send_msg
        });

        this.send_msg = '';
    }
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

## 참고로 보는 server side socket

server side 는 nestjs 로 간단하게 만들어 보았다. 실제로 쓸려면 고칠건 많다.

```typescript
import { SubscribeMessage, WebSocketGateway, WebSocketServer, WsResponse, OnGatewayInit, OnGatewayConnection, OnGatewayDisconnect } from '@nestjs/websockets';
import { Server, Socket } from 'socket.io';
import { Logger, UseGuards } from '@nestjs/common';
import { WsGuardGuard } from '../../common/auth/ws.guard.guard';

@UseGuards(WsGuardGuard)
@WebSocketGateway({ namespace: 'chat' })
export class ChatGateway implements OnGatewayInit, OnGatewayConnection, OnGatewayDisconnect {
    @WebSocketServer()
    server: Server;

    private logger: Logger = new Logger('ChatGateway');

    @SubscribeMessage('join')
    join(socket: Socket, data: any): WsResponse<any> {
        // room에 join한다

        const room = [data.user.id, data.to].sort().join('-');
        socket.join(room);

        return { event: 'room created', data: { room } };
    }

    @SubscribeMessage('chat message')
    async message(socket: Socket, data: any): Promise<WsResponse<any>> {
        this.logger.log(`message from client: ${data}`);

        const room = [data.user.id, data.to].sort().join('-');
        socket = socket.join(room).to(room);
        socket.emit('chat message', data);

        return { event: 'chat message', data };
    }

    afterInit(server: Server) {
        this.logger.log('Init');
    }

    handleDisconnect(client: Socket) {
        this.logger.log(`Client disconnected: ${client.id}`);
    }

    handleConnection(client: Socket, ...args: any[]) {
        this.logger.log(`Client connected: ${client.id}`);
    }
}
```

## WsGuardGuard

추가 예제로 소켓 접속시 아무나 접속 못하도록 jwt 체크를 위한 Guard.

위 예제에서 @UseGuards(WsGuardGuard) 요런식으로 적용시켜서 쓸 수 있다.

```typescript
import { CanActivate, ExecutionContext, Injectable, Inject, Logger } from '@nestjs/common';
import { Observable } from 'rxjs';
import * as jwt from 'jsonwebtoken';
import * as config from 'config';

@Injectable()
export class WsGuardGuard implements CanActivate {
    private logger: Logger = new Logger('WsGuardGuard');

    canActivate(context: ExecutionContext): boolean | Promise<boolean> | Observable<boolean> {
        try {
            const user = jwt.verify(context.switchToWs().getClient().handshake.query.access_token, config.get('JWT_SECRET'));

            context.switchToWs().getData().user = user;
            return Boolean(user);
        } catch (err) {
            console.log(err);
            this.logger.error('socket access token error');
        }

        return false;
    }
}
```
