---
layout: single
title: "📘[Flask] json관련 모듈"
toc: true
toc_sticky: true
toc_label: "목차"
categories: flask
excerpt: 
tag: [python, json]
---

# Flask JSON관련 모듈
아래 명령어를 코드 맨위에 선언해서 모듈을 가져온다.  
```java
import json
```

## get_json()
플라스크에서 기본적으로 제공하는 메서드임.  
REST API에서는 POST로 데이터를 전달할 때 json 형태 body를 통해서 파라미터를 전달하는 경우가 많음.  
이 때, request.get_json()으로 파이썬 데이터 형식으로 가져올 수 있음.  

[예시]
```json
{
  "user_id": "test01",
  "user_name": "테스트01",
}
```
```python
@user_bp.route('/create', methods=['POST'])
def create():
    print(request.is_json)
    params = request.get_json()
    print(params['user_id'])
    return 'ok'
```
```result
True
{'user_id': 'test01', 'user_name': '테스트01'}
test01
```

## jsonify()
사용자가 json data를 내보내도록 제공하는 flask의 함수임.  
jsonify()는 json response를 보내기 위해 이미 content-type header가 'application/json'로 되어 있는 flask.Response() 객체를 리턴한다.
jsonify는 json.dump를 사용하기 때문에 아스키 이스케이프 인코딩을 적용.  
프론트엔드에서는 다시 이 코드포인트들을 문자열로 변환해야 한다는 문제가 있음.  
<br>

정리
: content-type : application/json


## json.dump()
`MIME` type header를 추가해주어야 하는 encoded string을 리턴하는 메서드임.  
flask가 알아서 판단해 response를 자동으로 보내주도록 사용하기 때문에 직접적으로 사용할 수 있다. 다만 reponse header fields는 디폴트(text/html; charset=utf-8)로 처리된다.  
<br>

정리
: content-type : text/html; charset=utf-8
: jsonify보다 더 다양한 type을 받을 수 있음.  

### 3.1. MIME Type이란?
`MIME Type`은, "Multipurpose Internet Mail Extensions"의 약자로, 간단히 파일 변환을 뜻한다.
일반적으로 MIME Type은 `Type/Subtype`의 형태를 가진다.  
'/'문자로 구분된 두 개의 타입으로 파일의 타입을 명시하고, 타입 이름에는 공백이 허용되지 않음.  
타입은 카테고리, 서브타입은 개별 혹은 멀티파트 타입이 될 수 있다.  
<br>

[예시]  

| 타입          |설명|서브타입|
|-------------|---|---|
| text        |	텍스트로 표현되는 모든 문서를 나타내며 인간이 읽을 수 있는 데이터를 의미한다|text/plain, text/html, text/css, text/javascript ...|
| image       | 	텍스트로 표현되는 모든 문서를 나타내며 인간이 읽을 수 있는 데이터를 의미한다 |   image/jpeg, image/png, image/gif ... |
| application | 모든 종류의 바이너리 데이터를 나타냄 |  	application/xml, application/json, application/xhtml+xml, application/pdf ...  |  

- 일반적으로 특정 서브타입이 없으면 `text/plain`으로 사용함.
- 특정 타입이 없는 바이너리 데이터의 경우 `application/octet-stream`으로 사용함.

## json.loads()
