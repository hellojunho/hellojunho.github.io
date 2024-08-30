---
layout: single
title: "📘[WEB] GraphQL이란?"
toc: true
toc_sticky: true
toc_label: "목차"
categories: web
excerpt: ""
tag: [graphql]
---

# GraphQL?

`GraphQL` 은 API를 위한 쿼리 언어임.

타입 시스템을 사용하여 실행하는 서버사이드 런타임이다.

`GraphQL` 은 특정한 데이터베이스나 특정한 스토리지 엔진과 관계되어 있지 않고, 기존 코드와 데이터에 의해 대체된다.

SQL이 데이터베이스 시스템으로부터 데이터를 가져오는 목적을 가진다면, GraphQL은 `클라이언트가 데이터를 서버로부터 가져오는 것을 목적`으로 한다.

# GraphQL vs REST API

클라이언트가 서버로부터 데이터를 가져오는 것을 목적으로 하는 것은 GraphQL과 REST API 모두 같다.

*그럼 어떤 차이가 있을까?*

# GraphQL

## 보통 하나의 `엔드포인트`를 가진다.

### REST API 엔드포인트 예시

```
- example.com/class
- example.com/class/{반 index}
- example.com/class/{반 index}/students
```

### GraphQL 엔드포인트 예시

```
- example.com/grpahql
```

위의 예시처럼 REST API와 같이 여러 엔드포인트를 사용하는 경우는 직관적이긴 하지만 관리하기 힘들고, 많은 엔드포인트의 노출을 막기 위해 추가적인 처리가 필요하다.

이에 반해 GraphQL은 하나의 엔드포인트에 다른 쿼리를 요청하여 응답을 받기 때문에 REST API처럼 직관적이지는 않지만 관리와 노출 방지 방면에서는 유리하다.

## 원하는 데이터만 받을 수 있다.

REST API는 보통 여러 엔드포인트를 가지며, 각각의 엔드포인트가 동일한 응답을 반환한다.

하지만 GraphQL은 보통 하나의 엔드포인트만을 사용하고, 이를 요청하는 쿼리를 다르게 하여 다른 응답을 받는 방식이다.

또한, REST API는 엔드포인트마다 정해진 응답만을 받을 수 있다.

### REST API 응답 예시

```
// REST API request
GET, https://swapi.dev/api/people/1

// REST API response
{
    "name": "Luke Skywalker",
    "height": "172",
    "mass": "77",
    "hair_color": "blond",
    "skin_color": "fair",
    "eye_color": "blue",
    "birth_year": "19BBY",
    "gender": "male",
    "homeworld": "http://swapi.dev/api/planets/1/",
    "films": ["http://swapi.dev/api/films/1/", "http://swapi.dev/api/films/2/", "http://swapi.dev/api/films/3/", "http://swapi.dev/api/films/6/"],
    "species": [],
    "vehicles": ["http://swapi.dev/api/vehicles/14/", "http://swapi.dev/api/vehicles/30/"],
    "starships": ["http://swapi.dev/api/starships/12/", "http://swapi.dev/api/starships/22/"],
    "created": "2014-12-09T13:50:51.644000Z",
    "edited": "2014-12-20T21:17:56.891000Z",
    "url": "http://swapi.dev/api/people/1/"
}
```

### GraphQL 응답 예시

```
// GraphQL request
query {
    person(personID: 1) {
        name
        height
        mass
    }
}

// GraphQL response
{
    "data": {
        "person": {
            "name": "Luke Skywalker",
            "height": 172,
            "mass": 77
        }
    }
}
```

위 예시는 스타워즈의 인물 정보를 받아오는 예시이다.

REST API는 인물의 정보를 받아오면 필요한 정보 이외에도 많은 정보를 가져오게 된다.

하지만 GrpahQL은 쿼리를 적절하게 사용함으로써, 필요한 정보만 가져오도록 할 수 있다.

# GraphQL의 장점

1. HTTP 요청 횟수를 줄일 수 있다.
2. HTTP 응답 사이즈를 줄일 수 있다.
3. 프론트엔드와 백엔드 개발자의 부담을 덜 수 있다.

# GraphQL의 단점

1. 고정된 요청과 응답만 필요할 때는 쿼리로 인해 요청의 크기가 REST API보다 커질 수 있다.
2. HTTP 캐싱 사용 불가능
3. 파일 업로드 구현 방법이 정해져있지 않아 직접 구현해야 한다.

# 참고 자료

[GraphQL의 단점과 대안](https://www.bangseongbeom.com/graphql-downsides-alternatives.html)