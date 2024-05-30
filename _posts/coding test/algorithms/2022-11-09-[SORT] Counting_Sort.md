---
layout: single
title: "📘[Sort] Counting_Sort (계수 정렬)"
toc: true
toc_sticky: true
toc_label: "목차"
categories: algorithms
excerpt: 
tag: [sort]
---

# Counting_Sort (계수 정렬)

## 1. 계수 정렬이란?
주어진 배열의 값 범위가 작은 경우, O(n + k)의 시간복잡도를 갖는 정렬 알고리즘 (k: 주어진 배열의 가장 작은 요소부터 가장 큰 요소까지의 범위)  

- 장점: 입력 데이터의 비교 없이 이루어지는 정렬이므로, 정렬 속도가 빠름
- 단점: 크기가 한정되어 있는 데이터에 대해서만 정렬할 수 있음
- 정렬의 목적은 "탐색 기능의 향상"이고, 계수 정렬은 O(n + k)의 매우 빠른 속도를 가진 정렬이므로, 특정 조건에 대해서는 가장 빠른 알고리즘 중 하나임 (보통 O(n)으로 나옴)

---

## 2. 계수 정렬의 동작 과정
1. 입력 배열을 A, 최종 출력 배열을 B, 정렬을 위해 만드는 배열을 C라고 하자.  
2. 배열-C의 크기는 배열-A의 요소 중 가장 큰 값으로 지정한다. (ex) A=[1,5,3,3,2,1] -> C[5])  
3. 배열-A의 요소별 개수를 배열-C의 인덱스값과 일치시켜 저장한다. (ex) A=[1,5,3,3,2,1] -> C=[0,2,1,2,0,1])
4. 배열-C에서, 0번째 부터 차례대로 직전 요소의 값들과 더해가, 새로운 배열-C를 만든다. (ex) A=[1,5,3,3,2,1] -> C=[0,2,3,5,5,6]
5. 배열-B의 크기는 C의 마지막 인덱스의 값으로 지정한다. (ex) C[5]=6 -> B[6])
6. 배열-A의 마지막 노드부터 차례대로 배열-C와 비교하여 B에 대입한다. (ex) A[6]=1 -> C=[0,2,3,5,5,6]에서 1이 하나 빠져나가므로 C[1]--이 되어, C=[0,1,3,5,5,6] -> B=[null,null,1,null,null,null,null]
7. 배열-A의 첫 번째 노드까지 완료할 때까지 위 과정을 반복한다.

--- 

## 3. 예제코드
출처: ratsgo's blog [https://ratsgo.github.io/data%20structure&algorithm/2017/10/16/countingsort/]

```
# counting_sort (계수 정렬)

# A: 입력 데이터
# k : A의 요소 중 최댓값
def counting_sort(A, k):
    # B: 출력 데이터
    # -1로 초기화
    B = [-1]*len(A)

    # C: counting 데이터
    # 0으로 초기화
    C = [0] * (k + 1)

    # 동작과정
    for i in A:
        C[i] += 1

    # C 정렬
    for i in range(k):
        C[i+1] += C[i]

    # B 정렬
    for i in reversed(range(len(A))):
        B[C[A[i]] - 1] = A[i]
        C[A[i]] -= 1
    
    return B

input_data = [  1,3,2,4,3,
                2,5,3,1,2,
                3,4,4,3,5,
                1,2,3,5,2,
                3,1,4,3,5,
                1,2,1,1,1  ] 

max_in_input = max(input_data)
print(counting_sort(input_data, max(input_data)))

```