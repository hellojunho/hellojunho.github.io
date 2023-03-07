---
layout: single
title: "📘[Python] 파이썬 Counter 함수란?"
toc: true
toc_sticky: true
toc_label: "목차"
categories: python
excerpt: ""
tag: [counter]
---

# Counter 함수?
`Counter`함수는 `collections`모듈에 내장 되어있는 함수임.  
```python
from collections import Counter
```  

Counter 생성자는 여러 형태의 데이터를 인자로 받을 수 있다.  
먼저, 중복된 데이터가 저장된 배열을 인자로 넘기면 각 원소가 몇 번 나타나는지 출력해준다.   
```python
>>> Counter(["hi", "hey", "hi", "hi", "hello", "hey"])
Counter({'hi': 3, 'hey': 2, 'hello': 1})
```  
<br>

다른 방법으로는 문자열을 인자로 주면 각 문자가 몇 번 나타나는지 알 수 있다.  
```python
>>> Counter("hello world")
Counter({'h': 1, 'e': 1, 'l': 3, 'o': 2, ' ': 1, 'w': 1, 'r': 1, 'd': 1})
```  

# 챀고자료
[https://www.daleseo.com/python-collections-counter/](https://www.daleseo.com/python-collections-counter/)  
