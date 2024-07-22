---
layout: single
title: "📘[Docker] Docker로 MSA 구축하는 방법"
toc: true
toc_sticky: true
toc_label: "목차"
categories: docker
excerpt: ""
tag: [docker-compose, msa, python, django, fastapi]
---

# 기능 별 기술(프레임워크)이 다를 경우, Docker를 사용해 MSA를 구축하는 방법?!

우선 `Django` 기능 1개와 `FastAPI` 기능 1개를 조합하여 MSA를 구축해보자!

프로젝트 구조는 다음과 같다.

```
DOCKER_MSA/
├── django_web/
│   ├── Dockerfile
│   ├── manage.py
│   ├── config/
│   ├── requirements.txt
├── chatbot/
│   ├── Dockerfile
│   ├── main.py
│   ├── requirements.txt
├── docker-compose.yml
├── .gitmodules
├── .env
```

나는 `git submodule` 을 사용해 각 레포지토리를 하나의 레포지토리(Docker_MSA)에서 관리하고, 이 레포를 `Docker-Compose` 를 사용해 배포를 진행했다!

> 이 프로젝트는 현재 진행 중(24.07.22)이므로, 여러 프로젝트를 추가적으로 연결할 예정이고, 연결 중 발생한 에러 또한 정리할 예정이다.

## Docker_MSA/.env

```
DB_NAME=
DB_USER=
DB_PASSWORD=
DB_HOST=
DB_PORT=

OPENAI_API_KEY=
```

- `chatbot` 기능은 chatGPT를 사용하기 때문에 `OPENAI_API_KEY` 를 사용했다.

## Docker_MSA/.gitmodules

```python
[submodule "chatbot"]
	path = chatbot
	url = [Repository URL]

[submodule "django_web"]
	path = django_web
	url = [Repository URL]
	branch = test
```

- `django_web` 이라는 레포는 테스트를 위해 `test` 라는 브랜치를 만들었고, 이 브랜치를 연결함
- `Docker_MSA` 레포에서 이렇게 작성하면 `chatbot`과 `django_web` 레포를 한 번에 불러와 관리할 수 있다!
    - `git submodule update --init --recursive` 명령어를 사용하면, `chatbot`과 `django_web` 레포지토리의 데이터를 불러옴

## Docker_MSA/docker-compose.yml

```
version: '3.8'

services:
  db:
    image: postgres:16
    restart: always
    environment:
      POSTGRES_DB: ${DB_NAME}
      POSTGRES_USER: ${DB_USER}
      POSTGRES_PASSWORD: ${DB_PASSWORD}
    ports:
      - "${DB_PORT}:5432"
    volumes:
      - postgres_data:/var/lib/postgresql/data

  django_web:
    build:
      context: ./django_web
    ports:
      - "8000:8000"
    command: gunicorn config.wsgi:application --bind 0.0.0.0:8000
    env_file:
      - .env
    depends_on:
      - db
    networks:
      - app-network

  chatbot:
    build:
      context: ./chatbot
    ports:
      - "8001:8000"
    env_file:
      - .env
    depends_on:
      - db
    networks:
      - app-network

networks:
  app-network:
    driver: bridge

volumes:
  postgres_data:
```

- 여기서 각 `service` 의 `ports` 를 주의해야 한다!
    - 각 ports를 다르게 설정해줘야 포트 충돌이 발생하지 않게 배포할 수 있음

## django_web/Dockerfile

```docker
FROM python:3.9-slim

WORKDIR /app

# 필수 패키지 설치
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY . .

EXPOSE 8000

CMD ["gunicorn", "config.wsgi:application", "--bind", "0.0.0.0:8000"]
```

## chatbot/Dockerfile

```docker
FROM python:3.8

WORKDIR /app

COPY requirements.txt requirements.txt
RUN pip install --no-cache-dir -r requirements.txt

COPY . .

CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]
```

이렇게 작성한 후, `docker-compose up --build` 명령어를 실행하면 성공적으로 빌드를 마칠 수 있다!

## 배포가 완료되면?

배포가 완료되면 어떻게 접근하면 될까?

아까 `docker-compose.yml` 에서 각 서비스 별로 `port` 를 다르게 설정했다.

즉, 각 서비스 별로 접근하고 싶으면 `docker-compose.yml` 에서 지정한 `port` 로 접근하면 된다!

- `django_web`: [http://0.0.0.0:8000](http://0.0.0.0:8000/)
- `chatbot`: [http://0.0.0.0:8001](http://0.0.0.0:8001/)

> 💡 너무나도 기초적인 정보일 수 있고, 잘못된 정보 혹은 부족한 부분이 있으면 찾고 고쳐 코드와 글 모두 수정해 나아갈 예정!
> 

# 참고 자료

[Git - 서브모듈](https://git-scm.com/book/ko/v2/Git-도구-서브모듈)

[9.2: submodule 갱신 (update) 하기](https://nochoco-lee.tistory.com/88)