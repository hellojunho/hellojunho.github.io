---
layout: single
title: "DB와 SQL에 대해서..."
toc: true
toc_sticky: true
toc_label: "목차"
categories: DB, SQL
excerpt: "데이터베이스란?"
tag: [DB, SQL]
---
# DB와 SQL에 대해서...

## 데이터베이스란?
- 테이블이란?  
	- 데이터를 저장하는 행과 열  
	- - 여러개 존재 가능  
	
---

## 데이터 정의어 (DDL)
- Data Definition Language  


- 데이터를 담을 공간을 정의하는 언어  

- CREATE 문
	- 데이터베이스 생성, 테이블 생성  

- ALTER 문
	- 테이블 변경  

- DROP 문
	- 데이터베이스 삭제, 테이블 삭제

---

## 데이터 조작어 (DML)
- Data Manupulation Language

- 데이터 공간에 데이터를 추가, 수정, 삭제, 조회하는 언어

- SELECT 문
	- SELECT 조회할 필드명(속성) FROM 테이블명
	
- WHERE 검색조건
	- 필드명 = "값"  
	- - [ =, <, >, <=, >= ]
	- - WHERE 검색조건1 AND 검색조건2 ...  
	- - WHERE 검색조건1 OR 검색조건2 ...  
	
- LIKE 문
	- 필드(속성) LIKE "단어"  
	- - 단어와 일치하는 조건	  
	- - LIKE "단어%"  : 단어로 시작하는 패턴
	- - LIKE "%단어"  : 단어로 끝나는 패턴
	- - LIKE "%단어%" : 단어가 포함되는 패턴

- GROUP BY
	- GROUP BY 필드(속성)
	- - 같은 필드(속성)값으로 묶어준다.
	- - 주로 통계 함수와 함께 많이 쓰인다.

- COUNT()
	- 레코드 수
	
- MAX()
	- 최댓값	
	
- MIN()
	- 최솟값	
	
- SUM()
	- 합계	
	
- AVG()
	- 통계
	
- HAVING
	- 집계 함수의 조건

- ORDER BY
	- 정렬(오름/내림차순)
	
- DISTINCT
	- 중복제거  	
	
---

## 내장 함수, 부속질의, 뷰