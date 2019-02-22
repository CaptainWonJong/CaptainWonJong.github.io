---
layout: post
title: "Android APK Build Analysis"
description: "안드로이드 apk 빌드 분석"
date: 2019-02-20
tags: [Android, Build, Apk, Dalvik]
comments: true
share: true
---

## 안드로이드 apk 분석

3월 말까지 프로젝트가 진행중인 지금 바빠서 간단한 내용만 회고록에 작성하고 있다..  

오늘 정리할 내용은 안드로이드 apk 빌드 과정과 결과를 대충 정리하려고 한다.  

학부생때 실무 소스코드가 궁금해서 남의 앱 디컴파일 하던 취미가 있었다.  

사실 apk파일 안의 .dex 파일이나 .arsc 파일들이 무슨 동작을 하는지에 대해서는 궁금하지도 않았는데, 며칠 전 다른 앱을 디컴파일하다가 문득 궁금해져서 이것저것 구글링하다보니 재밌어서 정리하는 것이다.  


<br>

#### 빌드 프로세스
안드로이드 개발자 홈페이지와 MIT 에듀에 있는 이미지로 모든 설명을 생략한다.  

![build_1.png](https://captainwonjong.github.io/images/190220_apk_build/build_1.png)
<small>출처 : <https://developer.android.com/studio/build/?hl=ko></small>

![build_2.png](https://captainwonjong.github.io/images/190220_apk_build/build_2.png)  
![build_3.png](https://captainwonjong.github.io/images/190220_apk_build/build_3.png)  
<small>출처 : <https://stuff.mit.edu/afs/sipb/project/android/docs/tools/building/index.html></small>  
[참고사항] 위 이미지에서 .apk파일을 jarsigner를 사용하여 사이닝을 한 후 zipalign을 해준다고 하는데,  
SDK의 apksigner를 사용하면 zipalign 후 apksigner를 하게 된다. (순서가 바뀜)
  
  
  <br>
  
#### apk파일 뜯어보기
1. <b>classes.dex</b> : dex는 Dalvik EXecutable 의 약자로, java의 class파일이 Dalvik 형태로 변환된 파일이다.  
apk파일 안의 classes.dex파일로 보통 존재하게 되는데, 디컴파일 할 때 dex2jar를 사용하면 dex파일이 jar파일로 변환되어 jd-gui를 통해 jar파일을 뜯어봄으로써 java코드를 확인할 수 있다.  

2. <b>META-INF</b> : 인증서 관련 내용이  META-INF폴더 안에 들어가 있다. 폴더 안에는 Manifest.MF, CERT.SF, CERT.RSA 등의 파일이 들어가있는데, Manifest.MF에는 jar파일을 로딩할 때 java런타임 환경에서 사용하는 정보(버전, 작성자, 정책, jar파일 등등의 정보)가 들어있다. CERT.SF에는 모든 파일에 대한 SHA-1 지문값이, CERT.RSA파일에는 사이닝에 사용한 공개키 인증서 체이닝값, CERT.SF 파일의 사이닝된 내용이 포함되어있다.  
요약하면 인증서 관련 내용이 있다는 소리다.

3. <b>resources.arsc</b> : 컴파일된 리소스 관련 파일이다. arsc파일을 뜯어보면 개발할 때 res/values 폴더의 파일들이 있으며, 디컴파일을 해야 볼 수 있다. Apktool(apk로부터 리소스를 추출할 때 사용하는 툴)을 사용하면 확인/수정을 할 수 있다고 한다. SDK에 있는 AAPT(Android Asset Packaging Tool)를 사용해서도 내용을 확인할 수 있지만, 파일을 보려면 디컴파일을 해야한다고 한다.

4. <b>assets, res, lib, AndroidManifest.xml 등등......</b> : 외부 리소스(assets),  내부 리소스(res), 라이브러리(lib), app정보(manifest) 같은건 안드로이드 처음 배울 때 공부하는 내용이니 생략하자.

5. <b>smali</b> : smali는 An Assembler/Disassembler for Android's dex format의 약자고 dex 바이너리를 사람이 읽을 수 있도록 표현한 언어이다. 위에 설명한 dex파일은 달빅 '실행파일'이므로 사람이 읽기 어렵다. 그러므로 .dex <---> .smali 로 변환하여 인간이 읽을 수 있는 수준이 되어야 위변조를 할 수 있다. 참고로 smali 언어는 어셈블리어 기반의 언어라고 한다. 해커나 크래커, 보안 관련 전문가분들은 이 깊이까지 공부하실 것 같다..

6. <b>[메모용]</b> 공부하다가 알게된 추가내용(주제와 관련은 없음) : [여기](https://stackoverflow.com/questions/27548810/android-compiled-resources-resources-arsc)에 보면 arsc 관련 내용을 막 찾아보다가 적어두면 유용한 내용이 있어 메모 해 두려고 한다.  

<b>What's a complide XML file</b> - 컴파일 된 XML 파일은 리소스 참조가 해당 ID로 변경된 XML 파일과 같다. 예를 들어, ```@string/ok```는 0x7f000001로 바뀌는데, 이처럼 속성이 각각의 정수값으로 변경된다.  

<b>How Android resolves resources at runtimes</b> - ```inflater.inflate ()``` 메소드는 컴파일 된 xml 파일을 구문 분석하고 xml 노드를 인스턴스화하여 뷰 계층을 작성한다. 각 XML 노드는 Java 클래스 (예 : LinearLayout.java, RelativeLayout.java)에 의해 인스턴스화 되고, 인스턴스화하기 위해 inflater는 컴파일 된 xml 파일을 구문 분석하고 노드의 모든 속성을 수집하고 AttributeSet 유형의 압축 구조를 만들게 된다. 이 AttributeSet는 클래스 생성자에 전달되게 되고, 클래스 생성자는 AttributeSet의 실행에 대한 책임과 각 속성값을 분석하는 기능을 가지고 있다.
  
  
  <br>
  
#### Dalvik VM과 JVM :  
1. 안드로이드는 JVM을 사용하는 것이 아니다.  
Dalvik VM을 사용하는 것이며, 이는 JVM과 완전히 다르다.  
Android가 Java 언어를 사용하지만, 어플리케이션과 프레임워크 레벨만 Java로 쓰여져 있고, 나머지는 모두 C/C++ Native 코드가 동작하는 구조로 나뉘어져 있다.  
그리고 Dalvik VM과 Android Runtime, Framework의 관계를 알아가고, 잘 나뉘어진 Layer 덕분에 커널쪽은 전혀 손들 대지 않아도 된다. (루팅폰ㅠㅠ)  

2. Android SDK로는 Java소스를 Dalvik bytecode로 직접 컴파일 할 수 없다. 자바 컴파일러를 사용하여 자바 바이트코드를 생성한 뒤에, Dalvik 바이트코드로 변환하도록 SDK에 <b>dx</b>라는 툴을 포함시켜 두었다. 그래서 여러개의 .class파일들을 dx툴을 사용하여 하나의 .dex파일로 변환하는것이다.  
중요한 것은 .dex파일로 변환할 때에 자바 바이트코드를 Dalvik VM에서 사용하는 바이트코드로 변환하는데, 여기서 JVM과의 상관관계가 끊어지게 되는것!  
<small>(참고로 같은 .class 파일을 jar로 변환할 때 보다, .dex파일로 변환하면 크기가 조금 줄어든다고 한다.)</small>

3. dx툴을 사용해서 java 플랫폼 어플리케이션을 .dex으로 변환할 수 있는데, 이때 사용되는 Java는 Java SE라고 한다. SUN과의 라이센스 전쟁때문인 것 같다.  

4. Dalvik VM은 Dan Bornstein이 만들었으며 그 사람의 선조들이 살던 곳이 Dalvik이라는 곳이라고 한다..(TMI;;)  
위키피디아에 보면 JVM은 Stack-Based Architecture이고, Dalvik VM은 Register-Based Architecture 라고 하는데 두 개의 차이점이나 장단점은 공부를 더 해야 할 것 같다.  

5. ART는 Android Runtime의 약자로 앱을 설치할 때 완전히 네이티브 코드로 변환하여 설치하는데, 이 과정을 AOT(Ahead-Of-Time) 컴파일이라 한다. ART를 사용하면 코드 interpret 및 JIT(Just-In-Time) 컴파일하는 시간을 제거하여 퍼포먼스가 매우 향상될 수 있다.  

6. Android N 부터는 ART에 JIT를 적용해서 더 빠른 성능을 내도록 했다고 한다. 정확히는 최초 설치시에는 JIT를 사용하여 설치속도를 높이고, 기기를 사용하지 않을 때나 충전중일 경우 컴파일을 조금씩 하여, 자주 사용되는 앱을 AOT 방식으로 전환하는것으로 바뀌었다고 한다.  
JVM은 중간 언어를 읽어들일 때 한줄씩 읽어들이는 방식의 컴파일러인 interpreter이다.  
JIT는 인터프리터 형식으로 중간 언어를 읽지않고, 프로그램이 실행될 때 한꺼번에 읽어서 한번에 플랫폼이 알아들을 수 있는 언어로 번역을 한다.  
인터프리터 방식에 비해 속도가 빠르지만 당연히 Native 방식보단 느리다.  


<br>
더 이상 쓰면 끝도 없으니까 그냥 나머지는 Android Developers Blog나 developer.android.com 에서 보자.




