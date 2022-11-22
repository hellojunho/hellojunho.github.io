---
layout: single
title: "MST(최소신장트리)에 대해서..."
toc: true
toc_sticky: true
toc_label: "목차"
categories: algorithms, tree
excerpt: "MST(최소신장트리)란?"
tag: [algorithms, tree]
---

# 📘 MST(최소 신장 트리)에 대해서...
`MST`란 '최소신장트리'로, 가장 비용이 적은 경로로 이루어진 트리를 말한다.  
MST를 알기 전에 `Spanning Tree`즉, `신장트리`부터 먼저 알아야 한다.

---
## 1. Spanning Tree (신장트리)
`Spanning Tree`란 그래프 내의 모든 `vertex`를 포함하는 트리이다.  
- Spanning Tree는 그래프의 '최소 연결 부분 그래프'이다. (즉, 그래프에서 일부 edge를 선택해서 만든 트리이다)
    - 최소 연결 = `edge`의 수가 가장 적음.  
    - n개의 `vertex`를 갖는 그래프의 최소 `edge`의 수는 `n-1`개이다.  
    - n-1개의 edge로 연결되어 있으면 반드시 트리형태가 되고, 이 것을 `신장트리`라고 한다.  

### 1-1. Spanning Tree의 특징
- `DFS(깊이우선탐색)`, `BFS(너비우선탐색)`을 이용하여 그래프에서 신장트리를 찾을 수 있다.
- 하나의 그래프에는 다양한 신장트리가 존재할 수 있고, 그 중 가장 적은 비용의 신장트리를 `MST(최소신장트리)`라고 한다.  
- 신장트리는 특수한 형태의 트리이므로, 모든 `vertex`들이 `connected`되어 있어야 하며, `cycle`을 형성해서는 안된다.  
- n개의 vertex를 정확히 n-1개의 간선으로 연결한다.  

---
## 2. MST (최소신장트리)
`Spanning Tree` 중에서 사용된 `edge`의 `weight(가중치)`의 합이 가장 작은 트리이다. 
- 그래프에 있는 모든 vertex들을 가장 적은 비용의 edge들로 연결함

### 2-1. MST의 특징
- edge의 가중치의 합이 최소여야 한다.  
- n개의 vertex를 가지는 그래프에 대해 반드시 n-1개의 간선만 사용한다.
- `cycle`이 있어선 안된다.  

### 2-2. MST 구현 알고리즘
#### 2-2-1. Kruskal Algorithm
`greedy algorithm(탐욕 알고리즘)`을 이용하여 그래프의 모든 vertex를 최소 비용으로 연결하는 알고리즘  

[과정]
> 1. 그래프의 edge들을 weight의 오름차순으로 정렬한다.
> 2. 정렬된 edge리스트에서 순서대로 cycle을 형성하지 않는 edge를 선택한다.
>       - 가장 낮은 가중치를 먼저 선택
>       - cycle을 만드는 edge는 제외
> 3. 해당 edge를 현재의 MST의 집합에 추가한다.  

[예제코드]
- 파이썬([출처](https://techblog-history-younghunjo1.tistory.com/262))
```
import sys

v, e = map(int, input().split())
# 부모 테이블 초기화
parent = [0] * (v+1)
for i in range(1, v+1):
    parent[i] = i

# find 연산
def find_parent(parent, x):
    if parent[x] != x:
        parent[x] = find_parent(parent, parent[x])
    return parent[x]

# union 연산
def union_parent(parent, a, b):
    a = find_parent(parent, a)
    b = find_parent(parent, b)
    if a < b:
        parent[b] = a
    else:
        parent[a] = b

# 간선 정보 담을 리스트와 최소 신장 트리 계산 변수 정의
edges = []
total_cost = 0

# 간선 정보 주어지고 비용을 기준으로 정렬
for _ in range(e):
    a, b, cost = map(int, input().split())
    edges.append((cost, a, b))

# 간선 정보 비용 기준으로 오름차순 정렬
edges.sort()

# 간선 정보 하나씩 확인하면서 크루스칼 알고리즘 수행
for i in range(e):
    cost, a, b = edges[i]
    # find 연산 후, 부모노드 다르면 사이클 발생 X으므로 union 연산 수행 -> 최소 신장 트리에 포함!
    if find_parent(parent, a) != find_parent(parent, b):
        union_parent(parent, a, b)
        total_cost += cost

print(total_cost)
```

- 자바([출처](https://sskl660.tistory.com/72))
```
import java.util.Arrays;
import java.util.Scanner;

/*
sample input(첫 줄의 첫 숫자는 정점의 개수, 두 번째 숫자는 간선의 개수).
6 9
1 6 5
2 4 6
1 2 7
3 5 15
5 6 9
3 4 10
1 3 11
2 3 3
4 5 7
 */

public class Kruskal {
	static int V, E;
	static int[][] graph;
	// 각 노드의 부모
	static int[] parent;
	// 최종적으로 연결된 최소 신장 트리 연결 비용.
	static int final_cost;

	public static void main(String[] args) {
		// 그래프의 연결상태(노드1, 노드2, 비용)를 초기화.
		Scanner sc = new Scanner(System.in);
		V = sc.nextInt();
		E = sc.nextInt();
		graph = new int[E][3];
		for (int i = 0; i < E; i++) {
			graph[i][0] = sc.nextInt();
			graph[i][1] = sc.nextInt();
			graph[i][2] = sc.nextInt();
		}
		parent = new int[V];
		final_cost = 0;

		// 간선 비용 순으로 오름차순 정렬
		Arrays.sort(graph, (o1, o2) -> Integer.compare(o1[2], o2[2]));

		// makeSet
		for (int i = 0; i < V; i++) {
			parent[i] = i;
		}
		// 낮은 비용부터 크루스칼 알고리즘 진행
		for (int i = 0; i < E; i++) {
			// 사이클이 존재하지 않는 경우에만 간선을 선택한다(여기서는 최종 비용만 고려하도록 하겠다).
			if (find(graph[i][0] - 1) != find(graph[i][1] - 1)) {
				System.out.println("<선택된 간선>");
				System.out.println(Arrays.toString(graph[i]));
				union(graph[i][0] - 1, graph[i][1] - 1);
				final_cost += graph[i][2];
				System.out.println("<각 노드가 가리키고 있는 부모>");
				System.out.println(Arrays.toString(parent) + "\n");
				continue;
			}
		}
		
		System.out.println("최종 비용 : " + final_cost);
		sc.close();
	}

	private static void union(int a, int b) {
		a = find(a);
		b = find(b);
		if (a > b) {
			parent[a] = b;
		} else {
			parent[b] = a;
		}
	}

	private static int find(int x) {
		if (parent[x] == x)
			return x;
		else
			return find(parent[x]);
	}
}
```

#### 2-2-2. Prim Algorithm