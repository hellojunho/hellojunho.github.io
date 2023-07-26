---
layout: single
title: "📘[REST API] REST API란?"
toc: true
toc_sticky: true
toc_label: "목차"
categories: web
excerpt: ""
tag: [api]
---

# REST API란?
**REST API**란 단어 그대로 `REST`규칙을 따르는 API를 뜻한다.  
즉, REST API를 알기 위해서는 `REST`가 무엇인지부터 알아야한다.  

## REST?
**REST**란 아래와 같은 조건을 만족하는 규칙을 뜻한다.
<br>

1. HTTP URI(Uniform Resource Identifier)를 통해 자원(Resource)을 명시하고,  
2. HTTP Method(POST, GET, PUT, DELETE 등)를 통해  
3. 해당 자원(URI)에 대한 [CRUD](https://hellojunho.github.io/web/http%EB%9E%80/)를 적용하는 것
<br>

### REST의 구성요소
REST는 다음 3가지의 요소로 구성되어있다.
<br>

1. 자원(Resource): HTTP URI  
2. 자원에 대한 행위(Verb): HTTP Method  
3. 자원에 대한 행위의 내용(Representations): HTTP Message Pay Load
<br>

### REST의 특징
1. Server-Client(서버-클라이언트 구조)  
2. Stateless(무상태)  
3. Cacheable(캐시 처리 가능)  
4. Layered System(계층화)  
5. Uniform Interface(인터페이스 일관성)  

### REST의 장점
HTTP 프로토콜의 인프라를 그대로 사용하므로 REST API 사용을 위한 별도의 인프라를 구출할 필요가 없다.  
HTTP 프로토콜의 표준을 최대한 활용하여 여러 추가적인 장점을 함께 가져갈 수 있게 해 준다.  
HTTP 표준 프로토콜에 따르는 모든 플랫폼에서 사용이 가능하다.  
Hypermedia API의 기본을 충실히 지키면서 범용성을 보장한다.  
REST API 메시지가 의도하는 바를 명확하게 나타내므로 의도하는 바를 쉽게 파악할 수 있다.  
여러 가지 서비스 디자인에서 생길 수 있는 문제를 최소화한다.  
서버와 클라이언트의 역할을 명확하게 분리한다.  

### REST의 단점
표준이 자체가 존재하지 않아 정의가 필요하다.  
HTTP Method 형태가 제한적이다.  
브라우저를 통해 테스트할 일이 많은 서비스라면 쉽게 고칠 수 있는 URL보다 Header 정보의 값을 처리해야 하므로 전문성이 요구된다.  
구형 브라우저에서 호환이 되지 않아 지원해주지 못하는 동작이 많다.(익스폴로어)  

## REST API 규칙
1. URI는 동사보다는 명사를, 대문자보다는 소문자를 사용한다.  
2. 마지막에 슬래시(/)를 포함하지 않는다.  
3. 언더바(_) 대신 하이픈(-)을 사용한다.  
4. 파일확장자는  URI에 포함하지 않는다.  
5. 행위(delete, do .. 등)를 포함하지 않는다.  

*참고로 RESTful API와 REST API는 완전히 같은 것이 아니다! REST라고 모두 RESTful은 아님!*  

# 참고자료
[https://khj93.tistory.com/entry/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC-REST-API%EB%9E%80-REST-RESTful%EC%9D%B4%EB%9E%80](https://khj93.tistory.com/entry/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC-REST-API%EB%9E%80-REST-RESTful%EC%9D%B4%EB%9E%80)  
