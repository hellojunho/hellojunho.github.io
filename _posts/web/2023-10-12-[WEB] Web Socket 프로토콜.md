---
layout: single
title: "📘[WEB] Web Socket 프로토콜"
toc: true
toc_sticky: true
toc_label: "목차"
categories: web
excerpt: ""
tag: [websocket]
---

# 웹 소켓(Web Socket) 프로토콜
**웹 소켓**이란, 웹 환경에서 ` 양방향 통신`이 가능하도록 하는 프로토콜로, `Transport protocol`의 일종이다.  
즉, 웹 버전의 TCP 혹은 socket 이라고 이해할 수 있다.  
<br>

웹 소켓은 사용자의 브라우저로 서버 사이의 동적인 양방향 연결 채널을 구성하는데 WebSocket API를 통해 메시지를 보내고, 요청 없이 응답을 받는 것이 가능하다.  

## 웹 소켓의 특징
### 1. 양방향 통신(Full-Duplex)
`양방향 통신`이란, 데이터 송수신을 동시에 처리할 수 있는 통신 방법이다.  
클라이언트와 서버가 서로에게 원할 때 데이터를 주고 받을 수 있다.  
통상적인 HTTP 통신은 클라이언트가 요청을 보내는 경우에만 서버가 응답을 하는 `단방향 통신` 기법이다.  

### 2. 실시간 네트워킹(Real Time-Networking)
`실시간 네트워킹`은 웹 환경에서 `연속된 데이터를 빠르게 노출하는 네트워킹 방식`이다.  
예로는 채팅, 주식 등이 있다.  


## 왜 쓸까?
웹 소켓은 `Stateful protocol`이기 때문에 클라이언트와 한 번 연결 되면 계속 같은 라인을 사용해 통신하기 때문에 HTTP 사용시 필요없이 발생되는 HTTP와 TCP 연결 트래픽을 피할 수 있다.  
그리고 웹 소켓은 HTTP와 같은 포트(80)를 사용하기 때문에 기업용 어플레킹션에 적용할 때 방화벽 설정을 따로 하지 않아도 된다는 장점이 있다.  

# 참고자료
[https://duckdevelope.tistory.com/19](https://duckdevelope.tistory.com/19)  

[https://velog.io/@codingbotpark/Web-Socket-%EC%9D%B4%EB%9E%80](https://velog.io/@codingbotpark/Web-Socket-%EC%9D%B4%EB%9E%80)  