---
layout: single
title: "📘[Django] Generic View란?"
toc: true
toc_sticky: true
toc_label: "목차"
categories: django
excerpt: ""
tag: [python, django]
---

# Generic View가 뭐지?

`제네릭 뷰` 는 장고에서 자주 쓰는 뷰를 미리 만들어서 제공해주는 내장 기능이다.

`class` 로 구현되어 있기 때문에 → **상속** 받아 사용함

> 장고의 제네릭 뷰는 데이터베이스 컨텐츠의 뷰를 보여줄 때 장점이 많음
> 

## Generic View 종류

### Base View

> 뷰 클래스를 생성하고, 다른 제네릭 뷰의 부모 클래스가 되는 기본 제네릭 뷰
> 
- View: `최상위 부모 제네릭 뷰 클래스`
- TemplateView: 주어진 `템플릿으로 렌더링`
- RedirectView: 주어진 `URL로 리다이렉트`

### Generic Display View

> 객체의 목록 또는 하나의 객체 상세 정보를 보여주는 뷰
> 
- DetailView: 조건에 맞는 `하나의 객체` 출력
- ListView: 조건에 맞는 `객체 목록` 출력

### Generic Edit View

> 폼을 통해 객체를 생성, 수정, 삭제하는 기능을 제공하는 뷰
> 
- FormView: 폼이 주어지면 `해당 폼을 출력`
- CreateView: `객체를 생성하는 폼` 출력
- UpdateView: `기존 객체를 수정하는 폼`을 출력
- DeleteView: `기존 객체를 삭제하는 폼`을 출력

### Generic Date View

- ArchiveIndexView: `지정 날짜 필드를 역순`으로 정렬된 목록 출력
- YearArchiveView: `주어진 연도`에 해당하는 객체 출력
- MonthArchiveView: `주어진 달`에 해당하는 객체 출력
- DayArchiveView: `주어진 날짜`에 해당하는 객체 출력
- TodayArchiveView: `오늘 날짜`에 해당하는 객체 출력
- DateDetailView: 주어진 `연, 월, 일 PK`에 해당하는 객체 출력

## Generic View Overriding

### 속성 변수 오버라이딩

- model
    - 기본 뷰(View, Template, RedirectView)를 제외한 모든 제네릭 뷰에서 사용함
- queryset
    - 기본 뷰를 제외한 모든 제네릭 뷰에서 사용함
    - `queryset`을 사용하면 `model 속성은 무시`됨
- template_name
    - `TemplateView`를 포함한 모든 `제네릭 뷰`에서 사용함
    - 템플릿 파일 명을 문자열로 지정함
- context_object_name
    - 뷰에서 템플릿 파일에 전달하는 컨텍스트 변수명을 지정함
- paginate_by
    - `ListView`와 `날짜 기반 뷰`에서 사용함
    - 페이징 기능이 활성화 된 경우, 페이지당 출력 항목 수를 정수로 지정
- date_field
    - 날짜 기반 뷰에서 사용함
    - 이 필드의 타입은 `DateField` 또는 `DateTimeField` 임
- form_class
    - `FormView`, `CreateView`, `UpdateView` 에서 폼을 만드는데 사용할 클래스를 지정
- success_url
    - `FormView`, `CreateView`, `UpdateView`, `DeleteView` 에서 폼에 대한 처리가 성공한 후, 리다이렉트 할 URL 주소

### 메소드 오버라이딩

- def get_queryset()
    - 기본 뷰를 제외한 모든 제네릭 뷰에서 사용함
    - 기본으로 `queryset` 속성을 반환함
    - `queryset` 속성이 지정되지 않은 경우, 모델 매니저 클래스의 `all()` 메소드를 호출해 `QuerySet` 객체를 생성해 반환함
- def get_context_data(**kwargs)
    - 뷰에서 템플릿 파일에 넘겨주는 컨텍스트 데이터를 추가하거나 변경하는 목적으로 오버라이딩함
- def form_valid(form)

### 모델을 지정하는 3가지 방법

1. model 속성 변수 지정
2. queryset 속성 변수 지정
3. `def get_quesyset()` 메소드 오버라이딩

### 예제 코드

```python
from django.views.generic import ListView, DetailView
from .models import Question

class IndexView(ListView):
    template_name = 'cbvpolls/index.html'
    context_object_name = 'latest_question_list'

    def get_queryset(self):
        return Question.objects.order_by('-pub_date')[:5]

class DetailView(DetailView):
    model = Question
    template_name = 'cbvpolls/detail.html'

class ResultsView(DetailView):
    model = Question
    template_name = 'cbvpolls/results.html'
```

# 참고 자료

[Built-in class-based generic views | Django documentation](https://docs.djangoproject.com/en/5.0/topics/class-based-views/generic-display/)

[장고 마스터하기 - 10장 - generic view - 김땡땡's blog](https://yonghyunlee.gitlab.io/python/django-master-10/)

[[Django] 제네릭 뷰 (Generic View)](https://velog.io/@chldppwls12/django-generic-view)

[02) 클래스형 뷰 (CBV)](https://wikidocs.net/9623)