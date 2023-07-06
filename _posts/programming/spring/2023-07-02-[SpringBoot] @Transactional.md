---
layout: single
title: "📘[SpringBoot] @Transactional 애노테이션"
toc: true
toc_sticky: true
toc_label: "목차"
categories: spring
excerpt: ""
tag: [java]
---

# @Transactional
`@Transactional` 애노테이션은 메서드 내에서 실행되는 데이터베이스 작업을 하나의 트랜잭션으로 묶는 역할을 한다.  
트랜잭션은 데이터베이스의 작업의 `원자성(Atomicity)`, `일관성(Consistency)`, `독립성(Isolation)`, `지속성(Durability)`의 4가지 
속성을 보장하는 개념이다.  

트랜잭션에서 `롤백(Rollback)`은 작업 중에 **예외**가 발생하거나 **명시적으로 롤백을 지정한 경우**에 수행된다.  
롤백은 **트랜잭션 내에서 이전 상태로 되돌리는 작업**이다.  
이는 데이터 일관성을 유지하고, 데이터베이스를 일관된 상태로 유지하는데에 있어 중요한 작업이다.  
<br>

추가로, 롤백은 **데이터베이스의 작업이 모두 성공한 경우**에도 수행된다.  
데이터베이스의 작업이 모두 성공했다 하더라도, 트랜잭션은 여전히 유용하기 때문이다.  
여러 개의 작업이 하나의 트랜잭션으로 묶여있는 경우, 모든 작업이 성공했을 때만 최종적인 `커밋(Commit)`이 수행되어 
데이터베이스에 변경 내용이 영구적으로 반영되기 때문이다.  
<br>

이처럼 롤백을 수행하는 이유는 데이터의 일관성과 안정성을 유지하기 위해서이다.  
`@Transactional` 애노테이션을 사용하면 `롤백`을 통해 데이터베이스 작업의 원자성과 일관성을 보장하고, 예외 발생 시에 데이터의 무결성을 유지할 수 있다.

## 예제
`UserService`는 회원 정보를 수정하는 메소드를 정의한 클래스이다.  
`User`클래스에 `id`가 Long으로 지정되어 있어 수정이 완료되면 수정한 데이터의 id를 넘겨주기 때문에 반환하는 자료형이 Long으로 지정되어있다.  

[UserService]  
```java
@Transactional
public Long update(Long id, UserUpdateRequestDto requestDto) {
    User user = userRepository.findById(id).orElseThrow(() -> new IllegalArgumentException("not found"));

    user.update(requestDto.getName(), requestDto.getPhone(), requestDto.getAddress());

    return id;
}
```
