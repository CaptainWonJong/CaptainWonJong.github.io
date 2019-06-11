---
layout: post
title: "iOS, XCode, Swift Study"
description: "iOS, XCode, Swift 공부 내용 정리"
date: 2019-06-10
tags: [iOS, XCode, Swift]
comments: true
share: true
---

## 공부 내용 정리
 - 많은 도움을 주신 [Zedd 님의 블로그](https://zeddios.tistory.com/)에 감사인사를 드립니다.  
   
**CocoaPods**    
 1. <https://cocoapods.org/>에서 Swift의 라이브러리를 관리 (DI하는 역할만 본다면 Android의 Gradle과 비슷)  
 2. 코코아팟(or 코코아포드)을 사용한다면 라이브러리를 DI할 수 있도록 .xcworkspace 파일로 xcode를 실행해야 한다.  
 3. Swift 버전에 따라 지원하지 않는 라이브러리도 있으니 버전별 지원여부를 잘 확인해야 한다.  
 4. cocoapods의 라이브러리를 추가하기 위해서는 Podfile에 양식대로 추가 해 주면 된다. (ex. pod 'Alamofire', '4.8.0')  
 5. 뭔가 외부 라이브러리를 주입하는 방식이 Android보단 php와 비슷했다.  
   
 <br>
 
 **iOS**  
 1. delegate(대리자?, 대표자?)을 쉽게 이해할 수 있도록 풀어쓴 블로그는 [여기 Zedd님의 블로그](https://zeddios.tistory.com/8?category=682195)랑 [여기2](https://zeddios.tistory.com/10?category=682195)가 있다.  
 2. LifeCycle 역시 [Zedd 님의 블로그1](https://zeddios.tistory.com/10?category=682195)과 [블로그2](https://zeddios.tistory.com/44?category=682195)를 참조하자.   
 3. LifeCycle 관련 이미지는 아래와 같다.  
![lifecycle1.png](https://captainwonjong.github.io/images/190610_swift/lifecycle1.png){: width="300" height="300"}  
![lifecycle2.png](https://captainwonjong.github.io/images/190610_swift/lifecycle2.jpeg){: width="300" height="300"}  

<br>

**Swift**  
1. [Escaping Closure 관련 글(어려움ㅠ)](https://hcn1519.github.io/articles/2017-09/swift_escaping_closure)
2. [Extension(확장) 정리](http://minsone.github.io/mac/ios/swift-extensions-summary)

<br>

**Etc**
1. [Convert Objective-C to Swift Site](https://objectivec2swift.com/#/converter/code/)
2. [Concepts in iOS programming Site](https://developer.apple.com/library/archive/documentation/General/Conceptual/CocoaEncyclopedia/Introduction/Introduction.html#//apple_ref/doc/uid/TP40010810-CH1-SW1)
3. [ViewController 이름 변경 유의사항](https://macinjune.com/all-posts/web-developing/swift/xcode-swift-viewcontroller-%EC%9D%B4%EB%A6%84-%EB%B3%80%EA%B2%BD-%EC%8B%9C-%EC%9C%A0%EC%9D%98%EC%82%AC%ED%95%AD/)
4. [Api Test Site - reqres.in](https://reqres.in/)
