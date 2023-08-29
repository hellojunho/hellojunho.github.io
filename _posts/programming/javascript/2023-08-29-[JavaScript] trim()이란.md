---
layout: single
title: "📘[JavaScript] trim()이란?"
toc: true
toc_sticky: true
toc_label: "목차"
categories: web, javascript
excerpt: ""
tag: [javascript]
---

# trim()이란?
`JavaScript`에서 **trim()**이라는 메서드를 사용하게 됐다.  
trim()은 `문자열 좌우에서 공백을 제거`하는 함수이다.  
대부분의 언어에서 제공하고 있으며, 좌/우 측만 trim하는 메서드를 제공하기도 한다고 한다.  
<br>

js(javascript)에서 자주 사용하는데, IE8 이하에서는 제공되지 않는다.  

## trim() 예시
```
<!DOCTYPE html>
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
<title>자바스크립트 TRIM</title>
<script type="text/javascript" src="https://code.jquery.com/jquery-1.12.4.min.js"></script>
<!--[if lte IE 8]>
<script type="text/javascript">
String.prototype.trim = function() {
    return this.replace(/^\s+|\s+$/g,"");
}
</script>
<![endif]-->
<script type="text/javascript">
String.prototype.ltrim = function() {
    return this.replace(/^\s+/,"");
}
String.prototype.rtrim = function() {
    return this.replace(/\s+$/,"");
}

var str = " test ";

console.log(":" + str.trim() + ":");
console.log(":" + $.trim(str) + ":");
console.log(":" + str.ltrim() + ":");
console.log(":" + str.rtrim() + ":");
</script>
</head>
<body>
<h1>TRIM</h1>
</body>
</html>
```
<br>

**결과**  
```
:test:
:test:
:test :
: test:
```

## trim() 메서드의 사용법
자바스크립트 기능을 사용할 때의 코드이다.  
```
var str = " test ";
var trimStr = str.trim();
```  
<br>

JQuery를 사용할 때의 코드이다.  
```
var str = " test ";
var trimStr = $.trim(str);
```

# 참고자료
[https://offbyone.tistory.com/174](https://offbyone.tistory.com/174)  
