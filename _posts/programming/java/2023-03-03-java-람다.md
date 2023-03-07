---
layout: single
title: "📘[Java] 람다에 대해서..."
toc: true
toc_sticky: true
toc_label: "목차"
categories: java
excerpt: ""
tag: []
---

# 람다 함수가 뭔뎨?
`람다 함수`는 `익명 함수`를 지칭하는 용어이다.  
그럼 람다 함수를 이용한 표현식을 `람다 표현식`이라고 말할 수 있겠다!  

## 람다 표현식
`람다 표현식`은 간단하게 말하면 "메소드를 하나의 식으로 표현한 것"이다.  
<br>

[메소드]  
```java
int min(int x, int y) {

    return x < y ? x : y;

}
```

[람다 표현식]  
```java
(x, y) -> x < y ? x : y;
```   

[익명 클래스]
```java
new Object() {

    int min(int x, int y) {

        return x < y ? x : y;

    }

}
```
위의 예제처럼 메소드를 람다 표현식으로 표현하면, 클래스를 작성하고 객체를 생성하지 않아도 메소드를 사용할 수 있습니다.  
그런데 자바에서는 클래스의 선언과 동시에 객체를 생성하므로, "단 하나의 객체만을 생성할 수 있는 클래스를 `익명 클래스`"라고 합니다.  
따라서 자바에서 람다 표현식은 익명 클래스와 같다고 할 수 있다.  

## 익명(Anonymous) 함수, 클래스란?
자꾸 익명, 익명 하는데 익명 클래스, 익명 함수가 도대체 뭘까 싶었다.  

익명 함수
: 익명 함수는 말 그대로 이름이 없는 함수이다.  
: 공통적으로 `일급 객체`를 가지고 있다.  
: 일급 객체란, 일반적으로 다를 객체들에 적용 가능한 연산을 모두 지원하는 개체를 말한다.  

익명 클래스
: 익명 함수와 마찬가지로 이름이 없는 클래스를 말한다.  
: 클래스의 선언 객체의 생성을 동시에 하기 때문에 단 한번만 사용될 수 있다.  
: 쉽게 말해, 오직 하나의 객체만 생성할 수 있는 일회용 클래스를 말한다.  

## 람다 표현식 작성법
java8부터 람다 표현식을 지원한다.  
람다 표현식을 작성하는 방법은 화살표(->)를 사용하여 작성할 수 있다.  
<br>

[문법]  
```java
(매개변수목록) -> { 함수몸체 }
```  

1. 매개변수의 타입을 추론할 수 있는 경우에는 타입을 생략할 수 있다.  
2. 매개변수가 하나인 경우에는 괄호(())를 생략할 수 있다.  
3. 함수의 몸체가 하나의 명령문만으로 이루어진 경우에는 중괄호({})를 생략할 수 있다. (이때 세미콜론(;)은 붙이지 않음)  
4. 함수의 몸체가 하나의 return 문으로만 이루어진 경우에는 중괄호({})를 생략할 수 없다.  
5. return 문 대신 표현식을 사용할 수 있으며, 이때 반환값은 표현식의 결과가 된다. (이때 세미콜론(;)은 붙이지 않음)

## 예제
인프런 - 김영한 강사님의 강의 중 람다 표현식을 사용한 경우의 일부분이다.  

[원본]  
```java
ContextV2 context = new ContextV2();
        context.execute(new Strategy() {
            @Override
            public void call() {
                log.info("비즈니스 로직1 실행");
            }
        });
```  
<br>

[람다 표현식 적용]  
```java
ContextV2 context = new ContextV2();
        // 람다식으로 변형하려면, option + enter 누르기
        context.execute(() -> log.info("비즈니스 로직1 실행"));
```  
위의 예제를 보면, execute()의 함수에서 매개변수로 받는 객체인 Strategy의 본체인 log.info()를 화살표(->)를 사용하여 가리킨다.  

## 참고자료
[https://khj93.tistory.com/entry/JAVA-%EB%9E%8C%EB%8B%A4%EC%8B%9DRambda%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%B4%EA%B3%A0-%EC%82%AC%EC%9A%A9%EB%B2%95](https://khj93.tistory.com/entry/JAVA-%EB%9E%8C%EB%8B%A4%EC%8B%9DRambda%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%B4%EA%B3%A0-%EC%82%AC%EC%9A%A9%EB%B2%95)  
[http://www.tcpschool.com/java/java_lambda_concept](http://www.tcpschool.com/java/java_lambda_concept)  
[https://mindols.tistory.com/103](https://mindols.tistory.com/103)  