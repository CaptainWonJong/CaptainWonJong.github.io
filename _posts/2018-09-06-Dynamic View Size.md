---
layout: post
title: "Dynamic View Size with Glide"
description: "Glide를 이용한 동적 뷰 크기 조절"
date: 2018-09-06
tags: [Android, Glide]
comments: true
share: true
---

## Glide를 이용한 동적 뷰 크기 조절

이미지뷰의 경우 아래와 같이 adjustViewBounds 옵션만 주면 자동으로 width에 맞춰 비율을 유지한채 height값이 변하게 된다.
```xml
<ImageView
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:adjustViewBounds="true">
</ImageView>
```
```java
Glide.with(mContext).load(IMAGE URL).into(IMAGEVIEW ID);
```

근데 ViewPager나 기타등등의 View들은 자동으로 안바뀌어서 이번에 프로젝트를 하며 하나 만들어놓았다.

1. 스레드를 이용하여 서버에 있는 이미지 url에 접근한다. (여기서는 Glide가 알아서 해줌)
2. 이미지의 높이(px), 너비(px)값을 변수로 가지고 있는다.
3. layoutParams를 이용하여 View의 크기를 강제적으로 바꿔준다
    **이 때, ViewTreeObserver의 OnGlobalLayoutListener를 사용**
4. aHeight : aWidth = bHeight : bWidth 일 때, 
    bHeight = (aHeight * bWidth) / aWidth 인 것을 적용한다.
  
  
#### xml

```xml
<!-- layout to apply margin -->
<LinearLayout
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:layout_marginLeft="10dp"
    android:layout_marginRight="10dp">

    <!-- dynamic size view -->
    <include
        android:id="@+id/ic_main_dynamic_layout"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        layout="@layout/include_main_marketing_banner">
    </include>
</LinearLayout>
```

  
#### java

```java
/**
* 이미지 사이즈에 맞춰 View의 높이를 이미지 크기에 맞게 자동으로 변경
* (View, LinearLayout 적용가능)
* @param context
* @param imageUrl
* @param view
*/
public static void setAutoSizeView(Context context, String imageUrl, View view) {

    final View changeView = view;

    // image url에 연결해서 이미지의 width, height를 받아온다.
    Glide.with(context)
        .load(imageUrl)
        .override(Target.SIZE_ORIGINAL, Target.SIZE_ORIGINAL)
        .into(new SimpleTarget<GlideDrawable>() {
            @Override
                public void onResourceReady(GlideDrawable glideDrawable, GlideAnimation<? super GlideDrawable> glideAnimation) {
                    final int imageHeight = glideDrawable.getIntrinsicHeight();
                    final int imageWidth = glideDrawable.getIntrinsicWidth();

                    // 파라미터로 받아온 width, height를 view에 적용한다.
                    ViewTreeObserver vto = changeView.getViewTreeObserver();
                    vto.addOnGlobalLayoutListener(new ViewTreeObserver.OnGlobalLayoutListener() {
                        @Override
                        public void onGlobalLayout() {
                            changeView.getViewTreeObserver().removeGlobalOnLayoutListener(this);
                            int width = changeView.getMeasuredWidth();
                            int height = (width * imageHeight)/imageWidth;

                            changeView.setLayoutParams(new LinearLayout.LayoutParams(width, height));
                        }
                    });
                }
            });
}
```

















