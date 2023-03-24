---
layout: single
title: "📘[Python] 파이썬 set()"
toc: true
toc_sticky: true
toc_label: "목차"
categories: python
excerpt: ""
tag: [set]
---

# 파이썬 set이란?
`set`은 중복되지 않는 데이터를 담는 자료구조이다.  
아래와 같이 적용할 수 있다.  

```python
setA = set()
setA = {1, 2, 3, 1, 2}

print(setA)
```
```python
> {1, 2, 3}
```  

## 언제 사용할까?
`set` 자료구조는 보통 수학에서 `집합`을 표현할 때 사용한다.  
예를 들면, *문제에서 집합 A의 요소가 {1, 2, 3}* 와 같이 표현될 경우, `set()`을 사용해 나타낼 수 있다.  
수학적인 개념을 사용할 때 `set()`을 사용하는 만큼, 수학적 집합 연산을 지원한다.  
<br>

**특히 두 집합간의 연산인 교집합, 차집합, 합집합, 여집합을 쉽게 구할 수 있다.**  
<br>

그리고 집합 연산이 가능하기 때문에 데이터 처리나 알고리즘 구현 등에 유용하다고 한다.  

### 교집합
교집합은 `intersection()`을 사용하는 방법과 연산자 `&`를 사용하는 방법이 있다.  
```python
set1 = {1, 2, 3}
set2 = {3, 4, 5}

# set3 = set1.intersection(set2)
set3 = set1 & set2
```  

### 차집합
차집합은 `difference()`와 연산자 `-`를 사용하는 방법이 있다.  
```python
set1 = {1, 2, 3}
set2 = {3, 4, 5}

# set3 = set1.difference(set2)
set3 = set1 - set2
```
### 합집합
합집합은 `union()`을 사용하는 방법이 있다.  
```python
set1 = {1, 2, 3}
set2 = {3, 4, 5}

set3 = set1.union(set2)
```

### 여집합
여집합은 `symemetric_difference()`를 사용하는 방법이 있다.  
```python
set1 = {1, 2, 3}
set2 = {3, 4, 5}

set3 = set1.symmetric_difference(set2)
```