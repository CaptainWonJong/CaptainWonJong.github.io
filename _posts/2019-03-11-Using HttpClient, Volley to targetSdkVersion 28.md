---
layout: post
title: "Using HttpClient, Volley to targetSdkVersion 28"
description: "targetSdkVersion 28에서 HttpClient, Volley 사용하기"
date: 2019-03-11
tags: [Android, Network, Volley, HttpClient]
comments: true
share: true
---

## targetSdkVersion 28에서 HttpClient, Volley 사용하기

프로젝트 마무리단계가 되어가며 바쁜 나날을 보내고있다.  
3월 말까지 프로젝트를 끝내고, 4월 말~ 5월 초쯤 앱이 마켓에 올라갈 예정인데, 구글이 또 targetSdkVersion을 올해 말<small>(정확히는 신규앱 8월, 기존앱 11월)</small> 까지 올리라고 한다ㅠㅠ  
<https://android-developers.googleblog.com/2019/02/expanding-target-api-level-requirements.html>   
그리고 매년 targetSdkVersion을 매년 올리도록 요구한다고 하니 연초가 되면 항상 블로그를 확인해야겠다;;  

오늘은 소스 수정 없이 targetSdkVersion을 28로 올려서 앱을 실행시켰을 때, 내가 개발중인 앱에서 발생한 Exception에 관하여 설명한다.  

글을 들어가기전에 앞서 [여기](https://developer.android.com/about/versions/pie/android-9.0-changes-all#network-capabilities-vpn)에서 Android 9 에서 어떤 변경사항들이 있는지 확인해봐도 좋을 것 같다.  


#### 문제

내가 실무에서 개발하고 있는 앱은 API서버와 통신할 때 [Volley](https://developer.android.com/training/volley) 라이브러리를 사용한다.  
이 라이브러리는 google에서 만든 라이브러리인데 내 소스의 공통부에서 org.apache.http.impl.client.DefaultHttpClient 를 사용한다.  
결국 이 클래스를 타고타고 가면 AbstractHttpClient를 거쳐 HttpClient로 도착하게 된다.  

하.. [여기](https://developer.android.com/about/versions/pie/android-9.0-changes-all#apache-nonp)에 Apache HTTP 클라이언트 지원 중단 관련해서 뚜렷하게 적혀있는데ㅠㅠ  


#### 해결방법

우리들이 원하는건 늘 그랬듯이 최소한의 비용으로 최고의 품질을 만드는 것!  
최고의 품질은 그렇다 하더라도 어떻게든 공통 네트워크 모듈을 뜯어고치는 참사만은 발생하지 말아야 한다.  
해답은 생각보다 매우 간단했다.  

**AndroidManifest.xml**
```xml
<manifest
    ...>
    
    < ... />
    
    <application
        ... >
        
        // 아래 코드를 추가하자!
        <uses-library android:name="org.apache.http.legacy" android:required="false"/>
        
    </application>
</manifest>
```

이외에 gradle의 android에 ```useLibrary 'org.apache.http.legacy'``` 를 추가하고 optional.json 파일을 추가해주는 방법이나,  
<http://hc.apache.org/downloads.cgi>에서 jar파일을 다운받고 디펜던시에 추가해준다거나 하는 방법들도 있었지만 매니페스트에 위의 한 줄 추가해주는게 가장 간단한 방법이었다!  

**끝!**

