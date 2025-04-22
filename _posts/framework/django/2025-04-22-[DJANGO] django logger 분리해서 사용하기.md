---
layout: single
title: "📘[Django] django logger 파일 분리하기?"
toc: true
toc_sticky: true
toc_label: "목차"
categories: django
excerpt: ""
tag: [log, logger]
---


# 로그를 분리한다는 말이 뭘까?

전문적으로 존재하는 단어는 아니고, 그냥 스스로 지은 말이긴 함..!

내가 생각한 로그의 분리란, API 별로 로그 파일을 따로 관리하고 싶다는 말이다.

예를 들면 하나의 레포지토리가 있다고 할 때, 두 가지의 서비스가 결합된 형태일 수 있다.

즉, 엔드포인트가 두가지 갈래길로 나눠지는 경우를 말하고, `/api/endpoint-1`, `/api/endpoint-2` 를 기점으로 두 가지의 서비스를 하나의 프로젝트에서 관리하는 프로젝트를 의미한다!

*왜 이렇게 하는가? 레거시 프로젝트가 애초에 이렇게 되어있었고, 이게 올바른 방향인지 아닌지는 잘 모르겠기도 하고, 리팩토링 하기에도 시간이 너무 많이 소요될 것 같아서 일단 그대로.*.

## 그래서 어떻게 하는데?

우선 지금 진행 중인 프로젝트는 `aptifit` 이라는 프로젝트와 `dashboard` 프로젝트 두 가지가 존재한다.

이 두 프로젝트가 위에서 언급한 내용처럼 하나의 레포지토리에서 작성되었다.

즉 django 프로젝트에서  두 가지의 `app`이 있는 것과 같은 형태라고 보면 될 듯 하다. (실제로는 하나의 app에 존재하긴 하지만..!)

이 때 하나의 로그 파일에서 두 앱에 대한 로그들을 모두 관리하면( `DEBUG`, `INFO`, `WARNING`, `ERROR`, `CRITICAL` 레벨의 로그) 어떤 앱에서 찍힌 로그인지 한 눈에 관리하기 어려울 것 같아서 로그 파일을 분리하는 것에 대해 고민해보게 되었다.

우선 가장 먼저 ChatGPT에게 물어봤다.

> 🗣️  어떤 api에서는 a.log 파일에 로그를 작성하고, 또 다른 api에서는 b.log 파일에 로그를 작성하고 싶어.
> 
> 
> 예를 들면 views_*.py 파일들은 aptifit.log, dashboard_*.py에는 dashboard.log 파일에
> 

### 1. **별도 Logger 이름을 직접 설정하기**

각 파일에서 `logger = logging.getLogger(<그룹명>)` 을 쓰고, `settings.py`의 `LOGGING`에 해당 그룹용 핸들러를 분리해 주는 방식입니다.

```python
# views_orders.py
import logging
logger = logging.getLogger('aptifit')   # → aptifit.log 로 간다

def order_view(...):
    logger.info("Order view called")
    ...

# dashboard_students.py
import logging
logger = logging.getLogger('dashboard')  # → dashboard.log 로 간다

def students(...):
    logger.error("Student lookup error", exc_info=True)
    ...

```

```python
# settings.py

import os
BASE_DIR = os.path.dirname(os.path.dirname(__file__))

LOGGING = {
    'version': 1,
    'disable_existing_loggers': False,
    'formatters': {
        'verbose': {
            'format': '%(asctime)s %(levelname)-8s %(name)s %(message)s'
        },
    },
    'handlers': {
        'aptifit_file': {
            'level': 'DEBUG',
            'class': 'logging.FileHandler',
            'filename': os.path.join(BASE_DIR, 'logs/aptifit.log'),
            'formatter': 'verbose',
        },
        'dashboard_file': {
            'level': 'DEBUG',
            'class': 'logging.FileHandler',
            'filename': os.path.join(BASE_DIR, 'logs/dashboard.log'),
            'formatter': 'verbose',
        },
    },
    'loggers': {
        # getLogger('aptifit') → aptifit_file
        'aptifit': {
            'handlers': ['aptifit_file'],
            'level': 'DEBUG',
            'propagate': False,
        },
        # getLogger('dashboard') → dashboard_file
        'dashboard': {
            'handlers': ['dashboard_file'],
            'level': 'DEBUG',
            'propagate': False,
        },
    },
}

```

# 적용해보자!

## settings.py

```python

LOGGING = {
    "version": 1,
    "disable_existing_loggers": False,   # Django 기본 로거 유지
    "formatters": {
        "custom": {
            "format": "[{asctime}] [{filename}:{lineno}] [{levelname}] {message}",
            "style": "{",
            "datefmt": "%Y-%m-%d %H:%M:%S",
        },
    },
    "handlers": {
        "console": {
            "class": "logging.StreamHandler",
            "formatter": "custom",
            "level": "WARNING",
        },
        "aptifit_file": {
            "class": "logging.handlers.TimedRotatingFileHandler",
            "formatter": "custom",
            "filename": os.path.join(LOG_DIR, "aptifit.log"),
            "when": "midnight",
            "interval": 1,
            "backupCount": 7,
            "encoding": "utf-8",
            "level": "DEBUG",
        },
        "dashboard_file": {
            "class": "logging.handlers.TimedRotatingFileHandler",
            "formatter": "custom",
            "filename": os.path.join(LOG_DIR, "dashboard.log"),
            "when": "midnight",
            "interval": 1,
            "backupCount": 7,
            "encoding": "utf-8",
            "level": "DEBUG",
        },
    },
    "loggers": {
        # root: WARNING 이상만 콘솔로
        "": {
            "handlers": ["console"],
            "level": "INFO",
            "propagate": False,
        },
        # Django 자체 로그도 WARNING 이상만 콘솔
        "django": {
            "handlers": ["console"],
            "level": "INFO",
            "propagate": False,
        },
        # views_*.py 에서 getLogger('aptifit') 호출 시
        "aptifit": {
            "handlers": ["console", "aptifit_file"],
            "level": "DEBUG",
            "propagate": False,
        },
        # dashboard_*.py 에서 getLogger('dashboard') 호출 시
        "dashboard": {
            "handlers": ["console", "dashboard_file"],
            "level": "DEBUG",
            "propagate": False,
        },
    },
}
```

## views.py

```python
import logging

logger = logging.getLogger("aptifit")

...

```

```python
import logging

logger = logging.getLogger("dashboard")

...
```

분리하고자 하는 `views`를 각각 위처럼 `getLogger({log_file_name})` 처럼 작성하고, 필요한 곳에 로그를 찍으면?


![Image](https://github.com/user-attachments/assets/74ab1eee-48a9-4ed5-a2df-e8c9fb09ce15)


이렇게 로그 파일이 분리되어 작성되는 것을 볼 수 있다!!