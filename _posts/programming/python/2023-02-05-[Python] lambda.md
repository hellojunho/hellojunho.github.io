---
layout: single
title: "📘[Python] Lambda에 대해서..."
toc: true
toc_sticky: true
toc_label: "목차"
categories: python
excerpt: ""
tag: []

---

# Lambda에 대해서...

## 1. Lambda란
`Lambda란` 의미 익명함수를 지칭하는 용어 즉, 기존의 함수(명 등)을 선언하고 사용하던 방식과는 달리 바로 정의하여 사용할 수 있는 함수를 뜻한다.  

### 1.2. 형식
*형식은 어떻게 될까?*  
`lambda 인자 : 표현식`과 같이 나타낸다.  
```python
lambda x : x+1
```

### 1.3. 인자 부여
*인자는 어떻게 넣어?*  
람다 표현식을 괄호로 묶은 뒤에 다시 괄호를 붙이고 인수를 넣어 호출한다.  
[예시]  
```
(lambda x: x + 1)(1)
> 2
```  

### 1.4. 인자 2개
*인자를 두개 사용해보자!*  
```python
lambda x,y: x+y
```  
  
### 1.5. if문 사용
*if문도 사용이 가능하다!*   
```python
check_pass = lambda x: 'pass' if x>=70 else 'fail'
```  

## 2. 리스트를 사용해보자
key 인자를 정하지 않은 기본적인 sort에선, 튜플 순서대로 우선순위 기본 할당  
```python
a = [(1, 2), (5, 1), (0, 1), (5, 2), (3, 0)]

# 앞의 인자로 정렬됨
b = sorted(a)
b = [(0, 1), (1, 2), (3, 0), (5, 1), (5, 2)]
```

key 인자에 함수를 넘겨주면 우선순위가 정해짐.  
```python
c = sorted(a, key = lambda x : x[0]) 

c = [(0, 1), (1, 2), (3, 0), (5, 1), (5, 2)]

d = sorted(a, key = lambda x : x[1]) 

d = [(3, 0), (5, 1), (0, 1), (1, 2), (5, 2)]
```  


## 3. map 람다 표현식
*map으로도 람다 표현식을 사용할 수 있다*  
```python
list(map(lambda x: x+10, [1,2,3]))

> [11, 12, 13]
```  

## 4. filter()
*filter()에 람다 표현식을 사용하면 True값만 반환함!*  
```python
a = [8, 4, 2, 5, 2, 7, 9, 11, 26, 13]

result = list(filter(lambda x : x > 7 and x < 15, a))

> [8, 9, 11, 13]
```  

## 5. reduce() 표현식
*reduce()는 값을 누적시킨다.*  
```python
from functools import reduce t = [47, 11, 42, 13]

result = reduce(lambda x, y : x + y, t)

> 113
```  
> 참고: reduce()는 functools 모듈을 불러와서 사용하자

## 6. 예제
1. 숫자 크기대로 정렬  

아래와 같은 데이터가 있다고 하자.  
```python
data = [
    ["고구마",25000],
    ["바나나",123232],
    ["파인애플",4500],
    ["감자",3000],
    ["금귤",6000]
]
```  
가격을 기준으로 정렬하면  
```python
data.sort(key = lambda x:x[1])
print(data)
## 출력 값 
#[['감자', 3000], ['파인애플', 4500], ['금귤', 6000], ['고구마', 25000], ['바나나', 123232]]
```  

2. 단어 사전 순 정렬
아래와 같은 데이터가 있다고 하자.  
```python
data = ["나라","가구","봄","가을","도토리","낫","혹","가을 아침","나는 밥을 먹고 있다."]
```  
단어의 사전 순으로 정렬하면  
```python
data.sort(key = lambda x:(len(x),x))

print(data)
```  