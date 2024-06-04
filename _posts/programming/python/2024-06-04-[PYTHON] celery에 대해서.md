---
layout: single
title: "📘 [Python] Celery에 대해서"
toc: true
toc_sticky: true
toc_label: "목차"
categories: python
excerpt: ""
tag: [django]
---
# Celery란?

`Celery` 란, 동시성 프로그래밍에서 가장 많이 사용하는 기법 중 하나임.

분산 메시지 전달을 기반으로 동작하는 **비동기 작업 큐** 라고 한다.

- **즉, 실시간 처리에 중점을 두고 작업 예약을 지원하는 작업 큐** 인거임!!
    - 스케줄링도 지원하긴 함

Celery는 메시지를 전달하는 역할(`Publisher`)과 메시지를 `Message Broker` 에서 가져와 작업을 수행하는 `Worker` 의 역할을 담당한다.

## Worker?

`Worker` 는 웹 서비스에서 백엔드의 작업을 처리하는 별도의 `Frame` 임!

이로 인해 사용자에게 즉각적인 반응을 보여줄 필요가 없는 작업들로 인해 사용자가 느끼는 시간 지연을 최소화 할 수 있음.

이처럼 `Celery` 를 사용하면, API 서버를 개발하고 운영하면서 사용자 요청에 포함될 필요가 없는 불필요한 과정이나 무거운 쿼리 실행 등의 작업을 `Celery Task` 로 정의해서 `Broker(RabbitMQ, Redis 등)` 와 `Consumer(Celery Worker)` 를 이용해 비동기 처리하여 사용자에게 빠르게 응답을 제공할 수 있다!!

[사용자 요청에 포함될 필요 없는 작업 예시]

```
- 회원가입 축하 이메일 발송
- 주문 내역 다운로드
```

# Celery 구성 요소

## Broker

`Broker` 는 Task(Message)를 `Worker` 에게 전달하는 역할임

## Client

`Client` 는 Task를 **생성** 하는 역할임

## Worker

`Worker` 는 Task를 **실행**하고 **처리**하는 역할

# Celery 특징?

- `Client`나 `Worker`는 연결 유실이나 실패에 대해 자동으로 재시도 하며, `Broker`, `Worker`를 HA 구성하여 고가용성이 뛰어나다!
    - HA 구성: `High Availavility` → 고가용성 이라는 뜻임
- 단일 `Celery` 프로세스는 최적화 설정을 통해 `ms` 단위 정도의 대기 시간으로 분당 수백만 개의 작업을 처리할 수 있을 정도로 빠름
- `Celery`는 확장성이 매우 뛰어나 거의 모든 부분을 커스텀하여 사용할 수 있다!

# Celery 동작 구조

![image](https://github.com/hellojunho/hellojunho.github.io/assets/104587537/23708ef7-2f3c-4ef3-9ca1-9a59fb801b47)

Celery는 웹 서버에서 발생한 `요청(Task)` 을 `Message Broker` 에서 받아 Celery를 이용해 분산 처리함!

Celery에서는 작업이 완료되는 이벤트에 대해 DB Task를 수행함!

# Celery Broker?

`Broker` 는 `Client` 와 `Worker` 사이에서 메시지를 전달하는 역할임!

`5.0` 버전 기준으로 다음과 같은 `Broker` 를 지원한다고 한다.

- RabbitMQ
    - RabbitMQ의 인증 및 인가 방식을 그대로 사용할 수 있다!
    - 매우 효율적 & 광범위한 배포 및 테스트된 메시지 브로커임
    - 고급 라우팅 요구 사항이 있는 엔터프라이즈 메시징을 제공하는데에 적합
- Redis
    - 시작이 빠르고 가벼운 `Broker`
    - 하지만 안정적인 전달을 지원하지는 않는다
    - 시스템이 종료되는 경우, 몇 분 동안 작업에 대한 정보가 손실되는 것이 중요하지 않은 애플리케이션에 적합
    - `Redis` 자체가 메모리를 사용하여 저장하는 방법이라, 메모리가 부족한 상황에서는 임의로 `Key` 가 삭제될 수 있음 → `Task` 를 받아도 삭제될 수 있음
    - 메시지 손실 방지 기법이 없음ㅜㅜ
- Amazon SQS
- Zookeeper

# Celery 사용 예시

진행 중인 프로젝트에서 `Celery`를 사용하게 되었다.

처음 써보는 기술이기 때문에 셋업조차 낯설고 어려웠다.

다음은 프로젝트에 추가한 `Celery` 셋업 코드이다.

## config/celery.py

```python
import os
from celery import Celery
from celery.schedules import crontab

os.environ.setdefault('DJANGO_SETTINGS_MODULE', 'config.settings')

app = Celery("hellotarot")
app.config_from_object('django.conf:settings', namespace='CELERY')
app.autodiscover_tasks()

@app.task(bind=True)
def debug_task(self):
    print("Request: {0!r}".format(self.request))

app.conf.beat_schedule = {
    "update-status-5-minutes": {
        "task": "check_game_status",
        "schedule": crontab(second="*/1")
    }
}
```

### 코드 해석

```python
import os
from celery import Celery
from celery.schedules import crontab

os.environ.setdefault('DJANGO_SETTINGS_MODULE', 'config.settings')

app = Celery("hellotarot")
app.config_from_object('django.conf:settings', namespace='CELERY')
app.autodiscover_tasks()
```

- `crontab`: 셀러리 스케줄에서 `cron` 작업과 유사한 스케줄을 정의하는 데 사용한다.
    - `cron`: 유닉스 기반 시스템에서 시간 기반의 작업 스케줄러로, 사용자가 지정한 시간에 자동으로 스크립트 실행, 명령어 실행 등의 적업을 반복적으로 수행하도록 예약한다.
- 기본 Django 설정 모듈을 `config.settings`로 설정한다.
    - `config`디렉토리에 `settings.py`가 있고, 해당 파일에 프로젝트 구성이 포함되어 있어야 한다.
- `hellotarot`이라는 이름으로 `Celery 인스턴스`를 생성한다.
- Django 프로젝트 설정에서 Celery 설정을 로드한다. 특히 `CELERY`네임스페이스 내의 설정을 로드한다.
- 이렇게 하면 Django 설정에서 정의한 모든 사용자 지정 Celery 설정이 적용된다.
- 프로젝트 내에서 작업 정의(`@app.task` 로 데코레이션된 함수)를 자동으로 검색하여 작업 등록을 간소화 한다!

```python
@app.task(bind=True)
def debug_task(self):
  print("Request: {0!r}".format(self.request))
```

- `@app.task(bind=True)`: `debug_task`라는 이름의 작업을 정의함. 첫 번째 인수로 `self`를 허용하고, `bind=True` 데코레이터는 작업 메타데이터(`self` 인수)에 대한 엑세스를 허용한다.
- `print("Request: {0!r}".format(self.request))`: 작업 내에서 작업 요청의 세부 정보를 출력하는 용도이며, 디버깅에 용이하게 쓰인다.

```python
app.conf.beat_schedule = {
  "update-status-5-minutes": {
    "task": "check_game_status",
    "schedule": crontab(second="*/1")
  }
}
```

- `app.conf.beat_schedule`: 이 사전은 Celery Beat를 사용하여 주기적인 작업을 위한 스케줄을 정의한다.
    - `update-status-5-minutes`: 예약된 작업의 이름임
        - `task`: 실행할 작업의 이름임
            - `schedule`: `crontab`을 사용해 스케줄을 정의함
                - `crontab(second="*/1")`: 작업을 1초마다 실행 (second, minute, hour 설정 가능)

# 참고 자료

[[Celery] Python - Celery란?](https://velog.io/@sms8377/Celery-Python-Celery란)

[Python Celery란](https://kimeuichan.github.io/posts/python-celery/)