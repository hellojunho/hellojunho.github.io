---
layout: single
title: "📘[Python] 파이썬 오류 메시지 읽는 방법!"
toc: true
toc_sticky: true
toc_label: "목차"
categories: python
excerpt: ""
tag: [python, error, traceback]
---

# Python 오류 메시지 읽기

개발을 진행하다 보면 항상 오류 상황에 부딪힌다.

파이썬은 그런 오류가 발생했을 때 보통 `Traceback` 이라는 문구가 출력된다. 

이런 오류 메시지를 읽는 방법을 적어본다.

# Traceback

`Traceback` 이란, 파이썬에서 `특정 지점에서 함수 호출에 관한 정보를 담고있다` 정도로 생각할 수 있다.

만약 오류가 발생한다면, Traceback의 현재 상태를 출력하고, 오류를 콘솔에 띄운다.

```python
def hello(name, msg):
		print(name, ":", meg)
	
hello("Junho", "Hello")
```

예를 들어 위와 같은 코드가 있다고 하자.

위의 코드에서 `msg`대신 `meg` 를 파라미터로 전달한다.

meg는 함수 안에 정의가 된 파라미터가 아니니, 오류가 발생한다.

오류 코드는 아마도 아래와 같이 나올 것이다.

```
Traceback (most recent call last):
  File "error.py", line 4, in <module>
    hello("Junho", "Hello")
  File "error.py", line 2, in hello
    print(name, ":", meg)
NameError: name 'meg' is not defined. Did you mean: 'msg'?
```

## Traceback을 읽는 방법?

Traceback 메시지를 읽을 때는 **밑에서부터!!** 읽으면 된다.

```
NameError: name 'meg' is not defined. Did you mean: 'msg'?
```

먼저, 가장 아래 문구인 `NameError` 를 읽으면 된다.

“`meg` 라는 이름의 변수가 정의되지 않았다. `msg` 로 사용하려고 한거야?”

이 문구를 보면, `NameError` 라는 예외처리가 발생했고, 정의되지 않은 변수 사용이라는 예외 메시지를 담고있다.

이처럼 Traceback의 가장 아래 문구는 **예외처리와 왜 예외가 발생했는지에 대한 정보**를 담고있다.

다음으로 그 위의 문구를 보면,

```
File "error.py", line 2, in hello
    print(name, ":", meg)
```

이처럼 `함수 호출` 에 관한 정보를 담고있다.

위로 올라갈 수록 더 상위 함수를 나타내며, 현재는 맨 밑줄이기 때문에 오류가발생한 부분에 관한 정보를 확인할 수 있다.

만약 `in hello` 의 부분에서 hello 대신 `<module>` 이라면 최상위 코드에서 오류가 발생했다는 의미이다.

# 참고 자료

[[Python] 파이썬의 오류 메시지를 읽는 매우 간단한 방법](https://0xffffffff.tistory.com/70)