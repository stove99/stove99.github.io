---
layout: post
title: '안드로이드 스튜디오 4.2 Gradle View 에서 Task 목록이 안보일때'
keywords: 'android gradle'
categories: etc
image: https://picsum.photos/2000/1200?image=10
image-sm: https://picsum.photos/500/300?image=10
---

SHA1 값을 조회하기 위해 인터넷에 검색한데로 Gradle view 목록에 있는것들중 signingReport 를 실행할려고 했는데 아무리 봐도 그런게 안 보였다.

검색해 보니 4.2 로 업데이트 되면서 성능상 문제라나 뭐라 그래서 디폴트로 안보이게 처리를 했다고 한다.

뭔 이유가 있어서 안보이게 했겠지만 굳이 다시 보이게 하고 싶다면

File - Settings - Experimental 에 가서 Do not build Gradle task list during Gradle sync 체크박스를 해제하고

File - Sync Project with Gradle Files 를 한번 실행시켜 주면 된다.

<img src="/assets/attach/202105/android_studio_01.png" alt="drawing"/>


## 그냥 단축키와 명령어로 SHA1 값 알아내기

안드로이드 스튜디오에서 Ctrl 키를 두번 누르면 Run Anything 다이얼로그가 나오는데 거기다 gradle signingReport 를 입력하고 엔터치면 빠르게 확인 가능하다.

단축키와 명령어로 하면 왠지 있어보임. 잇힝

<img src="/assets/attach/202105/android_studio_02.png" alt="drawing"/>