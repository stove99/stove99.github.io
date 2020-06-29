---
layout: post
title: '[Angular] RxJS 로 input debounce 처리해서 효율적으로 검색 액션하기'
keywords: 'javascript'
categories: javascript
tags: javascript angular
---

## 소스

검색어 입력란에 키워드가 변경될 때마다 계속 검색 처리를 하면 서버에 부하도 많이 걸리고 쫌 별로다. 그렇다고 키워드 입력란 옆에 검색 버튼을 눌러서 검색하는것은 UX가 후져서 자존심히 허락하질 않는다.

고럴때 RxJS 의 pipe 및 debounce 를 활용하면 매우 있어보이면서 깔꼼하게 처리할 수 있다.

```javascript
import { Component, OnInit } from '@angular/core';
import { Subject, Observable } from 'rxjs';
import { map, filter, debounceTime, distinctUntilChanged, switchMap } from 'rxjs/operators';
import { HttpClient } from '@angular/common/http';

@Component({
    selector: 'app-root',
    template: `
        <input type="text" class="ml-2" [ngModel]="''" (ngModelChange)="onTextChange($event)" />

        <!-- async 파이프로 있어보이게 -->
        <div *ngIf="(searchResult$ | async) as searchResult">
            <h2>저장소 목록</h2>

            <ng-container [ngSwitch]="searchResult.items.length">
                <ng-container *ngSwitchCase="0">
                    No results found
                </ng-container>

                <ng-container *ngSwitchDefault>
                    <div *ngFor="let result of searchResult.items">
                        {{ result.name }}
                    </div>
                </ng-container>
            </ng-container>
        </div>
    `,
    styleUrls: ['./app.component.scss']
})
export class AppComponent implements OnInit {
    queries$ = new Subject<string>();
    searchResult$: Observable<any>;

    constructor(private http: HttpClient) { }

    ngOnInit() {
        this.searchResult$ = this.queries$.pipe(
            map((query: string) => query ? query.trim() : ''),  // 검색어 트림처리
            filter(Boolean),    // 트림결과 문자가 있는 경우
            debounceTime(500),  // 500ms debounce 처리
            distinctUntilChanged(), // 이전 입력값과 다른경우
            switchMap((id: string) => this.find(id)) // 검색 api 호출해서 결과값으로 Observable 변경
        );
    }

    onTextChange(id: string) {
        this.queries$.next(id);
    }

    private find(id: string): Observable<any> {
        return this.http.get<any>('https://api.github.com/search/repositories', { params: { q: id } });
    }
}
```
