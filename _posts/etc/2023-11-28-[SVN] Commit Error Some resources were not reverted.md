---
layout: single
title: "📘[SVN] Commit Error: Some resources were not reverted"
toc: true
toc_sticky: true
toc_label: "목차"
categories: etc
excerpt: ""
tag: [SVN]
---

# SVN 커밋 에러
SVN을 통해 내가 작업한 코드를 커밋(commit)하려고 하는데 아래와 같은 에러메시지와 함께 에러가 발생했다.  

[에러 메시지]  
```
Some resources were not reverted.
Unable to make field private java.lang.Throwable java.lang.Throwable.cause accessible: module java.base does not "opens java.lang" to unnamed module @24f4facb
```  

위 문제를 해결하기 위해 먼저 chatGPT에 물었더니 다음과 같은 해결책을 제시했다.  

![image](https://github.com/hellojunho/hellojunho.github.io/assets/104587537/18126386-a967-4897-9efd-a6e28dbe73d2)  

<br>

나는 인턴으로 근무를 하고 있던 중이었으므로, 사내 프로젝트의 개발 환경 자체를 내가 작업하는 코드에만 이기적으로 맞출 수가 없을 것 같다고 판단했다.  
다른 방법이 없으면 이렇게라도 해야겠지만, 먼저 chatGPT에게만 검색하는 것이 아니라 구글링을 먼저 하고 그래도 어려우면 선임 개발자분께 여쭤보려고 했다..  
<br>

그런데 구글링을 하니 한 블로그에서 해결책을 제시했다.  
방법은 그 `폴더를 지우고 업데이트를 다시 받으라는 것!`  

다행히 나는 노션에 코드를 백업해놨기 때문에 과감하게 폴더를 지우고 업데이트를 실행했다.  

그러니 다행히 정상적으로 커밋이 가능해졌다.  

화면 설계서에 적힌 요구사항대로 구현을 완료했는데, 커밋이 안돼서 프로젝트 마감이 늦어진다면 억울할 뻔 했다.  

# 참고자료
[[SVN] Some resources were not reverted](https://blog.naver.com/marnet/80202947208)  