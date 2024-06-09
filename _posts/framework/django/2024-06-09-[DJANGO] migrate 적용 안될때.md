---
layout: single
title: "📘[Django] migrate가 적용 안될 떄 해결 방법"
toc: true
toc_sticky: true
toc_label: "목차"
categories: django
excerpt: ""
tag: [python]
---

# 해결한 방법

- DB의 테이블을 전부 지운다. (postgreSQL 기준)

```
DROP SCHEMA public CASCADE;
CREATE SCHEMA public;
```

- 모든 app의 migrations 폴더에 있는 [initial.py](http://initial.py) 파일들을 지운다. (__pychace__, __init__.py는 지우면 안됨)
- `python [manage.py](http://manage.py) makemigrations` 수행
- `python [manage.py](http://manage.py) migrate` 수행

## 여기서 오류 발생할 경우

- error가 발생한 app만 지정해서 `python [manage.py](http://manage.py) makemigrations [app_name]`
- error가 발생한 지점에서 `python [manage.py](http://manage.py) migrate --fake-initial` 수행