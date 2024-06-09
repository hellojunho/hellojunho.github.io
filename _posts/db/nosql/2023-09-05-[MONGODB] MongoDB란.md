---
layout: single
title: "📘[DB] MongoDB란?"
toc: true
toc_sticky: true
toc_label: "목차"
categories: db
excerpt: ""
tag: [mongodb]
---

# MongoDB란?
**MongoDB**는 비관계형 데이터베이스 관리 시스템(DBMS)으로, 오픈소스로 제공되고 있다.  

테이블과 행 대신 유연한 문서를 활용해 다양한 데이터 형식을 처리하고 저장할 수 있다.  

`NoSQL(Not Only SQL)`을 기반으로 하는 데이터베이스 솔루션으로, 관계형 데이터베이스 관리 시스템(RDBMS)을 필요로 하지 않아 사용자가 다변량 데이터 유형을 쉽게 저장하고 쿼리할 수 있는 탄력적인 데이터 저장 모델을 제공한다.  
이는 데이터베이스 관리를 간소화하고, 높은 확장성을 보장한다는 장점이 있다.  

## MongoDB 장점
MongoDB는 다른 RDBMS와 달리 외래키를 사용하지 않는다.  
그렇게되면 데이터의 중복이 발생할 가능성이 있는데, MongoDB는 데이터의 중복을 허용한다.  

RDBMS에서 외래키로 묶여있는 데이터를 조회하려면 중복을 피해야하므로 2개의 테이블을 조회해야 한다.  
하지만, MongoDB에서는 모든 데이터를 들고있는 컬럼만 조회하면 되므로 select 횟수가 줄어든다.  

**즉, insert는 불편하지만, select에서는 퍼포먼스가 좋다.**  


# 참고자료
[MongoDB란?](https://www.ibm.com/kr-ko/topics/mongodb)  

[스프링부트5.0 채팅서버 만들기 - 2강 몽고DB 장점](https://www.youtube.com/watch?v=zGpTD6FZ2HU)