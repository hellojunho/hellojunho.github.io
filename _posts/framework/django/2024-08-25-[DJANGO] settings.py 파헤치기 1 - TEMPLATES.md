---
layout: single
title: "📘[Django] settings.py 파헤치기 1 - TEMPLATES"
toc: true
toc_sticky: true
toc_label: "목차"
categories: django
excerpt: ""
tag: [python]
---

# settings.py 파헤치기 1 - TEMPLATES

# 템플릿 파일을 찾는 방식?

django에서 templates 파일을 어떻게 찾고, 렌더링하는지 동작 방식을 제대로 알고 있나?

일단 난 *자동으로 찾아준다.* 라고 밖에 대답을 할 수 없었다.

이에 대해 [django 공식문서](https://docs.djangoproject.com/en/5.1/topics/templates/) 와 ChatGPT를 활용하여 알아보고자 한다.

# 공식 문서

```python
TEMPLATES = [
    {
        "BACKEND": "django.template.backends.django.DjangoTemplates",
        "DIRS": [],
        "APP_DIRS": True,
        "OPTIONS": {
            # ... some options here ...
        },
    },
]
```

## 구성 요소

1. [BACKEND](https://docs.djangoproject.com/en/5.1/ref/settings/#std-setting-TEMPLATES-BACKEND)
    - Django의 템플릿 백엔드 API를 구현하는 템플릿 엔진 클래스로 가는 파이썬 경로를 말한다.
    - 내장 백엔드로는 다음 2가지가 있다.
        - [**`django.template.backends.django.DjangoTemplates`**](https://docs.djangoproject.com/en/5.1/topics/templates/#django.template.backends.django.DjangoTemplates)
        - [**`django.template.backends.jinja2.Jinja2`**](https://docs.djangoproject.com/en/5.1/topics/templates/#django.template.backends.jinja2.Jinja2)
    - Django에서 제공하지 않는 템플릿 엔진을 사용하려면 `BACKEND` 를 완전히 정규화된 경로로 설정해야 한다.
        - 예: ‘mypackage.whatever.Backend’
2. [DIRS](https://docs.djangoproject.com/en/5.1/ref/settings/#dirs)
    - 기본 값: [] (빈 목록)
    - 엔진이 템플릿 소스 파일을 찾아야 하는 디렉토리(검색 순서)를 뜻한다.
3. [APP_DIRS](https://docs.djangoproject.com/en/5.1/ref/settings/#app-dirs)
    - 기본 값: False
    - 엔진이 설치된 애플리케이션 내부에서 템플릿 소스 파일을 찾아야 하는지 여부를 의미한다.
    - 하지만, `django-admin startproject` 명령어를 통해 만든 django 프로젝트는 기본 값이 `True` 이다.
4. [OPTIONS](https://docs.djangoproject.com/en/5.1/ref/settings/#std-setting-TEMPLATES-OPTIONS)
    - 기본 값: {} (빈 사전)
    - 템플릿 백엔드에 전달할 추가 매개변수이다.
    - 사용 가능한 매개변수는 템플릿 엔진에 따라 다르다.
    - 내장 템플릿 백엔드(엔진)의 옵션은 [**`DjangoTemplates`](https://docs.djangoproject.com/en/5.1/topics/templates/#django.template.backends.django.DjangoTemplates) 와 [`Jinja2`](https://docs.djangoproject.com/en/5.1/topics/templates/#django.template.backends.jinja2.Jinja2) 를 참조할 수 있다.**

# ChatGPT의 답변

```python
# settings.py
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',  # 템플릿 엔진 지정
        'DIRS': [os.path.join(BASE_DIR, 'templates')],  # 템플릿 디렉토리 지정
        'APP_DIRS': True,  # 각 앱의 'templates' 디렉토리에서 템플릿을 자동으로 찾음
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.debug',
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
            ],
        },
    },
]
```

## 구성 요소

1. BACKEND
    - `BACKEND` 는 Django에서 사용할 템플릿 엔진을 지정한다.
    - 기본적으로는 `'django.template.backends.django.DjangoTemplates'` 를 사용하며, 필요에 따라 `Jinja2` 와 같은 다른 템플릿 엔진을 지정할 수 있다.
2. DIRS
    - `DIRS` 는 템플릿을 찾을 디렉토리의 목록을 지정한다.
    - 이 디렉토리들은 Django가 템플릿을 검색할 때 우선적으로 참조한다.
    - `DIRS` 에 `os.path.join(BASE_DIR, 'templates')` 가 설정되어 있으면, Django는 `BASE_DIR/templates/` 경로에서 템플릿을 찾는다는 의미이다.
3. APP_DIRS
    - `APP_DIRS = True` 로 설정되어 있다면, Django는 각 애플리케이션 폴더 내의 `templates` 디렉토리에서 자동으로 템플릿을 찾는다.
    - 이 설정이 활성화되면 `DIRS` 에 명시된 디렉토리보다 우선순위가 낮다.
4. OPTIONS
    - `OPTIONS` 는 템플릿 엔진에 대한 추가 설정을 정의하는 부분이다.
    - `'contetxt_processors'` , `'loaders'` , `'string_if_invaild'` , `'debug'` 등과 같은 옵션이 포함될 수 있다.
    - 특히 `'context_processors'` 는 템플릿 렌더링 시 기본적으로 컨텍스트에 추가되는 변수를 정의한다. 예를 들어 `request` 객체나 `user` 객체를 자동으로 템플릿에 추가하는 역할이다.

## 템플릿 로딩의 동작 방식

1. 템플릿 경로 설정
    - Django는 먼저 `TEMPLATES` 설정을 찾고, 그 안의 `DIRS` 에 정의된 디렉토리 목록에서 템플릿을 찾는다.
    - `DIRS` 에서 템플릿을 찾지 못하면, 각 애플리케이션의 `templates` 디렉토리에서 템플릿을 찾는다. (`APP_DIRS=True` 인 경우)
2. 템플릿 엔진 처리
    - 템플릿을 찾았다면, Django는 `BACKEND` 에 정의된 템플릿 엔진을 사용해 템플릿을 로딩 및 파싱한다.
    - 로딩된 템플릿은 `render` 메서드를 통해 컨텍스트와 함께 렌더링되며, 최종 HTML이 생성된다.
3. 예외 처리
    - 지정된 경로에서 템플릿을 찾지 못하면 `TemplateDoesNotExist` 예외를 발생시킨다.

# 참고 자료
[Templates | Django documentation](https://docs.djangoproject.com/en/5.1/topics/templates/)