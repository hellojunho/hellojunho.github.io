---
layout: single
title: "📘[Django] settings.py 파헤치기 4 - MIDDLEWARE (SessionMiddleware)"
toc: true
toc_sticky: true
toc_label: "목차"
categories: django
excerpt: ""
tag: [python]
---

# SessionMiddleware란?

```python
from django.contrib.sessions.backends.base import SessionBase
from django.http.request import HttpRequest
from django.http.response import HttpResponse
from django.utils.deprecation import MiddlewareMixin

class SessionMiddleware(MiddlewareMixin):
    SessionStore: type[SessionBase] = ...
    def process_request(self, request: HttpRequest) -> None: ...
    def process_response(
        self, request: HttpRequest, response: HttpResponse
    ) -> HttpResponse: ...
```

`SessionMiddleware` 는 Django에서 사용자별로 상태 정보를 유지하는 데 필요한 기능을 제공하는 미들웨어이다.

이를 통해 로그인 상태 유지, 장바구니 등과 같은 기능을 구현할 수 있다.

세션 데이터는 서버 측에 저장되어 관리되며, 클라이언트는 쿠키를 통해 세션을 식별할 수 있다.

## [ChatGPT] SessionMiddleware의 주요 역할

### 세션 데이터 관리

`SessionMiddleware` 는 각 HTTP 요청에 대해 세션 데이터를 로드하고, 이를 `request.session` 이라는 요소에 저장함!

이 요소를 통해 세션 데이터를 읽고 쓸 수 있다!

> *views function에서 request 변수를 사용할 때 사용하면 될 듯!*
> 

### 세션의 생성 및 유지

Django의 세션은 `쿠키` 기반으로 관리됨.

`SessionMiddleware` 는 클라이언트에게 `sessionid` 라는 이름의 쿠키를 설정한다.

이 쿠키는 서버에 저장된 세션 데이터를 참조하는 고유한 키임!

또, 클라이언트가 서버에 요청을 보낼 때마다, Django는 `sessionid` 쿠키를 사용해 서버 측에서 해당 사용자의 세션 데이터를 로드함.

### 세션 데이터의 지속성

사용자가 웹사이트를 떠났다가 다시 돌아와도, 세션 데이터가 유지될 수 있도록 쿠키의 만료 시간을 설정할 수 있다.

세션 만료 시간은 Django 설정 파일의 `SESSION_COOKIE_AGE` 로 조절할 수 있고, 기본적으로 2주(1209600초)로 설정되어 있다!!

세션이 유효하지 않거나, 만료되면 새로운 세션이 생성됨.

```python
$ python manage.py shell
```

```python
>>> from django.conf import settings
>>> settings.SESSION_COOKIE_AGE
1209600
```

### 데이터의 영속성 및 저장소

Django는 다양한 세션 백엔드를 지원한다.

세션 데이터는 DB, 캐시, 파일 시스템, 암호화된 쿠키 등에 저장할 수 있다!

세션 백엔드는 `SESSION_ENGINE` 설정을 통해 변경할 수 있음.

예를 들어 ,`django.contrib.sessions.backends.db` 는 DB에 세션을 저장하며 기본 설정임.

또, `django.contrib.sessions.backends.cache` 는 캐시에 저장하며 `성능이 중요한 경우`에 사용될 수 있다.

### 세션 데이터의 보안

세션 데이터는 기본적으로 클라이언트에게 노출되지 않는다!

세션 ID만 클라이언트에 쿠키로 전달되고, 이 ID를 통해 서버에서 세션 데이터를 찾는 방식이다.

이 방식은 클라이언트 측에서 세션 데이터를 조작할 수 없게 하여 보안을 강화할 수 있다!

`SESSION_COOKIE_SECURE`, `SESSION_COOKIE_HTTPONLY` 등의 설정을 통해 세션 쿠키의 보안을 강화할 수 있다.

## [공식 문서] django.contrib.sessions.middleware.SessionMiddleware

- [**SESSION_CACHE_ALIAS**](https://docs.djangoproject.com/en/5.1/ref/settings/#std-setting-SESSION_CACHE_ALIAS)
    - 기본 값: ‘default’
    - [`cache-based session storage`](https://docs.djangoproject.com/en/5.1/topics/http/sessions/#cached-sessions-backend) 를 사용하는 경우, 사용할 캐시를 선택한다.
- [**SESSION_COOKIE_AGE**](https://docs.djangoproject.com/en/5.1/ref/settings/#std-setting-SESSION_COOKIE_AGE)
    - 기본 값: 1209600(초) - 2주
    - 세션 및 쿠키의 수명이다.
- [**SESSION_COOKIE_DOMAIN**](https://docs.djangoproject.com/en/5.1/ref/settings/#std-setting-SESSION_COOKIE_DOMAIN)
    - 기본 값: None
    - 세션 및 쿠키에 사용할 도메인을 뜻한다. “example.com”과 같은 문자열로 설정하거나 표준 도메인 쿠키의 경우, None을 사용한다.
    - 만약 [**`CSRF_USE_SESSIONS`**](https://docs.djangoproject.com/en/5.1/ref/settings/#std-setting-CSRF_USE_SESSIONS)와 함께 교차 도메인 쿠키를 사용하려면 ****CSRF 미들웨어의 참조자 확인을 위해 leading dot(예: example.com)을 포함해야 한다.
    - 이전에 표준 도메인 쿠키를 사용하던 사이트에서 교차 도메인 쿠키를 사용하도록 이 설정을 업데이트 하게되면, 기존 사용자 쿠키가 이전 도메인으로 설정되어 쿠키가 지속되는 한 로그인을 하지 못하는 경우가 발생할 수 있으니 주의하자.
    - 이 설정은 [**`django.contrib.messages`](https://docs.djangoproject.com/en/5.1/ref/contrib/messages/#module-django.contrib.messages)** 쿠키에도 영향을 미침!
- [**SESSION_COOKIE_HTTPONLY**](https://docs.djangoproject.com/en/5.1/ref/settings/#std-setting-SESSION_COOKIE_HTTPONLY)
    - 기본 값: True
    - 세션 쿠키에 [`HttpOnly`](https://owasp.org/www-community/HttpOnly) 플래그를 사용할지 여부를 선택하는 옵션이다. 이 옵션을 True로 설정하면 클라이언트 측 JavaScriptrk 세션 및 쿠키에 엑세스할 수 없다.
    - [`HttpOnly`](https://owasp.org/www-community/HttpOnly) 는 Set-Cookie-HTTP 응답 헤더에 포함된 플래그이다.
- [**SESSION_COOKIE_NAME**](https://docs.djangoproject.com/en/5.1/ref/settings/#std-setting-SESSION_COOKIE_NAME)
    - 기본 값: ‘sessionid’
    - 세션에 사용할 쿠키의 이름임!
    - 애플리케이션의 다른 쿠키 이름과 다르지 않다면, 원하는 이름으로 지정할 수 있다.
- [**SESSION_COOKIE_PATH**](https://docs.djangoproject.com/en/5.1/ref/settings/#std-setting-SESSION_COOKIE_PATH)
    - 기본 값: ‘/’
    - 세션 및 쿠키에 설정된 경로임!
    - 이 경로는 Django 설치 경로와 일치하거나 해당 경로의 상위 경로여야 한다. 동일한 호스트 이름으로 실행되는 여러 Django 인스턴스가 있는 경우 유용하며, 각 인스턴스는 서로 다른 쿠키 경로를 사용할 수 있다! 또, 각 인스턴스에는 자체 세션 쿠키만 표시된다.
- [**SESSION_COOKIE_SAMESITE**](https://docs.djangoproject.com/en/5.1/ref/settings/#std-setting-SESSION_COOKIE_SAMESITE)
    - 기본 값: ‘Lax’
    - 세션 및 쿠키의 `SameSite` 플래그 값이다.
    - 이 플래그는 사이트 간의 요청에서 쿠키가 전송되는 것을 방지하여 CSRF 공격을 방지하고, 세션 쿠키를 훔치는 일부 방법을 불가능하게 한다.
    - 설정 가능한 값
        - ‘Strict’ (엄격)
            - 일반 링크를 따르는 경우에도 모든 교차 사이트 탐색 컨텍스트에서 브라우저가 쿠키를 대상 사이트로 보내지 못하도록 한다.
        - ‘Lax’ (느슨함)
            - 외부 링크를 통해 사용자가 도착한 후에도 로그인한 세션을 유지하려는 웹 사이트에 보안과 사용성 간의 균형을 제공한다.
        - ‘None’
            - 모든 동일 사이트 및 교차 사이트 요청과 함께 세션 및 쿠키가 전송된다.
        - False
            - 플래그를 비활성화한다.
- [**SESSION_COOKIE_SECURE**](https://docs.djangoproject.com/en/5.1/ref/settings/#std-setting-SESSION_COOKIE_SECURE)
    - 기본 값: False
    - 세션 및 쿠키에 보안 쿠키를 사용할지에 대한 여부 옵션이다!
    - 이 옵션을 True로 설정할 경우, 쿠키가 “보안”으로 표시되어 브라우저는 HTTPS 연결에서만 쿠키를 전송할 수 있도록 한다.
    - 이 설정을 해제할 경우, 공격자가 `패킷 스니퍼` 로 암호화되지 않은 세션 및 쿠키를 캡쳐하여 탈취할 수 있으므로, 해제하는 것은 좋은 선택은 아님!
- [**SESSION_ENGINE**](https://docs.djangoproject.com/en/5.1/ref/settings/#std-setting-SESSION_ENGINE)
    - 기본 값: ‘django.contrib.sessions.backends.db’
    - Django가 세션 데이터를 저장하는 위치를 제어한다.
    - 다양한 엔진들
        - ‘django.contrib.sessions.backends.db’
        - ‘django.contrib.sessions.backends.file’
        - ‘django.contrib.sessions.backends.cache’
        - ‘django.contrib.sessions.backends.cached_db’
        - ‘django.contrib.sessions.backends.signed_cookies’
    - 자세한 내용은 [`Configuring the session engine`](https://docs.djangoproject.com/en/5.1/topics/http/sessions/#configuring-sessions) 를 참고할 수 있다!
- [**SESSION_EXPIRE_AT_BROWSER_CLOSE**](https://docs.djangoproject.com/en/5.1/ref/settings/#std-setting-SESSION_EXPIRE_AT_BROWSER_CLOSE)
    - 기본 값: False
    - 사용자가 브라우저를 닫을 때 세션을 만료할지에 대한 여부 옵션임!
    - [`Browser-length sessions vs. persistent sessions`](https://docs.djangoproject.com/en/5.1/topics/http/sessions/#browser-length-vs-persistent-sessions) 를 참고할 수 있다.
- [**SESSION_FILE_PATH**](https://docs.djangoproject.com/en/5.1/ref/settings/#std-setting-SESSION_FILE_PATH)
    - 기본 값: None
    - 파일 기반 세션 저장소를 사용하는 경우, Django가 세션 데이터를 저장할 디렉터리를 설정한다.
    - 기본 값(None)을 사용하는 경우, Django는 시스템의 표준 임시 디렉터리를 사용함!
- [**SESSION_SAVE_EVERY_REQUEST**](https://docs.djangoproject.com/en/5.1/ref/settings/#std-setting-SESSION_SAVE_EVERY_REQUEST)
    - 기본 값: False
    - 모든 요청에 대한 세션 데이터를 저장할지에 대한 여부를 정하는 옵션임!
    - 기본 값(False)를 사용하면 세션 데이터가 수정되었을 때(즉, 해당 사전 값이 할당되거나 삭제된 경우), 세션 데이터가 저장된다.
    - 이 설정이 활성화 되어있더라도 빈 세션은 생성되지 않는다.
- [**SESSION_SERIALIZER**](https://docs.djangoproject.com/en/5.1/ref/settings/#std-setting-SESSION_SERIALIZER)
    - 기본 값: ‘django.contrib.sessions.serializers.JSONSerializer’
    - 세션 데이터를 직렬화하는 데 사용할 serializer 클래스의 전체 가져오기 경로를 의미한다.
    - [`Session serialization`](https://docs.djangoproject.com/en/5.1/topics/http/sessions/#session-serialization) 를 참고할 수 있다!

# 또 다른 Django 세션 사용 방법?

Django에서는 `SessionMiddleware` 를 사용하는 것 이외에 `django-session-timeout` 이라는 라이브러리를 사용해서 세션 만료 등의 기능을 구현할 수 있다!

👉 [`django-session-timeout이 뭐지?`](https://hellojunho.github.io/django/Django-django-session-timeout/) 

보통 서드파티 라이브러리나 프레임워크 등의 경우는 기존에 존재하는 어떤 기술의 어떠한 불편한 점(?)을 개선하기 위해 등장한다.

> *django-session-timeout 은 어떤 점을 개선하기 위해 등장한거지?*
> 

사실 대부분의 경우에는 기존의 `SessionMiddleware` 로 충분하다. 

하지만, `SessionMiddleware` 는 사용자의 `비활성상태` 즉, 일정 시간 동안 활동이 없을 경우에 대한 부분이 부족하다고 한다.

이러한 부분을 `django-session-timeout` 라이브러리를 통해 해결할 수 있다.

또, `django-session-timeout` 은 세션 만료 시 리디렉션 할 페이지를 지정할 수 있다는 점도 있다.

일반적으로  보안이 중요한 경우 혹은 특별히 애플리케이션에서 사용자의 비활성상태애 따른 세션 만료 기능을 요구하는 경우에는 `SessionMiddleware` 와 `django-session-timeout` 을 적절하게 혼용해주면 될 것 같다..!

# 참고 자료

[Settings | Django documentation](https://docs.djangoproject.com/en/5.1/ref/settings/#id15)