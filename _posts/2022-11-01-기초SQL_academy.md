---
layout: single
title: "기초 SQL에 대해서..."
toc: true
toc_sticky: true
toc_label: "목차"
categories: DB, SQL
excerpt: "SQL이란?"
tag: [DB]
---
# 📘 기초 SQL에 대해서...
## SQL 이란?
- SQL (Structured Query Language)
> '관계형 데이터베이스 관리 시스템 (RDBMS)' 에서 자료를 처리하는 용도로 사용되는 구조적 데이터 질의 언어  

### 명령어의 종류
- 데이터 정의 언어 (DDL : Data Definition Language)
  > CREATE, DROP, ALTER ...  
- 데이터 조작 언어 (DML : Data Manipulation Language)
  > INSERT, UPDATE, DELETE, SELECT ...
- 데이터 제어 언어 (DCL : Data Control Language)
  > BEGIN, COMMIT, ROLLBACK ...  
---

<br>

## SQL 연산자
  ---
  |연산자|설명|
  |------|---|
  |=|같음|
  |<> 또는 !=|같지 않음|
  |>|초과|
  |<|미만|
  |>=|이상|
  |<=|이하|
  |BETWEEN|일정 범위 사이|
  |LIKE|패턴 검색|
  |IN|컬럼의 여러 가능한 값들 지정|
---

<br> 

## SELECT - 데이터 조회
> select [ all | distinct ] 속성명 from 테이블
> >   all : 전체 출력, 기본 값  
> >   distinct : 중복 제거  

<br>
---

## WHERE - 조건문
> where 조건식
- WHERE문의 조건식
> 속성명 연산자 값  
> > ex) price >= 10000 and price <= 20000  
      *10000 <= price <= 20000 인 데이터 출력*  
> > ex) publisher = '굿스포츠' or publisher = '대한미디어'  
      *publisher가 '굿스포츠' 혹은 '대한미디어'인 데이터 출력*

<br>
---

## GROUP BY - 그룹화
> group by 속성명
> >   ex) select 
          custid, 
          count(*) as '주문 개수', 
        from orders group by custid;  
- custid를 '주문 개수'에 대해서 그룹화
- 'as'는 별칭(alias)으로, 생략이 가능 -> count(*) '주문 개수'와 동일  
- 통계에 많이 쓰임

<br>
---

## HAVING
> having 조건식  
- having은 보통 group by와 같이 쓰는 경우가 많음
- 즉, 통계함수(집계함수)를 통한 조건식이 들어감 

<br>
---

## ORDER BY - 정렬
> order by 속성명 asc|desc
> > asc  : 오름차순 (작은 값 -> 큰 값) -> 기본 값   
> > desc : 내림차순 (큰 값 -> 작은 값)

<br>
---

## LIMIT
> limit 시작위치, 추출할 개수
> > ex) select * from book limit 0, 3;  
> > *index번호 1번 ~ 3번까지의 데이터 출력*

<br>
---