---
layout: single
title: "📘[Django] models.py 꿀팁?"
toc: true
toc_sticky: true
toc_label: "목차"
categories: django
excerpt: ""
tag: [python]
---

# [Django] models.py  사용하기

**❗️이건 그냥 소소한 꿀팁..?**

# python manage.py inspectdb > models.py

데이터베이스에서 직접 테이블을 만들었다면..?

터미널에 아래 명령어를 입력하자!

```
python manage.py inspectdb > models.py
```

그럼 django에서 데이터베이스에 접근해 테이블 정보를 읽어와 models.py를 생성해준다..!

> managed = False와 같은 옵션이 자동으로 추가되는데, 이런게 붙으면 django에서 DB 수정과 같은 작업을 못하므로, models.py가 만들어졌다고 끝이 아니라 적절하게 수정해줘야함!