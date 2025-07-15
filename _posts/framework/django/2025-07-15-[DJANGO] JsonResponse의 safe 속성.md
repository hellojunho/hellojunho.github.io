---
layout: single
title: "📘[Django] Django JsonResponse safe 속성?"
toc: true
toc_sticky: true
toc_label: "목차"
categories: django
excerpt: ""
tag: [rest_api]
---


# JsonResponse?

`JsonResponse`는 Django에서  HTTP 응답을 위해 사용하는 클래스임.

`Response` 클래스를 상속받은 JSON 형태의 HTTP 응답 클래스로, `dict`, `list`, `json 직렬화 가능한 객체` 등의 데이터를 JSON 형태로 response 할 수 있음.

JsonResponse 클래스는 Response Body로 전달되는 데이터를 자동으로 JSON으로 직렬화하여 처리하고, Content-Type 헤러를 `application/json`으로 설정하여 클라이언트에게 JSON 응답임을 명시한다.

즉, `Response` 클래스는 다양한 응답을, `JsonResponse`는 JSON 형태의 응답을 담당하는 놈들인 것이다.

여기서 JsonResponse에 `safe` 라는 옵션 필드가 존재하는데, 이게 뭔지 궁금하다.

## safe=False?

`JsonResponse`는 기본적으로 JSON 객체만 허용하도록 되어있음.

따라서 JSON 배열을 반환할 때는 `safe=False`를 해주는 것이다.

- `즉, json(dict) 이외의 형식도 변환 가능하다는 말임..!`
- 만약 `safe=True`로 하고, json 이외의 값을 보내면 `Type Error`가 발생할 것이다!

### safe 매개변수의 올바른 사용

```python
>>> response = JsonResponse({"key1":"value1", "key2":"value2"}, safe=True)
>>> response = JsonResponse({"key1":[1, 2, 3], "key2":"value1"}, safe=True)
>>> response = JsonResponse([1, 2, 3], safe=False)
```

### safe 매개변수의 잘못된 사용

```python
>>> response = JsonResponse([1, 2, 3], safe=True)
>>> response = JsonResponse({[1, 2, 3]}, safe=True)

TypeError: In order to allow non-dict objects to be serialized set the safe parameter to False.
```

여기서 safe라는 이름 때문에 보안적인 측면에서 취약하지 않을까 하는 의심이 드는데, 보안상의 문제가 발생할 가능성은 낮다고 한다.

JSON 배열을 반환할 때, `safe=False`를 설정하면 클라이언트가 JSON 배열에 대한 처리를 수행할 수 있도록 허용하는것이며, 이는 일반적으로 보안상의 문제를 발생시키는 요인이 아니기 때문이다.

하지만, 클라이언트가 JSON 배열을 안전하게 처리한다는 가정하에 그런 것이고, 클라이언트에서 적절한 처리를 하지 않는 경우에는 문제가 발생할 수 있다..

그러므로 클라이언트에서 JSON 배열을 안전하게 처리하지 못하는 경우라면 다른 방법을 고려해야 한다..!

# 참고 자료

[Response 와 JsonResponse](https://ravenkim97.tistory.com/293)

[[Django] - Serializers, JsonResponse](https://velog.io/@kimjihong/serializers-jsonresponse)