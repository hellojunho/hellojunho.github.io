---
layout: single
title: "📘[스프링부트와 AWS로 혼자 구현하는 웹 서비스] JPA란?"
toc: true
toc_sticky: true
toc_label: "목차"
categories: spring
excerpt: ""
tag: [java, jpa]
---

# JPA란?
현대 웹 에플리케이션에서 `관계형 데이터베이스(RDB)`는 절대 빠질 수 없는 요소이다.  
Oracle, MySQL, MsSQL 등을 쓰지 않는 웹 애플리케이션은 거의 없을정도로...  
그러다보니 **객체를 관계형 데이터베이스에서 관리**하는 것이 매우 중요하다.  
<br>

결국 현업 프로젝트의 대부분이 애플리케이션 코드보다 `SQL`이 많아지게 되었다.  
이유는 RDB가 SQL만 인식할 수 있기 때문이다.  
그래서 기본적인 `CRUD` SQL을 매번 생성해야한다.  
<br>

이런 SQL을 만드는 단순 방복 잡업도 있지만, `패러다임 불일치`라는 문제점이 존재한다.  
RDB는 **어떻게 데이터를 저장**할지에 초점이 맞춰진 기술이다.  
반대로 객체지향 프로그래밍 언어는 메시지를 기반으로 **기능과 속성을 한 곳에서 관리**하는 기술이다.  
<br>

이렇듯, RDB와 객체지향 프로그래밍 언어간의 `패러다임 불일치`가 발생한다.  
객체지향 프로그래밍에서 부모가 되는 객체를 가져오려면 어떻게 해야할까?  
```
User user = findUser();
Group group = user.getGroup();
```  

위와 같은 코드를 작성하면 되는데, 여기에 데이터베이스를 추가하면 아래와 같다.  
```
User user = userDao.findUser();
Group group = groupDao.findGroup(user.getGroup());
```  
위와 같이 User, Group 각각 따로 조회하게 된다.  
User와 Group이 어떤 관계인지 알 수 있을까?  
상속, 1:N 등의 다양한 객체 모델링을 데이터베이스로 구현할 수 없다.  

이러한 문제점을 해결하기 위해 등장한 것이 바로 `JPA`이다.  

# 그래서 JPA가 뭐냐고
서로 지향하는 바가 다른 2개의 영역을 중간에서 `패러다임 일치`를 시켜주기 위한 기술이다.  
즉, 개발자는 객체지향적으로 프로그래밍을 하고, JPA가 이를 RDB에 맞게 SQL을 대신 생성해서 실행한다.  
이로써 항상 개발자는 객체지향적으로 코드를 작성하여 SQL에 종속적인 개발을 하지 않아도 된다.  

## Spring Data JPA
`JPA`는 인터페이스로서, 자바 표준명세서이다.  
인터페이스인 JPA를 사용하기 위해서는 구현체인 `Hibernate`, `EclipseLink` 등이 있다.  
하지만, Spring에서 JPA를 사용할 때는 이 구현체들을 직접 다루지는 않는다.  
<br>

구현체들을 좀 더 쉽게 사용하고자 추상화시킨 `Spring Data JPA`라는 모듈을 사용한다!  
이들의 관계는 아래와 같다.  
```
Spring Data JPA -> Hibernate -> JPA
```  

### 장점1: 구현체 교체의 용이성
`구현체 교체의 용이성`이란, Hibernate 외에 다른 구현체로 쉽게 교체하기 위함이다.  
쉽게 생각하면 Hibernate가 수명을 다해 새로운 JPA 구현체가 대세로 떠오르게 될 때, Spring Data JPA를 사용하고 있다면 쉽게 갈아탈 수 있다.  
<br>

실제로 자바의 Redis 클라이언트가 Jedis에서 Lettuce로 대세가 넘어갈 때, Spring Data Redis를 사용한 유저들은 쉽게 교체를 했다고 한다.  

### 장점2: 저장소 교체의 용이성
데이터 트래픽이 많아질 수록 관계형 데이터베이스로는 감당이 안될 수 있다.  
이 때 MongoDB로 교체가 필요하다면 개발자는 Spring Data JPA에서 Spring Data MongoDB로 의존성만 교체하면 된다는 장점이 있다.  

## 주요 애노테이션

### @Entity
테이블과 링크될 클래스임을 나타낸다.  
기본값으로 클래스와 카멜케이스의 이름을 언어스코어 네이밍(_)으로 테이블 이름을 매칭한다.  
- 예: SalesManager.java -> sales_manager table 
<br>

**Entity 클래스에는 절대 setter메소드를 만들지 않는다.**  
이유는 해당 클래스의 인스턴스 값들이 언제 어디서 변해야 하는지 코드상으로 명확하게 구분할 수가 없어, 차후 기능 변경 시 복잡해질 수 있기 때문이다.  

대신, 해당 필드의 값 변경이 필요하면 명확히 그 목적과 의도를 나타낼 수 있는 매소드를 추가해야만 한다.  
<br>

그러면, 어떻게 데이터베이스에 값을 채워 삽입할까?
생성자 대신 `@Builder`를 통해 제공되는 빌더 클래스를 사용하면 된다.  

### @Id
해당 테이블의 PK 필드를 나타낸다.  

### @GeneratedValue
PK의 생성 규칙을 나타낸다.  
스프링부트 2.0에서는 `GenerationType.IDENTITY` 옵션을 추가해야만 SQL에서 PK에 대해 `auto_increment`가 적용된다.

### @Column
테이블의 칼럼을 나타내며 굳이 선언하지 않더라도 해당 클래스의 필드는 모두 칼럼이 된다.  
사용하는 이유는 기본값 외에 추가로 변경이 필요한 옵션이 있으면 사용한다.  

```java
@Column(length = 500, nullable = false)
private String name;
```  
위의 예시에서는 `@Column`에 `length=500, nullable=false` 옵션이 적용되었다.  
`length = 500`의 의미는 name 변수의 **최대 길이가 500으로 제한한다**는 의미이다.  
SQL에서 `varchar(500)`으로 볼 수 있다.  
이 때, UTF-8 인코딩 기준으로 한글은 한 글자를 3바이트로 표현하기 때문에 약 166자가 가능하다.  
<br>

`nullable = false`는 SQL에서 `not null`의 의미로 보면 된다.  
즉, 해당 컬럼이 **null값을 허용하지 않는다**는 의미이다.  
<br>

이렇게 `@Column(length = 500, nullable = false)`를 사용해 필드에 컬럼 매핑 정보를 지정하면 JPA가 해당 필드를 DB의 컬럼으로 매핑할 때 이 정보를 참고하여 
테이블을 생성하거나 업데이트 한다.  
이를 통해 DB의 스키마를 관리하면서 필드의 제약조건을 명시적으로 지정할 수 있다.  

### @Builder
`@Builder`는 Lombok 라이브러리에서 제공하는 애노테이션이다.  
이를 사용하면 **불변 객체를 생성하는 빌더 패턴을 자동으로 생성**할 수 있다.  
<br>

`@Builder` 애노테이션을 클래스 레벨에 적용하면 해당 클래스에 대한 빌더 클래스를 생성하고, 이 빌더 클래스를 사용하여 객체 생성 및 속성 값을 설정하는 메서드 체인 형식으로 
표현할 수 있다.  

**@Builder의 장점**  
1. 가독성이 좋아진다.  
   - 객체 생성 시에 `메서드 체인` 형식으로 속성 값을 설정할 수 있어 가독성이 향상된다.  
2. 필수 속성을 명시적으로 표현할 수 있다.  
   - 빌더 패턴에서 필수로 설정해야 하는 속성을 명시할 수 있어 객체의 `불변성`을 보장한다.  
3. 선택적 속성을 유연하게 설정할 수 있다.  
   - 선택적으로 설정할 수 있는 속성을 쉽게 추가할 수 있다.
<br>
   
**예시**  
[UserUpdateRequestDto]
```java
    ...
    @Builder
    public UserUpdateRequestDto(Long id, String name, String phone, String address) {
        this.id = id;
        this.name = name;
        this.phone = phone;
        this.address = address;
    }
    ...
```  
<br>

[UserControllerTest]  
```java
  ...
  @Test
      void update() {
        // given
        Long id = 3L;
        String name = "update_test2";
        String phone = "010-1111-1111";
        String address = "suwon";
  
        String url = "http://localhost:" + port + "/users/update/" + id;
  
        UserUpdateRequestDto requestDto = UserUpdateRequestDto.builder()
        .id(id)
        .name(name)
        .phone(phone)
        .address(address)
        .build();
  
        // when
        ResponseEntity<String> responseEntity = restTemplate.exchange(url, HttpMethod.PUT, new HttpEntity<>(requestDto), String.class);
  
    // then
    assertThat(responseEntity.getStatusCode()).isEqualTo(HttpStatus.OK);
    }
  ...
```