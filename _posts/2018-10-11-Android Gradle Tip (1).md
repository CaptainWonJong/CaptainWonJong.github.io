---
layout: post
title: "Android Gradle Tip (1)"
description: "Android Gradle Tip - Reduce App Size"
date: 2018-10-11
tags: [Android, Gradle]
comments: true
share: true
---
## Android Gradle Tip - Reduce App Size

안드로이드의 Gradle에서는 참 많은 일을 할 수 있다.  
사실 Gradle만 잘 설정해도 개발기간을 단축시킬 수 있다고 생각한다.  
오늘은 Gradle의 설정값을 추가하여 앱의 크기를 조금이나마 줄일 수 있는 방법을 알아보자.  

아래 코드를 **build.gradle(app)**에 추가해주자!

```gradle
android {
    packagingOptions.excludes = [
        'META-INF/DEPENDENCIES',
        'META-INF/LICENSE',
        'META-INF/LICENSE.txt',
        'LICENSE.txt',
        'LICENSE',
        'META-INF/license.txt',
        'META-INF/NOTICE',
        'NOTICE',
        'asm-license.txt',
        'META-INF/NOTICE.txt',
        'META-INF/notice.txt',
        'META-INF/ASL2.0',
        '.readme',
        '.readme.txt',
        'META-INF/maven/com.google.guava/guava/pom.properties',
        'META-INF/maven/com.google.guava/guava/pom.xml',
        'META-INF/maven/com.squareup.okhttp3/okhttp/pom.properties',
        'META-INF/maven/com.squareup.okio/okio/pom.properties',
        'META-INF/maven/com.squareup.okio/okio/pom.xml',
        'META-INF/maven/com.squareup.okhttp3/okhttp/pom.xml',
        'META-INF/MANIFEST.MF'
    ]
}
```
[여기](https://google.github.io/android-gradle-dsl/current/com.android.build.gradle.internal.dsl.PackagingOptions.html)를 보면 packagingOptions의 메소드에는 exclude, merge, pickFirst가 있고 Properties에는 doNotStrip, excludes, merges, pickFirsts가 있다고 한다.  
<small>자세한 내용은 위 사이트에 너무 잘 나와있기 때문에 생략한다.</small><br>  
위에 사용한 packagingOptions.excludes는 Gradle에서 빌드하고 APK 파일을 만들 때, 위의 해당 파일들을 제외하는 옵션으로 앱의 크기를 줄일 수 있다. <small>(아주 조금)</small>

#### 참고 )  
- **Gradle** : 빌드 자동화 툴 중 하나로, 아주 자세한 내용은 [여기](https://gradle.org/features/)에 나와있다.
- **Build** : 안드로이드에서의 빌드구성은 [여기](https://developer.android.com/studio/build/?hl=ko)에 아주 자세히 나와있다. 
