---
layout: single
title: "📘[Django] settings.py 파헤치기 3 - MIDDLEWARE (SecurityMiddleware)"
toc: true
toc_sticky: true
toc_label: "목차"
categories: django
excerpt: ""
tag: [python]
---

# SecurityMiddleware란?

```python
from typing import Any

from django.http.request import HttpRequest
from django.http.response import HttpResponse, HttpResponsePermanentRedirect
from django.utils.deprecation import MiddlewareMixin

class SecurityMiddleware(MiddlewareMixin):
    sts_seconds: int = ...
    sts_include_subdomains: bool = ...
    sts_preload: bool = ...
    content_type_nosniff: bool = ...
    xss_filter: bool = ...
    redirect: bool = ...
    redirect_host: str | None = ...
    redirect_exempt: list[Any] = ...
    def process_request(
        self, request: HttpRequest
    ) -> HttpResponsePermanentRedirect | None: ...
    def process_response(
        self, request: HttpRequest, response: HttpResponse
    ) -> HttpResponse: ...
```

`SecurityMiddleware` 에 대해 ChatGPT에게 물어봤다.

`SecurityMiddleware` 는 `django-admin startproject` 명령어를 통해 생성한 프로젝트라면 기본적으로 포함되는 미들웨어 중 하나로, Django 애플리케이션의 보안을 강화하는 여러 가능을 제공하는 미들웨어이다.

## [ChatGPT] SecurityMiddleware의 주요 역할

### HTTPS 리디렉션

`SECURE_SSL_REDIRECT` 설정이 `True` 인 경우, 모든 HTTP 요청을 HTTPS로 리디렉션한다.

이는 클라이언트가 안전한 HTTPS 프로토콜을 통해서만 서버에 접속하도록 강제하는 역할임!

이렇게 함으로써 데이터 전송 중 발생할 수 있는 도청 공격을 예방할 수 있다!

### HTTP Strict Transport Security (HSTS)

`SECURE_HSTS_SECONDS` 설정을 통해 HSTS 헤더를 설정할 수 있다.

이 헤더는 브라우저에게 특정 기간동안 해당 도메인에 대해 오직 HTTPS 연결만 허용하도록 지시하는 역할이다!

HSTS는 HTTPS로 사이트에 처음 접속한 이후 다시 HTTPS로 접속하지 못하게 하여, `중간자 공격(MITM)`  을 예방함!

만약 `SECURE_HSTS_INCLUDE_SUBDOMAINS` 를 `True` 로 설정하면,. 서브 도메인에서도 HSTS 정책이 적용된다..!

또, `SECURE_HSTS_PRELOAD` 설정을 사용하면 브라우저의 HSTS 프리로드 리스트에 도메인을 추가할 수 있다고 한다.

### X-Content-Type-Options 헤더 설정

응답 헤더에 `X-Content-Type-Options: nosniff` 헤더를 추가하면, 브라우저가 선언된 콘텐츠 유형(MIME 타입)을 무시하고 파일을 다른 방식으로 처리하지 않도록 한다.

이 헤더는 브라우저가 잘못된 MIME 타입을 강제로 해석하지 않도록 하여 `MIME 스니핑 공격` 을 방지한다!

### X-XSS-Protection 헤더 설정

`X-XSS-Protection: 1; mode-block` 헤더를 추가하여, 브라우저의 `XSS(교차 사이트 스크립팅)` 필터를 활성화하고 공격이 탐지될 경우 페이지 로드를 중지시킨다!

이 헤더는 기본적으로 모든 응답에 포함된다고 함.

### X-Frame-Options 헤더 설정

`클릭재킹(Clickjacking)` 공격을 방지하기 위해 응답 헤더에 `X-Frame-Options` 를 설정한다.

기본적으로 `DENY` 로 설정되어 있는데, 해당 페이지가 iframe으로 포함되지 못하도록 함!

이렇게 하면 다른 웹사이트에서 해당 페이지를 불법적으로 프레임에 포함시키는 공격을 예방할 수 있다고 한다..!

## [공식 문서] django.middleware.security.SecurityMiddleware

Django는 `request/reseponse` 주기에 따라 여러 가지 보안 강화를 위해 `SecurityMiddleware` 를 제공한다.

다음의 설정들을 각각 조작하여 활성화 및 비활성화 할 수 있다.

- [SECURE_CONTENT_TYPE_NOSNIFF](https://docs.djangoproject.com/en/5.1/ref/settings/#std-setting-SECURE_CONTENT_TYPE_NOSNIFF)
    - 기본 값: True
    - True 이면, 이미 해당 헤더가 없는 모든 응답에 [`X-Content-Type-Options:nosniff](https://www.notion.so/Python-Celery-task-bind-True-False-8c9ffa58286f4161abf58c895bc677b9?pvs=21)SecurityMiddleware` 헤더를 설정한다.
- [SECURE_CROSS_ORIGIN_OPENER_POLICY](https://docs.djangoproject.com/en/5.1/ref/settings/#std-setting-SECURE_CROSS_ORIGIN_OPENER_POLICY)
    - 기본 값: ‘same-origin’
    - None으로 설정하지 않으면 `SecurityMiddleware` 는 [`Cross-Origin Opener Policy`](https://docs.djangoproject.com/en/5.1/ref/middleware/#cross-origin-opener-policy) ****헤더가 없는 모든 응답에 대해 제공된 값으로 설정된다.
- [SECURE_HSTS_INCLUDE_SUBDOMAINS](https://docs.djangoproject.com/en/5.1/ref/settings/#std-setting-SECURE_HSTS_INCLUDE_SUBDOMAINS)
    - 기본 값: False
    - True인 경우, `SecurityMiddleware` 는 `includeSubDomains` 지시문을 [`HTTP Strict Transport Security`](https://docs.djangoproject.com/en/5.1/ref/middleware/#http-strict-transport-security)  헤더에 추가된다. 또, `SECURE_HSTS_SECONDS` 를 0이 아닌 값으로 설정하지 않으면 이 지시어는 아무런 영향을 미치지 않는다.
    - 이 값을 잘못 설정하게 되면 사이트가 돌이킬 수 없게 손상될 수 있으니 주의해야 한다.
- [SECURE_HSTS_PRELOAD](https://docs.djangoproject.com/en/5.1/ref/settings/#std-setting-SECURE_HSTS_PRELOAD)
    - 기본 값: False
    - True인 경우, `SecurityMiddleware` 가 [`HTTP Strict Transport Security`](https://docs.djangoproject.com/ko/5.1/ref/middleware/#http-strict-transport-security) 헤더에 `preload` 지시어를 추가한다. 또, [`SECURE_HSTS_SECONDS`](https://docs.djangoproject.com/ko/5.1/ref/settings/#std-setting-SECURE_HSTS_SECONDS) 를 0이 아닌 값으로 설정하지 않게되면 이 지시어는 아무런 영향을 미치지 않는다.
- [SECURE_HSTS_SECONDS](https://docs.djangoproject.com/en/5.1/ref/settings/#std-setting-SECURE_HSTS_SECONDS)
    - 기본 값: 0
    - 0이 아닌 정수 값으로 설정하면, `SecurityMiddleware` 가 아직 없는 모든 응답에 [`HTTP Strict Transport Security`](https://docs.djangoproject.com/ko/5.1/ref/middleware/#http-strict-transport-security) 헤더를 설정한다.
    - 잘못된 값으로 설정할 경우, 사이트가 돌이킬 수 없이 손상될 수 있으니 주의하자.
- [SECURE_REDIRECT_EXEMPT](https://docs.djangoproject.com/en/5.1/ref/settings/#std-setting-SECURE_REDIRECT_EXEMPT)
    - 기본 값: [] (빈 리스트)
    - URL 경로가 이 리스트의 정규식과 일치하면, 요청이 HTTPS로 리디렉션되지 않는다. `SecurityMiddleware` 는 URL 경로에서 `/` 를 제거하므로, 패턴에 `/` 를 포함시키지 않도록 해야한다.
- [SECURE_REFERRER_POLICY](https://docs.djangoproject.com/en/5.1/ref/settings/#std-setting-SECURE_REFERRER_POLICY)
    - 기본 값: ‘same-origin’
    - 이 설정된 경우 `SecurityMiddleware` 는 [`Referrer Policy`](https://docs.djangoproject.com/ko/5.1/ref/middleware/#referrer-policy) 헤더가 없는 모든 응답의 참조자 정책 헤더를 제공 받은 값으로 설정된다.
- [SECURE_SSL_HOST](https://docs.djangoproject.com/en/5.1/ref/settings/#std-setting-SECURE_SSL_HOST)
    - 기본 값: None
    - 문자열인 경우, 모든 SSL 리디렉션은 원래 요청된 호스트가 아닌 이 호스트로 연결된다. [`SECURE_SSL_REDIRECT`](https://docs.djangoproject.com/ko/5.1/ref/settings/#std-setting-SECURE_SSL_REDIRECT) 가 False인 경우, 이 설정은 아무런 영향을 미치지 않는다.
- [SECURE_SSL_REDIRECT](https://docs.djangoproject.com/en/5.1/ref/settings/#std-setting-SECURE_SSL_REDIRECT)
    - 기본 값: False
    - True인 경우, `SecurityMiddleware` 는 모든 non-HTTPS [`redirects`](https://docs.djangoproject.com/ko/5.1/ref/middleware/#ssl-redirect) 을 HTTPS로 리디렉션한다. ([`SECURE_REDIRECT_EXEMPT`](https://docs.djangoproject.com/ko/5.1/ref/settings/#std-setting-SECURE_REDIRECT_EXEMPT) 에 명시된 정규식과 일치하는 URL은 제외임.)

# 참고 자료

[Middleware | Django documentation](https://docs.djangoproject.com/en/5.1/ref/middleware/)