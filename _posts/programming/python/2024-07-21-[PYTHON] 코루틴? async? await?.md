---
layout: single
title: "📘 [Python] 코루틴? async? await?"
toc: true
toc_sticky: true
toc_label: "목차"
categories: python
excerpt: ""
tag: [coroutine, asyncio, fastapi]
---

# asyncio?

`asyncio`란, `async/await` 구문을 사용해 동시성 코드를 작성할 수 있도록 하는 라이브러리임!

`async` + `I/O` 의 합성어로 볼 수 있음.

`asyncio` 는 고성능 네트워크 및 웹 서버, 데이터베이스 연결 라이브러리, 분산 작업 큐 등을 제공하는 여러 파이썬 비동기 프레임워크의 기반으로 사용되고 있다.

또, 다음과 같은 작업들을 위한 **고수준 API** 집합을 제공하고 있다!

- 파이썬 `코루틴`들을 동시에 실행하고, 실행을 완전히 제어할 수 있음
- 네트워크 IO & IPC를 수행함
- 자식 프로세스를 제어함
- `큐`를 통해 작업을 분산함
- 동시성 코드를 `동기화`함

**고수준 API**가 있으면 **저수준 API**도 있겠지?

**저수준 API**는 라이브러리 & 프레임워크 개발자가 다음과 같은 작업을 할 수 있도록 도와줌!

- 네트워킹, 하위 프로세스 실행, OS 신호 처리 등을 위한 비동기 API를 제공하는 이벤트 루프 생성 및 관리
- `트랜스포트`를 사용한 효율적인 프로토콜 구현
- 콜백 기반 라이브러리와 `async/await` 구문의 사용으로 코드 간의 연결다리 구축

## 그러면 왜 asyncio를 쓰는데?

위 내용들은 그렇다치고, 왜 `asyncio`를 써야하는가..!

스레딩을 사용한 동시성 제어의 큰 단점은 바로 **느리다는 것..!**

`Global interpreter lock`때문에 실제로 여러 물리 스레드에서 실행되는 것이 아닌 메인 스레이드에서 모든 연산을 처리하기 때문이라고 함

그 와중에 멀티 스레드 프로그래밍에서 갖고 있는 문제점들 또한 그대로 갖고 있다는 단점이 존재한다..

스레딩을 통해 얻고자 하는 이점은 파일의 Read & Write, HTTP 통신 대기와 같은 `Blocking IO 대기 시간`임!!

그러면 개발자는 Blocking IO만 신경쓰면 되는데, 스레딩은 일단 너무 복잡함..ㅜ

그래서 `asyncio`가 이런 어려운 점을 도와주기 위해 탄생한 것이다!

`asyncio`는 쉽고, 명시적으로 흐름을 알 수 있으며 멀티스레드 프로그래밍에의 문제점을 무시할 수 있다고 함!

# 코루틴?

`코루틴`은 들어보기는 했다. (왜인지는 모르겠지만, 코틀린 관련 강의 혹은 자료를 보면 항상 따라다님)

코루틴이 뭘까? 예시를 들어 확인해본다!

기존에는 함수를 호출하면, 함수를 실행하고 그 동작이 끝나면 현재의 코드로 돌아왔다.

예를 들어 `calc` 함수 안에서 `add`를 호출했을 때, `add` 함수가 끝나면 다시 `calc` 함수로 돌아온다.

```python
def add(a, b):
    c = a + b    # add 함수가 끝나면 변수와 계산식은 사라짐
    print(c)
    print('add 함수')
 
def calc():
    add(1, 2)    # add 함수가 끝나면 다시 calc 함수로 돌아옴
    print('calc 함수')
 
calc()
```

심지어, `add` 함수가 끝나면 이 함수에 들어있던 변수와 계산식은 모두 사라진다..!

여기서 `calc` 와 `add` 의 관계를 생각해보면, `calc` 가 **메인 루틴**, `add` 는 `calc` 의 **서브 루틴**이다!

## 메인 루틴과 서브 루틴의 동작 과정

<img width="530" alt="메인-서브 루틴" src="https://github.com/user-attachments/assets/daac393e-98b8-467a-8dac-3e5d06cd9ff9">


## 코루틴의 동작 방식

`코루틴`은 `Cooperative Routine` 를 의미하는데, **서로 협력하는 루틴**이라는 뜻임! 

기존 `메인-서브 루틴`과는 다르게, 서로 종속된 관계가 아닌 서로 대등한 관계에서 동작한다.

![코루틴](https://github.com/user-attachments/assets/53bf6d77-fe3b-408f-8444-c5a112cfcb8b)


이처럼 코루틴은 함수가 종료되지 않은 상태에서 메인 루틴의 코드를 실행한 뒤에 다시 돌아와서 코루틴의 코드를 실행한다.

따라서 코루틴이 종료되지 않았으므로 코루틴의 내용도 계속 유지된다.

일반 함수를 호출하면 코드를 한 번만 실행할 수 있지만, 코루틴은 코드를 여러 번 실행할 수 있다는 장점이 있다!

- 이처럼 함수의 코드를 실행하는 지점을 `진입점(entry point)` 이라고 하는데, 코루틴은 진입점이 여러 개인 함수임

# 코루틴 동작 예시

코루틴은 제너레이터의 특별한 형태임!

제너레이터는 `yield` 로 값을 발생시키지만, 코루틴은 `yield` 로 값을 받아옴..!

```python
def number_coroutine():
    while True:        # 코루틴을 계속 유지하기 위해 무한 루프 사용
        x = (yield)    # 코루틴 바깥에서 값을 받아옴, yield를 괄호로 묶어야 함
        print(x)
 
co = number_coroutine()
next(co)      # 코루틴 안의 yield까지 코드 실행(최초 실행)
 
co.send(1)    # 코루틴에 숫자 1을 보냄
co.send(2)    # 코루틴에 숫자 2을 보냄
co.send(3)    # 코루틴에 숫자 3을 보냄
```

- 코루틴객체.send(값)
- 변수 = (yeild)

이처럼 코루틴에 값을 보내면서 코드를 실행할 때는 `send()` 를, send()가 보낸 값을 받아오려면 `(yield)` 형식을 사용한다!

> 코루틴에 대해서는 다른 테스트 코드와 함께 다시 깔끔하게 정리해보자!
> 

# async/await?

`async/await` 구문은 최신 파이썬 버전에서는 비동기 코드를 정의하는 매우 직관적인 방법이다!

```python
burgers = await get_burgers(2)
```

여기서 `await` 이 핵심인데, 파이썬에게 `burgers` 결과를 저장하기 전에 `get_burgers(2)` 의 작업이 완료되기를 기다리라고 말하는 의미임!

그러면? 파이썬은 그 동안 다른 작업을 수행해도 된다는 것이다!

여기서 `await` 가 동작하기 위해서는 이 구문이 `비동기를 지원하는 함수` 내부에 있어야하는데..

이 비동기 함수를 `async` 예약을 사용해 정의할 수 있다!

```python
async def get_burgers(number: int):
		burgers = await get_burgers(2)
		return burgers
```

즉, `await` 는 `async def` 안에서만 사용이 가능한 예약어인 것이다!

하지만 동시에 `async def` 로 정의된 함수들은 **대기** 되어야만 하기 때문에, `async def` ****를 사용한 함수들은 역시 `async def` 를 사용한 함수 내부에서만 호출될 수 있다!

> 최신 파이썬 버전은 `async` 및 `await` 문법과 함께 `코루틴` 이라고 하는 것을 사용하는 `비동기 코드` 를 지원한다!
> 

# 참고 자료

[코루틴과 태스크](https://docs.python.org/ko/3/library/asyncio-task.html)

[asyncio — Asynchronous I/O](https://docs.python.org/ko/3/library/asyncio.html)

[파이썬 코딩 도장: 41.1 코루틴에 값 보내기](https://dojang.io/mod/page/view.php?id=2418)

[Python3.8 asyncio, async/await 기초 - 코루틴과 태스크](https://kdw9502.tistory.com/6)

[동시성과 async / await - FastAPI](https://fastapi.tiangolo.com/ko/async/)