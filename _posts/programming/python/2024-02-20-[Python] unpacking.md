---
layout: single
title: "📘[Python] 파이썬 패킹/언패킹 문법에 대해서..."
toc: true
toc_sticky: true
toc_label: "목차"
categories: python
excerpt: ""
tag: [python, unpacking]
---

# Python 패킹과 언패킹
파이썬에는 `패킹(packing)`이라는 문법이 존재한다.  
이게 뭐냐?  
바로 **여러 개의 값을 하나의 변수에 묶어 담는 것**이다.  
이 때 변수에 담긴 값들은 `튜플(tuple)`형태로 묶인다.  

## *을 활용한 패킹
`*(애스터리스크)`를 활용해 패킹을 진행하는 방법은 `*변수`로 사용하는 것이다.  
이건 남은 요소들을 `리스트(list)` 형태로 패킹한다.  

### 예시
[입력]  
```python
my_list = [1, 2, 3, 4, 5]
a, *b, c = my_list

print(a, *b, c)
```  

[출력]  
```python
> 1, [2, 3, 4], 5
```

## 언패킹
`언패킹(Unpacking)`은 패킹된 변수의 값을 개별적인 변수로 분리하여 할당하는 것이다.  

### *을 활용한 언패킹
언패킹도 `*`를 활용하면 되는데, **시퀀스나 반복 가능한 객체를 각각 요소로 분리**시키는 것이다.  
함수의 인자로 전달할 때 주로 사용한다.  

### **을 활용한 언패킹
이는 딕셔너리의 키 값 쌍을 함수의 키워드 인자로 언패킹하는 방식이다.  

# 요약
- 패킹: 여러 개의 객체를 하나의 객체로 합침  
- 언패킹: 여러 개의 객체를 포함하고 있는 하나의 객체를 풀어줌  
  - \* 활용: 동일하게 위치 인자를 언패킹 하는 경우  
  - ** 활용: 키워드 인자를 언패킹 하는 경우  

# 참고자료
[https://wikidocs.net/22801](https://wikidocs.net/22801)  
[https://timedilation.tistory.com/33](https://timedilation.tistory.com/33)  