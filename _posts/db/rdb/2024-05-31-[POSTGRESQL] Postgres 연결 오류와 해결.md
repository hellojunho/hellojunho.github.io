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

## 오류 로그

```
$ cd ~/Library/Application Support/Postgres/var-16/
```

위 명령어를 입력하면 여러 파일을 볼 수 있는데, 그 중 `postgresql.log` 라는 파일이 있다.

여기에 보면 어떤 오류가 발생했는지 로그가 찍혀 이 로그를 보고 오류를 해결할 수 있다!!

```
$ cat postgresql.log
```

이번 경우의 에러 로그는 다음과 같다.

```
2024-05-31 03:30:49.745 KST [11411] LOG:  starting PostgreSQL 16.3 (Postgres.app) on aarch64-apple-darwin21.6.0, compiled by Apple clang version 14.0.0 (clang-1400.0.29.102), 64-bit
2024-05-31 03:30:49.747 KST [11411] LOG:  listening on IPv4 address "127.0.0.1", port 5432
2024-05-31 03:30:49.747 KST [11411] LOG:  listening on IPv6 address "::1", port 5432
2024-05-31 03:30:49.748 KST [11411] LOG:  could not bind Unix address "/tmp/.s.PGSQL.5432": Address already in use
2024-05-31 03:30:49.748 KST [11411] HINT:  Is another postmaster already running on port 5432?
2024-05-31 03:30:49.748 KST [11411] WARNING:  could not create Unix-domain socket in directory "/tmp"
2024-05-31 03:30:49.748 KST [11411] FATAL:  could not create any Unix-domain sockets
2024-05-31 03:30:49.748 KST [11411] LOG:  database system is shut down
```

5432번 포트가 이미 사용 중이라는 오류 같은데..

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