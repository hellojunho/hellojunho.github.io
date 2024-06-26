---
layout: single
title: "📘[DB] PK에 Integer말고 UUID?"
toc: true
toc_sticky: true
toc_label: "목차"
categories: db
excerpt: ""
tag: [uuid]
---

# UUID를 갑자기 왜 쓸까?

우선, 지금까지의 경험을 전부 떠올려봐도 PK에 Integer가 아닌 다른 타입을 사용한 적은 없었다.

이번 프로젝트를 진행하면서 UUID를 사용하자는 의견이 나왔고 이를 적용하려던 찰나, UUID가 뭐지 싶어서 찾아보게됨..

Integer형 PK는 항상 `auto increment` 옵션을 추가해 1, 2, 3, 4, … 와 같이 순차적으로 생성하여 사용할 수 있었다.

만약 A라는 사용자가 게시물을 올린다고 가정하자..! → PK = 1로 올라갔다고 생각하자.

이 때 B라는 나쁜놈이 나타나 자기 글을 수정할 때 API 요청 시 PK의 값을 1로 바꾼다면..? 

A가 올렸던 글이 B에 의해 수정되어버리는 사고가 발생하게된다.

그래서 비즈니스 로직에서 `해당 게시물을 작성한 소유자`와 `수정하려는 수정자` 가 일치하는지 확인할 필요가 있다.

그런데 이 비즈니스 로직이 복잡해진다면..?

그냥 게시물의 소유자를 확인하자고 복잡한 코드를 작성하는건 조금 귀찮을듯..

그래서 등장한게 `UUID` 임!!

UUID는 랜덤 값이므로, Increment를 해줄 필요도 없다.

## UUID의 구조

![image](https://github.com/hellojunho/hellojunho.github.io/assets/104587537/55a54305-cd38-4cbf-a801-58fe9be0caf3)


위처럼 5개의 필드로 구성되기 때문에 기존 increment를 사용해 생성하는 PK에 비해서 악의적인 PK 추적이 불가능하다.

당연히 단점도 존재하겠지..?

## UUID의 단점

### 1. 가독성 떨어짐

일단 당연히 읽기는 불편하다..

5개의 필드값에 랜덤 값이니, 한눈에 알아보기는 엄청 어렵다.

### 2. 정렬 어려움

테이블 정렬이 필요할 때, UUID를 사용해 테이블을 정렬하는건 어렵다.

때문에 시간필드 혹은 다른 고유 값을 갖는 필드를 기준으로 정렬을 하는 것이 좋다.

### 3. 용량의 거대함

Integer형 PK에 비해 길이가 압도적으로 길다.

PK에만 너무 큰 크기의 값이 할당되고, 해당 테이블과 연관된 테이블에서 FK를 쓰게된다면 또 그만큼의 용량이 추가적으로 사용되는 것이다.

# 참고 자료

[UUID vs Auto Increment 중 PK 선택하기](https://stir.tistory.com/294)