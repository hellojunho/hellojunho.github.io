---
layout: single
title: "📘[Spring] Mapping 애노테이션이란?"
toc: true
toc_sticky: true
toc_label: "목차"
categories: spring
excerpt: ""
tag: [java, annotation]
---

# Mapping이란?
웹에서 **매핑**이란 `해당 값이 다른 값을 가리키도록 하는 것`이다.  
예를들어, http://localhost:8080/web.html 이라는 페이지를 만들었다고 하면 이 url이 그대로 노출되게 된다.  
그렇게되면 보안상 매우 취약할 수 있다.  
이런 취약점을 방지하기 위해 web.html이 아닌 w이 web.html과 같다고 설정하여 보안성을 높일 수 있다.  

## Spring에서 사용하는 매핑 애노테이션
본격적으로 매핑 애노테이션들에 대해 작성한다.  

### @RequestMapping
요청 URL을 어떤 메소드가 처리할지 매핑해주는 애노테이션이다.  
url별로 다르게 요청이 들어오면 다른 처리를 해야하는 것이 당연하다.  
`~/hello/junho` 와 같은 요청과 `/hi/kong`의 요청은 엄연히 다르다.  
`/hello`와 `/hi`로 시작하는 요청을 각각 처리하기 위해 **@RequestMapping**을 사용한다.  

### @GetMapping
Http Method에는 CRUD를 위한 **GET, POST, PUT, DELETE**가 존재한다.  
**@GetMapping**은 이 중에 Get Method 역할을 수행한다.  
@GetMapping이 붙은 메소드는 요청에 대한 값을 `조회`하는 역할을 수행한다.  
<br>

[예제코드]  
```
    @GetMapping("/")
    public String list(Model model) {
        List<BoardDto> boardDtoList = boardService.getBoardlist();
        model.addAttribute("boardList", boardDtoList);

        return "board/list.html";
    }
```

### @PostMapping
**@PostMapping**은 당연히 Post Method 역할을 수행한다.  
@PostMapping이 붙은 메소드는 요청에 대해 값을 수정, 삭제 등의 작업을 수행할 수 있다.  
<br>

[예제코드]  
```
    @PostMapping("/post")
    public String write(BoardDto boardDto) {
        boardService.savePost(boardDto);
        return "redirect:/";
    }
```

### @PutMapping
**@PutMapping**은 Put Method 역할을 수행하고, Put은 데이터 수정을 위해 사용한다.  
하지만 보안상의 이유로 Put메소드보단 Post메소드를 선호한다고 한다.  
<br>

[예제코드]  
```
    @PutMapping("/post/edil/{no}")
    public String update(BoardDto boardDto) {
        boardService.savePost(boardDto);
        return "redirect:/";
    }
```

### @DeleteMapping
**@DeleteMapping**또한 Delete Method 역할을 수행하며, 데이터의 삭제를 담당한다.  
Delete도 Post로 대체 가능하며, 보안상의 이유로 Post를 선호한다고 한다.  
<br>

[예제코드]  
```
    @DeleteMapping("/post/{no}")
    public String delete(@PathVariable("no") Long id) {
        boardService.deletePost(id);

        return "redirect:/";
    }
```

# 참고자료
[Web에서 매핑이란? JSP Mapping](https://threeidiotscoding.tistory.com/26)  

[[Spring Boot + JPA + MySQL] 게시글 수정 & 삭제](https://velog.io/@max9106/Spring-Boot-JPA-MySQL-%EA%B2%8C%EC%8B%9C%EA%B8%80-%EC%88%98%EC%A0%95-%EC%82%AD%EC%A0%9C)  