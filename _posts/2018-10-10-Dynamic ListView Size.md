---
layout: post
title: "Android ListView Dynamic Height"
description: "ListView item 갯수만큼 동적 높이 조절"
date: 2018-10-10
tags: [Android, ListView]
comments: true
share: true
---
## ListView item 갯수만큼의 동적 높이 조절

ListView 에서 height를 wrap_content로 하면 item 하나만큼의 높이로 설정된다.  
item 전체만큼의 높이를 보여주고 싶다면 아래와 같은 코드가 필요하다.  
  

```java
// ListView 크기 조절
int totalHeight = 0;
for (int i = 0; i < mListViewAdapter.getCount(); i++) {
    View listItem = mListViewAdapter.getView(i, null, mListView);
    listItem.measure(0, 0);
    totalHeight += listItem.getMeasuredHeight();
}
ViewGroup.LayoutParams params = mListView.getLayoutParams();
params.height = totalHeight + (mListView.getDividerHeight() * (mListViewAdapter.getCount() - 1));
mListView.setLayoutParams(params);
```

