---
layout: post
title: "Automatic SMS verification without using SMS permission"
description: "SMS 권한을 선언하지 않고 SMS 인증(OTP 인증)을 하는 방법 (1)"
date: 2018-09-10
tags: [Android, SMS, Retriever, Permission]
comments: true
share: true
---

## 1) SMS Retriever API

SMS / Call log 퍼미션을 별도 분리하여 아래와 같이 지정된 앱에서만 사용 가능하도록 변경하라는 메일이 날아왔다.

```
SMS/다이얼러 기본 앱, 어시스턴스 앱, 백업 앱, 계정 관리, 고객 지원 앱, 컴페니언 앱(웨어러블 디바이스 같이 스마트폰과 연결하여 쓰는 단말의 컴패니언앱), 음성 메일, 콜러 차단앱(후후 등). 
```

그리하여 SMS 권한을 사용하지 않고 OTP 자동입력을 할 수 있도록 [SMS Retriever API](https://developers.google.com/identity/sms-retriever/), [SMS_Manager API](https://developer.android.com/reference/android/telephony/SmsManager), [SMS Intent](https://developer.android.com/guide/components/intents-common#SearchOnApp) 로 대체하여 사용해야 한다.  
  
오늘은 첫 번째로 **SMS Retriever API**를 사용하여 SMS가 왔을 때 인증번호를 자동으로 입력하는 방법을 알아보자. 
  
  ![SMS_Retriever_API.png](https://captainwonjong.github.io/images/img_sms_retriever_api_flow.png)
  <small> (SMS Retriever API의 구조) </small><br><br>
  
  
일단 이 API를 사용하려면 아래와 같은 규칙을 **반드시** 숙지하고 따라야 한다.
1. Be no longer than 140 bytes 
(문자 내용이 140byte를 초과하면 안된다.)
2. Begin with the prefix <#> 
(SMS 맨 앞에 "<#>" 문자가 반드시 포함되어야 한다.)
3. End with an 11-character hash string that identifies your app
(SMS 맨 마지막에 앱을 식별하는 11글자의 해시 문자열을 포함해야 한다. 해시 문자열을 확인하는 방법은 [여기](https://developers.google.com/identity/sms-retriever/verify#computing_your_apps_hash_string)를 클릭하면 확인할 수 있다.)

서버에서 문자를 보내야 하는 양식은 아래와 같다.
```
<#> XXX 앱의 인증번호는 [123456] 입니다.
FA+9qCX9VSu
```
맨 앞에 <#> 값과 맨 뒤에 11자리의 해쉬값 FA+9qCX9VSu*(이건 앱마다 다름)* 이 무조건 들어가야 한다.<br><br><br>

자 이제 소스코드를 보자.  <br><br>
### 1.. build.gradle의 dependencies에 아래와 같이 의존성을 주입해주자.  
(mac사용자면 "command + ;" 에서 넣어도 됨)  
(버전은 10.2 보다만 높으면 됨)
```
implementation 'com.google.android.gms:play-services:+'
```
<br>
### 2.. 메인이든 어디든 원하는 액티비티에 아래와 같이 SmsRetriever 클라이언트를 선언/실행 해준다.  
(registerReceiver와 unregisterReceiver는 원하는 곳에 해줘도 된다.   
otp 버튼을 누를 때 해줘도 되고, onCreate, onResume, onDestroy등 앱 특성에 맞에 개발자 마음대로..)

**Java**
```java
SmsRetrieverClient client = SmsRetriever.getClient(this);   // this = context
Task<Void> task = client.startSmsRetriever();

task.addOnSuccessListener(new OnSuccessListener<Void>() {
    @Override
    public void onSuccess(Void aVoid) {
        // retriever 성공
        IntentFilter intentFilter = new IntentFilter(SmsRetriever.SMS_RETRIEVED_ACTION);    // SMS_RETRIEVED_ACTION 필수입니다.
        registerReceiver(smsReceiver, intentFilter);
    }
});

task.addOnFailureListener(new OnFailureListener() {
    @Override
    public void onFailure(@NonNull Exception e) {
        // retriever 실패
    }
});
```

<br>**Kotlin**
```kotlin
val client = SmsRetriever.getClient(this /* context */)
val task = client.startSmsRetriever()
task.addOnSuccessListener {
    val intentFilter = IntentFilter()
    intentFilter.addAction(SmsRetriever.SMS_RETRIEVED_ACTION)   // SMS_RETRIEVED_ACTION 필수입니다.
    registerReceiver(smsBroadcast, intentFilter)
}

task.addOnFailureListener {
    otpTxtView.text = "Cannot Start SMS Retriever"
    Toast.makeText(this, "Error", Toast.LENGTH_LONG).show()
}
```

<br>
### 3.. Receiver를 등록해준다.

**AndroidManifest.xml**
```xml
<receiver
    android:name=".SmsReceiver"
    android:exported="true">

    <intent-filter>
        <action android:name="com.google.android.gms.auth.api.phone.SMS_RETRIEVED"/>
    </intent-filter>
</receiver>
```

<br>**SmsReceiver.java**
```java
public class SmsReceiver extends BroadcastReceiver {

    @Override
    public void onReceive(Context context, Intent intent) {

        if (SmsRetriever.SMS_RETRIEVED_ACTION.equals(intent.getAction())) { // SMS_RETRIEVED_ACTION 필수입니다.
            Bundle extras = intent.getExtras();
            Status status = (Status) extras.get(SmsRetriever.EXTRA_STATUS);

            switch(status.getStatusCode()) {
                case CommonStatusCodes.SUCCESS:
                    String message = (String) extras.get(SmsRetriever.EXTRA_SMS_MESSAGE);
                    // 여기에 SMS받은거 알아서 파싱하세요~~
                    break;
                case CommonStatusCodes.TIMEOUT:
                    // SMS Retriever에서 기본 Timeout 시간은 5분이라고 합니다.
                    break;
            }
        }
    }
}
```
<br>**SmsReceiver.kt**

```kotlin
class SmsReceiver : BroadcastReceiver() {

    override fun onReceive(context: Context, intent: Intent) {
        if (SmsRetriever.SMS_RETRIEVED_ACTION == intent.action) {
        val extras = intent.extras
        val status = extras!!.get(SmsRetriever.EXTRA_STATUS) as Status

        when (status.statusCode) {
            CommonStatusCodes.SUCCESS -> {
                var otp: String = extras.get(SmsRetriever.EXTRA_SMS_MESSAGE) as String
                if (otpReceiver != null) {
                    // 여기에 SMS받은거 알아서 파싱하세요~~
                }
            }

            CommonStatusCodes.TIMEOUT -> {
                // SMS Retriever에서 기본 Timeout 시간은 5분이라고 합니다.
            }
        }
    }
}

```

<br>
### 4.. 테스트 방법

친구 핸드폰을 사용하거나 스스로한테 아래와 같이 문자를 보내본다. (값이 잘 들어왔으면 파싱은 알아서)
```
<#> XXX 앱의 인증번호는 [123456] 입니다.
FA+9qCX9VSu
``` 
<br>
### END

<br><br><br>
### 추가코드

#### 1) [여기](https://developers.google.com/identity/sms-retriever/verify#computing_your_apps_hash_string) 에서 확인할 수 있는 해시 문자열 보는 소스코드 (저는 Util.java 에 넣어둡니다.)

```java

public static final String TAG = Util.class.getSimpleName();

private static final String HASH_TYPE = "SHA-256";
public static final int NUM_HASHED_BYTES = 9;
public static final int NUM_BASE64_CHAR = 11;

/**
* get App Signatures
*/
public static ArrayList<String> getAppSignatures(Context context) {
    ArrayList<String> appCodes = new ArrayList<>();

    try {
        // Get all package signatures for the current package
        String packageName = context.getPackageName();
        PackageManager packageManager = context.getPackageManager();
        Signature[] signatures = packageManager.getPackageInfo(packageName, PackageManager.GET_SIGNATURES).signatures;

        // For each signature create a compatible hash
        for (Signature signature : signatures) {
        String hash = getHash(packageName, signature.toCharsString());
        if (hash != null) {
            appCodes.add(String.format("%s", hash));
        }
        Log.d(TAG, String.format("이 값을 SMS 뒤에 써서 보내주면 됩니다 : %s", hash));
        }
    } catch (PackageManager.NameNotFoundException e) {
        Log.d(TAG, "Unable to find package to obtain hash. : " + e.toString());
    }
    return appCodes;
}

private static String getHash(String packageName, String signature) {
    String appInfo = packageName + " " + signature;
    try {
        MessageDigest messageDigest = MessageDigest.getInstance(HASH_TYPE);
        // minSdkVersion이 19이상이면 체크 안해도 됨
        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.KITKAT) {
            messageDigest.update(appInfo.getBytes(StandardCharsets.UTF_8));
        }
        byte[] hashSignature = messageDigest.digest();

        // truncated into NUM_HASHED_BYTES
        hashSignature = Arrays.copyOfRange(hashSignature, 0, NUM_HASHED_BYTES);
        // encode into Base64
        String base64Hash = Base64.encodeToString(hashSignature, Base64.NO_PADDING | Base64.NO_WRAP);
        base64Hash = base64Hash.substring(0, NUM_BASE64_CHAR);

        Log.d(TAG, String.format("\nPackage : %s\nHash : %s", packageName, base64Hash));
        return base64Hash;
    } catch (NoSuchAlgorithmException e) {
        Log.d(TAG, "hash:NoSuchAlgorithm : " + e.toString());
    }
    return null;
}
```
<br><br>
관련 소스코드 주소 : (Kotlin은 이미 좋은 소스코드가 있어 다른 github 주소 추천드립니다.)  
java : <https://github.com/CaptainWonJong/SMS_Retriever_API>  
kotlin : <https://github.com/chintandesai49/SMSRetrieverAPIDemo>   
<br>
참고사이트 :   
<https://developers.google.com/identity/sms-retriever/>  
<https://android.jlelse.eu/googles-sms-retriever-api-6540eb3c8e9c>  











