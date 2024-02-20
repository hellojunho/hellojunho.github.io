---
layout: single
title: "📘[Algorithms] 병합정렬(Merge_Sort)에 대해서"
toc: true
toc_sticky: true
toc_label: "목차"
categories: algorithms
excerpt:
tag: [sort]
---

# 병합정렬(Merge_Sort)란
`병합정렬`은 `분할정복(Divide and Conquer)`방식을 이용한 정렬 알고리즘이다.  
병합정렬은 `안정정렬`에 속한다.  

## 병합정복 과정
1. 리스트의 길이가 0 혹은 1이면 이미 정렬된 것으로 판단
2. 정렬되지 않은 리스트를 절반으로 잘라 두 개의 부분 리스트로 나눔 (분할)
3. 각 부분 리스트를 재귀 방식으로 병합정렬을 이용해 정렬 (정복)
4. 두 부분 리스트를 다시 결합시킴 (결합)

## 예시
<img width="502" alt="스크린샷 2023-05-01 오전 10 12 34" src="https://user-images.githubusercontent.com/104587537/235386968-ba82c0e0-87ba-4e09-b447-5b6adefc0bec.png">  

## 병합정렬의 시간복잡도
<img width="551" alt="스크린샷 2023-05-01 오전 10 15 37" src="https://user-images.githubusercontent.com/104587537/235387118-6e9532e9-f19f-462e-ad51-3a87327ebe51.png">  
<br>

병합정렬의 시간 복잡도는 n개의 데이터를 logN 단계를 거쳐 진행하기 때문에 `O(NlogN)`의 시간복잡도를 갖는다.  