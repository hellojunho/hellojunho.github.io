---
layout: single
title: "📘 [Python] 문자열 포맷? r-string vs f-string"
toc: true
toc_sticky: true
toc_label: "목차"
categories: python
excerpt: ""
tag: []
---

# r-string?

어떤 경우에는 문자열에 `이스케이프 시퀀스(”\n”)` 가 필요한 경우가 있다.

예를 들면 `파일 경로` 같은 경우가 있다!

```python
>>> dir1 = "D:\File"
>>> dir2 = "D:\newFile"

>>> print(dir1)
D:\File
>>> print(dir2)
D:
ewFile
```

`dir1` 을 print 해보면 파일 경로 그대로 출력이 되지만, `dir2` 의 경우에는 `\n`을 줄바꿈 문자로 인식해 `D:`와 `ewFile`로 줄바꿈되어 출력된다.

이런 경우, `r-string`을 사용하거나 `\`를 앞에 붙이고 이스케이프 코드를 작성하면된다.

```python
>>> new_dir1 = r"D:\newFile"
>>> new_dir2 = "D:\\newFile"

>>> print(new_dir1)
D:\newFile
>>> print(new_dir2)
D:/newFile
```

# f-string?

`f-string` 은 문자열 포매팅(formatting) 방식이다.

변수 값을 문자열 안에 포함시킬 때 자주 사용한다.

```python
>>> year = 2024
>>> new_str1 = "Hello" + " World " + year
>>> new_str2 = f"Hello World {year}"
```

`f-string`을 적용하기 전 방식대로 하면 `new_str1` 과 같이 선언하면 된다.

하지만 조금 더 직관적으로 볼 수 있도록 `f-string`을 적용해 `new_str2` 처럼 사용하면 더 좋다!

`:` 뒤에 정수를 전달하면 해당 필드의 최소 문자 폭이 된다!

이건 열을 줄 맞춤할 때 편하다.

```python
>>> table = {'Sjoerd': 4127, 'Jack': 4098, 'Dcab': 7678}
>>> for name, phone in table.items():
		print(f'{name:10} ==> {phone:10d}')

Sjoerd     ==>       4127
Jack       ==>       4098
Dcab       ==>       7678
```

추가로 `=`를 사용하면 이 값이 어떤 값인지 직관적으로 표현할 수 있다.

```python
>>> bugs = 'roaches'
>>> count = 13
>>> area = 'living room'
>>> print(f'Debugging {bugs=} {count=} {area=}')
Debugging bugs='roaches' count=13 area='living room'
```

## 문자열 format()?

문자열을 포매팅하는 방식으로는 `f-string` 말고도 또 있다.

바로 `format()` 메서드를 적용하는 것이다!

```python
>>> print('Hello World {}!"'.format(2024))
Hello World 2024!
```

문자열을 선언하고, 변수가 필요한 위치에 `{}`를 적고, 전달할 값을 `.format()` 의 인자로 전달하면 끝!

> 다른 포맷 방식이 있다..! 자주 사용하는 방식만 일단 적었고, 자세한 내용은 [python 공식 레퍼런스](https://docs.python.org/ko/3/tutorial/inputoutput.html) 를 참고하면 된다.
> 

# 참고 자료

[2.1.5 문자열](https://wikidocs.net/14633)

[7. 입력과 출력](https://docs.python.org/ko/3/tutorial/inputoutput.html)