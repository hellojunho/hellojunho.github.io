---
layout: single
title: "📘[Python] 파이썬 문자열 입력받기"
toc: true
toc_sticky: true
toc_label: "목차"
categories: python
excerpt: ""
tag: [algorithms]

---

# 파이썬 입력받기
기존에 내가 알고있던 데이터를 입력 받는 방법은 `input()`을 이용하는 방법이었다.  
코딩 테스트를 준비하며 공부하고, 구글링을 하는데 뜬금없이 `import sys`가 나타났다.  
아래를 보니 `a = list(map(int, sys.stdin.readline().split()))`과 같이 `sys`모듈을 사용하는 것을 확인했다.  

## import sys
`sys`모듈은 파이썬 인터프리터를 제어할 수 있는 방법을 제공한다.
sys 모듈을 사용하면 프롬프트를 바꿀 수가 있다.  

## sys.stdin.readline()
그러면 `sys.stdin.readline()`은 왜 사용하는걸까?  
기존 input()으로 데이터를 입력받을 때는 하나의 데이터를 입력받을 때 유용하다.  
하지만, 반복문으로 여러 개의 데이터를 입력받을 때는 input()으로 받게되면 시간초과가 발생할 수 있다.  

## sys.stdin.readline() 사용법

### 한 개의 정수 입력
```python
import sys
a = int(sys.stdin.readline())
```  
위와 같이 사용하면 된다.  
만약 `a = sys.stdin.readline()`과 같이 사용하게 된다면, 어떻게 될까?  

*sys.stdin.readline()은 한 줄 단위로 입력받기 때문에 개행문자(\n)까지 함께 입력된다. 개행문자를 제거하기 위해 int형으로 형변환이 필요하다.*  

### 정해진 개수의 정수를 한 줄에 입력받을 때
```python
import sys
a,b,c = map(int,sys.stdin.readline().split())
```  
map()은 반복 가능한 객체(리스트 등)에 대해 각각의 요소들을 지정된 함수로 처리해주는 함수이다.  
위와 같이 사용한다면 a,b,c에 대해 각각 int형으로 형변환을 할 수 있다.  


### 임의의 개수의 정수를 한줄에 입력받아 리스트에 저장할 때
```python
import sys
data = list(map(int,sys.stdin.readline().split()))
```  
list()는 자료형을 리스트형으로 변환해주는 함수이다.  
map()은 맵 객체를 만들기 때문에, 리스트형으로 바꿔주기 위해서 list()로 변환해준다.  

### 임의의 개수의 정수를 n줄 입력받아 2차원 리스트에 저장할 때
```python
import sys
data = []
n = int(sys.stdin.readline())
for i in range(n):
    data.append(list(map(int,sys.stdin.readline().split())))
```  

### 문자열 n줄을 입력받아 리스트에 저장할 때
```python
import sys
n = int(sys.stdin.readline())
data = [sys.stdin.readline().strip() for i in range(n)]
```