---
layout: post
title: "Android Gradle Tip (2)"
description: "Android Gradle Tip - Building with the console"
date: 2018-12-03
tags: [Android, Gradle]
comments: true
share: true
---

## Android Gradle Tip - Building with the console

안드로이드 개발을 하며 소스코드를 빌드하거나 디바이스에 인스톨 할 때는 항상 안드로이드 스튜디오의 버튼을 눌렀었다.  
조금 더 작업이 있다면 Build Variants에서 flavor를 나눠 여러가지 버전을 생성하여 빌드를 했었다.  

그러나 만약 git에 jenkins와 같은 지속적 통합(continuous integration) 도구를 사용하여 여러 버전의 apk파일을 만들어야 하는 일이 생긴다면 콘솔을 이용해야 한다.  

이 글은 '이런게 있다~' 라고만 정리한 글이니 자세한 내용이 필요하다면 아래 링크들을 통해 확인하는 것이 훨씬 도움이 될 것이다.  

해당 프로젝트 경로로 들어가  
Windows는
```linux
gradlew tasks
```
Mac은 
```linux
./gradlew tasks
```
와 같은 명령어를 치면 아래와 같은 화면이 나온다.  
사용하고 싶은 명령어를 사용하자;; (옆에 설명이 잘 되어있음)  

![Gradle_2_1.png](https://captainwonjong.github.io/images/181203_GradleTip2/Gradle_2_1.png)
![Gradle_2_2.png](https://captainwonjong.github.io/images/181203_GradleTip2/Gradle_2_2.png)


<br><br>
[여기](https://medium.com/@goinhacker/%EC%9A%B4%EC%98%81-%EC%9E%90%EB%8F%99%ED%99%94-1-%EB%B9%8C%EB%93%9C-%EC%9E%90%EB%8F%99%ED%99%94-by-gradle-7630c0993d09)에서 Gradle은 성능 향상을 위해서 증분 빌드, 작업 결과 캐싱, 증분 하위 작업, 데몬 프로세스 재사용, 병렬 실행, 병렬 다운로드 등의 기능을 지원한다고 하지만 구체적으로는 모르겠다.  
--> 이 점은 공부가 필요!  

[Gradle 공식 홈페이지](https://gradle.org/features/)에서 도큐먼트가 자세히 나와있으니 시간이 될 때마다 공부를 해보자!  

./gradlew 명령어를 통해 콘솔에서 빌드하는 방법을 더 자세히 알고싶다면 [Android Developer Site](https://developer.android.com/studio/build/building-cmdline?hl=ko)를 참고!  

다음에 무조건 포스트 하겠지만 빌드 변형 구성에 관한 내용은 [여기](https://developer.android.com/studio/build/build-variants?hl=ko)를 참고하고,  
Android Studio에서 앱 빌드 및 실행에 관한 내용은 [여기](https://developer.android.com/studio/run/?hl=ko)를 참고하자.

끝!


