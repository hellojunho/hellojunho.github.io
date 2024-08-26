---
layout: single
title: "📘 [Python] Ellipsis(…) vs pass"
toc: true
toc_sticky: true
toc_label: "목차"
categories: python
excerpt: ""
tag: []
---

# Ellipsis?

파이썬 코드를 읽다보면 `...` 으로 퉁 치는 코드가 자주 보인다.

최근에는 Django의 `CommonMiddleware` 클래스에서 확인할 수 있었다.

### Django의 CommonMiddleware

```python
from typing import Any

from django.http.request import HttpRequest
from django.http.response import HttpResponseBase, HttpResponsePermanentRedirect
from django.utils.deprecation import MiddlewareMixin

class CommonMiddleware(MiddlewareMixin):
    response_redirect_class: Any = ...
    def process_request(
        self, request: HttpRequest
    ) -> HttpResponsePermanentRedirect | None: ...
    def should_redirect_with_slash(self, request: HttpRequest) -> bool: ...
    def get_full_path_with_slash(self, request: HttpRequest) -> str: ...
    def process_response(
        self, request: HttpRequest, response: HttpResponseBase
    ) -> HttpResponseBase: ...

class BrokenLinkEmailsMiddleware(MiddlewareMixin):
    def process_response(
        self, request: HttpRequest, response: HttpResponseBase
    ) -> HttpResponseBase: ...
    def is_internal_request(self, domain: str, referer: str) -> bool: ...
    def is_ignorable_request(
        self, request: HttpRequest, uri: str, domain: str, referer: str
    ) -> bool: ...
```

`...` 가 갖는 의미가 무엇일까 궁금했다.

이 `...` 은 `Ellipsis` 라고 하는 파이썬 기호였고, 사전적으로는 `생략` , `생략부호` 라는 의미를 갖고 있다.

[파이썬 공식 문서](https://docs.python.org/3.9/library/constants.html#Ellipsis)에서 확인할 수 있었다.

결국 아무 동작도 하지 않는 것이므로, `pass` 처럼 사용할 수도 있는 것이었다.

> 일단 만들고 내부 구현은 나중에 자세히 하겠다는 의미로..!
> 

# pass?

`pass` 또한 `Ellipsis` 처럼 아무 동작을 수행하지 않는다는 것을 의미한다.

일부 코드가 구문 상 필요는 하지만 프로그램이 아무 작업도 하지 않기를 원하는 경우에 사용한다.

이렇게 말하면 어떤 경우에 사용해야하는 것인지 와닿지는 않는데, `추상클래스` 혹은 반복문에서의 경우를 생각하면 될 것 같다.

# 참고 자료

[파이썬 % // -> ** @ 등 파이썬 기호 완벽정리](https://modulabs.co.kr/blog/python-strangethings/)

[[python] 파이썬 pass 설명과 예제](https://blockdmask.tistory.com/535)