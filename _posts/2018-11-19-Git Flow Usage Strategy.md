---
layout: post
title: "Git Branch Usage Strategy"
description: "Git Branch 사용 전략"
date: 2018-11-19
tags: [git, git flow]
comments: true
share: true
---

## Git Branch 사용 전략

먼저 이 글은 안드로이드 프로젝트에 협업하며 사용 예정/사용 중/사용할 수도 있는 Git 전략을 메모합니다.  
12월에 프로젝트를 하나 진행할텐데, Git을 사용하며 Branch를 약속하는 도중 개발자분께 배운 내용을 잊지 않기 위에 메모합니다. <small>(다듬을 예정)</small>   

글을 쓰기에 앞서 Git-Flow를 잘 정리해 준 [우아한 형제들 기술 블로그](http://woowabros.github.io/experience/2017/10/30/baemin-mobile-git-branch-strategy.html)에 감사의 인사를 드립니다.  

만약 이 기술블로그를 알지 못했다면, Git-Flow 뿐 아닌 Github-Flow, Gitlab-Flow등을 모르고 살아왔을지도 모릅니다..ㅠㅠ  
<br>

이번에 Git에서 새롭게 알게된 점
1. git stash  
-> feature branch에서 commit을 하지 않고 develop branch 등 다른 branch로 이동을 할 수 없을 때, 클립보드처럼 현재 브랜치의 상태를 저장하고 다른 브랜치로 이동 가능
-> source tree 연동 시 아래 표시한 곳에 쌓이게 된다.

2. git과 jenkins의 연동  
-> 자동 빌드를 통한 apk 및 ipa 파일 확인  
-> develop branch에서 사용 시 유용  
-> 자동 빌드 시 console에서 빌드하는 것들을 jenkins에 설정 해 주어야 한다고 하지만 나는 아직 잘 모름;;

3. git과 jira의 연동  
-> feature branch에서 develop branch로 merge 할 때, jira를 통해 이슈 등 공유 가능  
-> feature 갯수만큼 jira의 이슈가 늘어나지만 나쁜 것은 아니라고 함.

4. git과 gitlab의 연동  
-> github 비공계처럼 사용하면 되지만, 브랜치에 권한을 줘서 develop이나 master에 푸시하기 전, 리더의 QA 후 리더가 푸시 가능  
(-> 푸시 권한 필요시 연동하면 좋음)  


며칠 간 해외여행에 예비군에 블로그를 할 시간이 많이 없었지만, 다시 힘내야 겠다.   
<small>(GDG Festa! Seoul 리뷰도 쓰고싶은데......)</small>  
<small>(Gradle Tip도 빨리 써야하는데......)</small>  
<small>(iOS 배포하는 방법과 이슈들도 적어놔야 하는데......)</small>  




