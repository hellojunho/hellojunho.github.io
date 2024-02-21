---
layout: single
title: "📘[Search] DFS에 대해서..."
toc: true
toc_sticky: true
toc_label: "목차"
categories: algorithms
excerpt:
tag: [search, graph, tree]
---

# 그래프 탐색
`그래프 탐색`이란, **하나의 정점으로부터 시작해서 차례대로 모든 정점들을 한 번씩 방문하는 동작**을 말한다.  
그래프 탐색에 쓰이는 알고리즘은 많지만, 그 중 가장 `DFS`에 대해서 정리한다.  


# DFS(Depth First Search) - 깊이 우선 탐색
`DFS`란, **루트노드에서 시작해, 다음 분기(branch)로 넘어가기 전에 해당 분기를 완벽하게 탐색하는 방법**을 말한다.  

![image](https://github.com/hellojunho/hellojunho.github.io/assets/104587537/d0881d39-4e8d-4b1a-a000-b36a9afad3a6)    

- 미로를 탐색할 때, 한 방향으로 갈 수 있을 때까지 가다가 더 이상 갈 수 없게 되면 다시 가장 가까운 갈림길로 돌아와서 
다른 방향으로 탐색을 다시 하는 것과 유사 -> **재귀와 동일한 형태의 알고리즘**  
- 즉, 넓게 탐색하기 전에 `깊게` 탐색하는 방식  
- 사용하는 경우: **모든 노드를 방문**하고자 하는 경우  
- BFS보다 조금 더 간단하지만, 속도는 느림  

## DFS 특징
- 자기 자신을 호출하는 **순환(재귀) 알고리즘**의 형태를 갖는다.  
- `전위 순회(Pre-Order Traversals)`를 포함한 다른 형태의 트리 순회는 모두 DFS의 한 종류  
- **어떤 노드를 방문했었는지 여부를 반드시 검사**해야한다. 안그러면 무한루프 위험  
<br>

![image](https://github.com/hellojunho/hellojunho.github.io/assets/104587537/1de0c790-2669-4624-9317-165f1a7a1a9a)  
1. a 노드(루트 노드)를 방문
   - 방문한 노드는 표시  
2. a와 인접한 노드들을 차례로 순회  
   - 인접한 노드가 없으면 종료  
3. a와 이웃한 노드 b를 방문했다면, a와 인접한 또 다른 노드를 방문하기 전에 b의 이웃 노드들을 전부 방문해야 함
   - b를 루트 노드로 DFS를 다시 시작 -> b의 이웃 노드 방문
4. b의 분기를 전부 완벽 탐색 했다면, 다시 a에 인접한 이웃 노드를 방문할 수 있다는 의미  
   - b의  분기를 전부 완벽 탐색한 뒤에야 a의 다른 이웃 노드로 갈 수 있다는 의미
   - 아직 방문 안된 정점이 없으면 종료
   - 방문 안한 점이 있으면 그 점을 시작으로 다시 DFS

## DFS 구현
DFS의 구현 방식은 2가지로 나눌 수 있다.  
1. 순환 호출 사용
2. 명시적인 스택 사용: 명시적인 스택을 사용해 방문한 정점들을 스택에 저장했다가 다시 꺼내서 작업  

### 스택/큐를 활용한 DFS 구현
1. 리스트를 활용한 구현  
```python
def dfs(graph, start_node):
 
    ## 기본은 항상 두개의 리스트를 별도로 관리해주는 것
    need_visited, visited = list(), list()
 
    ## 시작 노드를 시정하기 
    need_visited.append(start_node)
    
    ## 만약 아직도 방문이 필요한 노드가 있다면,
    while need_visited:
 
        ## 그 중에서 가장 마지막 데이터를 추출 (스택 구조의 활용)
        node = need_visited.pop()
        
        ## 만약 그 노드가 방문한 목록에 없다면
        if node not in visited:
 
            ## 방문한 목록에 추가하기 
            visited.append(node)
 
            ## 그 노드에 연결된 노드를 
            need_visited.extend(graph[node])
            
    return visited
```  
- list에서 pop을 활용하면 성능이 떨어지는 단점  

2. deque를 활용한 구현  
```python
def dfs2(graph, start_node):
    ## deque 패키지 불러오기
    from collections import deque
    visited = []
    need_visited = deque()
    
    ##시작 노드 설정해주기
    need_visited.append(start_node)
    
    ## 방문이 필요한 리스트가 아직 존재한다면
    while need_visited:
        ## 시작 노드를 지정하고
        node = need_visited.pop()
 
        ##만약 방문한 리스트에 없다면
        if node not in visited:
 
            ## 방문 리스트에 노드를 추가
            visited.append(node)
            ## 인접 노드들을 방문 예정 리스트에 추가
            need_visited.extend(graph[node])
                
    return visited
```  
- 스택/큐를 구현할 때는 collections 패키지에서 deque를 활용하는 것이 list보다 성능이 뛰어남  

### 재귀함수를 통한 DFS 구현
```python
def dfs_recursive(graph, start, visited = []):
## 데이터를 추가하는 명령어 / 재귀가 이루어짐 
    visited.append(start)
 
    for node in graph[start]:
        if node not in visited:
            dfs_recursive(graph, node, visited)
    return visited
```  
- visited 자료형을 기본 함수 인자로 포함시키고, 방문한 리스트들을 재귀를 통해 계속 visited에 담는 방식  

# 참고자료
[DFS 완벽 구현하기[Python]](https://data-marketing-bk.tistory.com/entry/DFS-%EC%99%84%EB%B2%BD-%EA%B5%AC%ED%98%84%ED%95%98%EA%B8%B0-%ED%8C%8C%EC%9D%B4%EC%8D%AC)  

[[알고리즘] 깊이 우선 탐색(DFS)이란](https://gmlwjd9405.github.io/2018/08/14/algorithm-dfs.html)  

[DFS/BFS 개념 및 문제 접근 방법](https://devraphy.tistory.com/629)  
