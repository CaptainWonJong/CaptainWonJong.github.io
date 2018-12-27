---
layout: post
title: "How to Delete Android Button Shadows"
description: "안드로이드 버튼 그림자 삭제 방법"
date: 2018-12-27
tags: [Android, Button, Style]
comments: true
share: true
---

## 안드로이드 버튼 그림자 삭제 방법

연말을 맞아 열심히 프로젝트 1달째에 들어가고 있어 매우 바쁜 와중에 나중에 잊어버리지 않도록 간단한 포스팅을 한다.ㅠㅠ

1. Button 속성 추가 방법
```xml
    <Button 
        ...
        style="?android:attr/borderlessButtonStyle" />
```
위 속성을 Button 에 추가 해 주면 된다.  
그러나 개발중인 앱은 style.xml에 커스텀한 버튼을 만들었기 때문에 아래와 같은 방법을 사용하였다.  
<br>

2. style.xml의 CustomButton에 속성 추가

```xml
<style name="MyCustomButton" parent="@android:style/Widget.Button">
    ...
    <!-- 아래 속성을 추가 -->
    <item name="android:stateListAnimator" tools:targetApi="lollipop">@null</item> 
</style>
```

#### 끝


참고사이트 :



