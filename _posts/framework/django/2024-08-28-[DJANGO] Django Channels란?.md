---
layout: single
title: "📘[Django] Django Channels란?"
toc: true
toc_sticky: true
toc_label: "목차"
categories: django
excerpt: ""
tag: [python, channels, socket]
---

# Django Channels

`Channels` 는 웹 소켓, 채팅 프로토콜, IoT 프로토콜 등을 처리하기 위해 Django를 HTTP 이상으로 확장하는 라이브러리이다.

*“왜 과제가 채팅 서비스일까?”에 대한 한 가지 답변이 될 수도 있는 개념인 듯 하다. (IoT 프로토콜 처리)*

Channels는 `ASGI` 라는 파이썬 사양을 기반으로 한다.

Django는 여전히 기존 HTTP를 처리하는 반면, Channels는 동기 및 비동기 스타일로 다른 연결을 처리할 수 있는 선택권을 제공한다.

## ASGI?

`ASGI` 는 Application Server Gateway Interface의 약자로, `비동기` 가 가능한 파이썬 웹 서버, 프레임워크 및 응용 프로그램 간의 표준 인터페이스를 제공하기 위한 WSGI의 후속 제품이다.

WSGI가 앱에 대한 표준을 제공했다면, ASGI는 비동기 및 동기 앱 모두에 대한 표준을 제공한다.

ASGI는 곧 WSGI에 대한 호환성을 가지면서, 비동기적인 요청을 처리할 수 있는 인터페이스이다.

ASGI 서버로는 보통 `uvicorn` 을 많이 사용한다고 한다.

## 세팅에 필요한 것들은 뭐지?

### Django

- HTTP 통신을 위한 웹 프레임워크

### Channels

- 웹 소켓 통신을 위한 라이브러리

### channels-redis

- 소켓을 열고 닫을 때 캐시 데이터를 사용하는데, 이 때 접근할 수 있도록 해주는 라이브러리

### Docker (권장)

# Django Channels 구성 요소

## Scope

`scope` 는 현재 요청의 세부 내역이 담긴 dict이다.

모든 `ASGI` 애플리케이션 함수는 `scope`, `receive`, `send` 인자를 갖는다.

## Consumer

Django에서 HTTP 요청을 처리하는 주체가 View인 것처럼, Channels에서는 `Consumer` 가 HTTP, 웹 소켓 요청을 처리하는 주체가 된다.

- `Class` 만 지원한다.
- Consumer에서 `self.scope` 을 사용해 현재 요청의 모든 내역을 조회할 수 있다.
- channel_layer를 통해 메시지를 주고 받는 부분까지 모두 지원하기 때문에 비즈니스 로직에 더욱 집중할 수 있다.

view에서는 url 매핑을 통해 요청을 처리하듯이, Channels에서는 3가지 기준으로 현재 요청을 처리할 Consumer Instance를 결정할 수 있다.

## 기준 1: 프로토콜 타입

`HTTP` 요청와 `웹 소켓` 프로토콜 요청 타입을 구분한다.

`ProtocolTypeRouter` 를 활용한다.

## 기준 2: 요청 URL 문자열로 분기

`URLRouter` 를 활용해 대다수의 Consumer 클래스가 갖는 개별 URL을 URLRouter에 등록한다.

## 기준 3: 채널명에 의한 라우팅

`channels worker` 에서 사용한다.

## Channels에서도 쿠키/세션 인증이 가능하다?

Django 웹 페이지에서의 쿠키/세션을 그대로 Consumer Instance에서 사용이 가능하다.

LoginView를 통해 인증된 유저가 웹 소켓으로 접속하면, 인증된 유저의 User Instance를 `scope[”user”]`로 조회가 가능하다.

이를 통해 인증 여부를 알 수 있고, 웹소켓 내에서의 로그인/로그아웃이 가능하다.

# 기본 세팅 시작

## mysite/asgi.py

```python
import os

from channels.auth import AuthMiddlewareStack
from channels.routing import ProtocolTypeRouter, URLRouter
from channels.security.websocket import AllowedHostsOriginValidator
from django.core.asgi import get_asgi_application

os.environ.setdefault('DJANGO_SETTINGS_MODULE', 'mysite.settings')

django_asgi_app = get_asgi_application()

import chat.routing  

application = ProtocolTypeRouter({
    "http": django_asgi_app,
    "websocket": AllowedHostsOriginValidator(
        AuthMiddlewareStack(
            URLRouter(
                chat.routing.websocket_urlpatterns
            )
        )
    ),
})
```

- 애플리케이션 실행 시, 웹 소켓 용 URL을 매핑
- `AllowedHostsOriginValidator` : 웹 소켓 열결의 Origin 헤더를 사용해 허용된 호스트에서 오는 요청만 처리
- `AuthMiddlewareStack` : 웹 소켓 요청에 대해 Django의 미들웨어를 사용해 사용자 인증

## mysite/settings.py

```python
...

ASGI_APPLICATION = "mysite.asgi.application"

# Channels Layer
CHANNEL_LAYERS = {
    "default": {
        "BACKEND": "channels_redis.core.RedisChannelLayer",
        "CONFIG": {
            "hosts": [('Redis 호스트', 6379)],
        },
    },
}
```

## chat/routing.py

```python
from django.urls import re_path

from . import consumers
from . import custom_consumers

websocket_urlpatterns = [
    re_path(r'ws/chat/(?P<room_name>\w+)/$', consumers.ChatConsumer.as_asgi()),
]
```

## chat/consumers.py

```python
import json
from asgiref.sync import async_to_sync
from channels.generic.websocket import WebsocketConsumer

class ChatConsumer(WebsocketConsumer):
	# 연결됬을 때 실행되는 함수 
     def connect(self):
     	# self.scope['url_route'] = /ws/localhost:8000/ws/chat/1 
        self.room_name = self.scope['url_route']['kwargs']['room_name']
        
        # 임의로 그룹명을 만든 과정 -> 수정가능
        self.room_group_name = 'chat_%s' % self.room_name

        # Join room group -> group 연결된 소켓을 같은 그룹명에 연결 
        # -> 같은 그룹명이면 같은 메세지 받음
        async_to_sync(self.channel_layer.group_add)(
            self.room_group_name,
            self.channel_name
        )

        self.accept()

	# 연결 해제 될때 실행되는 함수
    def disconnect(self, close_code):
        # Leave room group
        async_to_sync(self.channel_layer.group_discard)(
            self.room_group_name,
            self.channel_name
        )

    # 메세지를 보낼 때 실행
    def receive(self, text_data):

        # Send message to room group
        async_to_sync(self.channel_layer.group_send)(
            self.room_group_name,
            {
                'type': 'chat_message',
                'message': text_data
            }
        )

    # 메세지를 받을 때 실행
    def chat_message(self, event):
    	# 이벤트가 발생할 때 실행되므로 내부의 message를 꺼내야함
        # Send message to WebSocket
        self.send(text_data=json.dumps({
            'message': event['message']
        }))
```

# 참고 자료

[Django Channels란](https://hyun-am-coding.tistory.com/entry/Django-Channels란)

[CGI, WSGI, ASGI 정리](https://kangbk0120.github.io/articles/2022-02/cgi-wcgi-asgi)

[Django Channels — Channels 4.0.0 documentation](https://channels.readthedocs.io/en/stable/index.html)