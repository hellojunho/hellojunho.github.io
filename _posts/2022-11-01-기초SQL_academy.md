# 📘 기초 SQL에 대해서...

## SELECT - 데이터 조회
- select 속성명 from 테이블
- select [ all | distinct ] 속성명 from 테이블
  - all : 전체 출력, 기본 값
  - distinct : 중복 제거
---
## WHERE - 조건문
- where 조건식
- WHERE문의 조건식
  - 속성명 연산자 값
    - ex) price >= 10000 and price <= 20000
    - ex) publisher = '굿스포츠' or       publisher = '대한미디어'
---
## GROUP BY - 그룹화
- 통계에 많이 쓰임
- group by 속성명
  - ex) select 
	custid, 
	count(*) as '주문 개수', 
from orders group by custid;  
    - 'as'는 별칭(alias)으로, 생략이 가능 -> count(*) '주문 개수'와 동일
---
## HAVING
- having은 보통 group by와 같이 쓰는 경우가 많음
- 즉, 통계함수(집계함수)를 통한 조건식이 들어감 
- having 조건식

---
## ORDER BY - 정렬
- order by 속성명 asc|desc
  - asc  : 오름차순 (작은 값 -> 큰 값)
    - asc 설정은 기본 값
  - desc : 내림차순 (큰 값 -> 작은 값)
---
## LIMIT
- limit 시작위치, 추출할 개수