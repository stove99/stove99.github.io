---
layout: post
title: 'ionic 4 뒤로가기 버튼 두번으로 앱 종료처리'
keywords: 'javascript'
categories: javascript
tags: javascript ionic
---

ionic 4 에서 뒤로가기 버튼을 두번 눌럿을때 앱이 종료되도록 맹글어 보자.

version info

-   @ionic/angular : 4.7.1
-   @angular : 8.1.2

```typescript
import { Component, OnInit } from '@angular/core';
import { Platform, ToastController } from '@ionic/angular';
import { SplashScreen } from '@ionic-native/splash-screen/ngx';
import { StatusBar } from '@ionic-native/status-bar/ngx';

@Component({
    selector: 'app-root',
    templateUrl: 'app.component.html',
    styleUrls: ['app.component.scss']
})
export class AppComponent implements OnInit {
    back_clicked = 0;

    constructor(
        private toastCtrl: ToastController,
        private platform: Platform,
        private splashScreen: SplashScreen,
        private statusBar: StatusBar) {
        this.initializeApp();
    }

    initializeApp() {
        this.platform.ready().then(() => {
            this.statusBar.styleDefault();
            this.splashScreen.hide();
        });
    }

    ngOnInit() {
        this.appExitConfig();
    }

    private appExitConfig() {
        this.platform.backButton.subscribe(async () => {
            if (this.back_clicked === 0) {
                this.back_clicked++;

                const toast = await this.toastCtrl.create({
                    message: '뒤로가기 버튼을 한번 더 누르시면 앱이 종료됩니다.',
                    duration: 2000
                });
                toast.present();

                setTimeout(() => {
                    this.back_clicked = 0;
                }, 2000);
            } else {
                navigator['app'].exitApp();
            }
        });
    }
}
```
