---
layout: single
title: "📘 [Python] 파이썬은 테스트 코드를 어떻게 쓸까?"
toc: true
toc_sticky: true
toc_label: "목차"
categories: python
excerpt: ""
tag: [python, test]
---

# TDD(Test Driven Development)?

일반적으로는 프로그램 개발이 끝난 후 테스트를 진행한다. 

하지만 `TDD` 는 테스트 코드부터 먼저 작성하고, 그 테스트 코드를 통과하는 실제 코드를 나중에 만든다.

## TDD의 순서

1. 구현해야 할 내용을 정의한다. (Need)
2. 실패하는 테스트를 작성한다. (Test)
3. 테스트를 성공하는 코드를 작성한다. (Code)
4. 코드를 리팩토링한다. (Refactoring)
5. 구현해야 할 내용을 완성할 때까지 위의 1 ~ 4번의 과정을 반복한다.

`Simple Code` 가 TDD의 궁극적인 목표임!

### Simple Code란?

**작동하는 깨끗한 코드** 를 말한다!

코드가 단순하다는 의미보다는  `중복이 없고 누가 봐도 명확한 코드를 의미한다`.

TDD를 적용해 코드를 작성하는 것은 Simple Code를 작성하는 지름길이다!

# Unit Test (단위 테스트)

```
import unittest

sssss
# 실제코드
def leap_year(year):
    pass

# 테스트코드
class LeapYearTest(unittest.TestCase):
    def test_leap_year(self):
        pass

# 테스트를 진행
if __name__ == '__main__':
    unittest.main()
```

단위 테스트 모듈을 사용할 때의 규칙은 아래와 같다.

- 테스트 코드는 unittest의 `TestCase` 클래스를 상속받아 작성한다.
- `test_leap_year()` 처럼 test~ 로 시작하는 메서드는 자동으로 실행된다.

다음으로 테스트를 진행할 때 자주 사용하는 검증 메소드는 아래와 같다.

- assertTrue(a) : a가 참인지 조사한다.
- assertFalse(a) : a가 거짓인지 조사한다.
- assertEqual(a, b) : a와 b가 같은지 조사한다.

# Pytest?

`pytest` 는 말 그대로 python을 test 하는 **Framework** 임!

👉 [pytest 공식 문서](https://docs.pytest.org/en/stable/)

특히 데이터 분석 분야에서 많이 사용되고 있다고 한다!

> pandas에서도 pytest를 통해 코드 테스팅을 진행하고 있고, 심지어 pandas 공식 문서에서도 test의 중요성을 알리며 pytest의 사용법을 함께 설명하고 있다!
> 

다른 예시로는 `SQLAlchemy` 가 있다!

python의 ORM 기술 중 하나인 `SQLAlchemy` 는 DB 관련 개발에서 많이 활용되는 오픈소스임!

여기서도 테스트의 중요성과 pytest의 사용법을 함께 알려주고 있다.

👉 [SQLAlchemy 공식 문서 중 test 파트](https://www.sqlalchemy.org/develop.html#testing)

이처럼 pytest는 여러 분야, 기술에서 사용되고 있는 것을 확인할 수 있었다! → 그만큼 유용한 테스트 도구라는 뜻이겠지..!

## pytest 시작하기

### Install

```
$ pip install -U pytest
```

위 명령어로 설치하고, 

```
$ pytest --version
```

이걸로 버전 확인을 해보자!

## Test code 작성하기 예제

```
# test.py

# 테스트를 해보고 싶은 함수
def func(x):
		return x + 1
		
# 테스트 함수 -> 위에서 test~로 시작하는 함수가 테스팅 함수라고 언급했었다!
def test_answer():
		assert func(3) == 5
```

```
$ python test.py
```

- 위 코드는 당장 직접 계산해봐도 False가 나오는 것을 알 수 있다!
- 이를 실행하면 False가 결과로 나오는 동시에, 어느 부분에서 오류가 발생했는지 알려주고, 어떤 플랫폼에서 작동하고 있으며, 어떤 에러가 발생한건지 마지막으로 요약을 통해 총 몇 개의 테스트가 pass & fail 인지를 나타내준다. → 이에 걸린 총 시간도 결과로 함께 줌 ㄷㄷㄷ

## Structure

위의 예제는 `[test.py](http://test.py)`를 작성하고 터미널에 `python test.py` 명령어를 통해 테스트를 실행했다. 

또한 한 파일에 일반 함수와 테스트 코드가 공존했다!

하지만, 실제 프로젝트에 적용할 때는 테스트 코드를 따로 관리하고, 이에 적합하게 구조를 구성해놓는 것이 효율적이다.

그래서 테스트 코드는 프로젝트 코드들과 달리 `test` 라는 디렉토리에 함께 관리한다!

```
project/
    core_code/
        __init__.py
        sample_code1.py
        sample_code2.py
        sample_code3.py
    tests/
        test_sample1.py
        test_sample2.py
        test_sample3.py
```

```
$ python -m pytest tests
```

위 구조로 구성하고, 이 명령어를 통해 tests 파일들을 실행할 수 있다!

## pytest fixtures??

`fixtures` 라는게 있다던데..?

이건 pytest의 특징 중 하나로, **수행될 테스팅에 있어 필요한 부분들을 갖고 있는 코드 또는 리소스** 를 의미한다!

`데코레이터` 와 함께 사용한다!

### fixtures 예제

```
# calculator.py
class Calculator(object):
    """Calculator class"""
    def __init__(self):
        pass

    @staticmethod
    def add(a, b):
        return a + b

    @staticmethod
    def subtract(a, b):
        return a - b

    @staticmethod
    def multiply(a, b):
        return a * b

    @staticmethod
    def divide(a, b):
        return a / b
```

### test - 1

```
# test_calculator.py
from src.calculator import Calculator
def test_add():
    calculator = Calculator()
    assert calculator.add(1, 2) == 3
    assert calculator.add(2, 2) == 4

def test_subtract():
    calculator = Calculator()
    assert calculator.subtract(5, 1) == 4
    assert calculator.subtract(3, 2) == 1

def test_multiply():
    calculator = Calculator()
    assert calculator.multiply(2, 2) == 4
    assert calculator.multiply(5, 6) == 30

def test_divide():
    calculator = Calculator()
    assert calculator.divide(8, 2) == 4
    assert calculator.divide(9, 3) == 3
```

- `calculator = Calculator()` 가 중복되는걸 볼 수 있다!
- 이걸 더 깔끔하게 고쳐보자!

### test - 2

```
import pytest
from src.calculator import Calculator

@pytest.fixture
def calculator():
    calculator = Calculator()
    return calculator

def test_add(calculator):
    assert calculator.add(1, 2) == 3
    assert calculator.add(2, 2) == 4

def test_subtract(calculator):
    assert calculator.subtract(5, 1) == 4
    assert calculator.subtract(3, 2) == 1

def test_multiply(calculator):
    assert calculator.multiply(2, 2) == 4
    assert calculator.multiply(5, 6) == 30
```

- `calculator = Calculator()` 를 calculator()에 정의해 한 번만 작성된 걸 볼 수 있다!
- 이처럼 fixture를 사용해 정의한 함수를 파라미터로 사용해 테스트를 위한 클래스를 가져올 수 있음.

### [conftest.py](http://conftest.py) ???

만약 테스트를 위한 클래스를 불러오는 로직이 여러 테스트 파일에서 사용된다고 생각해보자!

그럼 `@pytest.fixture` 가 붙은 메서드를 따로 파일로 빼서 관리하고, 테스트 파일에서 import 해서 사용하면 좀 더 편하겠지?

그래서 `[conftest.py](http://conftest.py)` 라는 파일을 만들고, 여기에 fixture 메서드를 관리하면 조금 더 코드가 Simple Code에 가까워진다!

```
# conftest.py

@pytest.fixture
def calculator():
    calculator = Calculator()
    return calculator
```

# 참고 자료

[unittest — Unit testing framework](https://docs.python.org/ko/3/library/unittest.html)

[107 작성한 코드를 테스트하려면? ― unittest](https://wikidocs.net/132725)

[pytest: helps you write better programs — pytest documentation](https://docs.pytest.org/en/stable/)

[[pytest] python 코드를 테스트 해봅시다.](https://binux.tistory.com/47)