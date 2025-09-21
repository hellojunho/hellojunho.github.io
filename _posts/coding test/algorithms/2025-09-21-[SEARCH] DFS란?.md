---
layout: single
title: "📘[Search] DFS란?"
toc: true
toc_sticky: true
toc_label: "목차"
categories: algorithms
excerpt:
tag: [search, dfs, graph, tree]
---

# DFS란?

알고리즘 문제 해결 방법 중 하나인 `DFS`.

`깊이 우선 탐색`의 약자로, 그래프의 깊은 부분을 우선적으로 탐색하는 알고리즘임.

시작 노드에서 출발해서, 한 방향으로 갈 수 있는 한 깊게 탐색한 후, 더 이상 갈 곳이 없으면 이전 분기점으로 돌아가 다른 경로를 탐색함.

왜냐면 DFS는 스택이나 재귀를 사용해서 구현할 수 있고, 모든 노드를 방문하고자 할 때 유용하기 때문임.

DFS는 경로의 존재 여부, 그래프의 연결 요소 찾기 등에 사용됨.

## python 수도코드

```python
def DFS(graph, node, visited):
    if node in visited:
		    return
   	visited.append(node)
   	
   	for i in graph[node]:
   	    if i not in visited:
   	        DFS(graph, i, visited)
```

- `if node in visited:`: 방문 기록에 node가 있으면?
    - `return` : 한 번 방문한 노드는 또 다시 방문하지 않기 때문에 종료
- `visited.append(node)`: 방문 기록에 node가 없으면 해당 node 방문 기록에 추가
- `for i in graph[node]:`: node의 leaf 노드들을 탐색
- `if i not in visited:`: node의 leaf 노드들 중  방문 기록에 없으면?
    - `DFS(graph, i, visited)`: DFS를 다시 실행(재귀)

## 적용 사례

DFS는 BFS와 같이 그래프를 전체 탐색하는 알고리즘임.

이 방법들을 사용하여 풀 수 있는 대표적인 문제들이 `미로 찾기`, `퍼즐 게임`, `네트워크 연결 상태 확인` 등이 있음.

단, DFS는 가능한 모든 경로를 탐색하며 해결책을 찾고, BFS는 최단 경로를 찾는 방식에서 차이가 있긴 함!

코딩테스트 준비할 때 필수 알고리즘임..!