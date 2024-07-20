---
layout: single
title: "📘 [Python] try-except, raise?"
toc: true
toc_sticky: true
toc_label: "목차"
categories: python
excerpt: ""
tag: []
---

# try ~ except문

`try ~ except` 는 파이썬에서 사용하는 예외 처리 문법이다.

비슷한 경우로는 java에서 `try ~ catch`가 있다.

`try ~ except`문의 기본 구조는 다음과 같다.

```python
try:
		예외처리 하고 싶은 코드
except ValueError:
		예외처리 방식
```

예외처리를 하고 싶은 코드에는 일반적인 로직이 들어가면 된다.

예외처리 방식 부분에는 예외가 발생했을 때, 어떤 메시지 혹은 값 등을 반환 시킬지 적으면 된다.

## ErrorList

- `AssertionError`: [assert](https://docs.python.org/ko/3/reference/simple_stmts.html#assert) 문이 제대로 작동하지 않을 때 발생한다.
- `IndexError`: 참조 하려는 **인덱스가 범위를 벗어날 때** 발생한다.
- `KeyError`: 참조 하려는 **키가 기존 키 집합에서 찾을 수 없을 때** 발생한다.
- `KeyboardInterrupt`: 사용자가 인터럽트 키(`Control + C`, 혹은 `Delete`)를 누를 때 발생하며, 모든 Exception을 잡는 코드에 의해 인터프리터가 종료하는 것을 막지 못하도록 `Exception` 상위에 있는 `BaseException`을 직접 계승한다.
- `MemoryError`: 메모리가 부족하지만, 가비지 컬렉터가 일부 객체의 삭제를 함으로써 복구될 수 있는 경우 발생한다.
- `NameError`: 참조하는 지역, 전역 변수 혹은 함수, 클래스 등을 찾을 수 없을 때 발생한다.
- `OSError`: 시스템 함수가 **시스템 관련 에러**를 돌려줄 때 발생한다. (파일을 찾을 수 없는 경우 혹은 디스크 용량이 없는 경우)
- `OverflowError`: 산술 연산의 **결과가 너무 커서** 표현 할 수 없을 때, 혹은 **정수 범위를 벗어났을 때** 발생한다.
- `RecursionError`: 최대 **재귀 깊이**가 초과하였을 때 발생한다.
- `TypeError`: 연산이나 함수가 **부적절한 데이터 타입**의 객체에 적용 되었을 때 발생한다.
- `ValueError`: 연산이나 함수가 **부적절한 값을 가진** 객체에 적용 되었을 때 발생한다.
- `ZeroDivisionError`: 나누기, 나머지 연산의 **두 번째 인자가 0**일 때 발생한다.

## 예외 핸들링 방법?

특정 에러만 핸들링하는 방법은 에러 객체의 이름을 except 옆에 작성하면 된다.

- 예) `except ValueError:`

또, 튜플 형태로 작성이 가능한데, 만약 except절 옆에 아무 것도 없다면, 나머지 모든 에러를 핸들링한다. → 주의가 필요한 방법

- 예) `except (ZeroDivisionError, OverflowError):`

만약 에러를 제네릭하게 받고 싶으면 `Exception as e` 를 사용한다.

- 예) `except Exception as e:`

# 그럼 raise가 뭘까?

`raise`는 인위적으로 에러를 발생시키는 방법이다.

`raise`는 꼭 try와 함께 쓰여야하는 것만은 아니다!

만약 try 절에 쓰인 경우, try절이 정상적으로 수행되는 도중에 예외를 찾으면 즉시 예외를 발생시킨다.

아래는 try, except, raise의 예시다.

```python
def three_multiple():
    try:
        x = int(input('3의 배수를 입력하세요: '))
        if x % 3 != 0:                                 # x가 3의 배수가 아니면
            raise Exception('3의 배수가 아닙니다.')    # 예외를 발생시킴
        print(x)
    except Exception as e:                             # 함수 안에서 예외를 처리함
        print('three_multiple 함수에서 예외가 발생했습니다.', e)
        raise    # raise로 현재 예외를 다시 발생시켜서 상위 코드 블록으로 넘김
 
try:
    three_multiple()
except Exception as e:                                 # 하위 코드 블록에서 예외가 발생해도 실행됨
    print('스크립트 파일에서 예외가 발생했습니다.', e)
```

# 참고 자료

[파이썬 코딩 도장: 38.3 예외 발생시키기](https://dojang.io/mod/page/view.php?id=2400)

[Python에서 try, except, raise로 예외 처리 하기.](https://justkode.kr/python/try-except/)