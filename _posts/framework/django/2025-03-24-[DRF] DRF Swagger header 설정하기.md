---
layout: single
title: "📘[Django] DRF Swagger header 설정하기"
toc: true
toc_sticky: true
toc_label: "목차"
categories: django
excerpt: ""
tag: [swagger, rest api]
---


# Swagger?

`Swagger` 란, 개발한 REST API를 편리하게 문서화 해주고, 이를 통해서 관리 및 제 3의 사용자가 편리하게 API를 호출해보고 테스트할 수 있는 도구임.

DRF 에서 Swagger를 사용하려면 `drf-yasg` 를 설치해줘야한다.

`drf_yasg` 는 Django로 정의된 API를 문서화할 수 있는 패키지로, `Django Rest Framework_Yet Another Swagger Generator` 의 약자임.

## 세팅 시작

### drf_yasg 설치

```python
$ pip install drf_yasg
```

### settings.py 추가

```python
INSTALLED_APPS = [
		...
		"rest_framework",
		"drf_yasg",
]
```

### [urls.py](http://urls.py) 추가

[`urls.py`](http://urls.py) 에는 3단계를 거쳐 설정해야한다.

1. get_schema_view/openapi/AllowAny import
    
    ```python
    from django.contrib import admin
    from django.urls import path, include
    from django.conf.urls import url
    from rest_framework.permissions import AllowAny
    from drf_yasg.views import get_schema_view 
    from drf_yasg import openapi
    ```
    
2. schema_url_patterns (swagger에서 API 문서로 보고 싶은 URL들을 정의함)
    
    ```python
    schema_url_patterns = [ 
        path('', include('blog.urls')),
        ]
    
    schema_view_v1 = get_schema_view(
        openapi.Info(
            title="Open API",
            default_version='v1',
            description="시스템 API",
            terms_of_service="https://www.google.com/policies/terms/",
        ),
        public=True,
        permission_classes=(AllowAny,),
        patterns=schema_url_patterns,
    ) 
    ```
    
3. urlpatterns (Swagger를 보기 위한 엔드포인트 정의함)
    
    ```python
    urlpatterns = [
        path('admin/', admin.site.urls),
        path('',include('blog.urls')),
        url(r'^swagger(?P<format>\.json|\.yaml)$', schema_view_v1.without_ui(cache_timeout=0), name='schema-json'),
        url(r'^swagger/$', schema_view_v1.with_ui('swagger', cache_timeout=0), name='schema-swagger-ui'),
        url(r'^redoc/$', schema_view_v1.with_ui('redoc', cache_timeout=0), name='schema-redoc'),
    ]
    ```
    

### API 데코레이터 추가

DRF 에서는 요청한 request 구분을 API 데코레이터를 통해 구분한다. (FBV 기준)

- FBV: Function Based View → API 요청을 처리할 부분을 함수로 구현

FBV를 사용할 때에는 `api_view(["GET"])` 과 같이 선언해줘야함.

## Swagger를 사용한 새로운 테스트 기능?

HTTP 요청을 보낼 때, 헤더 값을 포함시켜야 할 때가 있다.

예를 들면, 로그인 된 회원에 대해서만 어떠한 뷰를 보여줘야 할 때, 헤더에 해당 유저를 구분하는 토큰 값을 넣는다던지..

이럴 때, Swagger UI에서 직접 헤더에 값을 넣는 방법을 적용할까?

기본 Swagger UI에는 이런 역할은 없다.

아래 코드를 추가하면 됨.

아래는 `accessToken`, `refreshToken` 값을 헤더에 포함시켜 요청을 보낼 수 있도록 하는 예시임.

### settings.py

```python
SWAGGER_SETTINGS = {
    'USE_SESSION_AUTH': False,
    'SECURITY_DEFINITIONS': {
        'AccessToken': {
            'type': 'apiKey',
            'in': 'header',
            'name': 'Authorization',
            'description': 'Add accessToken in format: Bearer <accessToken>',
        },
        'RefreshToken': {
            'type': 'apiKey',
            'in': 'header',
            'name': 'Refresh-Token',
            'description': 'Add refreshToken directly',
        },
    },
}
```

이 코드를 추가하면 됨!

# 참고 자료

[[DRF] Concept part - Swagger : drf-yasg](https://jaeseo0519.tistory.com/142)