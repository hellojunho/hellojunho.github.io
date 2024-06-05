---
layout: single
title: "📘[Django] 진짜 간단한 Celery 실험"
toc: true
toc_sticky: true
toc_label: "목차"
categories: django
excerpt: ""
tag: [python, celery]
---

# Celery 실험 시작

`Celery` 에 대해서 이해는 되지만, 정작 적용하려고 보니 어떻게 동작하고 어떻게 고도화 해야하는 건지 눈 앞이 캄캄했다.

무엇이든지 시작이 반이니까, 1+1 수준의 간단한 실험으로 시작해보려고 한다.

**👉 1초마다 1씩 값을 증가시키고, 그 값을 확인하는 앱이다!**

## Django project & app

```python
mkdir celery_test

cd celery_test
```

- `celery_test` 라는 폴더를 만들고, 그 폴더로 접속한다.
    - 개인적으로 폴더 아래에 프로젝트를 `config` 라는 이름으로 생성하는 것을 선호함.

```python
django-admin startproject config .
```

- django project를 생성한다.

```python
python manage.py startapp api
```

- `api` 라는 이름의 앱을 생성한다.

## celery & redis install

```python
python -m venv venv

source venv/bin/activate
```

- 가상환경을 만들고, 실행한다.

```python
pip install celery
```

- django 3.1 부터는 기본으로 지원된다고 한다.

```python
pip install redis
```

- redis를 사용하기 위해 설치해준다.
    - 왜 redis? → 진짜 너무 간단한 실험이라서 비교적 설정이 쉬운 redis 선택

## config/settings.py

```python
INSTALLED_APPS = [

		...

		"api",
]

# Redis and Celery settings
REDIS_HOST = 'localhost'
REDIS_PORT = 6379
REDIS_DB = 0
REDIS_URL = f'redis://{REDIS_HOST}:{REDIS_PORT}/{REDIS_DB}'

CELERY_BROKER_URL = REDIS_URL
CELERY_RESULT_BACKEND = REDIS_URL
```

- 생성한 앱을 추가해주고
- redis 세팅을 위해 추가해준다.

## config/celery.py

```python
from __future__ import absolute_import, unicode_literals
import os
from celery import Celery
from celery.schedules import crontab

os.environ.setdefault('DJANGO_SETTINGS_MODULE', 'config.settings')

app = Celery('config')
app.config_from_object('django.conf:settings', namespace='CELERY')
app.autodiscover_tasks()

@app.task(bind=True)
def debug_task(self):
    print(f'Request: {self.request!r}')

app.conf.beat_schedule = {
    'increment-every-second': {
        'task': 'api.tasks.increment_number',
        'schedule': 1.0,  # 1 second
    },
}
```

- `os.environ.setdefault('DJANGO_SETTINGS_MODULE', '{PROJECT_NAME}.settings')`: 환경변수 세팅이다. → `{PROJECT_NAME}` 에는 프로젝트의 이름이 들어가면 된다. (난 `config`가 들어감)
- `app = Celery('{PROJECT_NAME}')`: `config` 라는 이름으로 Celery 인스턴스를 생성한다.
- 그 아래는 셀러리 기본 세팅이다.
- `app.conf.beat_schedule`: 셀러리 비트 설정이다. 실제 스케줄링을 돌릴 시간의 기준을 설정한다. 여기서는 1초.

## api/tasks.py

```python
import redis
from celery import shared_task
from django.conf import settings

# Redis에 연결
r = redis.Redis.from_url(settings.REDIS_URL)

@shared_task
def increment_number():
    # 현재 값을 가져옵니다. 값이 없으면 0으로 초기화합니다.
    current_value = r.get('current_number')
    if current_value is None:
        current_value = 0
    else:
        current_value = int(current_value)

    # 값을 1씩 증가시킵니다.
    current_value += 1

    # Redis에 저장합니다.
    r.set('current_number', current_value)

    return current_value
```

- `r = redis.Redis.from_url(settings.REDIS_URL)`: redis에 연결하는 설정이다.
- `@shared_task`: Celery의 작업임을 나타낸다. 이 작업은 `Worker`로 실행될 수 있다.
    - 아래 함수는 task의 이름이겠지
- redis에 `current_number`라는 컬럼 값을 가져온다.
- 만약 없으면 0으로 초기화한다.
- 본격적인 task 시작으로, `current_value`를 1씩 증가한다.
- 증가한 값을 `r.set("current_number", current_value)` 를 통해 redis에 저장한다.
- 결과를 리턴한다.

## api/views.py

```python
from django.http import JsonResponse
import redis
from django.conf import settings

r = redis.Redis(host=settings.REDIS_HOST, port=settings.REDIS_PORT, db=settings.REDIS_DB)

def show_hello(request):
    current_value = r.get('counter')
    if current_value is None:
        current_value = 0
    else:
        current_value = int(current_value)
    return JsonResponse({'counter': current_value})
```

- `r = redis.Redis(host=settings.REDIS_HOST, port=settings.REDIS_PORT, db=settings.REDIS_DB)`: redis 연결을 불러온다.
    - 아까보다 길어졌는데, 결국 똑같은거임. `settings.py`를 확인해보자.
- `current_value = r.get('counter')`: redis에서 `counter`라는 키의 값을 가져온다.
    - 여기서 알 수 있는게, `tasks.py`에서 redis에 값을 저장할 때, `{counter : current_number}` 로 저장했겠구나 싶은거임.
- `current_value` 값을 가져오고, 없으면 0으로 초기화, 있으면 int로 형변환 후 JSON 응답을 내보낸다.

## 실행하기

일단, 굉장히 많은 터미널을 사용했다..

모든 명령은 `루트 디렉토리(manage.py가 존재하는 위치)` 에서 진행한다.

### 1. redis 실행하기

```python
redis-server
```

- 만약 포트가 사용중이면?
    - `lsof -i :6379` → `sudo kill -9 [PID]`

### 2. redis cli 실행하기

```python
redis-cli
```

- 저장된 값 보고싶으면?
    - `GET current_number`

### 3. Celery worker 실행하기

```python
celery -A config worker --loglevel=info
```

### 4. Celery Beat 실행하기

```python
celery -A config beat --loglevel=info
```

### 5. runserver

```python
python manage.py runserver
```

### 6. api 접속해서 확인

나는 url을 [`http://127.0.0.1:8000/api/show_hello/`](http://127.0.0.1:8000/api/show_hello/) 로 설정했다.

# 궁금한 내용

`api/[tasks.py](http://tasks.py)`에서 current_value를 return 하는데, `api/views.py` 에서 redis를 연결하고, 거기서 값을 `get` 하는 것이 아니라, tasks.py의 값을 불러오면 될까?

> 사실 당연히 돼야하는거라고 생각하긴 했지만 일단 테스트 진행했음.
> 

## api/views.py

```python
from django.http import JsonResponse
import redis
from django.conf import settings

# r = redis.Redis(host=settings.REDIS_HOST, port=settings.REDIS_PORT, db=settings.REDIS_DB)

# def show_hello(request):
#     current_value = r.get('counter')
#     if current_value is None:
#         current_value = 0
#     else:
#         current_value = int(current_value)
#     return JsonResponse({'counter': current_value})

from .tasks import increment_number

def show_hello(request):
    current_value = increment_number()
    if current_value is None:
        current_value = 0
    else:
        current_value = int(current_value)
    return JsonResponse({'counter': current_value})
```

- 이전 내용은 그냥 일단 주석처리하고, `increment_number` 를 import해서 실행했다.
- 정상적으로 숫자가 올라간다.

# 참고자료

[First steps with Django — Celery 5.4.0 documentation](https://docs.celeryq.dev/en/stable/django/first-steps-with-django.html#using-celery-with-django)