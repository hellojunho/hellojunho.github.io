---
layout: single
title: "📘[Java] DAO, DTO, VO에 대해서..."
toc: true
toc_sticky: true
toc_label: "목차"
categories: java
excerpt: ""
tag: [spring, web]
---

# DAO, DTO, VO가 뭘까?
`DAO`, `DTO`, `VO`는 모두 데이터와 연관된 객체라는 공통점이 있다.  
<br>

`Spring`으로 개발을 할 때 어떤 객체가 데이터를 사용하고, 어떤 객체가 데이터베이스와 직접 연동하는지가 어려워 많이 해맸다.  
여기에 그런 역할을 하는 객체들인 `DAO`, `DTO`, `VO`에 대해 정리하겠다.  

## DAO (Data Access Object)
`DAO`란, `데이터를 사용`할 수 있는 기능이 담긴 클래스이다.  
DB에 **접근하기 위한 로직**과 **비즈니스 로직**을 분리하기 위해 사용한다.  
그래서 `DB Connection`로직까지 설정되어 있는 경우가 많고, DB를 사용해 데이터를 `CRUD`하는 기능을 전담한다.  
<br>

*`Spring`에서 `@Repository`애노테이션이 붙은 클래스가 `DAO`라고 합니다.*  

### CRUD란?
`CRUD`란, 데이터를 생성, 읽기, 수정, 삭제 하는 등의 기능을 말한다.  

|CRUD| 설명        |
|---|-----------|
|**C**reate| 데이터를 "생성" |
|**R**ead| 데이터를 "읽기" |
|**U**pdate| 데이터를 "수정" |
|**D**elete| 데이터를 "삭제" |

### 예제코드
[MemberDAO]  
```java
package org.practice.di.persistence;

import org.practice.di.domain.StudentVO;

public interface MemberDAO {
	public StudentVO read(String id) throws Exception;
	public void add(StudentVO student) throws Exception;
}  
```
<br>

[MemberDAOImpl]  
```java
package org.practice.di.persistence;

import java.util.HashMap;
import java.util.Map;

import org.practice.di.domain.StudentVO;

public class MemberDAOImpl implements MemberDAO {
	
	private Map<String, StudentVO> storage = new HashMap<String, StudentVO>();
	
	public StudentVO read(String id) throws Exception {
		return storage.get(id);  
	}
	public void add(StudentVO student) throws Exception { 
		storage.put(student.getId(), student);
	}
}
```

## DTO (Data Transfer Object)
`DTO`란, `데이터 저장`을 담당하는 클래스이다.  
`Controller`, `Service`, `View` 등의 계층 간 `데이터 교환`을 위해 사용되는 객체로 생각할 수 있다.  
<br>

**로직을 갖고있지 않은 순수한 데이터 객체 -> getter/setter & toString() 메소드만 포함하기 때문에 `가변`적인 속성을 띈다.**  
<br>

*유저가 데이터를 입력하고, 그 데이터를 DB에 넣는 과정을 생각해보자.*  
- 유저가 자신의 브라우저에서 데이터를 입력
- form에 있는 데이터를 `DTO`에 담아 전송
- 해당 `DTO`를 받은 서버가 `DAO`를 이용해 데이터베이스로 데이터를 삽입

### 예제코드
[Service]  
```java
package org.practice.di.service;

import org.practice.di.domain.StudentVO;

public interface MemberService {
		public StudentVO readMember(String id) throws Exception;
		public void addMember(StudentVO student) throws Exception;
}
```  
<br>

[MemberServiceImpl]  
```java
package org.practice.di.service;

import org.practice.di.domain.StudentVO;
import org.practice.di.persistence.MemberDAO;

public class MemberServiceImpl implements MemberService {
  
  private MemberDAO memberDAO;
    
  public MemberServiceImpl(MemberDAO memberDAO) {
    this.memberDAO = memberDAO;
  }

  /*
  public void setMemberDAO(MemberDAO memberDAO) {
    this.memberDAO = memberDAO;
  }
  */
  
  public StudentVO readMember(String id) throws Exception {
    return memberDAO.read(id);
  }
  
  public void addMember(StudentVO student) throws Exception {
    memberDAO.add(student);
  }

}
```  

## VO (Value Object)
`VO`란, `데이터 저장`을 담당하는 클래스로, `DTO`와 비슷하다.  
그래서 서로 혼용하여 사용하기도 하지만, `VO`는 `값`을 위해 사용하는 객체로, `불변`적인 속성을 갖고있다.  

### 예제코드
[StudentVO]  
```
package org.practice.di.domain;

public class StudentVO {
	private String id;
	private String passwd;
	private String username;
	private String snum;
	private String depart;
	private String mobile;
	private String email;
	public String getId() {
		return id;
	}
	public void setId(String id) {
		this.id = id;
	}
	public String getPasswd() {
		return passwd;
	}
	public void setPasswd(String passwd) {
		this.passwd = passwd;
	}
	public String getUsername() {
		return username;
	}
	public void setUsername(String username) {
		this.username = username;
	}
	public String getSnum() {
		return snum;
	}
	public void setSnum(String snum) {
		this.snum = snum;
	}
	public String getDepart() {
		return depart;
	}
	public void setDepart(String depart) {
		this.depart = depart;
	}
	public String getMobile() {
		return mobile;
	}
	public void setMobile(String mobile) {
		this.mobile = mobile;
	}
	public String getEmail() {
		return email;
	}
	public void setEmail(String email) {
		this.email = email;
	}
	@Override
	public String toString() {
		return "StudentVO [id=" + id + ", passwd=" + passwd + ", username=" + username + ", snum=" + snum + ", depart="
				+ depart + ", mobile=" + mobile + ", email=" + email + "]";
	}

}
```
## DTO와 VO의 차이

| 종류 | 용도 | 동등 결정 | 가변 / 불변 | 로직 |
| --- | --- | --- | --- | --- |
| DTO | - 계층 간 데이터 전달 | - 속성값이 모두 같아도 같은 객체가 아닐 수 있음 | - setter 존재 시 가변,- setter 비 존재 시 불변 | - getter/setter 이외의 로직이 불필요함 |
| VO | - 값 자체를 표현 | - 속성값이 모두 같으면 같은 객체 | - 불변 | - getter/setter 이외의 로직을 가질 수 있음 |

# 참고자료
[https://melonicedlatte.com/2021/07/24/231500.html](https://melonicedlatte.com/2021/07/24/231500.html)  
[https://lemontia.tistory.com/591](https://lemontia.tistory.com/591)  
[https://dkswnkk.tistory.com/500](https://dkswnkk.tistory.com/500)