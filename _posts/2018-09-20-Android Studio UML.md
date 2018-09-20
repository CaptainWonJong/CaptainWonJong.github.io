---
layout: post
title: "How to draw a UML using Plugin in Android studio"
description: "Android Studio에서 Plugin을 사용하여 UML을 그리는 방법"
date: 2018-09-20
tags: [Android, UML, SimpleUML]
comments: true
share: false
---
# Simple UML Plugin
### 여담
안드로이드 스튜디오 프로젝트에서 UML을 통해 전체적인 구조를 파악하는 방법이 있다.  
나의 경우에는 github에서 코드를 다운받고, 전체적인 구조를 보고싶을 때 매우 유용하다고 생각한다.
물론 본인이 짠 코드의 구조를 보고 싶을 때도 유용하다.  

이 플러그인은 물론 처음부터 UML을 그릴 때도 사용 가능하지만, 이미 만들어진 코드의 구조를 볼 때 더 많이 사용하고 있다.  
처음부터 그릴땐 웹의 draw.io, UMLet 등등 다른 UML툴이 많으니 굳이 안드로이드 스튜디오를 켜서 플러그인에서 그릴 필요는 없을 것 같다.  

사용법은 아래와 같다.

### 사용법
1. [다운로드 사이트](https://plugins.jetbrains.com/plugin/4946-simpleumlce) 에서 **simpleUMLCE_8205.jar** 파일을 다운로드 받는다.
![download.png](https://captainwonjong.github.io/images/180920_SimpleUML/img_download.png)  
   
2. 파일  
    --> 설정(Command + , or Ctrl + Alt + S)  
    --> Plugins 탭 선택  
    --> Install plugin from disk  
    --> simpleUMLCE_8205.jar Open(확인)  
    --> Android restart  
    ![setting.png](https://captainwonjong.github.io/images/180920_SimpleUML/img_setting.png)
    <small> 
3. 안드로이드 스튜디오에서 현재 열려있는 파일의 패키지 이름 우클릭  
    --> Add to SimpleUML Diagram  
    --> New Diagram  
    <small> 제 프로젝트가 아니어서 프로젝트 이름, 클래스 이름은 가렸습니다.</small>
    ![result.png](https://captainwonjong.github.io/images/180920_SimpleUML/img_result.png)

<small>
Tip 1) 위 이미지에서 초록색 원이 있는 버튼을 클릭하면 자동으로 정렬된다.  
Tip 2) 위 이미지에서 파란색 화살표가 있는 탭에서 우클릭을 하면 플러그인 위치를 옮길 수 있다.(Top, Right, Left, Bottom)
</small>

# 끝

다운로드 사이트 :   
<https://plugins.jetbrains.com/plugin/4946-simpleumlce>
  
참고사이트 :  
<https://code.i-harness.com/ko-kr/q/1054838>
