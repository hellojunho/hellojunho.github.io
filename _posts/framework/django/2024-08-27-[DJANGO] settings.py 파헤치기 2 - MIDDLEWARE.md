---
layout: single
title: "📘[Django] settings.py 파헤치기 2 - MIDDLEWARE"
toc: true
toc_sticky: true
toc_label: "목차"
categories: django
excerpt: ""
tag: [python]
---

# 미들웨어가 뭘까?

Django의 MIDDLEWARE를 알기 위해 [Django 공식 문서](https://docs.djangoproject.com/en/5.1/topics/http/middleware/)를 참조했다.

`미들웨어` 는 Django의 요청/응답 처리에 대한 `후크 프레임워크` 이다.

또한, Django의 입력/출력을 전역적으로 변경하기 위한 가볍고 저수준의 `플러그인` 시스템이다.

각 미들웨어의 구성 요소는 특정 기능을 수행하는 목적을 갖고 있는데, 예를 들어 Django의 [`AuthenticationMiddleware`](https://docs.djangoproject.com/en/5.1/ref/middleware/#django.contrib.auth.middleware.AuthenticationMiddleware) 세션을 사용하여 사용자를 요청과 연결하는 미들웨어 구성 요소가 포함되어 있다. 

> 즉, 미들웨어는 서로 다른 애플리케이션이 서로 통신하는 데 사용되는 소프트웨어인 것이다.
> 

# Django MIDDLEWARE 활성화

미들웨어 구성 요소를 활성화하려면 Django의 settings.py에서 [**`MIDDLEWARE`](https://docs.djangoproject.com/en/5.1/ref/settings/#std-setting-MIDDLEWARE)** 리스트에 추가하면 된다.

 [**`MIDDLEWARE`](https://docs.djangoproject.com/en/5.1/ref/settings/#std-setting-MIDDLEWARE)** 에서 미들웨어 구성 요소는 문자열(미들웨어 팩토리의 클래스 또는 함수 이름에 대한 전체 Python 경로)로 표현된다.

`django-admin startproject` 명령어를 사용하여 생성한 프로젝트의 기본 MIDDLEWARE 목록은 다음과 같다.

```python
MIDDLEWARE = [
    "django.middleware.security.SecurityMiddleware",
    "django.contrib.sessions.middleware.SessionMiddleware",
    "django.middleware.common.CommonMiddleware",
    "django.middleware.csrf.CsrfViewMiddleware",
    "django.contrib.auth.middleware.AuthenticationMiddleware",
    "django.contrib.messages.middleware.MessageMiddleware",
    "django.middleware.clickjacking.XFrameOptionsMiddleware",
]
```

Django를 설치할 때는 사실 미들웨어가 필요하지 않아, MIDDLEWARE가 비어있어도 된다.

하지만, 최소한  [**`CommonMiddleware`**](https://docs.djangoproject.com/en/5.1/ref/middleware/#django.middleware.common.CommonMiddleware)를 사용할 것을 강력히 권장한다고 한다.

미들웨어는 다른 미들웨어에 종속될 수 있어, `미들웨어의 순서가 중요하다!` 

예를 들면 `AuthenticationMiddleware` 는 인증된 사용자를 세션에 저장하므로 `SessionMiddleware` 다음에 실행되어야 한다.

## default MIDDLEWARE 파헤치기!

- [django.middleware.security.SecurityMiddleware](https://hellojunho.github.io/django/DJANGO-settings.py-%ED%8C%8C%ED%97%A4%EC%B9%98%EA%B8%B0-3-MIDDLEWARE-(SecurityMiddleware)/)
- django.contrib.sessions.middleware.SessionMiddleware
- django.middleware.common.CommonMiddleware
- django.middleware.csrf.CsrfViewMiddleware
- django.contrib.auth.middleware.AuthenticationMiddleware
- django.contrib.messages.middleware.MessageMiddleware
- django.middleware.clickjacking.XFrameOptionsMiddleware

# 참고 자료

[Middleware | Django documentation](https://docs.djangoproject.com/en/5.1/topics/http/middleware/)