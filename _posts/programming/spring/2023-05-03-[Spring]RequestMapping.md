---
layout: single
title: "📘[Spring] ReqestMapping(매핑 설정 방식)에 대해서..."
toc: true
toc_sticky: true
toc_label: "목차"
categories: spring
excerpt: ""
tag: [java, request]
---

# ReqestMappin: 매핑 설정 방식

## 방식1: @PathVaiable
URL 경로 내의 변수 값을 `@Pathvariable 적용변수`로 전달한다.  

```
@RequestMapping(value="/example/{msg}", method = RequestMethod.GET)
public String getUesrTest(@PathVariable("msg") String msg) {
    ...
}
```  

## 방식2: @RequestParam
요청 파라미터 값을 `@RequestParam 적용변수`로 전달한다.  

```
@RequestMapping(value="/example", method=RequestMethod.GET)
public String getUserTest(@RequestParam("msg") String msg) {
    ...
}
```  
## 방식3: @ModelAttribute
요청 파라미터 값을 `@ModelAttribute 적용변수`로 전달한다.  

```
@RequestMapping(value="/example", method=RequestMethod.GET)
public String getUserTest(@ModelAttribute("msg") String msg) {
    ...
}
```  

## 방식4: RequestMapping(value={"/~", "/~"})
```
@RequestMapping(value={"/example1", "/example2"}, method=RequestMethod.GET)
public String getUserTest(@ModelAttribute("msg") String msg) {
    ...
}
