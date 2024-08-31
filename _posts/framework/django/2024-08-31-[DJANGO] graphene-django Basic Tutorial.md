---
layout: single
title: "📘[Django] graphene-django Basic Tutorial"
toc: true
toc_sticky: true
toc_label: "목차"
categories: django
excerpt: ""
tag: [graphql, graphene-django]
---

# [공식 문서] Basic Tutorial 시작!

[`graphene-django의 공식 문서`](https://docs.graphene-python.org/projects/django/en/latest/tutorial-plain/)를 보며 진행했다.

`graphene-django`는 Django로 쉽게 작업할 수 있도록 설계된 여러 가지 추가 기능이 있다.

이 튜토리얼의 주요 초점은 `Django ORM`에서 Graphene 객체 유형으로 모델을 연결하는 방법을 잘 이해하는 것이다.

## Django 프로젝트 세팅하기

```
pip install django graphene_django

django-admin startproject cookbook .
python manage.py startapp ingredients
```

```
python manage.py migrate
```

```python
cookbook
|
| - cookbook
| | - asgi.py
| | - settings.py
| | - urls.py
| | - wsgi.py
|
| - ingredients
| | - migrations
| | - admin.py
| | - models.py
| | - tests.py
| | - views.py
|
| - db.sqlite3
| - manage.py

```

### ingredients/models.py

```python
from django.db import models

class Category(models.Model):
    name = models.CharField(max_length=100)

    def __str__(self):
        return self.name

class Ingredient(models.Model):
    name = models.CharField(max_length=100)
    notes = models.TextField()
    category = models.ForeignKey(
        Category, related_name="ingredients", on_delete=models.CASCADE
    )

    def __str__(self):
        return self.name
```

### cookbook/settings.py

```
INSTALLED_APPS = [
    ...
    
    "ingredients",
]
```

```python
python manage.py makemigrations
python manage.py migrate
```

## Sample Data 로드하기

[`Sample Data`](https://raw.githubusercontent.com/graphql-python/graphene-django/master/examples/cookbook/cookbook/ingredients/fixtures/ingredients.json) 를 다운로드(복사)해서, `ingredients/ingredients.json` 파일로 붙여넣자.

### ingredients/ingredients.json

```json
[
    {
      "fields": {
        "name": "Dairy"
      },
      "model": "ingredients.category",
      "pk": 1
    },
    {
      "fields": {
        "name": "Meat"
      },
      "model": "ingredients.category",
      "pk": 2
    },
    {
      "fields": {
        "category": 1,
        "name": "Eggs",
        "notes": "Good old eggs"
      },
      "model": "ingredients.ingredient",
      "pk": 1
    },
    {
      "fields": {
        "category": 1,
        "name": "Milk",
        "notes": "Comes from a cow"
      },
      "model": "ingredients.ingredient",
      "pk": 2
    },
    {
      "fields": {
        "category": 2,
        "name": "Beef",
        "notes": "Much like milk, this comes from a cow"
      },
      "model": "ingredients.ingredient",
      "pk": 3
    },
    {
      "fields": {
        "category": 2,
        "name": "Chicken",
        "notes": "Definitely doesn't come from a cow"
      },
      "model": "ingredients.ingredient",
      "pk": 4
    }
  ]
```

이 작업을 완료했으면, 아래의 명령어를 실행해보자!

```python
python manage.py loaddata ingredients/ingredients.json

Installed 6 object(s) from 1 fixture(s)
```

### ingredients/admin.py

그 다음, `python [manage.py](http://manage.py) createsuperuser` 를 통해 관리자를 생성하고 데이터를 만들 수 있다.

또, `superuser`의 작업을 위해 admin에 등록해준다.

```python
from django.contrib import admin
from cookbook.ingredients.models import Category, Ingredient

admin.site.register(Category)
admin.site.register(Ingredient)
```

## Hello GraphQL - Schema & Object Types

Django 프로젝트에 `query`를 수행하려면 다음 작업이 필요하다.

- 객체 유형이 정의된 `schema`
- 쿼리를 입력으로 받고, 결과를 반환하는 `views`

`GraphQL`은 사용자에게 익숙한 계층 구조가 아니라 `그래프` 구조로 객체를 표현한다.

이 표현을 만들기 위해서는 그래프에 표시될 각 객체의 유형에 대해 알아야 한다.

이 그래프에는 모든 접근이 시작되는 `root` 유형이 존재한다.

이것이 아래의 `Query` 클래스이다.

각 Django 모델에 대한 GraphQL 타입을 생성하기 위해, Django 모델의 필드에 해당하는 GraphQL 필드를 자동으로 정의하는 `DjangoObjectType` 클래스를 서브클래스로 만든다.

후에, 해당 타입을 `Query` 클래스에 필드로 나열한다.

### cookbook/schema.py

`cookbook` 에 `schema.py` 파일을 생성한다.

```python
import graphene
from graphene_django import DjangoObjectType

from cookbook.ingredients.models import Category, Ingredient

class CategoryType(DjangoObjectType):
    class Meta:
        model = Category
        fields = ("id", "name", "ingredients")

class IngredientType(DjangoObjectType):
    class Meta:
        model = Ingredient
        fields = ("id", "name", "notes", "category")

class Query(graphene.ObjectType):
    all_ingredients = graphene.List(IngredientType)
    category_by_name = graphene.Field(CategoryType, name=graphene.String(required=True))

    def resolve_all_ingredients(root, info):
        # We can easily optimize query count in the resolve method
        return Ingredient.objects.select_related("category").all()

    def resolve_category_by_name(root, info, name):
        try:
            return Category.objects.get(name=name)
        except Category.DoesNotExist:
            return None

schema = graphene.Schema(query=Query)
```

## 거의 다 했다!

`graphene_dango`를 사용하기 위해 `INSTALLED_APPS`에 추가해주고, `GRAPHENE` 속성을 추가해준다.

### cookbook/settings.py

```python
INSTALLED_APPS = [
    ...
    "graphene_django",
]

... 

GRAPHENE = {
		"SCHEMA": "cookbook.schema.schema"
}
```

### cookbook/urls.py

후에 아래와 같이 `urls.py`에 정의하여 사용할 수 있다.

```python
from django.contrib import admin
from django.urls import path
from django.views.decorators.csrf import csrf_exempt

from graphene_django.views import GraphQLView

urlpatterns = [
    path("admin/", admin.site.urls),
    path("graphql", csrf_exempt(GraphQLView.as_view(graphiql=True))),
]
```

`RESTful API`와 달리 GraphQL에 엑세스하는 url은 단 하나뿐이다.

이 url에 대한 요청은 `Grpahene`의 `GraphQLView` 에서 처리한다.

`GraphQLView`는 GraphQL 엔드포인트 역할을한다.

추가로, GraphiQL을 원하기 때문에 `graphiql=True` 옵션을 지정한다.

만약 `settings.py`에서 `SCHEMA`를 지정하지 않았다면, 여기에서 직접 지정할 수도 있다!

```python
from django.contrib import admin
from django.urls import path
from django.views.decorators.csrf import csrf_exempt

from graphene_django.views import GraphQLView

from cookbook.schema import schema

urlpatterns = [
    path("admin/", admin.site.urls),
    path("graphql", csrf_exempt(GraphQLView.as_view(graphiql=True, schema=schema))),
] 
```

## 이제 진짜 테스트!

테스트를 위해 서버를 실행시켜보자!

```python
python manage.py runserver

...
... http://127.0.0.1:8000/
...
```

[`http://127.0.0.1:8000/graphql/`](http://127.0.0.1:8000/graphql/)로 들어가보자!

그러면 GraphQL query를 실행할 수 있는 공간이 나온다.

여기에서 query를 수행하여 원하는 값들을 뽑아오면 된다!!

```python
query {
  allIngredients {
    id
    name
  }
}
```

```python
{
  "data": {
    "allIngredients": [
      {
        "id": "1",
        "name": "Eggs"
      },
      {
        "id": "2",
        "name": "Milk"
      },
      {
        "id": "3",
        "name": "Beef"
      },
      {
        "id": "4",
        "name": "Chicken"
      }
    ]
  }
}
```

```python
query {
  categoryByName(name: "Dairy") {
    id
    name
    ingredients {
      id
      name
    }
  }
}
```

```python
{
  "data": {
    "categoryByName": {
      "id": "1",
      "name": "Dairy",
      "ingredients": [
        {
          "id": "1",
          "name": "Eggs"
        },
        {
          "id": "2",
          "name": "Milk"
        }
      ]
    }
  }
}
```

```python
query {
  allIngredients {
    id
    name
    category {
      id
      name
    }
  }
}
```

## 결론?

이처럼 GraphQL은 매우 강력하며, Django 모델을 통합하면 서버를 빠르게 시작할 수 있다.

`django-filter` 및 `automatic pagination`과 같은 기능을 수행하려면 [`Relay Tutorial`](https://docs.graphene-python.org/projects/django/en/latest/tutorial-relay/#relay-tutorial)을 계속 진행해봐야 한다!

# 참고 자료

[Graphene-Python](https://docs.graphene-python.org/projects/django/en/latest/tutorial-plain/)