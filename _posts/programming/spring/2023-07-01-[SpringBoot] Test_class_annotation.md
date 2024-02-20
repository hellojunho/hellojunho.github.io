---
layout: single
title: "📘[스프링부트와 AWS로 혼자 구현하는 웹 서비스] Test 클래스에 사용되는 애노테이션 및 코드 설명2"
toc: true
toc_sticky: true
toc_label: "목차"
categories: spring
excerpt: ""
tag: [java]
---

# Test Class 2
```
package com.jojoldu.book.springboot.web.dto;

import org.junit.jupiter.api.Test;

import static org.assertj.core.api.Assertions.assertThat;


class HelloResponseDtoTest {

    @Test
    public void lombok_function_test() {

        // given
        String name = "test";
        int amount = 1000;

        // when
        HelloResponseDto dto = new HelloResponseDto(name, amount);

        // then
        assertThat(dto.getName()).isEqualTo(name);
        assertThat(dto.getAmount()).isEqualTo(amount);

    }

}
```  
<br>

## assertThat
`assertThat`은 `assertj`라는 테트 검증 라이브러리의 검증 메소드이다.  
검증하고 싶은 대상을 메소드 인자로 받고, 메소드 체이닝이 지원되어 `isEqualTo()`와 같은 메소드를 이어 사용할 수 있다.  
<br>

*왜 Junit의 assertThat이 아닌 assertj의 assertThat을 사용했을까?*  
assertj의 다음 장점들 때문이다.  
1. CoreMatchers와 달리 추가적으로 라이브러리가 필요하지 않다.  
- Junit의 assertThat을 사용하면 is()와 같이 CoreMatchers 라이브러리가 필요하다.  
2. 자동완성이 좀 더 확실하게 지원된다.  
- IDE에서는 CoreMatchers와 같은 Matcher 라이브러리의 자동완성 지원이 약하다.  

### isEqualTo()
assertj의 `동등 비교` 메소드이다.  
assertThat에 있는 값과 isEqualTo의 값을 비교해 같을 때만 성공하는 역할이다.  

## org.junit.Test와 org.junit.jupiter.api.Test
`org.junit.Test`와 `org.junit.jupiter.api.Test`는 모두 JUnit 프레임워크에서 테스트 메소드를 지정하기 위한 애노테이션이다.  
<br>

하지만, `org.junit.jupiter.api.Test`는 `org.junit.Test`와 다르게 Junit5에서 도입된 애노테이션이다.  
Junit5는 Java8 이상을 지원하며, 향상된 기능과 유연성을 제공한다.  
<br>
결국 Junit5 에서는 `org.junit.jupiter.api.Test`를 사용해야한다.

# Test Class 3
[HelloControllerTest]  
```
import static org.hamcrest.Matchers.is;
import org.junit.jupiter.api.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.autoconfigure.web.servlet.WebMvcTest;
import org.springframework.test.context.junit4.SpringRunner;
import org.springframework.test.web.servlet.MockMvc;
import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.get;
import static org.springframework.test.web.client.match.MockRestRequestMatchers.content;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.jsonPath;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.status;


 @Test
    public void helloDto가_리턴된다() throws Exception {
        String name = "hello";
        int amount = 1000;

        mvc.perform(
                        get("/hello/dto").param("name", name)
                                .param("amount", String.valueOf(amount)))
                .andExpect(status().isOk())
                .andExpect(jsonPath("$.name", is(name)))
                .andExpect(jsonPath("$.amount", is(amount)));
    }
```  

## param
API 테스트를 할 때 사용될 요청 파라미터를 설정한다.  
단, 값은 `String`만 허용되며, 숫자/날짜와 같은 데이터도 등록할 때는 문자열로 변경해야한다.  

## jsonPath()
JSON 응답값을 필드별로 검증할 수 있는 메소드로, `$`를 기준으로 필드명을 명시한다.  
여기서는 name, amount를 검증하므로, `$.name`, `$.amount`로 검증한다.  

