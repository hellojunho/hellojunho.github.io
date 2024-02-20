---
layout: single
title: "📘[Spring] Thymeleaf란?"
toc: true
toc_sticky: true
toc_label: "목차"
categories: spring
excerpt: ""
tag: [java, thymeleaf]
---

# Thymeleaf란?
**Thymeleaf**란 Spring에서 가장 밀고있는 `View Template(뷰 템플릿)`이다.  
뷰 템플릿은 컨트롤러가 전달하는 데이터를 이용해 동적으로 화면을 구성할 수 있게 해준다.  
기존에는 **JSP + Spring**의 조합을 가장 많이 사용했지만, 요즘들어 Thymeleaf의 점유율도 많아지고 있다.
<br>

Thymeleaf는 템플릿 엔진 역할을 수행해 동적 컨텐츠 자리 표시자를 통해 템플릿(html 파일)을 만들 수 있다.  
이러한 표현식은 `th:text`, `th:if`, `th:each` 등과 같은 기호로 사용된다.

## 동적으로 구성?
Thymeleaf는 **화면을 동적으로 구성**한다고 했는데 이게 뭘까?  
웹은 **정적 웹**과 **동적 웹**으로 나뉜다.  
<br>

먼저 정적 웹은 말그대로 별다른 동작이 없는 웹이다.  
대표적으로 회사 소개 페이지가 있다.  
별다른 로그인이 없고, 페이지는 디바이스, 상황에 상관 없이 항상 일정한 화면을 제공한다.  
<br>

반면 동적 웹은 **클라이언트의 요청에 따라 다양한 화면을 제공**한다.  
로그인 후 마이페이지를 들어가면 각기 다른 정보를 확인할 수 있는 것 처럼.  
이런 것 처럼 동적 웹의 경우에는 웹 서버에서 단순히 HTML, CSS, JavaScript 파일만 제공하는 것이 아니라, `웹 어플리케이션 서버(WAS)`와 직접 통신을 해야한다.  

## 의존성 추가
Thymeleaf는 Spring에서 밀고있는 템플릿이긴 하지만, JSP와 달리 의존성을 따로 추가해줘야 한다.  

[pom.xml]  
```xml
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-thymeleaf</artifactId>
</dependency>
```  
[build.gradle]  
```groovy
dependencies {
...
	implementation 'org.springframework.boot:spring-boot-starter-thymeleaf'
	}
```  

## 예시
Thymeleaf를 사용하는 방식에는 4가지가 있다.  
1. 변수식: ${}  
2. 메시지 방식: #{}  
3. 객체변수식: *{}  
4. 링크 방식: @{}
<br>

아래와 같은 컨트롤러가 있다고 해보자.  
[WebController]  
```java
@RequestMapping("/test")
	public String test(Model model) {
        model.addAttribute("test","model값");
		return "test";
	}
```  
`Model`은 객체정보를 뜻하고 model에 addAttribute()를 통해 값을 추가해준다.  
이 값은 `key:value`의 형식으로, `test:model값`으로 들어가게된다.  
그리고 이 값은 html변수로 넘길 수 있다.  
<br>

[test.html]  
```html
<!DOCTYPE html>
<html lang="ko" xmlns:th="http://www.thymeleaf.org">
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
    <p th:text="${test}"></p>  
</body>
</html>
```  
위와 같이 **<html lang="ko" xmlns:th="http://www.thymeleaf.org">**를 설정해준다.  
이는 "이 html 문서는 Thymeleaf를 사용한다"라는 의미이다.  
그리고 `th:text=${test}`와 같은 태그를 사용하고, 아까 위에서 `test:model값`의 데이터를 가져오기 위해 키 값인 `test`를 태그에 감싸주면된다.  


# 참고자료
[https://lifere.tistory.com/entry/Spring-Boot-Thymeleaf-%ED%83%80%EC%9E%84%EB%A6%AC%ED%94%84-%EC%82%AC%EC%9A%A9%EB%B0%A9%EB%B2%95-%EB%B0%8F-%EC%8B%9C%EC%9E%91%ED%95%98%EA%B8%B0](https://lifere.tistory.com/entry/Spring-Boot-Thymeleaf-%ED%83%80%EC%9E%84%EB%A6%AC%ED%94%84-%EC%82%AC%EC%9A%A9%EB%B0%A9%EB%B2%95-%EB%B0%8F-%EC%8B%9C%EC%9E%91%ED%95%98%EA%B8%B0)  
[https://m.blog.naver.com/PostView.naver?blogId=bgpoilkj&logNo=221982228705&categoryNo=20&proxyReferer=](https://m.blog.naver.com/PostView.naver?blogId=bgpoilkj&logNo=221982228705&categoryNo=20&proxyReferer=)  
[https://maily.so/grabnews/posts/ce76c9](https://maily.so/grabnews/posts/ce76c9)