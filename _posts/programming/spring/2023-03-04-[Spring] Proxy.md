---
layout: single
title: "📘[Spring] Proxy(프록시)란?"
toc: true
toc_sticky: true
toc_label: "목차"
categories: spring
excerpt: ""
tag: [java, proxy]
---

# Proxy(프록시)란?
`Proxy(프록시)`란 사전적 의미로는 "대리"라는 의미로, 프로토콜에 있어서 `대리 응답`을 하는 역할이라고 할 수 있다.  
보안상의 문제로 직접 통신을 주고 받을 수 없는 사이에서 프록시를 이용해서 중계를 하는 개념이다.  
이런 서버를 `프록시 서버`라고 한다.  


## 프록시 서버의 특징?
프록시 서버는 클라이언트와 서버의 입장으로 볼 때, 서로 반대의 역할을 하는 것처럼 보인다.  
클라이언트가 프록시를 바라보면 프록시가 "서버"의 역할을 하는 것처럼 보이고, 서버가 프록시를 바라보면 "클라이언트"의 역할을 하는 것처럼 보인다.  
<br>

```
[서버] <-> [프록시 서버] <-> 클라이언트
```  

*그러면 프록시는 보안적인 측면이 아니면 사용하지 않아도 될까?*  
프록시는 단순 보안 문제때문에만 사용하는 것은 아니다.  
프록시는 `프록시 서버`에 요청된 내용들을 `캐시`를 이용해 저장한다.  
데이터를 저장해두면, 나중에 다시 요청할 때 처음부터 처리하는 것이 아니라 저장해둔 데이터를 가져다 쓰면 되므로 `전송 시간 절약`적인 측면에서도 장점이 있어 많이 사용한다.  

### 예시
**예시 1.**  
내가 엄마한테 라면을 사달라고 부탁함. -> 엄마는 "집에 라면 있는데?"라고 답함. -> 그러면 나는 내 예상보다 더 빨리 라면을 먹을 수 있게 됨.
<br>

이유는 라면이 집에 `캐시`되었기 때문이라고 생각할 수 있음!  
이는 프록시의 장점 중 하나인 `접근 제어, 캐싱`으로 볼 수 있다.   
<br>

**예시 2.**  
아빠한테 자동차 주유를 부탁 -> 아빠가 주유도 해줬는데 세차까지 덤으로 해줌 -> 내가 부탁한(기대한) 것 외에 부가 기능을 얻음.
<br>

프록시의 장점 중 하나인 `부가기능 추가`로 볼 수 있다. 
<br>

**예시 3.**  
예를 들어, 엄마한테 라면을 사달라고 했는데 엄마가 동생한테 시켰다고 해보자.  
나는 요청을 엄마한테 해서 이후 과정은 모른다.  
그래도 결국 동생이 라면을 사와 나에게 도착하기만 하면 된다.  
<br>

이 것은 프록시의 장점 중 하나인 `프록시 체인`으로 볼 수 있다.  

## 프록시의 주요 기능
프록시를 통해서 할 수 있는 일은 크게 2가지로 구분할 수 있다.  
1. 접근 제어  
   - 권한에 따른 접근 차단  
   - 캐싱 -> 캐싱도 접근 제어 중 하나임  
   - 지연 로딩  
2. 부가 기능 추가  
   - 원래 서버가 제공하는 기능에 더해서 부가 기능을 수행  
   - 예) 요청 값이나, 응답 값을 중간에 변형함  
   - 예) 실행 시간을 측정해서 추가 로그를 남김  
<br>

프록시 객체가 중간에 있으면 크게 `접근제어`와 `부가기능 추가`로 나뉜다.  

## 프록시 서버
프록시 서버는 2가지로 나눌 수 있다.  
1. Forward Proxy
2. Reverse Proxy

### Forward Proxy
`포워드 프록시`는 클라이언트 대신 프록시 서버가 목적지 서버에 통신을 구성해주는 프록시를 말한다.  
포워드 프록시를 사용하면 아래와 같이 통신이 구성된다.  
```
클라이언트 -> [IP: 200.000.0.0] 프록시 서버 -> [http://example.co.kr] 목적지 서버 -> 프록시 서버 -> 클라이언트
```  
클라이언트는 웹 서버에 직접적으로 접속하는개 아니라 포워드 프록시에 접속하고, 프록시 서버가 웹 서버에 접속해 요청을 전달한다.  
요청을 처리한 후 응답을 다시 **웹 서버 -> 프록시 서버 -> 클라이언트**의 경로를 통해 전달한다.  

#### Forward Proxy의 장점
1. 캐시 저장
   - 프록시 서버에 사용자 요청을 캐시를 이용해 저장하여 전송시간을 절약할 수 있다.  
2. URL 필터링
   - 외부 접근은 모두 프록시 서버를 통해야 하므로, 목적지 서버로 가려면 프록시 서버에서 URL 필터링을 통한 후 갈 수 있다.  


### Reverse Proxy
`리버스 프록시`는 포워드 프록시와는 달리 **웹 서버쪽에 위치**한다.  
<br>

*그러면 어떻게 되는데?*  
클라이언트의 접근을 최초로 받아 요청에 해당하는 웹 서버에 배분해주는 역할을 한다.  
포워드 프록시는 [IP:200.000.0.0]을 가진 서버였다면, 리버스 프록시는 아래와 같다.  
```
클라이언트 -> [http://example.co.kr] 프록시 서버 -> [IP: 200.000.0.0] 웹 서버1
                                           -> [IP: 300.000.0.0] 웹 서버2
```  
클라이언트가 `http://example.co.kr/content1` 에 접근하면 웹 서버1로, `~/content2`에 접근하면 웹 서버2로 배분해준다.  

#### Reverse Proxy의 장점
1. 부담 분산  
설정으로 `정적 콘텐츠`와 `동적 콘텐츠`의 보는 곳을 나누어, 메모리 사용량의 효율화를 할 수 있다.  
로드 밸런스와 같이 사용하면 더욱 부담을 분산할 수 있다.  

2. 캐시 저장  
포워트 프록시와 동일하게 동일한 데이터를 얻을 때에 프록시 서버가 저장했던 내용을 돌려준다.  

3. 시큐리티 대책, 바이러스 대책  
통신시 프록시 서버에 집약되어 프록시 서버 내에 시큐리티 대책, 바이러스 대책을 구현하여 Web 서버로의 부정 액세스, 사용 등을 방지할 수 있다.  

# 참고자료
[https://engineer-mole.tistory.com/288](https://engineer-mole.tistory.com/288)  
[https://milkye.tistory.com/202](https://milkye.tistory.com/202)  