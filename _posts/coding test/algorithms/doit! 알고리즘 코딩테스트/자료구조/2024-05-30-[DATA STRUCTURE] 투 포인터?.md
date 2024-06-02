---
layout: single
title: "📘[Data Structure] 투 포인터?"
toc: true
toc_sticky: true
toc_label: "목차"
categories: algorithms
excerpt:
tag: [python]
---

# 투 포인터?

`투 포인터` 는 2개의 포인터로 알고리즘의 시간 복잡도를 최적화 시킬 수 있다.

알고리즘은 매우 간단하다!

## 투 포인터 이동 원칙?

[1, 2, 3, 4, 5, 6, 7, 8, 9, 10] 이라는 배열이 있다고 가정해보자.

여기서 `start_index` 와 `end_index` 를 지정해야한다!

보통 start_index ~ end_index 사이의 합 혹은 두 값의 합을 구하는 경우가 많은데, 이를 `sum` 이라고 하자.

1. sum > n
    1. sum -= start_index
    2. start_index += 1
2. sum < n
    1. end_index += 1
    2. sum += end_index
3. sum == n
    1. end_index += 1
    2. sum += end_index
    3. count += 1

위와 같은 원칙을 갖고 포인터를 이동시킬 수 있다!

쉽게 생각해보면 두 개의 포인터가 존재하고, 하나의 포인터(end_index)가 먼저 증가하면서 값을 측정하다가 문제의 요구에 충족하면 count를 1 증가한 후에 start_index를 끌어오는 방식이다.

# 관련 문제

[백준[2018] - 수들의 합](https://www.acmicpc.net/problem/2018)

[백준[1940] - 주몽](https://www.acmicpc.net/problem/1940)

[백준[1253] - 좋은 수](https://www.acmicpc.net/problem/1253)
- [1253] 좋은 수 문제는 다시 풀어보자..