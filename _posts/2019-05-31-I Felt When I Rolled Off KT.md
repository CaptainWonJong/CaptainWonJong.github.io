---
layout: post
title: "I Felt When I Rolled Off KT"
description: "KT에서 롤오프하며 느낀점"
date: 2019-05-31
tags: [Android, KT]
comments: true
share: true
---

## 2018년 12월에 만난 KT

약 1년 반동안 KT에서 프로젝트를 맡아왔었다.  

처음 2018년 1월에는 모바일개발자 4명이 KT멤버십, KT패밀리박스를 맡아 일을 진행했었고,  

그 해 3월쯤 4명의 개발자가 각각 멤버십과 패밀리박스의 안드로이드, iOS를 책임지고 담당하게 되었다.  

그렇게 KT패밀리박스 안드로이드의 개발자가 되었다.  

처음 KT패밀리박스의 스토어 평점은 3점 초반대였고, 2018년 3월 무리한 일정과 불안정한 KT 내부 라이브러리 적용으로 인해 앱의 크래시율은 2%까지 치솟았었다.  

그러나 꾸준한 안정화 작업과 추가 개발로 인해 패밀리박스 앱은 2019년 5월 기준 평점 4.3까지 올라와있다. (iOS는 4.6)  

오늘부터는 지금까지 패밀리박스의 2번의 고도화 작업(V3.0, V4.0)을 하고,  
SM과제를 통해 앱의 안정화 + 추가기능개발을 하고,  
최근 관리자앱을 새로 개발하며 느낀점과 적용한 기술들을 하나씩 적어가려고 한다.  

(물론 KT 보안에 위배되지 않도록 적을 것이다..ㅎㅎ)

그리고 이 글은 분명 미래의 나에게 좋은 영양제가 될 수 있을 것이다.  

![familybox.jpeg](https://captainwonjong.github.io/images/190531_roll_off_kt/familybox.jpeg)  



## 2018년 12월부터 지금까지 만난 사람들

KT에서 일을 하며 내가 가장 크게 얻은 것 중 하나는 사람이었다.  

내가 온전히 KT패밀리박스의 안드로이드를 개발할 수 있도록 도움을 주신 분들이 많았다는 것만으로도 큰 행복이었다.    

프로젝트를 하며 모두 같은 목표를 바라봐야 하고, 목표를 향해 달려가다 부딪히는 상황이 생겼을 때 현명하게 극복하는 방법을 가장 많이 고민했었다.  

그런데 고민이 무색할 정도로 크게 부딪히는 일 없이 비교적 순조롭게 프로젝트가 진행되었다고 생각한다.  
<small>(물론 트러블이 아예 없지는 않았다.. 크흠..)</small>  

같이 일을 하시는 분들께서 내가 부족할 때 많이 도와주셨기 때문이라고 생각하며 항상 감사드리고 있다.  

프로젝트는 결코 혼자서 완성할 수 없기에, KT에서 같이한 모든 사람들과 시간들이 가치있었다.  

![football.jpg](https://captainwonjong.github.io/images/190531_roll_off_kt/football.jpg)  



## 2018년 12월부터 지금까지 나는?

사람은 완성되기 위하여 성장해야 한다. 인생은 결국 나 하나 만들기이다.  

고민했다. 약 1년 6개월동안 KT에 있으며 어떤것을 배우고 성장할지, 나는 어떤 개발자가 되고 싶은지, 그리고 무엇을 하고싶은지.  

고민하고 있다. 나는 사람들에게 어떻게 더 가치있는 프로그램을 만들어 줄 수 있는 개발자가 될 수 있을지.  

지난 1년 6개월동안 항상 고민하며 일을 했었다. 그리고 고민을 해결하기 위해 당장 할 수 있는 것 부터 시작했다.  

내가 당장 할 수 있는 것은 부족한 실력을 메꾸기 위해 계속 공부하는 것이었고, 너무 감사하게도 공부한 내용을 실무에 적용할 수 있는 기회가 너무 많이 있었다.  

물론 공부할 수 있도록 옆에서 도와주신 분들도 많이 있었다. <small>감사합니다.</small>  

다음 포스팅부터는 내가 공부한 내용들을 실무에 적용한 내용들을 적으려한다. 물론 KT보안에 위배되지 않는 선까지만..  

물론 목표와 고민은 아직도 내 머릿속에 꾸준히 맴돌고 있다.  

![study.jpg](https://captainwonjong.github.io/images/190531_roll_off_kt/study.jpg)  



## 지금부터 앞으로의 나는?

대충 살고싶다.  

대충 살아도 잘 살고싶다.  

그래서 대충 살아도 잘 살 수 있도록 항상 올바른 방향으로 열심히 살아가야겠다.  

일단 좀 쉬고, 지금까지 안드로이드 공부했던 것들을 실무에 적용한 내용들을 블로그에 하나씩 적으려고 한다.  

예전에 귀찮아서 구체적으로 못적었던 Git-Flow를 KT패밀리박스에 맞게 살짝 변형한 Git 관리 방법이라던가,  

4.0 고도화 작업을 할 때 메인에 적용했던 MVVM 패턴이라던가,  

최근에 Kotlin, Rx, Dagger등을 공부하며 개발한 관리자앱 등등..  

열심히 적어놓으면 나중에 대충 봐도 기억이 나겠지..?  

일단 적기 전에 친구들 만나서 좀 놀아야겠다ㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋ



## 끝
