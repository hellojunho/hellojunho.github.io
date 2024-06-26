---
layout: single
title: "📘[DB] Redis란?"
toc: true
toc_sticky: true
toc_label: "목차"
categories: db
excerpt: ""
tag: [redis]
---

# Redis란?
**Redis**란, `Key:Value` 구조의 비정형 데이터를 저장하고 관리하기 위한 오픈소스 기반의 비관계형 데이터베이스 관리 시스템이다.  
데이터베이스, 캐시, 메시지 브로커로 자주 사용되며 인메모리 데이터 구조를 가진 저장소이다.  
Redis는 `Memcached`와 비슷한 캐시 시스템으로 동일한 기능을 제공하면서 `영속성`과 다양한 데이터 구조와 같은 부가 기능을 지원한다.  

<img width="693" alt="스크린샷 2023-08-30 오후 3 04 10" src="https://github.com/hellojunho/hellojunho.github.io/assets/104587537/8a1823fd-7f4e-4a2a-9aa3-10cfa1f39a67">


## Redis를 사용하는 이유
*이미 데이터베이스가 있는데도 Redis를 사용하는 이유는 무엇일까?*
<br>

데이터베이스는 데이터를 물리 디스크에 직접 쓰기 때문에 서버에 문제가 발생하여 다운되더라도 데이터다 손실되지 않는다는 장점이 있다.  
하지만, 매번 디스크에 접근해야하기 때문에 사용자가 많아질 수록 부하가 늘어 느려질 수 있다.  
<br>

이렇게 사용자가 늘어 데이터 베이스가 과부하가 되는 문제를 해결하기 위해 캐시 서버를 도입했다.  
그리고 이러한 캐시 서버로 이용할 수 있는 것이 바로 **Redis**이다.  

## 캐시 서버(Cache Server)
캐시 서버는 **Look Aside Cache pattern**과 **Write Back pattern** 이 있다.  

### Look Aside Cache
- 클라이언트가 데이터를 요청  
- 웹서버는 데이터가 존재하는지 캐시 서버에 먼저 확인을 하고  
- 캐시 서버에 데이터가 있으면 DB에 데이터 조회를 하지 않고 캐시 서버에 있는 결과값을 클라이언트에게 바로 반환(Cache Hit) -> 속도가 빠름  
- 캐시 서버에 데이터가 없으면 DB에 데이터를 조회하여 캐시 서버에 저장하고 결과값을 클라이언트에게 반환(Cache Miss)  

### Write Back
- 웹서버는 모든 데이터를 캐시 서버에 저장   
- 캐시 서버에 특정 시간동안 데이터가 저장됨  
- 캐시 서버에 있는 데이터를 DB에 저장  
- DB에 저장된 캐시 서버의 데이터를 삭제

## Redis의 특징
**Key:Value** 구조를 사용하기 때문에 **쿼리가 필요 없다.** 
*NoSQL!*
<br>

데이터를 디스크에 쓰는 것이 아닌 메모리에서 데이터를 처리하기 때문에 속도도 빠르고!  
String, Lists, Sets, Sorted Sets, Hases 자료 구조를 지원한다.  
    - String: 가장 일반적인 Key, Value 구조의 형태  
    - Sets: String의 집합으로, 여러 개의 값을 하나의 Value에 담을 수 있다. 포스트의 `태깅`같은 곳에 사용할 수 있음.  
    - Sorted Sets: 중복된 데이터를 담지 않는 Set 구조에 정렬 알고리즘을 적용하여, 랭킹보드 같은 정렬이 필요한 서버 구현에 사용할 수 있다.  
    - Lists: Array 형식의 데이터 구조이다. 처음과 끝에 데이터를 삽입/삭제는 빠르지만, 중간에 데이터를 삽입/삭제 하는 것은 어렵다.  
그리고 `Single Threaded`이다!  
    - 한 번에 하나의 명령만 처리할 수 있다는 의미이다. 그렇기 때문에 중간에 처리 시간이 긴 명령어가 들어오면 앞에 있는 명령어 처리를 기다려야한다.  
<br>

*즉, 고성능 키-값 저장소로써 문자열, 리스트, 해시, 셋, 정렬 셋 형식의 데이터를 지원하는 NoSQL이다!*  

## Redis의 영속성
Redis는 영속성을 보장하기 위해 데이터를 디스크에 저장할 수 있다.  
서버가 다운되더라도 디스크에 저장된 데이터를 읽어서 메모리에 로딩한다.  
이 때 데이터를 디스크에 저장하는 방식은 크게 두 가지 방식이 있다.  
<br>

### RDB(ShapShottion) 방식
순간적으로 메모리에 있는 내용을 디스크에 전체를 옮겨담는 방식  
즉, 특정한 간격마다 메모리에 있는 redis 데이터 전체를 디스크에 쓰는 것이다. (백업 용이)  
<br>

Redis는 인메모리 데이터를 주기적으로 파일에 저장하는데, Redis 프로세스가 장애로 인해 종료되더라도 해당 파일을 읽어들이면 이전의 상태를 동일하게 복구할 수 있다.  

우리가 직접 세팅하지 않더라도 Redis는 자동으로 `.rdb`라는 확장자 파일에 인메모리 데이터를 저장하도록 세팅되어있다.  

RDB 방식은 일명 **스냅샷을 뜬다** 라고 하는데, 이 말의 의미는 특정 시점의 메모리에 있는 데이터를 바이너리 파일로 저장한다는 뜻이다. (바이너리 파일을 직접 읽는 것은 불가능...)  
그렇기 때문에 특정 시점으로 데이터를 복구하는 것이 가능하며, redis 데이터의 versioning 또한 가능하다.  

*.rdb 파일은 AOF 파일보다 사이즈가 작아 로딩 속도가 상대적으로 빠르다는 특징이 있다.*  
*.rdb 파일은 RDBMS 라는 의미가 아닌 스냅샷 파일을 의미한다.*  


### AOF(Append On File)
Redis의 모든 write/update 연산 자체를 모두 log파일에 기록하는 형태이다.  
즉, 명령이 실행될 때 마다 데이터를 파일에 기록하여 데이터의 손실이 거의 없다.  
<br>

default로 `appendonly.aof` 파일에 기록되며, 조회를 제외한 insert/update/delete 명령어가 실행될 때 마다 기록된다.  
서버가 재시작되면, log에 기록된 write/update 연산을 재실행하는 형태로 데이터를 복구하는 방식이다.  

다음과 같은 순서로 데이터를 저장한다.  
1. 클라이언트가 Redis에 업데이트 관련 명령을 요청한다.  
2. Redis는 해당 명령을 AOF에 저장한다.  
3. 파일쓰기가 완료되면 실제로 해당 명령을 수행해서 Redis 메모리에 내용을 변경한다.  
<br>

이처럼, 연산이 발생할 때 마다 매번 기록하기 때문에 특정 시점이 아니라 **현재시점까지의 log**를 기록할 수 있으며, 기본적으로 `non-blocking`으로 동작한다.  
<br>

*AOF는 log파일에 대해서만 append하기 때문에 log write 속도가 빠르며, 서버가 다운되더라도 데이터가 사라지지 않는다.*  
*RDB는 바이너리 파일이라서 수정이 불가능 했지만, AOF log파일이 text파일이므로 편집이 가능하다.*  



## Redis 사용 주의점
- 서버에 장애가 발생했을 경우, 그에 대한 운영 플랜이 필수적이다. 인메모리 데이터 저장소의 특성상, 서버에 장애가 발생했을 경우 데이터 유실이 발생할 수 있기 때문이다.  
- 메모리 관리가 중요하다.  
- 싱글 스레드의 특성 상, 한 번에 하나의 명령만 처리할 수 있어 처리시간이 오래 걸리는 요청, 명령은 피하는 것이 좋다!  

---

# Redis 설치하고 실행하기
## Redis 설치(install)
Redis는 [공식 홈페이지](https://redis.io/download/)에서 쉽게 설치할 수 있다.  
<br>

다른 방법으로는 Redis CLI(Command Line Interface)를 사용하는 것이다.  
먼저, 공식 홈페이지에서 Redis 소스코드를 다운로드하고, 터미널에서 다운로드한 Redis 소스코드를 압축 해제한다.  
<br>

**압축해제**  
> $ tar xvzf redis-6.2.1.tar.gz

**압축해제 후 폴더 이동**  
> $ cd redis-6.2.1

**Redis 빌드**
> $ make

**Redis 실행**  
> $ src/redis-server

**Redis CLI 실행**  
> $ src/redis-cli

### HomeBrew로 설치하기
**homebrew 업데이트**  
> $ brew update

**Redis 설치**  
> $ brew install redis

**Redis 실행**  
> $ redis-server

**Redis CLI 실행**  
> $ redis-cli

*Redis는 데이터를 메모리에 저장한다고 했다! 그래서 메모리 용량이 중요한 것은 당연하다!*  
*Redis 서버를 실행하고나면, Redis 클라이언트를 사용해 Redis 데이터베이스에 데이터를 저장하고 조회할 수 있다!*  

## Redis 기본 명령어
|명령어| 설명                                                                                                                                                                                                      |
|---|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|SET| Key-Value 형태의 데이터를 저장한다.                                                                                                                                                                                |
|GET| Key에 해당하는 Value를 가져온다.                                                                                                                                                                                  |
|DEL| Key에 해당하는 데이터를 삭제한다.                                                                                                                                                                                    |
|INCR| Key에 해당하는 Value를 1증가시킨다.                                                                                                                                                                                |
|DECR| Key에 해당하는 Value를 1감소시킨다.                                                                                                                                                                                |
|HSET| Hash 형태의 데이터를 저정한다. `key, field, value의 3가지 인자`를 받아 저장. 예) HSET user id 1234: `user`라는 hash에 `id`라는 field와 `1234`라는 value를 저장                                                                           |
|HGET| `key, field의 2가지 인자`를 받아 데이터를 조회한다. 예) HGET user id: `user`라는 hash에서 `id`라는 field에 해당하는 value를 조회                                                                                                       |
|HMSET| hash 형태의 데이터를 여러 개 저장합니다. key, field1, value1, field2, value2…와 같은 형태로 인자를 받아 여러 개의 데이터를 저장한다. 예) HMSET user id 1234 name John age 25: `user`라는 hash에 `id, name, age`라는 field와 각각에 해당하는 `value`를 저장합니다. |
|HGETEALL| key 1개 인자만 받아 해당하는 hash의 모든 field와 value를 조회한다. 예) HGETALL user: `user`라는 hash에 저장된 모든 데이터를 조회합니다.                                                                                                      |




# 참고자료
[Redis란? 레디스의 기본적인 개념 (인메모리 데이터 구조 저장소)](https://wildeveloperetrain.tistory.com/21)
<br>

[Redis란 무엇일까? 간단하게 알아보기!](https://devlog-wjdrbs96.tistory.com/374)
<br>

[Redis 시작하는 방법](https://umanking.github.io/2023/03/05/redis-starter/)  

[[REDIS] 📚 캐시 데이터 영구 저장하는 방법 (RDB / AOF)](https://inpa.tistory.com/entry/REDIS-%F0%9F%93%9A-%EB%8D%B0%EC%9D%B4%ED%84%B0-%EC%98%81%EA%B5%AC-%EC%A0%80%EC%9E%A5%ED%95%98%EB%8A%94-%EB%B0%A9%EB%B2%95-%EB%8D%B0%EC%9D%B4%ED%84%B0%EC%9D%98-%EC%98%81%EC%86%8D%EC%84%B1)