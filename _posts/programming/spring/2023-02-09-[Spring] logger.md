---
layout: single
title: "📘[Spring] Logger에 대해서..."
toc: true
toc_sticky: true
toc_label: "목차"
categories: spring
excerpt: ""
tag: [java, logger]

---

# 📘Logger에 대해서...
모든 행위와 이벤트 정보를 시간의 경과에 따라 기록한 데이터 시스템 상에서 `로그` 를 생성하는 과정을 `로깅(Logging)` 이라고 한다.
<br>

로깅(logging)의 장점
: 개발 프로그램의 디버깅 예기치 못한 문제의 원인 파악 시스템 및 사용자의 동작 패턴 분석 해킹(침입)의 비정상 동작의 기록을 감지 분석을 통한 통계화

## 1. Logger 라이브러리
Logback, Log4J 등이 있지만 스프링부트에서 이 것들을 통합해서 쓰는 것이 바로 `SLF4J`이다.  

## 2. Log 선언
[1번 예제]    
```java
// 1번
private final Logger log = LoggerFactory.getLogger(getClass());
// 2번
private static final Logger log = LoggerFactory.getLogger(Xxx.class)
// 3번
@Slf4j
public class LogTestController {
}
```
<br>

[2번 예제]  
```java
    public TraceStatus begin(String message) {
        TraceId traceId = new TraceId();
        Long startTimeMs = System.currentTimeMillis();
        log.info("[{}] {}{}", traceId.getId(), addSpace(START_PREFIX, traceId.getLevel()), message);    // 로그 호출

        // 로그 출력
        return new TraceStatus(traceId, startTimeMs, message);
    }
```  


## 3. 로그 레벨
*로그에는 레벨이 있다!*  
```java
trace -> debug -> info -> warn -> error   
```  
- 일반적으로 개발 서버는 debug, 운영 서버는 info로 설정함.  

[로그 호출]  
```java
@Slf4j
@RestController
public class LogTestController {

    @RequestMapping("/log-test")
    public String logTest(){
        // 로그 라이브러리 이용한 출력
        log.trace("trace log={}", name);
        log.debug("debug log={}", name);
        log.info("info log={}", name);
        log.warn("warn log={}", name);
        log.error("error log={}", name);

        return "ok";
    }
}
```
