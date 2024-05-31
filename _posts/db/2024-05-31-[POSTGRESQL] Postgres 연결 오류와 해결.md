---
layout: single
title: "📘[POSTGRESQL] Postgres 연결 오류와 해결"
toc: true
toc_sticky: true
toc_label: "목차"
categories: db
excerpt: ""
tag: [postgresql]
---

# Postgres 애플리케이션 오류

`macos` 환경에서 PostgreSQL 서버를 실행하기 위해 `Postgres` 라는 애플리케이션을 사용하고 있다.

어느 순간, Postgres 애플리케이션을 사용해 서버를 사용하려고 하자 빨간색 점과 함께 오류 메시지가 나타났고, `start` 버튼을 누르니 아래와 같은 오류 메시지와 함께 서버가 실행되지 않았다.

```
mac postgres could not start postgresql server.

pg_ctl: could not start server examine the log output.
```

> 캡쳐를 할걸 그랬다.
> 

## 해결 방법

여러 번의 삽질 결과 아래와 같은 방법으로 해결을 할 수 있었다!

```
$ ps aux | grep postgres
```

위 명령어로 postgres 애플리케이션이 사용 중인 프로세스의 정보(?)를 전부 찾는다.

> 원래는 `sudo lsof -i :5432` 명령어로 진행했으나, 이 방법으로 하면 프로세스가 안죽었다.. 이유는 아직 못찾음.. (찾으면 업데이트 예정)
> 

그럼 여러 개의 프로세스가 나올텐데,,

```
$ sudo kill -9 <PID>
```

실행 중인 프로세스를 죽이자..!

이렇게 하면 다시 정상적으로 실행된다!!
![스크린샷 2024-05-31 오전 9 29 33](https://github.com/hellojunho/hellojunho.github.io/assets/104587537/dd7d6ad9-c46f-47f3-a9d0-8661fb0773f6)


# 참고자료

[How can I start PostgreSQL server on Mac OS X?](https://stackoverflow.com/questions/7975556/how-can-i-start-postgresql-server-on-mac-os-x)

[Problem with Postgres: "pg_ctl: could not start server"](https://stackoverflow.com/questions/64554974/problem-with-postgres-pg-ctl-could-not-start-server)

[[Mac OS] 현재 열려있는 포트 확인 및 닫기](https://pangtrue.tistory.com/348)