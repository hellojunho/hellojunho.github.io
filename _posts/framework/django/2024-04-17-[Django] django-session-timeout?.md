---
layout: single
title: "📘[Django] django-session-timeout이 뭐지?"
toc: true
toc_sticky: true
toc_label: "목차"
categories: django
excerpt: ""
tag: [python, django]
---

# django-session-timeout?

`django-session-timeout` 은 **세션을 종료할 수 있는 시간을 지정** 할 수 있는 패키지이다.

- 로그인을 진행하고, 일정 시간이 지나면 자동으로 로그아웃 되도록

한 번 로그인한 후, 해당 계정이 자동으로 로그아웃이 되지 않는다면 보안상 문제가 발생할 수도 있다.

그래서 많은 서비스들은 사용자의 **마지막 활동 시점**을 기준으로 일정 시간동안 활동이 없으면 세션을 만료시킨다.

django에서는 이 패키지 외에도 django 자체에서 제공하는 `django.contrib.sessions`을 사용할 수도 있다!

# django-session-timeout 설치하기

```python
$ pip install django-session-timeout
```

위 명령어를 통해 패키지를 다운받고, 아래와 같이 `MIDDLEWARE_CLASSES` 에도 코드를 추가해준다.

```python
MIDDLEWARE_CLASSES = [
    # ...
    'django.contrib.sessions.middleware.SessionMiddleware',
    'django_session_timeout.middleware.SessionTimeoutMiddleware',
    # ...
]
```

# [settings.py](http://settings.py)에 세션 값 지정하기

## SESSION_EXPIRE_SECONDS

```python
SESSION_EXPIRE_SECONDS = 3600
```

- SECONDS인 만큼 초(sec)를 기준으로 설정
- 위와 같이 3600을 지정하면, 1시간의 세션 시간을 설정

## SESSION_EXPIRE_AFTER_LAST_ACTIVITY

```python
SESSION_EXPIRE_AFTER_LAST_ACTIVITY = True
```

- `LAST_ACTIVITY` 인 것 처럼 사용자의 마지막 활동 시점을 의미
- True로 지정했으니, 사용자의 마지막 활동 시점을 기준으로 세션을 초기화 한다는 의미
- 이 옵션을 작성하지 않으면 기본으로 1시간 후에 종료하게 됨
- 기본으로는 1초로 지정되어있고, 1초 뒤에 다시 세션을 시작함

## SESSION_EXPIRE_AFTER_LAST_ACTIVITY_GRACE_PERIOD

```python
SESSION_EXPIRE_AFTER_LAST_ACTIVITY_GRACE_PERIOD = 60
```

- `SESSION_EXPIRE_AFTER_LAST_ACTIVITY` 옵션의 기본값이 1인데, 위 옵션을 지정하면 60초로 지정할 수 있음

## SESSION_TIMEOUT_REDIRECT

```python
SESSION_TIMEOUT_REDIRECT = 'redirect url'
```

- 세션이 종료된 후, 이동할 url을 작성
- 로그인 화면 혹은 메인 화면을 주로 설정함

# 참고 자료

[[DJANGO] 장고 자동 로그아웃](https://ssilook.tistory.com/entry/Django장고-자동-로그아웃)

[[Django] django-session-timeout은 뭐야?](https://hangjastar.tistory.com/204)