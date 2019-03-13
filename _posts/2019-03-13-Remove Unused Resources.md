---
layout: post
title: "Remove Unused Resources"
description: "사용하지 않는 리소스 삭제하기"
date: 2019-03-13
tags: [Android]
comments: true
share: true
---

## 사용하지 않는 리소스 삭제하기

지금 개발하는 앱은 현재 구글마켓, 원스토어에 배포되고 있는 앱이고, 3월 말까지 개발하는 것은 이를 고도화 하는 작업이다.  
물론 UI/UX도 완전히 바뀌고, 앱 구조도 유지보수가 더욱 편하도록 MVC -> MVVM 패턴을 적용중이다.  
아 물론 AAC도 많이 사용하고 있다.  

그러나! UI/UX를 교체하며 수많은 drawable에 이미지파일이 무의미하게 남아있는 경우가 있는데 이를 어떻게 찾고 삭제할 수 있는지 알아보자.  

내가 알고있는 방법은 아래와 같이 3가지가 있는데, 2, 3번의 경우 나중에 깊게 알아보자

1. Removed Unused Resources...
2. Lint
3. Console

1번은 안드로이드 스튜디오에서 제공하는 기본 기능중의 하나로, 사용하지 않는 리소스를 모두 삭제해주는 기능이고,  
2번은 Lint(Analyze > InpectCode)를 통해 찾은 후 삭제하는 방법이고 (Lint는 그 외의 기능도 많이 있다.)  
3번은 Console에서 명령어를 통해 제거하는 방법을 생각했는데 이미 오픈소스가 있었다ㅠ > [여기](https://lsit81.tistory.com/entry/%EC%95%88%EB%93%9C%EB%A1%9C%EC%9D%B4%EB%93%9C-%EC%82%AC%EC%9A%A9%ED%95%98%EC%A7%80-%EC%95%8A%EB%8A%94-%EB%A6%AC%EC%86%8C%EC%8A%A4-%EC%9E%90%EB%8F%99-%EC%A0%9C%EA%B1%B0)를 참고하자.  

1번의 방법은 아래와 같이 **Refactor > Removed Unused Resources...** 를 클릭하면 된다.  

![RUR.png](https://captainwonjong.github.io/images/190313_rm_res/RUR.png)

**끝!**

