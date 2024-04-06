---
layout: single
title: "📘[Django] Django & postgreSQL 연동하기"
toc: true
toc_sticky: true
toc_label: "목차"
categories: django
excerpt: ""
tag: [python, django, django]
---

# 먼저 DB를 생성하자!

```
> psql postgres
```

```
postgres=# create database [database_name];
```

## 그 다음 DB에 접속해야겠지

```
postgres=# \connect[database_name];
```

## 그 다음엔 새로운 유저를 만들고, 몇 가지 설정을 해야함

```
=# create user root with password 'password';
```

- root: 유저 이름
- password: 비밀번호 → 따옴표로 감싸주기

```
alter role root set client_encoding to 'utf-8'

alter role root set timezone to 'Asia/Seoul';

grant all privileges on database [database_name] to root;
```

### 여기까지는 PostgreSQL 설정이었고!

---

# 이제 Django 세팅 시작!

## psycopg2 설치

```
> pip install psycopg2
```

## settings.py

```
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': 'project',
        'USER': 'root',
        'PASSWORD': 'password',
        'HOST': 'localhost',
        'PORT': '',
    }
}
```

> 만약! **AssertionError: database connection isn't set to UTC** 이런 에러가 뜨면 django와 psycopg2의 버전 호환 문제임. psycopg2를 다운그레이드 하거나, settings.py에서 `USE_TZ = True` 를 삭제하면 된다.
> 

# DB 테이블 정보를 models.py로 받아오자!

*DB table이 존재한다는 가정하에 진행!*

```
> python manage.py inspectdb > models.py
```

- 테이블 정보가 models.py에 옮겨 담아진다!

> **# Unable to inspect table ‘table_name’** 오류가 발생하면 `GRANT SELECT ON TABLE [table_name] TO [user_name];` 으로 권한을 부여한 후 다시 해보자.
> 
- 모든 테이블에 대한 권한 부여하기 - 주의하여 사용(권장하지 않음)
    
    ```
    GRANT ALL PRIVILEGES ON ALL TABLES IN SCHEMA public TO [사용자명];
    ```
    

# models.py에 데이터가 생겼으니, 마이그레이션 해야겠지?

```
> python manage.py startapp [app_name]
```

```
INSTALLED_APP = [
	...
	'app_name', # 앱 추가
	...
]
```

**여기서, 기존 models.py를 앱 안의 models.py에 덮어씌워주자.**

```
> python manage.py makemigrations
```

```
> python manage.py migrtate
```

> 여기서 **django_migrations table (permission denied for schema public
LINE 1: CREATE TABLE "django_migrations" ("id" bigint NOT NULL PRIMA...**
이런 오류가 발생할 수도 있는데, psql에서 `alter database [database_name] owner to [user_name];` 명령어를 실행한 후에 다시 해주면 된다. → 프로젝트 DB의 `소유자` 로 권한을 주는거라고 함.
> 

# 참고 자료

[CREATE TABLE "django_migrations" ("id" bigint NOT NULL PRIMA](https://stackoverflow.com/questions/74217259/create-table-django-migrations-id-bigint-not-null-prima)