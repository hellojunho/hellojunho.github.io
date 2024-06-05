---
layout: single
title: "📘[Django] Celery란?"
toc: true
toc_sticky: true
toc_label: "목차"
categories: django
excerpt: ""
tag: [python, celery]
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


# Celery 동작 구조

![image](https://github.com/hellojunho/hellojunho.github.io/assets/104587537/2327557e-8576-438b-8f35-e7692aee130b)

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

## Message Broker

`Message Broker` 란, 송신자의 이전 메시지 프로토콜로부터의 메시지를 수신자의 이전 메시지 프로토콜로 변환하는 중간 모듈임!

`Kafka`, `ActiveMQ`, `RabbitMQ`, `Redis` 등이 여기에 해당됨

# 참고 자료

[[Celery] Python - Celery란?](https://velog.io/@sms8377/Celery-Python-Celery란)

[Python Celery란](https://kimeuichan.github.io/posts/python-celery/)