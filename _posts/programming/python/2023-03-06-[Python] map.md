---
layout: single
title: "📘[Python] 파이썬 map이란?"
toc: true
toc_sticky: true
toc_label: "목차"
categories: python
excerpt: ""
tag: [map]
---

# Python map 함수가 뭘까?
`map`은 리스트의 요소를 지정된 함수로 처리해주는 함수다.  
map은 원본 리스트를 변경하지 않고 새 리스트를 생성한다.  

## map 구조와 예제
**map(function, iterable)**  
`map`함수의 기본 구조는 위와 같다.  
<br>

map을 리스트, 튜플에 적용해보자.  
: list(map(function, list))
: tuple(map(function, tuple))

[적용예제1]  
```python
a = [1, 2, 3, 4, 5]
for i in range(len(a)):
    a[i] = int(a[i])
...

print(a)
```  
[적용예제1-결과]  
```python
> [1, 2, 3, 4]
```  
<br>

[적용예제2]  
```python
a = [1, 2, 3, 4, 5]
a = list(map(int, a))
print(a)
```  
[적용예제2- 결과]  
```python
> [1, 2, 3, 4]
```  
리스트를 map함수를 사용해 한 줄로 표현할 수 있다.  

[그림 설명]  
<img width="833" alt="스크린샷 2023-03-06 오전 9 57 48" src="https://user-images.githubusercontent.com/104587537/222997101-305eb702-9f2f-4245-b9a4-ac57a4fea9ae.png">  

## map은 리스트에만 사용가능한 것이 아님!
사실 map은 리스트 뿐만 아니라 모든 반복 가능한 객체를 넣을 수 있다.  
[예제]  
```python
a = list(map(str, range(10)))
print(a)
```  
```python
['0', '1', '2', '3', '4', '5', '6', '7', '8', '9']
```

## input()에 map을 사용할 수 있다!
input()으로 데이터를 입력받고, split()으로  여러 개를 입력받을 수 있었다.  
input().split()의 결과를 정수, 실수로 변환할 때도 map을 사용해 변환할 수 있다.  
*어떻게 가능할까?*  
input().split()의 결과가 `문자열 리스트`라서 가능하다.  
[예제1]  
```python
a = map(int, input().split())   # 입력: 10 20
print(a)    # <map object at 0x03DFB0D0>
print(list(a))  # [10, 20]
```  
<br>

사실 map이 반환하는 맵 객체는 이터레이터라서 변수 여러 개에 저장하는 `언패킹(unpacking)`이 가능하다!  
<br>

*언패킹이 뭔데?*  
한 변수의 데이터를 각각의 변수로 반환하는 것 → 묶인 걸 푼다!  

[예제2]   
```python
x = input().split()
m = map(int, x)
a, b = m    # 맵 객체는 변수 여러 개 저장할 수 있음
```  


# 참고자료
[https://dojang.io/mod/page/view.php?id=2286](https://dojang.io/mod/page/view.php?id=2286)  
[https://roi-data.com/entry/%ED%8C%8C%EC%9D%B4%EC%8D%AC-%ED%8C%A8%ED%82%B9-packing-%EC%96%B8%ED%8C%A8%ED%82%B9-unpacking](https://roi-data.com/entry/%ED%8C%8C%EC%9D%B4%EC%8D%AC-%ED%8C%A8%ED%82%B9-packing-%EC%96%B8%ED%8C%A8%ED%82%B9-unpacking)  