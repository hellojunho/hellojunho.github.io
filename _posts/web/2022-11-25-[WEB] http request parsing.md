---
layout: single
title: "📘[Web] HTTP 요청 파싱에 대해서..."
toc: true
toc_sticky: true
toc_label: "목차"
categories: web
excerpt: ""
tag: [web]
---

# 📘 HTTP 요청 파싱에 대해서...
`파싱`이란 어떤 값에 대해서 내가 원하는 값을 뽑아오는 동작이라고 생각할 수 있다.  

Http에 대한 정보는 "[Http에 대해서...](https://hellojunho.github.io/web/http%EB%9E%80/)"에서 조금 더 자세히 기록했다.  

## 1. Http 요청 형식
1. GET
2. POST
3. PUT
4. DELETE
5. OPTIONS

### 1-1. Http 요청 메시지의 구조
![image](https://user-images.githubusercontent.com/104587537/203951182-6888c39f-384d-4223-9f96-b033ab882063.png)

- request line
Http request의 첫 라인이며 3부분으로 구성된다.
    - Http Method : `GET`, `POST`등의 동작 정의
    - Request target : request가 전송되는 uri
    - Http Version

- request headers
request에 대한 `meta정보`를 담고있으며, `키:값`형식으로 되어있음.  
    - Host : request가 전송되는 target의 host url
    - accept : 해당 request가 받을 수 있는 `response`의 타입
    - user-Agent : 요청을 보내는 클라이언트에 대한 정보
    - Content-Length : request 메시지의 길이

- request message body
request 메시지의 본문 내용을 담고있음.
    - 실제 메시지/내용이 들어있음
    - `xml`이나 `json`데이터가 들어갈 수 있음.
    - `GET`은 body가 대부분 없음

### 1-2. Http 응답 메시지의 구조
![image](https://user-images.githubusercontent.com/104587537/203954473-dd5960bc-0d5c-4c57-bc15-57813649352b.png)

- status line
reponse의 `상태(state)`를 나타냄.

- response headers
`request`의 헤더와 동일함
    - response에서만 사용되는 헤더값이 있다.
        - ex) User-Agent 대신 Server

- response body
실제 응답하는 데이터를 나타냄

---

## 2. Http Request 파싱코드 (Python)
`Python 3.11.0` 을 사용하여 Http 요청에 대한 결과를 파싱해보자.  
```
# https://foss4g.tistory.com/1617 

import requests
from bs4 import BeautifulSoup

# 지정한 url에 따른 html_doc을 통해 해당 페이지의 html 확인
r = requests.get('https://search.naver.com/search.naver?where=news&sm=tab_jum&query=%EC%9B%94%EB%93%9C%EC%BB%B5')

html_doc = r.text
# print(html_doc)

# html_doc을 Beautiful Soup을 통해 열기
soup = BeautifulSoup(html_doc, 'html.parser')
# print(soup.prettify())

# for link in soup.find_all('a'):
#     print(link.get('href'))

news_tit = soup.findAll("a", {"class":"news_tit"})
# print(news_tit)

# 네이버 뉴스 제목 파싱
for i in range(len(news_tit)):
    print(news_tit[i].get('title'))
```

---
## 3. Postman
`Postman`이란 `API`개발에 관해 여러 편의 기능을 제공하는 툴이다.  
API개발을 빠르고 쉽게 할 수 있게 도와주고, 개발된 API를 테스트 & 문서화, 공유 기능 등이 있다.  
다음으로 API 요청에 대한 응답을 확인할 수 있다.

### 3-1. Posman 사용하기
[www.postman.com](https://www.postman.com/)에서 postman을 먼저 다운 받는다.  

[실행화면]  
![image](https://user-images.githubusercontent.com/104587537/204682308-555ee6c7-6c17-4ff7-b50b-3794e0ab1f84.png) 
처음 실행화면에서 `WORKSPACE`를 생성하고 api요청 테스트한 workspace이다.  
경기도 병원 위치에 대한 open api를 [공공데이터포털]()에서 키 값을 받아 사용해보았다. 
위 사진에 검색창 옆을 보면 `GET`이 있는데, `POST`, `PUT`등 다양한 요청방식을 사용할 수 있고, 자신이 원하는 api를 전송하면 아래에 api에 대한 데이터가 출력된다.  