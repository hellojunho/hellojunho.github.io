---
layout: single
title: "📘[스프링부트와 AWS로 혼자 구현하는 웹 서비스] org.hibernate.dialect.MySQL5InnoDBDialect 에러 해결"
toc: true
toc_sticky: true
toc_label: "목차"
categories: spring
excerpt: ""
tag: [java]
---

# 에러 코드
```
Error while extracting response for type [class java.lang.Long] and content type [application/json;charset=UTF-8]; nested exception is org.springframework.http.converter.HttpMessageNotReadableException: JSON parse error: Cannot deserialize instance of java.lang.Long out of START_OBJECT token; nested exception is com.fasterxml.jackson.databind.exc.MismatchedInputException: Cannot deserialize instance of java.lang.Long out of START_OBJECT token
at [Source: (PushbackInputStream); line: 1, column: 1]
org.springframework.web.client.RestClientException: Error while extracting response for type [class java.lang.Long] and content type [application/json;charset=UTF-8]; nested exception is org.springframework.http.converter.HttpMessageNotReadableException: JSON parse error: Cannot deserialize instance of java.lang.Long out of START_OBJECT token; nested exception is com.fasterxml.jackson.databind.exc.MismatchedInputException: Cannot deserialize instance of java.lang.Long out of START_OBJECT token
at [Source: (PushbackInputStream); line: 1, column: 1]
```

# org.hibernate.dialect.MySQL5InnoDBDialect
`스프링부트와 AWS로 혼자 구현하는 웹 서비스` 책을 따라 학습하다 보니, `application.properties` 파일에 **spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.MySQL5InnoDBDialect** 이라는 설정을 추가해준다.  
<br>

이 설정의 의미는 다음과 같다.  
`spring.jpa.properties.hibernate.dialect`는 MySQL 데이터베이스와 상호작용할 때 사용할 Hibernate 언어를 지정한다는 의미이다.  
Hibernate가 SQL문을 생성하고, 기본 데이터베이스와 상호작용하는 방법을 결정한다.  
<br>

`org.hibernate.dialect.MySQL5InnoDBDialect`는 Hibernate가 MySQL 버전 5과 상호작용한다는 의미이다.  
즉, 호환하는 MySQL의 버전이 5라는 뜻이다.  


# 에러 발생 및 해결
`application.properties`에 **spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.MySQL5InnoDBDialectspring.jpa.properties.hibernate.dialect=org.hibernate.dialect.MySQL5InnoDBDialect** 이 설정을 한 후에, 테스트 코드를 실행해보면 설정을 추가하기 전에는 정상적으로 동작하던 코드가 갑자기 에러가 발생함과 동시에 정상 동작하지 않는다.  

이 경우에 나의 MySQL 버전을 먼저 확인하면 된다.  
나의 MySQL 버전 확인 방법은 아래와 같다.  
<br>

[application.properties]  
```
spring.jpa.show-sql=true
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.MySQL57Dialect
spring.jpa.properties.hibernate.dialect.storage_engine=innodb
spring.datasource.hikari.jdbc-url=jdbc:h2:mem://localhost/~/test;MODE=MYSQL
spring.h2.console.enabled=true
```  
위 설정에서 `jdbc:h2:mem://localhost/~/test`부분은 본인의 h2 데이터베이스 경로를 입력하면 된다.  

# 참고자료
[https://github.com/jojoldu/freelec-springboot2-webservice/issues/612](https://github.com/jojoldu/freelec-springboot2-webservice/issues/612)  
<br>

[https://herojoon-dev.tistory.com/166](https://herojoon-dev.tistory.com/166)  
