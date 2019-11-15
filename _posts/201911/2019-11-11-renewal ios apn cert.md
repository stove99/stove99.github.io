---
layout: post
title: 'IOS 푸시인증서 갱신하기(*.p12)'
keywords: 'ios apns p12'
categories: etc
tags: etc
---

## csr 맨들기 (csr 이 이미 있다면 다음으로 넘어가도 됨)

키체인 - 인증서 지원 - 인증기관에서 인증서 요청... - 이메일주소입력, 디스크에 저장됨 선택 후 csr 파일 생성

<img src="/assets/attach/201911/capture0.png" style="width:500px;">

## <https://developer.apple.com/account/> 에 가서 인증서 갱신

1. 왼쪽 메뉴중 Certificates, Identifiers & Profiles 클릭

    <img src="/assets/attach/201911/capture1.png" style="width:600px;">

2. 왼쪽 메뉴중 Identifiers 로 이동 후 오른쪽 목록중에서 갱신을 원하는 앱 선택

    <img src="/assets/attach/201911/capture2.png" style="width:600px;">

3. Push Notifications 옆 Edit 버튼 클릭

    <img src="/assets/attach/201911/capture3.png" style="width:600px;">

4. Create Certificate 버튼 클릭(프로덕션쪽에 걸로..), 기존 인증서는 Revoke해서 삭제하던지 한다.

    <img src="/assets/attach/201911/capture4.png" style="width:600px;">

5. 1번에서 생성했던 csr 파일을 업로드 한다음에 Continue 버튼클릭

    <img src="/assets/attach/201911/capture5.png" style="width:600px;">

6. 인증서 파일을 다운로드 후 더블클릭하면 키체인에 등록된다.

    <img src="/assets/attach/201911/capture6.png" style="width:600px;">

7. 키체인에서 왼쪽 목록중 인증서 항목을 클릭하고 추가된 인증서를 뽓 열어보면 아래에 하나가 튀어나오는데 거기서 마우스 오른쪽 버튼 클릭후 내보니기를 하면 p12 인증서가 생성된다.

    <img src="/assets/attach/201911/capture7.png" style="width:600px;">

8. 생성된 인증서를 서버에 있는 인증서과 교체하면 끝.
