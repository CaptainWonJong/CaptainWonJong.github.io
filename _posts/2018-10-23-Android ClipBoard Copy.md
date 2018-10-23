---
layout: post
title: "Android ClipBoard Copy"
description: "Android 클립보드에 Text 복사하기"
date: 2018-10-23
tags: [Android, ClipboardManager, ClipData]
comments: true
share: true
---

## Android 클립보드로 텍스트 복사하기 (소스코드)

안드로이드 클립보드로 텍스트를 복사하는데는 SDK 11버전을 기준으로 2가지로 나뉜다.  
2가지 방법 다 알아보겠지만, minSdkVersion을 11아래로 설정한적이 없어서 옛날버전은 무시해도 될 것 같다;;  

#### 1. Honeycomb 이하 버전에서 클립보드로 텍스트 복사하기
```java
String strCopy = "YOUR COPY TEXT";

ClipboardManager clipboardManager = (ClipboardManager) getSystemService(Context.CLIPBOARD_SERVICE);
clipboardManager.setText(strCopy);
```
안드로이드 스튜디오에서 clipboardManager.setText(strCopy); 을 사용하면 setText가 deprecated 되어 취소선이 그어질 것이다ㅠㅠ  
그러니 아래 소스코드를 사용하자!  

#### 2. Honeycomb 윗 버전에서 클립보드로 텍스트 복사하기
```java
String strLabel = "YOUR LABEL";
String strCopy = "YOUR COPY TEXT";

ClipboardManager clipboardManager = (ClipboardManager) mContext.getSystemService(Context.CLIPBOARD_SERVICE);
ClipData clipData = ClipData.newPlainText(strLabel, strCopy);
clipboardManager.setPrimaryClip(clipData);
```

그밖의 유용한 클립보드 관련 내용들은 아래와 같다.  
  
클립보드 동작 개념 : 
<br>
<http://dktfrmaster.blogspot.com/2016/10/blog-post.html>  

Toss 앱처럼 계좌번호와 같은 특정 양식의 텍스트를 복사하면 서비스에서 탐지하여 윈도우매니저를 통해 앱으로 연결해 주는 방법 : 
<br>
<https://bit.ly/2yVgQrs>
<br>
<https://stackoverflow.com/questions/34626143/how-to-get-notified-when-user-copied-something-in-android> 

<br>
참고사이트 :
<https://developer.android.com/reference/android/content/ClipboardManager/>
<https://developer.android.com/reference/android/content/ClipData>


