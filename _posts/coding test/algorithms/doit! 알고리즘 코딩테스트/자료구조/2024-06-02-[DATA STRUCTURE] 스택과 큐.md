---
layout: single
title: "📘[Data Structure] 스택과 큐?"
toc: true
toc_sticky: true
toc_label: "목차"
categories: algorithms
excerpt:
tag: [python]
---

# 스택과 큐

`스택` 과 `큐` 는 리스트에서 조금 더 발전한 형태의 자료구조임.

스택과 큐는 구조는 비슷하지만, 처리 방식에서 차이를 보인다.

## 스택

`스택(stack)` 은 삽입과 삭제 연산이 `후입선출(LIFO)` 로 이뤄지는 자료구조임.

후입 선출은 삽입과 삭제가 한 쪽에서만 일어난다는 특징이 있다.

> 컵을 생각하면 좋을듯!
> 

파이썬에서는 리스트를 이용해 스택을 쉽게 구현할 수 있다!

### 파이썬의 스택

- top: 삽입과 삭제가 일어나는 위치를 뜻함.
- s.append(): top 위치에 새로운 데이터를 삽입하는 연산
- s.pop(): top 위치에 현재 있는 데이터를 삭제하고 확인하는 연산
- s[-1]: top 위치에 현재 있는 데이터를 단순 확인하는 연산

> 스택은 DFS, 백트래킹 문제에서 효과적이므로 반드시 이해하고 넘어가자!
> 

## 큐

`큐(queue)` 는 삽입과 삭제 연산이 `선입선출(FIFO)` 로 이뤄지는 자료구조임.

스택과 다르게 먼저 들어온 데이터가 먼저 나간다!

그래서 삽입과 삭제가 양방향에서 이뤄진다는 특징이 있다.

> 파이프를 생각하면 좋을듯!
> 

### 파이썬의 큐

- rear: 큐에서 가장 끝 데이터를 가리키는 영역
- front: 큐에서 가장 앞의 데이터를 가리키는 영역
- s.append(data): rear 부분에 새로운 데이터를 삽입하는 연산
- s.popleft(): front 부분에 있는 데이터를 삭제하고 확인하는 연산
- s[0]: 큐의 맨 앞(front)에 있는 데이터를 확인할 때 사용하는 연산

> 큐는 BFS에서 자주 사용한다!
> 

## 우선순위 큐?

`우선순위 큐(priority queue)` 는 값이 들어간 순서와 상관 없이 **우선순위가 높은 데이터가 먼저 나오는 자료구조**임.

큐 설정에 따라 front에 항상 최댓값 또는 최솟값이 위치한다.

일반적으로 `힙(heap)` 을 이용해 구현하는데, 힙은 트리 종류 중 하나임!

# 관련 문제
| ⭐️ 문제는 구글링해서 다른 사람들의 코드를 보고 푼 코드 혹은 제대로 이해하지 못해 풀지 못한 문제들

[스택 수열](https://www.acmicpc.net/problem/1874)

[오큰 수](https://www.acmicpc.net/problem/17298) ⭐️

[카드 2](https://www.acmicpc.net/problem/2164)

- `오큰 수` 문제는 다시 풀어보자