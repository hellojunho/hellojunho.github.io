---
layout: single
title: "📘[Django] unique_together 제약조건?"
toc: true
toc_sticky: true
toc_label: "목차"
categories: django
excerpt: ""
tag: []
---


# unique_together 옵션?

Django Model을 작성하다보니, class Meta에 `unique_together`를 사용할 일이 있었다.

이건 정확히 어떤 의미의 옵션일까?

`Django 모델에서 2개 이상의 필드가 합쳐졌을 때, 중복을 허용하지 않도록`하는 제약 조건이다.

즉, `복합 유니크 키(Composite Unique Key)`를 설정할 때 사용한다.

## 기본 문법

```python
class MyModel(models.Model):
    field1 = models.CharField(max_length=100)
    field2 = models.CharField(max_length=100)

    class Meta:
        unique_together = ('field1', 'field2')
```

여기서는 `field1`, `field2`의 조합이 중복되면 안된다는 의미이다.

예를 들면

```python
MyModel.objects.create(field1="apple", field2="banana")  # 1
MyModel.objects.create(field1="apple", field2="banana")  # 2
MyModel.objects.create(field1="apple", field2="melon")   # 3
```

- `1`: 가능한 조합
- `2`: 불가능한 조합 (`IntegrityError`)
- `3`: 가능한 조합

## 보통 어떨 때 쓸까?

- 한 명의 사용자가 같은 이메일을 두 번 등록하면 안되는 경우
    - `unique_together = ("user", "email")`
- 회사 + 부서 이름의 조합은 고유해야 하는 경우
    - `unique_together = ("company", "department")`
- 날짜 + 사용자 조합으로 출석 기록 중복 방지
    - `unique_together = ("user", "date")`