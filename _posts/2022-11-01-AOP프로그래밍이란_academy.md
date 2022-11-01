---
layout: single
title: "AOP에 대해서..."
toc: true
toc_sticky: true
toc_label: "목차"
categories: programming
excerpt: "AOP란?"
tag: [programming, aop]
---
# 📘 AOP에 대해서...
---
## AOP란?
- Aspect : 관점 (핵심)
- 프록시 (Proxy) : 중요(핵심)기능을 대신 수행
- AOP : "대신 핵심 기능을 수행한다" -> 프록시 형태로!

---
## AOP 애노테이션
- @Aspect 공통 기능을 정의한 클래스
- @Pointcut : 공통 기능의 적용 범위
- @Around : 공통 기능, 핵심 기능을 대신 수행해주는 메서드
	- 반환값 : Object
	- 매개변수 : ProceedingJoinPoint joinPoint.proceed() //핵심 메서드 대신 호출
- @EnableAspectAutoProxy : 설정 부분을 자동화 시켜주는 애노테이션

--- 

## 프록시 실행 순서 설정
- 프록시는 기본적으로 인터페이스를 기반으로 실행됨 (인터페이스를 상속받음)
- ProxyTargetClass -> true -> 클래스 기반 프록시
### 순서 설정 방법
1. 기본적으로는 Bean 등록된 순서대로 실행
2. @Order(순서) : 프록시가 수행되는 순서 지정

---
## 파일 위치 접근 방법 (execution 명시자)
- 접근제한자 반환값 패키지명.클래스명 메서드 (...)
- ex) public * aopex.*.*

- * : 모든 값
- aopex.* : aopex 패키지의 바로 하위의 모든 클래스
- aopex..* : aopex 패키지를 포함하고, 그 하위의 모든 패키지의 클래스

- () : 매개변수가 없는 메서드
- (..) : 0개 이상의 매개변수 (매개변수가 있어도 되고, 없어도 됨 - 모든 메서드)
- (*) : 1개의 매개변수
- (*,*) : 2개의 매개변수
- (Integer, ..) : 첫 번쨰 매개변수의 타입은 Integer

---

### 예제
> - 애스터리스크    
 aopex.* : aopex 바로 하위 클래스  
> - ..*  
 aopex..* : aopex 패키지를 포함한 모든 하위 패키지의 클래스  
> -  ()  
 매개변수 없음  
> -  (..)  
 	0개 이상의 매개변수  
> - (*)  
	 1개의 매개변수  
> - (*,*)  
	 2개의 매개변수  
> - (Long, ..)  
	첫 번째 배개변수의 타입은 Long  

---
