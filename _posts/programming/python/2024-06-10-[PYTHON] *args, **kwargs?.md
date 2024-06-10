---
layout: single
title: "📘 [Python] *args, **kwargs?"
toc: true
toc_sticky: true
toc_label: "목차"
categories: python
excerpt: ""
tag: []
---
# \*, \*\* 가 뭘까 진짜?

`*`는 C에서 포인터를 사용할 때 자주 봤다.

파이썬에서도 `*`이 보이니 살짝 당황했지만, 이게 포인터처럼 주소를 의미하는 것은 아니라는 것을 알고 안심했다.

먼저 *가 1개 쓰이는 `*args`에 대해서 알아보자.

## \*args?

`*args`는 *arguments의 줄임말이다.

즉, `*args`가 아닌 `*doc` 이런 식으로 다른 이름이 와도 된다.

이 `*args`는 여러 개의 인자를 `함수`로 받을 때 사용한다!

### 예시

```python
def peoples(*args):
    for name in args:
        print(name)
    print(type(args))
		    

people_names = ["짱구", "맹구", "철수", "훈이", "유리"]

peoples(people_names)
```

이 코드를 실행해보면 아래와 같은 출력 결과가 나온다.

```python
['짱구', '맹구', '철수', '훈이', '유리']
<class 'tuple'>
```

이처럼 인자로 들어간 `값(args)`은 `튜플`로 인식되어 처리되는 것을 알 수 있다!

즉, 여러 개의 인자를 받기 위해서 `*args` 의 형태로 함수 파라미터를 받는거였다.

## \*\*kwargs?

`kwargs`는 keyword argument의 줄임말로, 키워드를 제공한다.

### 예시

```python
def introduce(**kwargs):
    for key, value in kwargs.items():
        print(f"{key} : {value}")

introduce(name="junho")
```

이렇게 이번에는 인자를 `**kwargs`로 받았다.

이걸 출력해보면 다음과 같은 결과가 나온다.

```python
name : junho
```

즉, `**kwargs`는 {keyword : value}의 형태로 함수를 호출하는 역할이고, `dictionary` 형태로 함수 내부에 전달된다.

## \*args와 \*\*kwargs에도 순서가 있다!

python 내부에서는 `*args`와 `**kwargs` 를 입력하는 데에도 순서가 있다!

<b>val > *args > \*\*kwargs</b> 순서다! (오른쪽으로 갈 수록 우선순위 늦음)  

### 예시

```python
def func1(name, *args, **kwargs):
		pass

def func2(**kwargs, *args):
		pass

def func3(*args, name):
		pass
```

- `func1`: OK
- `func2`: NO
- `func3`: NO

# 참고 자료

[[나름 중급 파이썬1] *args와 **kwargs](https://brunch.co.kr/@princox/180)