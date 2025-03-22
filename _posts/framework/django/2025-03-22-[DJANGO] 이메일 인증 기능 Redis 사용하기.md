---
layout: single
title: "📘[Django] 이메일 인증 기능 Redis 사용하기"
toc: true
toc_sticky: true
toc_label: "목차"
categories: django
excerpt: ""
tag: [redis]
---

# Redis 설정하기

[`settings.py`](http://settings.py) 에 아래와 같이 `Redis` 설정을 추가한다.

```python
CACHES = {
    "default": {
        "BACKEND": "django_redis.cache.RedisCache",
        "LOCATION": "redis://127.0.0.1:6379",
        "OPTIONS": {
            "CLIENT_CLASS": "django_redis.client.DefaultClient",
        },
        "TIMEOUT": 300,  # 캐시 기본 유효 시간
    }
}
```

`RedisCache` 를 사용하고, 캐시에 저장되는 값은 5분(300초)의 `타임아웃` 을 부여함.

<aside>
💡

***왜 타임아웃을 걸까?***

이메일 인증 기능은 회원의 이메일로 인증 코드를 발송하고, 해당 인증 코드를 회원이 입력하여 해당 이메일이 유효한 이메일인지 인증하는 기능임.

이 때, 같은 이메일로 여러 번 인증 코드를 발송하는 경우도 발생할 수 있을텐데, 그러면 이 때 여러 개의 인증 코드가 발급된다.

사용자는 발급된 인증 코드 중 아무거나 사용해도 인증이 완료되는 상태가 발생함.

이런 상황을 방지하기 위해 `타임아웃` 을 걸어 인증 코드의 만료 시간을 설정하고, 시간이 만료되면 버리는 방식을 선택함.

</aside>

## 실행 방법

```python
$ python manage.py runserver
```

이러면 서버가 실행되고..

먼저 `Redis` 를 다운받고, Django에서 사용하기 위해 `Django-Redis` 라이브러리를 설치함.

```python
$ pip install django-redis
```

또 다른 터미널을 열어서 `Redis` 를 열어준다.

*아래는 `Windows` 기준*

### redis-server 열기

```python
[Redis 설치 경로]$ .\redis-server.exe

[20748] 08 Jan 18:26:12.431 # Warning: no config file specified, using the default config. In order to specify a config file use C:\Users\Aptimizer\Redis-x64-3.0.504\redis-server.exe /path/to/redis.conf
                _._
           _.-``__ ''-._
      _.-``    `.  `_.  ''-._           Redis 3.0.504 (00000000/0) 64 bit
  .-`` .-```.  ```\/    _.,_ ''-._
 (    '      ,       .-`  | `,    )     Running in standalone mode
 |`-._`-...-` __...-.``-._|'` _.-'|     Port: 6379
 |    `-._   `._    /     _.-'    |     PID: 20748
  `-._    `-._  `-./  _.-'    _.-'
 |`-._`-._    `-.__.-'    _.-'_.-'|
 |    `-._`-._        _.-'_.-'    |           http://redis.io
  `-._    `-._`-.__.-'_.-'    _.-'
 |`-._`-._    `-.__.-'    _.-'_.-'|
 |    `-._`-._        _.-'_.-'    |
  `-._    `-._`-.__.-'_.-'    _.-'
      `-._    `-.__.-'    _.-'
          `-._        _.-'
              `-.__.-'

[20748] 08 Jan 18:26:12.434 # Server started, Redis version 3.0.504
[20748] 08 Jan 18:26:12.434 * DB loaded from disk: 0.000 seconds
[20748] 08 Jan 18:26:12.434 * The server is now ready to accept connections on port 6379

```

### redis-cli 열기

```python
[Redis 설치 경로]$ .\redis-cli.exe -h 127.0.0.1 6379

127.0.0.1:6379> 
```

- `redis-cli` 에서 캐시된 데이터를 확인할 수 있음

**이 정도만 열어놓으면, 이메일 인증 코드 발송 기능은 정상적으로 동작함**

## 인증 코드 확인 방법?

<aside>
💡

***인증 코드를 발송했는데, 인증 코드가 `Redis` 에 정상적으로 들어갔는지 보고싶다면?***

우선 나는 `email_verification:{email}` 의 포멧으로 이메일을 받고 있음.

이걸 바탕으로 `Redis` 에서 데이터를 확인하려면?

1. 모든 키 확인: Redis에 저장된 키를 확인

```python
$ KEYS email_verfication:*
```

1. 특정 키의 값 확인: 반환된 키 중 하나의 값을 확인

```python
$ GET email_verification:test@gmail.com
```

이러면 아래처럼 해당 이메일(KEY)에 대한 값(VALUE)가 반환됨

```python
"123456"
```

1. Redis 값 삭제: 값을 삭제는 방법

```python
$ DEL email_verification:test@gmail.com
```

- Django에서 커넥션을 사용해 삭제할 수도 있음
</aside>

## 왜 Redis를 사용할까?

- 이메일 인증 코드는 `짧은 유효 기간` 동안만 필요하다는 부분
    - 이런 임시 데이터를 DB에 저장하는 것은 비효율적
    - Redis와 같은 `인메모리 DB` 에 저장하는 것이 효율적
- 데이터가 만료되면 자동으로 삭제(`EXPIRE` 설정)되므로, 추가적인 데이터 정리가 필요 없다는 장점
- 고속 데이터 처리
    - Redis는 인메모리 DB라서 일반 DB보다 속도가 훨씬 빠름
    - 이메일 인증은 빠른 속도가 중요하기 때문 → 여러 사용자가 동시에 인증 코드를 요청하는 경우..
- DB 부담 감소
    - Redis는 `<KEY:VALUE>` 형식을 사용하기 때문에, 별도의 DB 테이블 설계 필요 없음
    - 인증 코드와 이메일만 저장 → 부담 감소