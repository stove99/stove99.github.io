---
layout: post
title: 'Firebase Storage CORS 설정하기'
keywords: 'git'
categories: etc
image: https://picsum.photos/2000/1200?image=10
image-sm: https://picsum.photos/500/300?image=10
---

Firebase Storage 에 저장된 이미지를 base64 인코딩 할려고 fetch() 메소드로 이미지 데이터를 가져오려고 했으나 CORS 오류가 났다.

그래서 CORS 설정하는 방법을 찾아 보았다.

1. [Google Cloud Platform 콘솔](https://console.cloud.google.com/)에 접속한다.
2. 상단에 있는 콘솔 접속 아이콘으로 클릭해서 콘솔에 접속한다.
<img src="/assets/attach/202106/fbcors01.png">
3. 편집기 열기 버튼을 클릭해서 편집기를 오픈한 다음 File - New File 로 적당한 이름의 json 파일을 생성한다. 나는 cors.json 으로 했다.
<img src="/assets/attach/202106/fbcors02.png">
4. cors.json 파일의 내용은 요렇게 했다. 모든 도메인에 대해 허용하려면 * 로 설정하면 되고, 싫으면 원하는 도메인만 배열에다 추가해 주면 된다. ex) "origin": ["https://example.com"]
```json
[
  {
    "origin": ["*"],
    "method": ["GET"],
    "maxAgeSeconds": 3600
  }
]
```
5. 파일을 저장한 다음 편집기 상단에 있는 터미널 열기 버튼을 클릭해서 터미널로 돌아온 다음 아래 명령으로 CORS 설정을 적용한다.
```bash
gsutil cors set cors.json gs://내프로젝트버켓주소
```
6. 내 프로젝트 버켓 주소는 Firebase Console 에 접속해서 Storage 메뉴로 들어가면 확인할 수 있다.
<img src="/assets/attach/202106/fbcors03.png">