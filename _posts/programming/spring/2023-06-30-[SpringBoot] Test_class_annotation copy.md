---
layout: single
title: "📘[SpringBoot] Test 클래스에 사용되는 애노테이션 및 코드 설명"
toc: true
toc_sticky: true
toc_label: "목차"
categories: spring
excerpt: ""
tag: [java, annotation]
---

# Test Class 1
```
package com.jojoldu.book.springboot.web;

import org.junit.jupiter.api.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.autoconfigure.web.servlet.WebMvcTest;
import org.springframework.test.context.junit4.SpringRunner;
import org.springframework.test.web.servlet.MockMvc;
import org.springframework.test.web.servlet.ResultActions;
import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.get;
import static org.springframework.test.web.client.match.MockRestRequestMatchers.content;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.status;

@RunWith(SpringRunner.class)
@WebMvcTest(controllers = HelloController.class)
class HelloControllerTest {

    @Autowired
    private MockMvc mvc;

    @Test
    public void return_hello() throws Exception {
        String hello = "hello";

        mvc.perform(get("hello")).andExpect(status().isOk()).andExpect(content().string(hello));
    }
}
```  
<br>

## @RunWith(SpringRunner.class)
테스트를 진행할 때 `Junit`에 내장된 실행자 외에 다른 실행자를 실행시킨다.  
여기서는 `SpringRunner`라는 스프링 실행자를 사용한다.  
즉, 스프링부트 테스트와 Junit 사이에 연결자 역할을 하는 것이다.  

## @WebMvcTest
여러 스프링 애노테이션 중, `Web`에 집중할 수 있는 애노테이션으로, 선언 시에는 `@Controller`, `@ControllerAdivce` 등을 사용할 수 있다.  
단, `@Service`, `@Component`, `@Repository`와 같은 애노테이션은 사용할 수 없다.  
위 코드에서는 컨트롤러만 사용하기 때문에 선언했다.  

## @Autowired
스프링이 관리하는 `빈(bean)`을 주입 받는다.  

## @private MockMvc mvc
웹 API를 테스트할 때 사용한다.  
스프링 MVC 테스트의 시작점으로, 이 클래스를 통해 `HTTP GET, POST`등에 대한 API 테스트를 할 수 있다.  

## @mvc.perform(get("/hello))
`MockMvc`를 통해 `/hello` 주소로 `GET`요청을 한다.  
체이닝이 지원되어 아래와 같이 여러 검증 기능을 이어서 선언할 수 있다.  

### .andExpect(status.isOk())
`mvc.perform`의 결과를 검증한다.  
`HTTP Header`의 `Status(200, 404, 500 등)`를 검증하는 역할을 담당한다.  
위 코드에서는 isOk() -> 200(OK)인지 아닌지를 검증한다.  

### .andExpect(content().string(hello))
`mvc.perform`의 결과를 검증한다.  
응답 본문의 내용을 검증하는 역할로, Controllerdptj "hello"를 리턴하기 때문에 이 값이 맞는지 아닌지를 검증하는 역할을 수행한다.  

# 참고자료
교재: 스프링부트와 AWS로 혼자 구현하는 웹 서비스