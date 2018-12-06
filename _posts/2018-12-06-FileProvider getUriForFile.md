---
layout: post
title: "FileProvider - android.os.FileUriExposedException"
description: "android.os.FileUriExposedException 해결방법"
date: 2018-12-06
tags: [Android, Exception, FileUriExposedException]
comments: true
share: true
---

## FileUriExposedException 이슈 해결방법

이번 2018년 11월이 되어 관리하는 앱의 targetSdkVersion을 23 -> 26으로 변경해야 하는 이슈가 생겼었다.  
그런데, targetSdkVersion을 변경하며, notificationChannel에만 집중하다보니 놓친 부분이 생기고 말았다..ㅠㅠ  
그 이슈는 targetSdkVersion 24에 변경사항 중 하나인 Uri.fromFile에 관한 이슈였고, 이미 많은 블로그에서도 이 이슈를 해결한 사람들이 많이 있었다.  

그러나 나는 삽질을 했기에, 다음에 같은 삽질을 하지 않기 위해서 + 많은 개발자들이 삽질을 하지 않기 위해서 FileUriExposedException 해결방법을 적으려 한다.  

자세한 구글의 요구사항은 [여기](https://developer.android.com/about/versions/nougat/android-7.0-changes#sharing-files)에 나와있다.  

내가 관리하는 앱은 회원가입을 하거나 내정보를 변경할 때, 프로필 사진을 등록/변경/Crop하는 기능에서 위의 문제점이 생기게 되었다.  


<br>
#### 1. 소스코드 분기

이를 해결하는 방법은 기존 기능에서 Uri.fromFile(imageFile) 을 분기처리하여 처리하는 것이었다.  

나는 Android Developer에서는 22.1.0 버전에 추가되었다고 하여 위의 기능을 분기처리를 하였다.  

```java
// N버전부터 FileProvider.getUriForFile을 사용
if (Build.VERSION.SDK_INT > Build.VERSION_CODES.M) {
    mUri = FileProvider.getUriForFile(mContext, mContext.getPackageName() + ".provider", photoFile);
} else {
    mUri = Uri.fromFile(photoFile);
}

takePictureIntent.putExtra(MediaStore.EXTRA_OUTPUT, mUri);
startActivityForResult(takePictureIntent, REQUEST_IMAGE_CAPTURE);
```
FileProvider.getUriForFile이 4버전대의 단말기에서 정상적으로 동작하지 않을 수 있다고 하여 SdkVersion 24 기준으로 나누게 되었다.  
(관리하는 앱의 minSdkVersion은 14이다..)  


<br>
#### 2. Manifest에 provider 등록

아래와 같이 provider를 등록한다.
```xml
<provider
    android:name="android.support.v4.content.FileProvider"
    android:authorities="${applicationId}.provider"
    android:exported="false"
    android:grantUriPermissions="true">
    <meta-data
        android:name="android.support.FILE_PROVIDER_PATHS"
        android:resource="@xml/paths" />
</provider>
```

- 중요한건 Manifest의 ${applicationId}.provider와 분기처리한 곳의 mContext.getPackageName() + ".provider"가 동일해야 한다.  
나는 앱 패키지명 + .provider로 등록하였다.
- 또 중요한건 @xml/paths을 설정해 주어야 한다. 


<br>
#### 3. res/xml/paths.xml 생성 및 등록
아래와 같이 paths.xml을 생성한다.
```xml
<?xml version="1.0" encoding="utf-8"?>
<paths xmlns:android="http://schemas.android.com/apk/res/android">
    <files-path name="마음에드는 이름1" path="Pictures" /> <!-- Context.getFilesDir(). -->
    <external-path name="마음에드는 이름2" path="Pictures" /> <!-- Environment.getExternalStorageDirectory(). -->
    <external-files-path name="마음에드는 이름3" path="Pictures" /> <!-- Context#getExternalFilesDir(String) Context.getExternalFilesDir(null). -->
    <cache-path name="마음에드는 이름4" path="Pictures" /> <!-- getCacheDir(). -->
    <external-cache-path name="마음에드는 이름5" path="Pictures" /> <!-- Context.getExternalCacheDir(). -->
</paths>
```
paths.xml파일이 제일 중요하다. 만약 paths.xml 파일의 path에서 파일을 찾을 수 없다면 FileProvider.getUriForFile에서 IllegalArgumentException을 뱉는다.  
(이 IllegalArgumentException 때문에 삽질했다..)  

- 나는 이미지를 파일로 저장할 때, Environment.DIRECTORY_PICTURES 로 저장해서 .Pictures로 path를 한 것이다.
- 위에꺼는 예시때문에 5개를 적은 것이고, 실제로 사용한 경로는 external-files-path 밖에 없다.

#### 4. 삽질한 내용을 뜯어보자.
삽질을 해결한 방법은 FileProvider.getUriForFile을 타고타고 디버깅을 해서 해결했다.  
처음에는 '대충 바꾸면 되겠지' 라는 마음으로 시작해서 간단할 줄 알았는데 paths.xml을 제대로 쓰지 못해 삽질을 했다. (name과 path의 기능을 반대로 생각)  

FileProvider을 타고 들어가면 위의 paths.xml의 path를 검사하고 확인하는 부분은 아래 소스에서 실행된다.
```java
static class SimplePathStrategy implements PathStrategy {
    ...
    @Override
    public Uri getUriForFile(File file) {
    ...
    
    // Find the most-specific root path
    Map.Entry<String, File> mostSpecific = null;
    for (Map.Entry<String, File> root : mRoots.entrySet()) {
        final String rootPath = root.getValue().getPath();
            if (path.startsWith(rootPath) && (mostSpecific == null
                || rootPath.length() > mostSpecific.getValue().getPath().length())) {
            mostSpecific = root;
        }
    }
    
    // Exception
    if (mostSpecific == null) {
        throw new IllegalArgumentException(
            "Failed to find configured root that contains " + path);
    }
    
    ......
    }
}

```

위에서 Find the most ~~~ 주석이 있는 곳이 paths.xml의 path를 반복문을 돌려 확인하는 부분이고, 만약 일치하지 않는다면 아래 Exception으로 빠져 IllegalArgumentException을 던지게 된다.  


<br>
#### 결론
대충해서 몇 시간동안 고생하지 말고, 천천히 꼼꼼히 확실하게 하자ㅠㅠ  


#### 끝


참고사이트 :
<https://developer.android.com/reference/android/support/v4/content/FileProvider#geturiforfile>  
<https://developer.android.com/about/versions/nougat/android-7.0-changes#sharing-files>  
<http://kyome.tistory.com/9>  


