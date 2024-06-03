---
layout: single
title: "📘[Docker] Dockerfile 기본구조와 작성 방법"
toc: true
toc_sticky: true
toc_label: "목차"
categories: docker
excerpt: ""
tag: [dockerfile, python]
---

# Dockerfile 기본 구조

`Dockerfile` 은 각 줄이 명령어로 구성되며, 각 명령어는 이미지의 한 레이어를 정의한다.

기본적인 명령어는 `FROM`, `COPY`, `RUN`, `CMD`, `EXPOSE`, `ENTRYPOINT`, `ENV` 등이 있다!

## Dockerfile 명령어

### 1. FROM

```python
FROM ubuntu:20.04
```

- 이미지를 기반으로 하는 베이스 이미지를 지정함.
- Dockerfile은 항상 `FROM`으로 시작한다.

### 2. RUN

```python
RUN apt-get update && apt-get install -y python3
```

- 컨테이너 내부에서 명령어를 실행함.
- 일반적으로 패키지 설치와 같은 작업에 사용됨.

### 3. COPY

```python
COPY . /app
```

- 로컬 파일을 컨테이너의 파일 시스템으로 복사함.

### 4. ADD

```python
ADD <https://example.com/file.tar.gz> /app
```

- `COPY` 와 유사하지만, URL에서 파일을 가져오거나 압축 파일을 자동으로 해제할 수 있음.

### 5. CMD

```python
CMD ["python3", "/app/main.py"]
```

- 컨테이너가 시작될 때 실행될 명령을 지정함.
- Dockerfile에서 하나만 사용 가능함!

### 6. ENTRYPOINT

```python
ENTRYPOINT ["python3", "/app/main.py"]
```

- 컨테이너가 시작될 때 실행될 명령을 지정함.
- `CMD`와 차이점?
    - 설정한 명령어는 컨테이너 실행 시, 추가 인자로 덮어쓸 수 없다는 점이다!

### 7. ENV

```python
ENV APP_HOME /app
```

- 환경 변수를 설정함.

### 8. EXPOSE

```python
EXPOSE 80
```

- 컨테이너에서 노출할 포트를 지정함.
- 문서화가 목적이며, 실제 포트 바인딩은 `docker run -p` 명령어로 수행된다!

### 9. WORKDIR

```python
WORKDIR /app
```

- 작업 디렉토리를 설정한다.
- `RUN`, `CMD`, `ENTRYPOINT` 명령어는 이 디렉토리에서 실행됨.

### 10. USER

```python
USER appuser
```

- 명령어를 실행할 사용자의 계정을 설정함.

### 11. VOLUME

```python
VOLUME /data
```

- 호스트와 컨테이너 간의 디렉토리를 공유함.

### 12. ARG

```python
ARG VERSION=1.0
```

- 빌드 시에만 사용할 수 있는 변수를 설정함.

# Dockerfile 작성 예시

> Python & Flask 애플리케이션을 위한 Dockerfile 예제
> 

```python
# 베이스 이미지로 Python 3.10을 사용
FROM python:3.10-slim

# 작업 디렉토리 설정
WORKDIR /app

# 의존성 파일을 현재 작업 디렉토리(./)에 복사하고 설치
COPY requirements.txt ./
RUN pip install --no-cache-dir -r requirements.txt

# 애플리케이션 파일 복사
COPY . .

# 컨테이너 실행 시 실행할 명령어
CMD ["python3", "app.py"]
```

- `FROM python:3.10-slim`
    - `slim` 버전은 필요 없는 패키지가 포함되지 않아 이미지의 크기가 작음.
- `WORKDIR /app`
    - 컨테이너 내에서 작업 디렉토리를 설정.
    - `/app` 디렉토리는 애플리케이션 파일이 복사될 디렉토리를 의미함.
- `pip install --no-cache-dir -r requirements.txt`
    - `--no-cache-dir` 옵션으로 pip 캐시를 사용하지 않도록 하여 이미지의 크기를 줄임.
- `COPY . .`
    - 현재 디렉토리의 모든 파일을 컨테이너의 현재 작업 디렉토리(”/app”)로 복사함.
    - 애플리케이션 코드와 필요한 파일들이 컨테이너에 복사됨.
- `CMD ["python3", "app.py"]`
    - 컨테이너가 시작될 때 실행할 명령어로, `python3 app.py` 명령어가 실행된다!

## Dockerfile 빌드하기

```python
# Dockerfile을 빌드하여 이미지를 생성
docker build -t myapp:latest .
```

- `t`: 생성할 이미지의 태그를 지정
- `.`: Dockerfile의 위치를 지정