---
layout: single
title: "HTTP 요청 파싱에 대해서..."
toc: true
toc_sticky: true
toc_label: "목차"
categories: WEB
excerpt: ""
tag: [WEB]
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
## 2. Postman