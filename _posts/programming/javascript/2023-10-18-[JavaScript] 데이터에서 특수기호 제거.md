---
layout: single
title: "📘[JavaScript]데이터에서 특수기호를 제거하는 법"
toc: true
toc_sticky: true
toc_label: "목차"
categories: javascript
excerpt: ""
tag: [web]
---

# 문자열 데이터에서 공백을 제거하고 싶다.
먼저 문자열을 입력받았을 때, 문자열 앞뒤의 공백을 제거하는 함수로는 `$.trim()` 함수가 있다.  
이 함수는 문자열의 양 끝에 있는 공백을 제거해 순수한 문자열 값을 얻는 데에 사용한다.  
<br>


# 문자열 데이터에서 특수기호를 제거하고 싶다.
문자열 데이터에서 특수기호(예: 콤마(,))를 제거하고 싶다면 `replace()` 함수를 사용하면 된다.  
<br>

[예시]  
```
function removeCommas(str) {
    return str.replace(/,/g, '');
}

```  
위 코드는 JavaScript의 함수로, 주어진 문자열 `str`에서 모든 콤마(,)를 제거하는 역할을 수행한다.  
`/, /g`는 정규 표현식을 사용하여 문자열에서 모든 콤마를 찾는 역할을 한다.
<br>

- replace(): 함수는 문자열에서 특정 패턴을 찾아 다른 문자열로 대체하는 JavaScript의 문자열 메서드이다.  
- / , /g: 정규 표현식의 패턴으로, `/`로 둘러싸인 부분은 정규 표현식이고, `,`는 찾을 대상 문자를 나타낸다. `/g`는 전역 검색 플래그를 나타내며, 문자열 내의 모든 콤마를 찾아야 한다는 의미이다.  
- '': 콤마를 찾았다면, 콤마를 대체할 문자열이다. 여기서는 빈 문자열로 대체하므로 모든 콤마를 제거한다.  

예를 들어, `removeCommas("1,000,000")`를 호출한다면 `1,000,000 -> 1000000` 으로 바뀌어 반환되는 것이다.  