---
layout: single
title: "📘 [Python] Celery task 지정 시, bind=True/False 옵션?"
toc: true
toc_sticky: true
toc_label: "목차"
categories: python
excerpt: ""
tag: [celery]
---

# bind 옵션?

`app.task(bind=True)` 혹은 `@shared_task(bind=True)` 와 같이 Celery task를 지정하는 것을 많이 보고, 사용해왔다.

이 때, `bind=False` 로 바꾸면 어떻게 될까?

사실 bind=True와 False 는 큰 차이를 보이지는 않는다.

내가 정의한 task가 `self` 를 인자로 받고, `self.request` 와 같이 self를 참조한 데이터를 필요로 하는 경우에 `bind=True` 로 하면 된다.

예를 들어 살펴보자.

## @app.task(bind=True)

```python
@app.task(bind=True)
def debug_task(self):
    print(f'Request: {self.request!r}')
```

`@app.task(bind=True)` 에서 `bind=True` 는 Celery task 메서드가 자동으로 첫 번째 인자로 `self` 를 받도록 하는 옵션이다.

이 경우 self는 task 인스턴스를 가리키고, 이를 통해 task의 속성(`self.request`, `self.retry` 등)에 접근할 수 있다.

## @app.task(bind=False)

```python
@app.task(bind=False)
def debug_task():
    print(f'Request: {self.request!r}')  # Error 발생
```

`@app.task(bind=False` 에서 `bind=False` 는 Celery task 메서드가 `self` 를 인자로 받지 않도록 한다.

즉, task가 Celery task 인스턴스에 바인딩 되지 않는다는 소리다.

그렇기 때문에 `self` 에 접근이 불가능하고, 연쇄적으로 Celery task 인스턴스의 속성(`self.request`, `self.retry` 등)에 접근 불가능하다.

# 결론

즉, `bind=True` 와 `bind=False` 는 큰 차이를 보이지는 않는다.

Celery task 인스턴스 속성에 접근할 필요가 없다면 두 경우 모두 특별한 에러없이 정상적으로 동작한다.

하지만, Celry task 인스턴스 속성에 접근해야하는 경우가 있다면 반드시 `bind=True` 를 설정해주도록 하자.