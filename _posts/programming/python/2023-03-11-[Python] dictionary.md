---
layout: single
title: "📘[Python] 파이썬 딕셔너리(dictionaty)"
toc: true
toc_sticky: true
toc_label: "목차"
categories: python
excerpt: ""
tag: [dictionary, list]
---

# 파이썬 딕셔너리란?
기존 리스트는 아래와 같다.  
```python
li = [1, 2, 3, 4, 5]
```  
<br>

딕셔너리는 `key:value`의 쌍으로 이루어져있다.  
*JSON형식과 비슷하다고 이해했다.. -> 주관적*  
```python
dic = {'a':1, 'b':2, 'c':3, 'd':4, 'e':5}
```  
<br>

딕셔너리는 리스트와 다르게 `중괄호{}`로 묶어 사용한다.  

## 키 이름이 중복되면?
딕셔너리에서 키 이름이 중복되면 어떻게 될까?  
```python
dic = {'a':1, 'a':2, 'c':3, 'd':4, 'e':5}
print(dic['a'])
print(dic)
```  
위와 같이 `a`가 중복된 상황에서 a의 value를 출력하면 어떻게 될까?  
<br>

```python
> 2
> {'a':2, 'c':3, 'd':4, 'e':5}
```  
이 처럼 키가 중복되면 뒤에 있는 값으로 대체되고, 중복된 키 값은 삭제된다.  

## 딕셔너리 키의 자료형
딕셔너리에서 키 값에 올 수 있는 값들의 자료형들은 다양하다.  
1. 문자열
2. 정수
3. 실수
4. 불(boolean)
5. 자료형을 섞어서 사용 가능
<br>

*그러면 value에는?*  
값에는 리스트와 딕셔너리를 포함한 모든 자료형을 사용할 수 있다.  
*여기서 키 값에 딕셔너리를 사용하는 것을 보고 JSON과 비슷하다고 생각했음*  

## 예제 - 백준-18870(좌표 압축)
리스트에 값들을 입력받고, 각 요소보다 작은 값의 개수를 출력하는 예제가 좌표 압축 문제이다.  
여기서 딕셔너리를 사용하는데, 딕셔너리를 사용하는 것이 익숙치 않아 구글링을 해야만 했다.  
<br>

```python
import sys
N = int(sys.stdin.readline())
arr = list(map(int,sys.stdin.readline().split()))
arr2 = []

# 중복제거 & 정렬 -> 리스트로 변환
arr2 = list(sorted(set(arr)))

dic = {arr2[i] : i for i in range(len(arr2))}

for i in arr:
    print(dic[i], end=' ')
```  
arr을 `sorted`로 정렬하고, `set`으로 중복 요소를 제거한다.  
딕셔너리를 활용해 arr2의 요소들을 비교한다.  
이 전에 이미 중복이 제거되었고 오름차순으로 정렬되었으니, `키:값`의 형태로 데이터를 저장해주면 된다.  
arr2는 오름차순으로 정렬되었고, arr2[0]의 경우 arr2[0]보다 작은 값은 0개이고, i=0이므로 i와 arr2[0]보다 작은 값의 개수가 일치한다.  
같은 동작으로 비교해보면 i의 값과 각 요소 별로 본인보다 작은 값의 개수와 일치한다.  

# 참고자료
[백준-18870](https://www.acmicpc.net/problem/18870)  
[https://eunhee-programming.tistory.com/116](https://eunhee-programming.tistory.com/116)  
[https://dojang.io/mod/page/view.php?id=2213](https://dojang.io/mod/page/view.php?id=2213)  