---
layout: single
title: "📘[Django] RabbitMQ를 메시지 브로커로 사용하는 Celery!"
toc: true
toc_sticky: true
toc_label: "목차"
categories: django
excerpt: ""
tag: [python, celery, rabbitmq]
---

# RabbitMQ를 메시지 브로커로 사용하는 Celery!

Django에서 Celery를 사용하여 비동기 처리를 하는 예제를 작성한 바 있다.

👉 [**📘[Django] 진짜 간단한 Celery 실험**](https://hellojunho.github.io/django/DJANGO-%EC%A7%84%EC%A7%9C-%EA%B0%84%EB%8B%A8%ED%95%9C-Celery-%EC%8B%A4%ED%97%98/)

*기존에는 메시지 브로커로 Redis 를 사용했는데, 이번엔 RabbitMQ 를 사용해보려 한다.*

# RabbitMQ?

`RabbitMQ` 란, `AMQP(Advanced Message Queuing Protocol)` 를 구현한 오픈소스 `Message Broker`이다.

- `AMQP(Advanced Message Queuing Protocol)` : 신뢰성 있는 메시지 전달, 라우팅, 보안 등을 지원하는 표준 프로토콜

`RabbitMQ` 는 producer → consumer 로 메시지를 전달할 때, 이 중간 과정에서 브로커 역할을 하는 녀석이다.

이 과정에서 메시지를 일시적으로 저장하고, `Queue`에 쌓아두며, 다양한 방식으로 전달할 수 있다.

*언제 사용하지?*

- 요청을 많은 사용자에게 전달할 때
- 요청에 대한 처리시간이 길 때
- 많은 작업이 요청되어 처리를 해야할 때

## 구성

### Producer

- 요청을 보내는 주체
- 보내고자 하는 메시지를 exchange에 publish함

### Consumer

- Producer로부터 메시지를 받아 처리하는 주체

### Exchange

- Producer로부터 전달받은 메시지를 어떤 queue로 보낼지 결정하는 공간
- 4가지 타입이 존재함 (메시지 전달 방식)
    - Direct
    - Fanout
    - Topic
    - Headers

### Queue

- 메시지를 저장하는 공간
- 메시지는 큐에 쌓였다가 `소비자(Consumer)`가 이를 꺼내 처리할 때까지 대기 상태를 유지

### Binding

- Exchange와 Queue 사이의 연결을 정의
- 모든 메시지는 `Exchange`가 가장 먼저 수신하고, `Exchange` 타입과 `Binding` 규칙에 따라 적절한 `Queue`로 전달
- 보통 사용자가 특정 `Exchange`가 특정 `Queue`를 `Binding`하도록 정의함 (fanout은 예외)

## [ChatGPT] RabbitMQ의 장점

### 신뢰성

- `RabbitMQ`는 메시지의 안전한 전달을 보장하며, 메시지 손실을 방지하는 다양한 옵션을 제공함

### 유연성

- 다양한 라우팅 옵션과 플러그인 시스템을 통해 유연한 메시지 흐름 제어가 가능함

### 성능

- 높은 처리량을 지원하고, `클러스터링`을 통해 성능을 확장할 수 있음

### 커뮤니티 및 지원

- 오픈 소스인 만큼 다양하고 활발한 커뮤니티가 존재하고, 다양한 문서와 예제 등을 찾을 수 있음

## [ChatGPT]  RabbitMQ의 단점

### 복잡성

- 초기 설정 및 관리가 복잡할 수 있음
- 고급 기능 사용 시, 깊은 이해가 필요할 수 있음 → 러닝 커브

### 대규모 시스템에서의 한계

- 매우 큰 규모의 시스템에서는 `RabbitMQ`의 성능이 제한될 수 있음
- 대안으로 `Reids`, `Kafka` 등을 사용할 수 있음

# Django + Celery + RabbitMQ 세팅

## RabbitMQ 설치

```
sudo apt-get update
sudo apt-get upgrade
sudo apt-get install rabbitmq-server
```

## RabbitMQ 세팅

```
# user 생성
rabbitmqctl add_user [user_name] [password]
# vhost 생성
rabbitmqctl add_vhost [vhost_name]
# user tag 생성
rabbitmqctl set_user_tags [user_name] [user_tag_name]
# user 권한 설정 - ".*" ".*" ".*" 는 configure, write, read
rabbitmqctl set_permissions -p [vhost_name] [user_name] ".*" ".*" ".*"

# 예시
rabbitmqctl add_user kyungtaeklee 123456
rabbitmqctl add_vhost playr_host
rabbitmqctl set_user_tags kyungtaeklee kyungtaeklee_tag
rabbitmqctl set_permissions -p playr_host kyungtaeklee ".*" ".*" ".*"﻿
```

## Celery 설치

```
#Celery 설치
pip install celery

#celery 백엔드 설치
pip install -U django-celery-results
python manage.py migrate django_celery_results
```

## Celery 세팅

```python
INSTALLED_APPS = [ 
    ...
    'django_celery_results',
]

CELERY_BROKER_URL = 'amqp://[user_name]:[password]@localhost/[vhost_name]'
CELERY_RESULT_BACKEND = 'django-db'
CELERY_CACHE_BACKEND = 'django-cache'

CELERY_ACCEPT_CONTENT = ['application/json']
CELERY_TAST_SERIALIZER = 'json'
CELERY_RESULT_SERIALIZER = 'json'
CELERY_TIMEZONE = 'Asia/Seoul'

DJANGO_CELERY_RESULTS_TASK_ID_MAX_LENGTH = 191
```

여기까지 세팅을 완료 했다면, [**📘[Django] 진짜 간단한 Celery 실험](https://hellojunho.github.io/django/DJANGO-%EC%A7%84%EC%A7%9C-%EA%B0%84%EB%8B%A8%ED%95%9C-Celery-%EC%8B%A4%ED%97%98/)** 에서 진행한 것 처럼 Celery task의 세팅을 진행해주면 된다.

- CELERY_RESULT_BACKEND: 태스크 결과를 저장할 위치
- CELERY_CACHE_BACKEND: 태스크 결과나 기타 캐시 데이터를 저장할 위치

## 서버 실행

`Django`에서 `Celery`를 사용해 비동기 작업을 진행하려면 `Django`, `Celery`, `Message Broker`의 서버 3개가 모두 실행되어야 한다.

`Docker-Compose` 를 사용해 한 번에 서버를 모두 실행하는 것을 선호하는 편이다.

이번에는 `Message Broker`로 Redis가 아닌 `RabbitMQ`이므로 아래와 같이 실행시키면 된다.

```
# RabbitMQ
sudo rabbitmq-server

# Celery
celery -A config worker -l info

# Django
python manage.py runserver
```

# 참고 자료

[[Django + Celery + RabbitMQ] 분산 비동기 작업 수행을 위한 Celery](https://velog.io/@ssssujini99/Django-Celery-RabbitMQ-분산-비동기-작업-수행을-위한-Celery)

[📘[Django] 진짜 간단한 Celery 실험](https://hellojunho.github.io/django/DJANGO-진짜-간단한-Celery-실험/)

[[Django] Celery + RabbitMQ 세팅](https://blog.naver.com/lastingchild/222348184974)