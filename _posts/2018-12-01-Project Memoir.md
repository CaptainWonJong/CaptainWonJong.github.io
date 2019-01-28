---
layout: post
title: "Project Memoir"
description: "프로젝트 회고록 (18.12.01 ~ 19.03.31)"
date: 2018-12-01
tags: [Android, Memoir]
comments: true
share: true
---

## 프로젝트 개발 회고록 (18.12.01 ~ 19.03.31)

#### [실수] 18.12.18
1. 디자인팀에서 준 나인패치 파일을 drawable에 넣었더니 깨짐현상 발생  
--> drawable 파일이 아닌 drawable-xxhdpi 폴더에 넣으니 정상적으로 동작함  
**디자인팀에게 제작한 해상도를 물어보고 해상도에 맞게 폴더를 구성해서 넣을것.**   

2. git 브랜치 줄기를 잘못 나눠 꼬임현상 발생  
--> remote 브랜치와 local 브랜치에 대한 개념이 부족하여 발생한 문제로, 잘못된 브랜치는 삭제하거나 다른 브랜치와 merge하여 해결  
**Git에 대한 개념을 확실히 잡고, 사용방법을 커맨드를 사용하여 공부할 것.**  

#### [정보] 18.12.19
1. @BindingAdapter는 전체 공통으로 커스텀한 동작을 만들지만, 잘 쓰진 않는다.
2. Observablefield vs LiveData 비교 한다면 LifeCycle의 관리 유무가 크기 때문에 LiveData를 많이 사용한다.
3. MutableLiveData vs ImMutableLiveData 비교 한다면 주로 MutableData를 사용하게 되는데 초기화를 하지 않아도 동작이 잘 된다(?)고 하는데 **추가로 공부가 필요**하다. 


#### [정보] 18.12.20
1. 구글은 정말 천재인 것 같다. <https://developer.android.com/jetpack/docs/guide?hl=ko>  
2. 전역으로 옵저버블한 변수나 객체를 어떻게 처리할 수 있을까?  


#### [정보] 19.01.25

1. @BindingAdapter 에서 2가지 이상의 파라미터를 필요로 할 때, 아래와 같이 사용한다.  

```java
@BindingAdapter({"bind:setProfileImage", "bind:setProfileType"})
public static void setProfileImage(ImageView imageView, String imageUrl, String imageType) {
... 
}
```

```xml

<ImageView
android:layout_width="40dp"
android:layout_height="40dp"
bind:setProfileImage="@{viewModel.myModel.image}"
bind:setProfileType="@{viewModel.myModel.imageType}"/>
```  

#### [정보] 19.01.28

1. git을 사용하다 commit을 잘못 했을 때 되돌리는 방법  
- 먼저 잘못 commit한 branch로 간다.  
- 아래 명령어를 실행하면 바로 이전으로 되돌릴 수 있다.  
```
git reset HEAD^
```
2. HEAD는 무엇일까?
- 사용자가 commit을 할 때 아래 사진과 같이 이상한 문자열이 있는데, 이것이 commit한 곳을 가르키는 HEAD 주소값(?) 이다.  
- 자세한 원리는 [여기](https://bit.ly/2B7zPkt)를 참고하자. 아주 자세히 나와있다.  
