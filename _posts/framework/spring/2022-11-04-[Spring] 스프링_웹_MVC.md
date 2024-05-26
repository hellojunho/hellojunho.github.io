---
layout: single
title: "📘[Spring] Spring MVC"
toc: true
toc_sticky: true
toc_label: "목차"
categories: spring
excerpt: 
tag: []
---

# Spring MVC란?
`Spring MVC`는 MVC패턴을 사용해 웹을 구성(사용자 요청을 처리)할 수 있도록 하는 Spring이 지원하는 프레임워크이다.  

## Spring MVC의 구조
Spring MVC의 주요 구성요소는 Model, View Controller이다.  
하지만, 이들이 유기적으로 동작하려면 다양한 구성요소가 필요하다.  
<br>

### DispatcherServlet(Front Controller)
`DispatcherServlet`이란, 가장 앞단에서 HTTP Request를 처리하는 Controller이다.  
간단하게 생각하면 **사용자 요청을 가장 먼저 받아, 해당 요청을 처리할 Controller를 지정해주는 역할**이다.  

### Handler(Controller)
`Handler`는 DistpatcherServlet에 의해 배정된 Controller이다.  
Controller는 요청을 처리하기 위한 Model을 호출해 데이터베이스와 연관된 처리를 수행하도록 하거나, View를 반환하는 역할을 한다.  

### ModelAndView
`ModelAndView`는 Handler 처리 결과 후 응답한 View와 View에 전달할 값을 저장하는 객체이다.  
다음과 같은 생성자들이 필요하다.  
`ModelAndView(String viewName)` : 응답할 view 설정  
`ModelAndView(String viewName, Map values)` : 응답할 view와 view로 전달할 값들을 저장한 Map 객체  
`ModelAndView(String viewName, String name, Object value)` : 응답할 view 이름, view로 넘길 객체의 name-value

### View Resolver
`View Resolver`란 앞서 Handler에서 요청을 처리한 후, ModelAndView를 반환하는데, 이 후에 ModelAndView를 알맞은 View로 전달하기 위해 
DispatcherServlet에 의해 View Resolver가 호출된다.  
<br>

View Resolver는 ModelAndView 객체를 View 영역으로 전달하기 위해 알맞은 View 정보를 설정하는 역할을 한다.  

### 동작 순서
![image](https://github.com/hellojunho/hellojunho.github.io/assets/104587537/ae581efd-c93d-4dc7-845e-7c0d58245784)

## MVC
`MVC`란 **Model View Controller**의 약자로 `디자인 패턴(design pattern)`중 하나이다.  
Model, View, Controller는 각자 수행하는 역할이 다른데, 이 컨테이너들이 연결되어 하나의 프로젝트를 구성한다.
<br>

![image](https://github.com/hellojunho/hellojunho.github.io/assets/104587537/80a84d9c-fc28-4749-96db-06ea04c3e2ad)

### Model
**모델(Model)** 은 요청에 따른 비즈니스 로직을 처리하는 역할을 수행한다.   
흔히 `DTO, DAO, VO`와 같은 객체를 사용한다.  
<br>

`DAO(Data Access Object)`: 데이터베이스에 접근하는 객체로, 데이터베이스 접근 로직과 비즈니스 로직을 분리하기 위해 사용한다.  
`DTO(Data Transfer Object)`: 계층간 데이터를 전달하는 객체로, getter/setter 외의 로직은 필요하지 않다.  
`VO(Value Object)`: 값 그 자체를 나타내는 객체로, 로직을 포함할 수 있고, 불변성 보장을 위해 생성자를 사용해야한다.
<br>

모델의 특징은 다음과 같다.  
1. 사용자가 편집하길 원하는 모든 데이터를 가지고 있어야 한다.  
2. View나 Controller에 대해 어떤 정보도 알지 않아야 한다.  
3. 변경이 일어나면 변경 통지에 대한 처리방법을 구현해야 한다.  
<br>

DTO (Data Transfer Object) : 데이터 전달  
DAO (Data Access Object) : 데이터에 접근
Service : DAO, DTO, 검증로직, 기타로직을 사용하여 하나의 기능을 구현한 모델  

### View
**뷰(View)** 는 사용자 인터페이스를 말한다.  
`.html`, `.jsp`, `.mustache`와 같은 파일들로 생각할 수 있다.  
<br>

뷰의 특징은 다음과 같다.  
1. 모델이 가지고 있는 정보를 따로 저장해서는 안된다.  
2. 모델이나 컨트롤러와 같이 다른 구성요소들을 몰라야 한다.  
3. 변경이 일어나면 변경 통지에 대한 처리방법을 구현해야 한다.  

### Controller
**컨트롤러(Controller)** 는 사용자 요청을 처리하는 일종의 `창구`의 역할을 한다.  
<br>

컨트롤러의 특징은 다음과 같다.  
1. 모델이나 뷰에 대해 알고있어야 한다.  
2. 모델이나 뷰의 변경을 모니터링 해야 한다.  

## Spring MVC와 Thymeleaf 연동 설정
["https://mvnrepository.com"]("https://mvnrepository.com")에서 아래 키워드를 검색하여 의존성 추가 필요  

- servlet-api  
- servlet-jsp.api  
- spring-webmvc  
- thymeleaf-spring5
- thymeleaf-layout  
- thymeleaf-jsr310 (thymeleaf extras java8time)  
- sjf4j  
- Logback Classic Module  
- web.xml  
    - servlet  
    - filter
    
## Thymeleaf(타임리프)
타임리프란, 스프링부트에서 공식적으로 지원하는 view 템플릿이다.  
JSP와 달리 타임리프 문서는 html 확장자를 갖고있어 서버 업싱도 동작 가능함

# 참고자료
[Spring MVC Framework란 무엇인가?](https://kotlinworld.com/326)  
[[JAVA] DTO와 VO의 차이](https://maenco.tistory.com/entry/Java-DTO%EC%99%80-VO%EC%9D%98-%EC%B0%A8%EC%9D%B4)  
[[Spring] ModelAndView](https://velog.io/@modsiw/Spring-ModelAndView)  
[[Spring MVC] 스프링 MVC 뷰 리졸버(View Resolver)](https://ittrue.tistory.com/237)  
[DAO, DTO, VO 란? 간단한 개념 정리](https://melonicedlatte.com/2021/07/24/231500.html)