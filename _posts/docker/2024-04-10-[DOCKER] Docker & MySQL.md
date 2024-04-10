---
layout: single
title: "📘[Docker] Docker & MySQL"
toc: true
toc_sticky: true
toc_label: "목차"
categories: docker
excerpt: ""
tag: [docker, mysql, db]
---

# 먼저 Docker에서 MySQL을 실행시켜보자

## 도커 버전 확인

```bash
$ docker -v
```

```bash
Docker version 20.10.10 build b485636
```

## MySQL 이미지 pull 받기

```bash
$ docker pull mysql
```

```bash
Using default tag: latest
latest: Pulling from library/mysql
8b0617b3cebc: Pull complete
16ae022566ed: Pull complete
eecd18f4775b: Pull complete
556cfab8150e: Pull complete
ce811470e9ce: Pull complete
c2d2c48356cf: Pull complete
ada74d40ba87: Pull complete
3b7b27488bbe: Pull complete
7c89304473a0: Pull complete
0be02980a719: Pull complete
Digest: sha256:0f2e15fb8b47db2518b1428239ed3e3fe6a6693401b2cf19552063562cfc2fc4
Status: Downloaded newer image for mysql:latest
docker.io/library/mysql:latest
```

## Docker image 리스트 확인 → image pull이 됐는지!

```bash
$ docker images
```

```bash
REPOSITORY   TAG       IMAGE ID       CREATED        SIZE
mongo        latest    [image_id]     6 days ago       723MB
mysql        latest    [image_id]     2 weeks ago      638MB <-- mysql!
httpd        latest    [image_id]     2 months ago     194MB
```

## Docker MySQL 컨테이너 생성 및 실행

```bash
$ docker run --name [컨테이너 이름] -e MYSQL_ROOT_PASSWORD=[MYSQL 접근 비밀번호] -d -p 3306:3306 mysql:latest
```

```bash
# 예시
$ docker run --name mysqldb -e MYSQL_ROOT_PASSWORD=1234 -d -p 3306:3306 mysql:latest
```

- `-d`: 백그라운드에서 실행이 가능하게 하는 옵션
- `-e`: 환경 변수를 설정하기 위해 사용 → `MYSQL_ROOT_PASSWORD`의 환경 변수를 설정함
- `-p`: 포트 설정 → `[HOST_PORT]:[CONTAINER_PORT]`

## 실행 중인 Docker 컨테이너 확인

```bash
$ docker ps
```

```bash
CONTAINER ID   IMAGE          COMMAND                  CREATED          STATUS         PORTS                               NAMES
[컨테이너_id]    mysql:latest   "docker-entrypoint.s…"   35 minutes ago   Up 2 seconds   33060/tcp, 0.0.0.0:2206->3306/tcp   mysqldb
```

## 컨테이너 시작하기

```bash
$ docker start [컨테이너_이름]
```

## 컨테이너 종료하기

```bash
$ docker stop [컨테이너_이름]
```

## 컨테이너 재시작

```bash
$ docker restart [컨테이너_이름]
```

## 컨테이너 접속 → MySQL Shell로 입장!

```bash
$ docker exec -it [컨테이너_이름] /bin/bash
```

## 컨테이너 삭제

```bash
$ docker rm [컨테이너_이름]
```

## Docker image 삭제

```bash
$ docker rmi [image_id] or [image_name]
```

# 참고 자료

[[Docker] Docker를 사용하여 Mysql 컨테이너 생성하고 접속하기](https://tear94fall.tistory.com/6)