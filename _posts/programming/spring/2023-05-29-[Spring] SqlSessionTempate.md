---
layout: single
title: "📘[Spring] sqlSessionTemplate 설정 방식에 대해서..."
toc: true
toc_sticky: true
toc_label: "목차"
categories: spring
excerpt: ""
tag: [java, sqlSessionTemplate]
---

# SqlSessionTemplate이란?
개발을 하면서 `DAO`인터페이스를 구현하고, `Mapper`를 작성할 것이다.  
`MyBatis`를 사용해 DAO를 구현하는 경우에는 `SqlSessionTemplate`라는 것을 사용해 구현하는 경우가 대부분이다.  

## SqlSesseionTemplate의 역할과 설정
DAO의 작업에서 가장 번거로운 작업은 데이터베이스와 연결을 맺고, 작업이 완료된 후에 연결을 `close()`하는 것이다.  
MyBatis를 사용한다면, `mybatis-spring` 라이브러리에서 이 작업을 처리할 수 있는 `SqlSessionTemplate` 클래스를 제공해 번거로움을 줄일 수 있다.  
<br>

SqlSessionTemplate은 MyBatis의 SqlSession 인터페이스를 구현한 클래스로, 기본적인 트랜잭션의 관리나 스레드 처리의 안정성 등을 보장해주고, 데이터베이스의 연결과 종료를 책임진다.  
<br>

*얘는 SqlSessionFactory를 생성자로 주입해 설정한다.*  

**[root-context.xml 설정]**  
```
   <bean id="sqlSession" class="org.mybatis.spring.SqlSessionTemplate" destroy-method="clearCache">
        <constructor-arg name="sqlSessionFactory" ref="sqlSessionFactory"></constructor-arg>
   </bean>
```  