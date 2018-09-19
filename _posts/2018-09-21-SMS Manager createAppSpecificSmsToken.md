---
layout: post
title: "Automatic SMS verification without using SMS permission"
description: "SMS 권한을 선언하지 않고 SMS 인증(OTP 인증)을 하는 방법 (2)"
date: 2018-09-21
tags: [Android, SMS, SMS Manager, createAppSpecificSmsToken, Permission]
comments: 정
share: false
---

# 소스코드 및 블로그 내용 수정중입니다.

## 1) SMS Manager - createAppSpecificSmsToken

오늘은 두 번째로 **SMS Manager**를 사용하여 SMS가 왔을 때 인증번호를 자동으로 입력하는 방법을 알아보자. 
SMS Retriever API를 사용하여 SMS 권한 없이 OTP 자동입력을 하는 방법은 [여기](https://captainwonjong.github.io/2018-09-10/SMS-Retriever-API/)를 확인하자.
  
SMS Manager를 통한 OTP 자동입력 구조는 아래 그림과 같다.

마찬가지로 SMS Manager의 createAppSpecificSmsToken을 사용하려면 **반드시** Android Oreo 버전 이상부터 사용할 수 있다.  
그러니 Version별 분기처리를 하거나, minSdkVersion 을 26이상으로 바꿔줘야 하는데 아마 대부분의 앱들의 minSdkVersion이 26보다 낮을테니 Version별로 분기처리를 하는걸 추천한다.  
<small>(targetSdkVersion, compileSdkVersion은 당연히 26 이상으로..)</small>  
  
<br>
#### 일단 이 API를 사용하려면 아래와 같은 규칙을 **반드시** 숙지하고 따라야 한다.
1. Android Version O 이상 사용 가능.
2. 안드로이드에서 생성한 토큰값을 **메세지에 반드시 포함**하고 있어야 한다.
3. 하나의 토큰은 하나의 메세지에만 유효하다. (토큰을 1번 발급받고, 같은 메세지가 10번 온다고 해서 10번 인증되는것이 아니다.)

<br>
서버에서 문자를 보내야 하는 양식은 아래와 같다.
```
[Web발신]
MyApp SMS인증번호는 [000000]입니다. 인증번호를 입력해주세요 
FA+9qCX9VSu
```
<small>맨 뒤의 "FA+9qCX9VSu" 라는 토큰값은 메세지에 무조건 들어가야 하지만, 위치는 무관하다.</small>  
<br>



#### SMS를 보내는 API서버가 있다면 아래와 같은 순서로 OTP 자동입력기능을 구현한다.
1. 안드로이드에서 SMS Manager의 createAppSpecificSmsToken으로 토큰을 생성하여 서버에 전달한다.
2. 서버에서는 안드로이드에서 생성한 토큰값을 메세지에 포함하여 전달한다.
3. 안드로이드에서는 메세지에 온 토큰값을 비교하여 유효성을 체크하고, 토큰값이 같다면 메세지를 파싱하여 OTP번호를 자동입력한다.

<br><br>
자 이제 소스코드를 보자. <br>

### 1. 토큰을 생성한다.
```java
SmsManager smsManager = SmsManager.getDefault();
String smsToken = smsManager.createAppSpecificSmsToken( /* Input Your PendingIntent */ ); 
```
createAppSpecificSmsToken 에서는 PendingIntent를 파라미터로 사용한다.  
<small>**참고)** PendingIntent의 개념은 [여기(영어)](https://android.jlelse.eu/intent-vs-pendingintent-8ef2ad5824ed)와, [여기(한글)](http://techlog.gurucat.net/80)에 잘 나와 있다.</small>  

##### 2. 토큰을 서버로 전달한다. (코드 생략)
##### 3. 서버에서 메세지가 오면 PendingIntent가 실행된다.

개인적으로는 SMS Retriever API보다 간단한 것 같다.  
개발한 예제는 아래와 같다.

<br><br>
관련 소스코드 주소 :   
java : <https://github.com/CaptainWonJong/SMS_Manager>  
kotlin : 

참고사이트 :  
<https://developer.android.com/reference/android/telephony/SmsManager>
<https://code.tutsplus.com/tutorials/android-o-phone-number-verification-with-sms-token--cms-29141>

