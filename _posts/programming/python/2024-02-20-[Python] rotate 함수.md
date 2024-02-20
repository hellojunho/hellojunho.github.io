---
layout: single
title: "📘[Python] rotate함수"
toc: true
toc_sticky: true
toc_label: "목차"
categories: python
excerpt: ""
tag: [python, rotate]
---

# rotate()란?
`rotate()`는 리스트의 원소들을 주어진 거리만큼 회전시키는 함수입니다.  
rotate()의 파라미터로는 정수가 올 수 있으며, 해당 정수만큼 회전시키는 함수입니다.  

## 예시(양수 파라미터)
```
from collections import deque

queue = deque([i for i in range(5)])
queue.rotate(1)

print(queue)    # [4, 0, 1, 2, 3]
```  

## 파라미터
파라미터에 양의 정수가 오면 오른쪽(시계 방향)으로 회전합니다.   
반대로 음의 정수가 오면 왼쪽(반시계 방향)으로 회전합니다.  

## 예시(음수 파라미터)
위에서 양의 정수가 오는 케이스에 대해 예시가 있으니 음의 정수가 오는 경우를 예시로 들어보겠습니다.  

```
from collections import deque

queue = deque([i for i in range(5)])
queue.rotate(-1)

print(queue)    # [1, 2, 3, 4, 0] 
```  

*2023-05-12 글인데, 카테고리를 잘못 적어서 이제야 올린다..*  