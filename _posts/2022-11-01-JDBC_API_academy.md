---
layout: single
title: "JDBC & API에 대해서..."
toc: true
toc_sticky: true
toc_label: "목차"
categories: API, JAVA
excerpt: "JDBC와 API란?"
tag: [JAVA, API]
---
# 📘 JDBC API에 대해서... - 프로그래밍 표준 정의

## API(Application Programming Interface)

---

## SQLException (예외처리)

#### 1. Exception 
- 컴파일 시 체크되는 예외
- 오류가 있으면 컴파일 되지 않음
- 무조건 적절한 처리가 필요한 예외
- try ~ catch, throws (예외 전가) 필수


#### 2. RuntimeException
- 실행되는 동안 체크되는 예외
- 오류가 있더라도 컴파일 됨
- try ~ catch, throws가 필수는 아님
- 유연성을 가지고 체크를 해주어야야 함


#### 3. JDBCTemplate
- 스프링이 자원의 연결, 해제를 대신 해줌
- 예외처리의 유연성
	- SQLException -> DataAccessException(RuntimeException)
	- try~catch, throws가 필수는 아님
	
- 커넥션 풀
	- DB연결 객체를 미리 여러개를 만들어 놓는 것
	
- List<T> query
	- 반환값 T
	- 검색(SELECT)
	- query(String sql, RowMapper mapper)
	- map -> 검색 -> 매칭
	
- int update
	- 반환값: 반영된 레코드 수
	- 추가(INSERT), 수정(UPDATE), 삭제(DELETE)
	
---

## 설치 (pom.xml)
1. spring-context
2. spring-jdbc
	- 일종의 api
3. tomcat-jdbc
	- spring-jdbc의 구현체
4. mysql-connector-java

---