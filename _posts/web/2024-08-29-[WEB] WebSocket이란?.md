---
layout: single
title: "📘[WEB] WebSocket이란?"
toc: true
toc_sticky: true
toc_label: "목차"
categories: web
excerpt: ""
tag: [django]
---

# WebSocket?

웹 소켓은 `Transport Protocol`의 일종으로, 두 프로그램 간의 메시지 교환을 위한 통신 방법 중 하나이다

웹 소켓은 서버와 클라이언트 간에 `소켓 Connection`을 유지하여 언제든 양방향 통신을 가능하게 하는 기술이다.

실시간 웹 애플리케이션 구현에 주로 사용된다.

- SNS
- 메신저
- … 등

## 웹 소켓의 특징

### 1. 양방향 통신 (Full-Duplex)

- 데이터 송수신을 동시에 처리할 수 있는 통신 방법
- 클라이언트와 서버가 서로에게 원할 때 데이터를 주고 받을 수 있음
- HTTP는 요청 시에만 응답을 주는 단방향 통신

### 2. 실시간 네트워킹 (Real Time-Networking)

- 웹 환경에서 연속된 데이터를 빠르게 노출시킬 수 있음

## 웹 소켓을 왜 쓰지?

기존 웹 애플리케이션은 서버와 클라이언트 간의 통신을 대부분 HTTP를 통해 진행했다.

또, HTTP는 Request/Response 기반의 `Stateless` 프로토콜이다.

- 서버-클라이언트 간의 연결이 영구적이지 않음
- 클라이언트에서 Request를 할 때만 서버가 Response를 하는 방식 (단방향)

이런 경우에는 서버의 데이터가 업데이트 되더라도 클라이언트에는 업데이트를 진행하지 않는 한 데이터의 변경이 이루어지지 않는 문제가 발생한다.

- Ajax과 같은 기술을 통해 어느정도 해결이 가능하지만, 빠른 업데이트가 생명인 서비스의 경우에는 이 또한 부적절할 수 있음

이런 문제의 해결을 위해 사용되는 것이 `웹 소켓` 이다.

## 그럼 웹 소켓은 어떻게 연결하지?

일단 웹 소켓은 HTTP와 달리 `Stateful` 프로토콜이다.

때문에, 클라이언트와 한 번 연결이 되면 계속 같은 라인을 사용해 통신하기 때문에 불필요한 HTTP, TCP 연결 트래픽을 피할 수 있다.

또, 웹 소켓은 HTTP와 마찬가지로 80번 포트를 사용하기 때문에 방화벽 재설정이 필요 없다는 장점도 있다고 한다.

## 작동 원리는?

서버-클라이언트 간의 웹 소켓은 HTTP 프로토콜을 통해 이루어진다.

*엥 HTTP가 왜 나와?*

만약 연결이 정상적으로 이루어진다면, 서버-클라이언트 간의 웹 소켓 연결이 이루어지고 일정 시간이 지나면 HTTP 연결은 자동으로 끊어진다.

즉, 최초 접속 시에만 HTTP의 위에서 HandShaking을 하기 때문에 HTTP Header를 사용하고 연결을 끊는 것이다.

# Django에서 웹 소켓을 사용하는 방법?

## socketio

Django, Flask, FastAPI 등과 같은 Python 기반의 프레임워크나 Python을 기반으로 작성하는 프로그램에서 웹 소켓을 사용하는 방법 중 하나가 [`socketio`](https://socket.io/docs/v2/) 이다.

[`socketio`](https://socket.io/docs/v2/) 는 Node.js 를 기반으로 동작하는 소켓 라이브러리이다.

> *이건 다음에 글을 작성하며 개념을 정리하고, 튜토리얼 진행 후 업데이트 해야겠다..!*
> 

## Django Channels

Django에서는 특히 실시간 웹 통신 및 소켓 프로그래밍을 지원하기 위해 `Django Channels` 라는 라이브러리를 지원한다.

👉 [Django Channels란?](https://hellojunho.github.io/django/DJANGO-Django-Channels%EB%9E%80/)

# 참고자료

[WebSocket 이란?](https://duckdevelope.tistory.com/19)

[Web Socket 이란?](https://velog.io/@codingbotpark/Web-Socket-이란)

[Introduction | Socket.IO](https://socket.io/docs/v2/)

[📘[Django] Django Channels란?](https://hellojunho.github.io/django/DJANGO-Django-Channels란/)