---
layout: single
title: "📘[Django] graphene-django란?"
toc: true
toc_sticky: true
toc_label: "목차"
categories: django
excerpt: ""
tag: [graphql, graphene-django]
---

# [공식 문서] Graphene?

구글에 `Graphene-django` 에 대해 검색하면 아래의 페이지가 나온다

👉 [Django - Graphene-Python](https://docs.graphene-python.org/en/latest/)

[`Introduction`](https://docs.graphene-python.org/en/latest/quickstart/#introduction)을 통해 `Graphene` 이 무엇인지 대략 알 수 있었다.

 공식 문서는 `Graphene`을 다음과 같이 소개하고 있다.

## **What is GraphQL?**

`Graphene`이란, 코드 우선 접근 방식을 사용하여 Python에서 GraphQL API를 구현하는 도구를 제공하는 `라이브러리`이다.   
Graphene의 코드 우선 접근 방식과 `Apllo Server(JavaScript)` 또는 `Ariadne(Python)`과 같은 스키마 우선 접근 방식을 비교하여 Graphene의 GraphQL API 구축 방식을 확인할 수 있다.  
`GraphQL 스키마 정의 언어(SDL)`를 작성하는 대신, 서버에서 제공하는 데이터를 설명하는 Python 코드를 작성할 수 있다.  
Graphene은 가장 널리 사용되는 웹 프레임워크 및 ORM을 위한 통합 기능을 완벽하게 갖추고 있다.  
Graphene은 GrpahQL 사양을 완벽하게 준수하는 스키마를 생성하고 릴레이 호환 API를 구축하기 위한 도구와 패턴 또한 제공하고 있다.

## Welcome to Graphene-Django

위 페이지에 들어가서 둘러보면 `Django` 뿐만 아니라 `Flask`, `SQLAlchemy`, `FastAPI` 등과 같은 다양한 기술들과 통합된 자원을 제공하고 있었다..!

이 중에서 [`Graphene-Django`](https://docs.graphene-python.org/projects/django/en/latest/) 를 클릭해보자!

`Graphene-Django`는 Graphene 위에 구축되었다.  
Graphene-Django는 Django 프로젝트에 GraphQL 기능을 쉽게 추가할 수 있는 몇 가지 추가 추상화 기능을 제공한다.  

> 공식 문서에서 제공하는 튜토리얼들을 반드시 따라하며 익혀보자!
> 

# [ChatGPT] Graphene-Django에 대해 자세히 설명해줘

> 🗣️ Graphene-Django에 대해 자세히 설명해줘
> 

`Graphene-Django` 는 Django 웹 프레임워크에서 GraphQL API를 구축하기 위한 강력한 도구임.

GraphQL은 REST API의 대안으로, 클라이언트가 필요한 데이터를 정확히 요청하고 받을 수 있도록 하는 `쿼리 언어` 다!

`Graphene-Django` 는 GraphQL을 Django와 통합하여 보다 효율적 & 유연한 APi를 구축할 수 있게 한다.

## 주요 특징

### 1. 간단한 GraphQL 스키마 정의

Django의 모델을 기반으로 GraphQL 스키마를 쉽게 정의할 수 있다.

model의 필드가 자동으로 GraphQL 타입으로 매핑되고, 이를 통해 복잡한 쿼리와 `mutation`을 간단하게 처리할 수 있다!

### 2. Django ORM과의 통합

Django ORM과 자연스럽게 통합되어, DB에서 데이터를 효율적으로 가져오고 처리할 수 있다.

이를 통해 GraphQL 쿼리를 통해 데이터를 조회하거나 변경하는 것이 쉬워진다!

### 3. 간편한 Mutation 처리

DB의 상태를 변경(CRUD 등)하는 작업인 `Mutation` 을 간단하게 처리할 수 있다!

`Mutation` 클래스와 메서드를 정의하여 클라이언트가 요청한 작업을 안전하게 수행함.

### 4. Django 기능과의 통합

Django의 인증 시스템, 필터링, 페이징 기능 등을 GraphQL API에서 쉽게 사용이 가능하다.

이처럼 Django의 기본 기능들과 잘 연동되도록 설계되어 있어, 개발자에게 편의성을 제공함!

### 5. Relay 지원

`Relay` 는 페이징, 필터링, 연결 등을 처리하는 데 유용한 표준이다.

`Graphene-Django` 는 `Relay` 사양을 지원하여, 복잡한 클라이언트-서버 간의 데이터 상호작용을 관리할 수 있다!

# 참고 자료

[Graphene-Python](https://docs.graphene-python.org/projects/django/en/latest/)

[Graphene-Python](https://docs.graphene-python.org/en/latest/)

[Introduction to GraphQL | GraphQL](https://graphql.org/learn/)