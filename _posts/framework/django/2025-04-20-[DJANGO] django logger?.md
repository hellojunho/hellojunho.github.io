---
layout: single
title: "📘[Django] django logger?"
toc: true
toc_sticky: true
toc_label: "목차"
categories: django
excerpt: ""
tag: [log, logger]
---


# Logger?

프로젝트를 진행하다보면 여러 오류가 발생한다.

이걸 하나하나 `print()` 를 사용해서 서버 오류에 찍는게 과연 맞을까?

이럴 때 사용하는게 `logging`이다!

`logging`은 시스템의 상태, 에러를 기록 및 관리해주는 것을 의미한다.

Django는 로깅 모듈이 존재하기 때문에 이걸 사용하면 된다!


## settings

```python
(... 생략 ...)

# 로깅설정
LOGGING = {
    'version': 1,
    'disable_existing_loggers': False,
    'filters': {
        'require_debug_false': {
            '()': 'django.utils.log.RequireDebugFalse',
        },
        'require_debug_true': {
            '()': 'django.utils.log.RequireDebugTrue',
        },
    },
    'formatters': {
        'django.server': {
            '()': 'django.utils.log.ServerFormatter',
            'format': '[{server_time}] {message}',
            'style': '{',
        },
    },
    'handlers': {
        'console': {
            'level': 'INFO',
            'filters': ['require_debug_true'],
            'class': 'logging.StreamHandler',
        },
        'django.server': {
            'level': 'INFO',
            'class': 'logging.StreamHandler',
            'formatter': 'django.server',
        },
        'mail_admins': {
            'level': 'ERROR',
            'filters': ['require_debug_false'],
            'class': 'django.utils.log.AdminEmailHandler'
        },
    },
    'loggers': {
        'django': {
            'handlers': ['console', 'mail_admins'],
            'level': 'INFO',
        },
        'django.server': {
            'handlers': ['django.server'],
            'level': 'INFO',
            'propagate': False,
        },
    }
}

```

> 출처: https://wikidocs.net/77522
> 

### version

`version`은 고정값 1을 사용한다.

별 의미 없는 설정이 아니라, `logging`모듈이 업그레이드 되어도 현재 설정을 유지시켜주는 안전장치임!

### disable_existing_loggers

`disable_existing_loggers` 항목은 `False`로 설정함.

만약 `True`로 설정하면 기존에 설정된 로거들을 사용하지 않는 설정이 됨.

기존 로거 설정을 버리려면 True로!


### filters

`filters`는 특정 조건에서 로그를 출력하거나 출력하지 않기 위한 옵션임.

`require_debug_false`필터는 `DEBUG=False`인지를 판단하는 필터임. (true는 당연히 반대 필터에서!)

조건 판단을 위한 클래스인 `django.utils.log.RequireDebugFalse` & `django.utils.log.RequireDebugTrue` 를 호출하여 DEBUG 옵션의 값을 확인함!


### formatters

`formatters`는 로그를 출력할 `형식`을 지정한다!

위 예시에서는 아래 형식을 사용했다.

- server_time: 서버 시간
- message: 출력 내용


### handlers

`handler`는 로그의 출력 방법을 정의함.

다음은 `DEFAULT_LOGGING` 설정에 등록된 핸들러임.

- console: 콘솔에 로그를 출력함. 로그 레벨이 `INFO`이상이고, `DEBUG=True`일 때만 로그를 출력함.
- django.server: `python [manage.py](http://manage.py) runserver`로 동작하는 개발 서버에서만 사용하는 핸들러로, 콘솔에 로그를 출력함.
- mail_admins: 로그 내용을 이메일로 전송하는 핸들러로, 로그 레벨이 `ERROR` 이상이고, `DEBUG=False`일 때만 동작한다.
이 핸들러를 사용하려면 환경설정 파일에 `ADMINS` 항목을 추가하고, 관리자 이메일을 등록해야 함. (예: ADMINS = [’aaa@test.org’]) 추가로 메일 전송을 위한 SMTP 설정은 필수임!


### loggers

로그를 출력하는 프로그램에서 사용하는 `로거(logger)`를 의미함!

`DEFAULT_LOGGING` 설정에는 다음과 같은 로거들이 등록되어 있음!

- django: django가 사용하는 로거임. 로그 레벨이 `INFO`이상일 경우에만 로그를 출력함.
- django.server: 개발 서버가 사용하는 로거임. 로그 레벨이 `INFO` 이상일 경우에만 로그를 출력함.
`’propagate: False’`는 django.server가 출력하는 로그를 django 로거로 전달하지 않는다는 의미임.
만약 이게 `True`이면 최상위 패키지명이 django로 동일하기 때문에 django.server 하위 패키지에서 출력하는 로거에도 출력되고, django 로거에도 출력되어 이중으로 로깅됨. (이중 로깅 → 굳이?)


## Logging Level?

위에서 자꾸 `로그 레벨`이라고 하는데, 이게 뭘까?

로그 레벨은 말 그대로 로그를 찍을 때, 해당 로그의 수준을 나타낸다.

총 5단계의 레벨로 구성되어있다.

- [1단계] `DEBUG`: 디버깅 목적으로 사용
- [2단계] `INFO`: 일반 정보 출력 목적으로 사용
- [3단계] `WARNING`: 경고 정보를 출력할 목적으로 사용 (비교적 작은 문제)
- [4단계] `ERROR`: 오류 정보를 출력할 목적으로 사용 (비교적 큰 문제)
- [5단계] `CRITICAL`: 아주 심각한 문제를 출력할 목적으로 사용

> DEBUG < INFO < WARNING < ERROR < CRITICAL
> 


### 로그 레벨은 어떤 상황에 적용하는게 좋을까? (ChatGPT)

> 🗣️ django logging을 사용하는데, 각 로깅 레벨 (debug, info, warning, error, critical, exception … ) 별로 어떤 상황에 적용하는게 좋은지 궁금해.
예를 들면, try-except 구문 안에서 Order라는 model에 접근해 데이터를 조회하는 상황이 있다고 했을 때, DoesNotExist 예외가 발생했을 때 어떤 로그 레벨을 사용해 기록을 남기는지 알려줘.
> 

| 레벨 | 메서드 | 용도 | 출력 예시 |
| --- | --- | --- | --- |
| DEBUG | `logger.debug(...)` | 개발·디버깅 단계에서만 필요한, 아주 상세한 내부 상태 정보 | 함수 진입/종료, 변수값, SQL 쿼리 결과 등 |
| INFO | `logger.info(...)` | 운영 시에도 알아두면 좋은, 정상적인 흐름 내 이벤트 | 어플리케이션 시작/종료, 주요 비즈니스 이벤트(주문 생성 완료 등) |
| WARNING | `logger.warning(...)` | 예상은 하지만 주의가 필요한 상황 | 존재하지 않는 주문 조회 시도, 외부 API 지연 응답 등 |
| ERROR | `logger.error(...)` | 실제 오류(예외) 발생, 처리는 되었지만 문제로 이어질 수 있는 상황 | DB 쓰기 실패, 네트워크 요청 실패 등 |
| EXCEPTION | `logger.exception(...)` | `ERROR`와 동일하나, 스택 트레이스(traceback)까지 함께 찍음 (예외 핸들링 블록 내에서 사용) | 예상치 못한 예외가 발생해 트레이스백이 필요할 때 |
| CRITICAL | `logger.critical(...)` | 서비스 전체를 중단시킬 수 있는 치명적 오류 | 인프라(디스크 풀부족, 메모리 OOM) 등 |

> `DoesNotExist` 예외가 발생하는건 개발자가 사용자 요청 흐름에서 충분히 예상 가능한 문제이므로 `WARGNING`을 사용하면 된다.
이 외의 예외 중, 예상치 못한 에러에 대해서는 `EXCEPTION`을 사용하면 된다. (이 때는 어떤 예외가 발생했는지 `스택 트레이스`가 필요함)
> 


### 레벨 별 출력 가이드라인

- **DEBUG**
    - **대상**: 개발자
    - **내용**: 내부 변수, 함수 진입/종료, SQL 쿼리, 타이밍 측정 등
    - **활용**: 로컬 개발 환경에서만 활성화. 운영 환경에선 보통 꺼둠.
- **INFO**
    - **대상**: 운영자/개발자
    - **내용**: 정상적인 비즈니스 이벤트(주문 접수, 이메일 발송 성공 등)
    - **활용**: 운영 로그에 남겨놓아, 서비스 흐름 모니터링에 활용.
- **WARNING**
    - **대상**: 운영자
    - **내용**: 비정상적이지만 치명적이지 않은 상황(존재하지 않는 리소스 조회 등)
    - **활용**: 너무 잦으면 노이즈가 될 수 있으니, 정말 “주의가 필요한” 경우만.
- **ERROR**
    - **대상**: 운영자
    - **내용**: 예외 발생으로 특정 기능이 실패했으나, 전체 서비스는 지속되는 수준
    - **활용**: Sentry·ELK 같은 에러 모니터링 시스템과 연동해 경고·알림.
- **EXCEPTION**
    - **대상**: 운영자·개발자
    - **내용**: `ERROR` + 스택 트레이스. 예외의 원인 파악이 필요할 때
    - **활용**: `except Exception:` 블록 안에서 예외 디버깅용으로 사용.
- **CRITICAL**
    - **대상**: 운영자
    - **내용**: 전체 서비스 중단 가능성 있는 오류
    - **활용**: 알림·자동 장애복구 트리거에 연결.

# 실제 적용

## settings.py

```python
...

LOG_DIR = os.path.join(BASE_DIR, 'logs')
os.makedirs(LOG_DIR, exist_ok=True)  # logs 디렉토리 없으면 자동 생성

LOGGING = {
    'version': 1,
    'disable_existing_loggers': False,  # Django 기본 로거 끄지 않음
    'formatters': {
        'custom': {
            'format': '[{asctime}] [{filename}:{lineno}] [{levelname}] {message}',
            'style': '{',
            'datefmt': '%Y-%m-%d %H:%M:%S',  # 서버 시간 형식
        },
    },
    'handlers': {
        'console': { # 콘솔에 출력
            'class': 'logging.StreamHandler',
            'formatter': 'custom', # formatters의 custom을 따름
            'level': 'INFO', # INFO 이상만 출력
        },
        'file': { # 파일에 출력
            'class': 'logging.FileHandler',
            'formatter': 'custom',
            'filename': os.path.join(LOG_DIR, 'django.log'),
            'level': 'DEBUG', # DEBUG 이상만 출력
        },
    },
    'loggers': {
        '': {  # root logger
            'handlers': ['console', 'file'],
            'level': 'INFO',
            'propagate': False,
        },
        'django': {  # Django 내부 로거도 포함하려면 이 항목 유지
            'handlers': ['console', 'file'],
            'level': 'INFO',
            'propagate': False,
        },
    }
}

```

- `LOG_DIR`: 로그를 저장할 위치를 지정함
- `os.makedirs`: 없으면 생성하도록
- `format`: 로그 한 줄의 출력 포맷
    - `{asctime}`: 로그 출력 시각 (서버 시간)
    - `{filename}`: 로그를 호출한 파일명
    - `{lineno}`: 호출한 라인 번호
    - `{levelname}`: 로그 레벨 (INFO, ERROR 등)
    - `{message}`: 실제 로그 메시지
- `style`: 문자열 포맷 스타일을 `str.format()` 방식으로 지정 (`{}` 사용 가능)
- `datefmt`: 시간 출력 형식 (`strftime` 형식)
- `handler`
    - `StreamHandler`: 터미널(콘솔)로 출력
    - `formatter`: 위에서 만든 `custom` 포맷 사용
    - `FileHandler`: 지정된 파일(`logs/django.log`)에 로그 출력
- `loggers`
    - `''`
        - 이름이 빈 문자열 `''`인 것은 **모든 앱과 모듈에 공통 적용되는 루트 로거**임
        - `handlers`: 콘솔과 파일 모두에 로그 출력
        - `level`: `INFO` 이상만 출력
        - `propagate=False`: 로그가 중복 전파되지 않도록 설정 (상위 로거로 안 보내게 막음)
    - `django`
        - Django 내부 동작 (예: 요청 처리, static serve 등)의 로그 출력
        - `INFO` 이상의 로그가 `console`과 `file`로 모두 기록됨

로그 출력 예시

```python
[2025-04-14 15:32:00] [views.py:42] [INFO] SUPER_ADMIN 요청, 전체 유저 수: 87
```


## 만약 로그 파일을 분리해서 저장하고 싶다면?

ChatGPT는 신이다.

기존 `settings.py`에서 `file` 설정을 아래처럼 수정해볼 수 있다.

```python
'file': {
    'class': 'logging.handlers.TimedRotatingFileHandler',
    'formatter': 'custom',
    'filename': os.path.join(LOG_DIR, 'django.log'), # logs/django.log 라는 이름으로 저장됨
    'when': 'midnight',            # 매일 자정마다 회전
    'interval': 1,
    'backupCount': 7,              # 7일간 백업 파일 보관
    'encoding': 'utf-8',
    'level': 'INFO',
},
```

- `logging.handlers.TimedRotatingFileHandler`**:** 시간(날짜) 기준으로 파일을 회전시키는 클래스
- `when`: 언제마다 회전 시킬건지?
- `backupCount`: 며칠간의 백업 파일을 보관할건지?

아래처럼 바꾸면 일정 용량이 넘어가면 바꾸도록 할 수도 있음!

```python
'class': 'logging.handlers.RotatingFileHandler',
'maxBytes': 1024 * 1024 * 5,  # 5MB
'backupCount': 5,             # 5개까지 보관
```

- `logging.handlers.RotatingFileHandler`: 로그 파일의 용량이 일정 용량이 넘어가면 회전시키는 클래스
- `maxBytes`: 최대 용량 지정
- `backupCount`: 최대 보관 개수


# 참고 자료

[Logging | Django documentation](https://docs.djangoproject.com/en/4.0/topics/logging/)

[4-14 로깅](https://wikidocs.net/77522)

[Django에서 logging 사용하기](https://velog.io/@tett_77/Django%EC%97%90%EC%84%9C-logging-%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0)