---
layout: single
title: "📘[H2] Database "~" not fount 오류"
toc: true
toc_sticky: true
toc_label: "목차"
categories: db
excerpt: ""
tag: [h2]
---

# H2 Database 연결 시 발생 오류
H2데이터 베이스 연결 시험을 누르니 아래와 같은 오류 코드가 노출되면서 연결에 실패한다.  
```
Database "/Users/username/aaa" not found, either pre-create it or allow remote database creation (not recommended in secure environments) [90149-224] 90149/90149 (도움말)
org.h2.jdbc.JdbcSQLNonTransientConnectionException: Database "/Users/junho/aaa" not found, either pre-create it or allow remote database creation (not recommended in secure environments) [90149-224]
    at org.h2.message.DbException.getJdbcSQLException(DbException.java:690)
    at org.h2.message.DbException.getJdbcSQLException(DbException.java:489)
    at org.h2.message.DbException.get(DbException.java:223)
    at org.h2.message.DbException.get(DbException.java:199)
    at org.h2.engine.Engine.throwNotFound(Engine.java:189)
    at org.h2.engine.Engine.openSession(Engine.java:72)
    at org.h2.engine.Engine.openSession(Engine.java:222)
    at org.h2.engine.Engine.createSession(Engine.java:201)
    at org.h2.server.TcpServerThread.run(TcpServerThread.java:175)
    at java.base/java.lang.Thread.run(Thread.java:1589)

    at org.h2.message.DbException.getJdbcSQLException(DbException.java:690)
    at org.h2.engine.SessionRemote.readException(SessionRemote.java:650)
    at org.h2.engine.SessionRemote.done(SessionRemote.java:619)
    at org.h2.engine.SessionRemote.initTransfer(SessionRemote.java:163)
    at org.h2.engine.SessionRemote.connectServer(SessionRemote.java:438)
    at org.h2.engine.SessionRemote.connectEmbeddedOrServer(SessionRemote.java:330)
    at org.h2.jdbc.JdbcConnection.<init>(JdbcConnection.java:125)
    at org.h2.util.JdbcUtils.getConnection(JdbcUtils.java:288)
    at org.h2.server.web.WebServer.getConnection(WebServer.java:811)
    at org.h2.server.web.WebApp.test(WebApp.java:978)
    at org.h2.server.web.WebApp.process(WebApp.java:242)
    at org.h2.server.web.WebApp.processRequest(WebApp.java:177)
    at org.h2.server.web.WebThread.process(WebThread.java:154)
    at org.h2.server.web.WebThread.run(WebThread.java:103)
    at java.base/java.lang.Thread.run(Thread.java:1589)
```
<br>

[사진]  
<img width="840" alt="스크린샷 2023-12-18 오전 12 19 52" src="https://github.com/hellojunho/hellojunho.github.io/assets/104587537/7cb56298-1983-4315-b73e-6ef2ca9d9174">
<br>

위와 같은 오류는 `"User/username/"`의 위치에 `aaa.mv.db`파일이 존재하지 않아서 발생하는 오류이다.  
H2 데이터베이스는 기본적으로 자체적인 데이터 파일을 생성하고 관리하는 데이터베이스다.  
이 데이터 파일은 `.h2.db`확장자를 가지며, 일반적으로 데이터베이스와 동일한 디렉터리에 위치하며, 이 파일은 데이터베이스의 실제 데이터를 저장한다.  
<br>

H2 데이터베이스는 연결을 시도할 때, 파일이 존재하지 않으면 새로운 데이터베이스를 생성하고 이에 해당하는 `데이터 파일(.mv.db)`을 만들기 때문에 직접 데이터 파일을 만들지 않아도 된다.  
하지만, 이 때 위와 같은 오류 코드와 함께 연결에 실패하게 된다면 이 데이터 파일을 직접 만들어줘야 한다.  
<br>

나는 오류 코드에 나온 디렉터리에 빈 텍스트 파일을 만들어 확장자를 `.mv.db`로 바꾸어 데이터 파일을 생성해주었다.  
이렇게 하면 연결에 성공할 수 있다.  

# 결론
**연결에 시도하면 H2 데이터베이스가 있는 디렉터리 혹은 데이터 파일이 존재할 디렉터리 위치에 [파일명.mv.db] 파일을 직접 생성한 후 다시 연결을 해보자.**
