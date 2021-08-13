---
layout: post
title: 'Spring Boot : Intellij 2021.2 에서 클래스 변경시 Hotswap 하기'
keywords: 'java,spring boot'
categories: java
tags: java springboot
image: https://picsum.photos/2000/1200?image=1047
image-sm: https://picsum.photos/500/300?image=1047
---

인터넷 검색을 하니 다 옛날 버전에서 설정하는 것 밖에 없고 대충 따라해도 쪽바로 동작을 안했다.

그래서 2021.2 버전에 맞게 다시 정리를 해 보았다.

1. File > Settings... > Tools > Action on Save 에서 Build Project 체크
   <img src="/assets/attach/202108/sc1.png">

2. Advanced Settings 에 있는 Compiler 설정 체크
   <img src="/assets/attach/202108/sc2.png">

3. 상단 실행 툴바쪽 Edit Configurations 로 가서 hotswap 설정 (참고로 ultimate 버전만 지원함)
   <img src="/assets/attach/202108/sc3.png">
   <img src="/assets/attach/202108/sc4.png">

4. springboot 어플리케이션 실행시 debug 모드로 실행해야 hotswap 동작함
   <img src="/assets/attach/202108/sc6.png">

5. 실행 후 잠깐 다른 브라우저나 탐색기 프로그램으로 포커스를 이동했다 다시 intellij 로 돌아오면 요런 팝업이 뜨는데 체크 후 reload 클릭
   <img src="/assets/attach/202108/sc5.png">

요렇게 하면 java 클래스를 수정하고 잠시 후면 변경된 내용이 리스타트 없이 hotswap 된다.
