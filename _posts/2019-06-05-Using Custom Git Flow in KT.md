---
layout: post
title: "Using Custom Git Flow in KT"
description: "KT에서 사용한 커스텀 깃플로우 전략"
date: 2019-06-05
tags: [Git, KT]
comments: true
share: true
---

## Git Flow   
먼저 아주 유명한 Git Flow 이미지를 보자  
  
![gitFlow.png](https://captainwonjong.github.io/images/190605_git_flow/gitFlow.png)  
  
## Custom?
Git Flow는 크게 5개의 브랜치를 사용하고 있다.  
**master, develop, release, hotfix, feature**  

그러나 내가 KT에서 사용할 때는 사실 5개까지 필요가 없어 release branch를 삭제하고 아래와 같이 용도를 변경하였다.  
1. **master** : 현재 상용버전으로 구글 마켓과 원스토어에 올라간 apk의 버전  
2. **develop** : QI 이후 최신의 검증버전 ( - 검증 때 마다 push)   
3. **hotfix** : 배포 전 검증 중 긴급 수정 시 push  
4. **feature** : 새로운 요구사항 및 고도화 등의 신규개발 시 개발 기능이나 WBS의 마일스톤 아이디로 트리형식의 구조로 생성  
  
<br>
위와 같이 release를 삭제하고 4개의 브랜치를 사용하였다.  
  
특히나 많이 사용한 브랜치는 당연히 feature 브랜치로, 주로 아래와 같이 사용하였다.  
  
1. 4.0 고도화 등 신규개발이 필요할 때 WBS의 마일스톤 ID 및 기능 ID로 신규 브랜치 생성  
(ex : feature/04.00.00/M1/1.3, feature/04.00.00/M1/1.4, feature/04.00.00/M2/4.1 등등..)  
2. 유지보수하며 생긴 SM 과제 기능으로 신규 브랜치 생성  
(ex : feature/03.00.02/Benefit, feature/03.00.02/Navigation, feature/03.00.03/MarketingBanner, 등등..)  
  
<small>(다시 보니 마켓 업데이트 버전도 앞에 적어주었었다.)</small>  
  
개인적으로는 커맨드로 입력하는것도, 소스트리를 이용하는것도 좋았지만 시각적으로 브랜치 구조를 확인하기에는 소스트리가 최고였던 것 같다.  
그리고 위와같이 "/" 를 통해 브랜치의 깊이를 나눠준다면 소스트리에서 브랜치가 트리구조로 내려가는것도 확인하였다.  

왜 release를 제외했는지는 내부적인 구조때문에 이야기 할 순 없지만 내가 찾은 KT패밀리박스에서의 git branch 전략은 저게 최선이었던 것 같다.ㅠㅠ  
  
사실 처음에는 안드로이드는 혼자 담당하는데 다른 사람들처럼 SVN 쓰듯이 1자로 쭉 add, commit, push만 할까도 생각했었다.  
그러나 평생 나 혼자 개발할 것이 아니기에 패밀리박스를 하며 1인 다역을 해서라도 깃을 사용하여 협업하는 습관을 기르고 싶었기에 위와 같은 전략을 세우게 되었다.  
KT가 내부적으로 조금 더 간소화된 프로세스를 가지고 있었다면, branch도 줄지 않았을까..ㅎㅎ  

## 끝
