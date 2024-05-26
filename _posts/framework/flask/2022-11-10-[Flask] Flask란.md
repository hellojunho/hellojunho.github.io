---
layout: single
title: "📘[Flask] Flask에 대해서..."
toc: true
toc_sticky: true
toc_label: "목차"
categories: flask
excerpt:
tag: [python, framework]
---

# Flask에 대해서...
## 1. Flask란?
`플라스크(Flask)`란, 파이썬 기반으로 동작하는 `웹 프레임워크`이다.
다른 파이썬 기반의 웹 프레임워크인 `Django`에 비해 구현되어 있는 기능이 부족하다는 단점이 있는 반면, 부족한 기능들을 직접 구현하여 쓸 수 있다는 면에서 가볍다는 장점이 있다. 
특히 API 서버를 만들기에 매우 편리한 프레임워크로 알려져있다.

[파이썬 웹 프레임워크 종류]
- Django
- Flask
- FastAPI
- WebPy  
...  

### 1-1. Flask의 장점
- `가볍게` 배울 수 있다!
    - Python, HTML + CSS + JS만 할 줄 알면 금방 배운다!  
- `가볍게` 사용할 수 있다!'
    - 코드 몇 줄이면 금방 만든다!  
- `가볍게` 배포할 수 있다!
    - `virtualenv`에 Flask깔고 바로 배포하면 된다!  
- 기능들을 직접 구현하여 사용할 수 있어 `가볍다`
    - Django와 달리 제공 기능이 적지만, 단점을 장점으로 승화시켜 기능을 필요한 만큼 구현하여 애플리케이션을 가볍게 만들 수 있다!  

### 1-2. Flask의 단점
- `Django`에 비해 자유도는 높으나 제공하는 기능이 적음
- 복잡한 애플리케이션을 만들기 위해서 해야할 작업들이 많음  

### 1-3. Flask와 Django중 선택
가벼운 애플리케이션은 Flask, 무거운 애플리케이션은 Django를 사용하는 것이 옳다? ⇨ 어느정도 맞는 말이지만, 자신이 자신 있는 프레임워크를 선택하는 것이 중요하다!


--- 

## 2. 라우팅
서버에 접속하는 주소는 다양한데, 서버와 주소간의 관계를 명확하게 해주는게 필요함
- 예시
>   
    1. http://localhost: 5000/   : 홈페이지로 가는 주소  
    2. http://localhost: 5000/read/1/ : id=1인 글을 읽는 주소  
    3. http://localhost: 5000/create/ : 생성하는 주소  
    4. http://localhost: 5000/upadate/1 : id=1을 갱신하는 주소  
- 어떤 주소가 어떤 역할을 담당하고, 어떤 요청을 어떤 함수가 처리할 것인지를 연결하는 등의 작업을 '라우팅(routing)'이라고 함. 
    - 이를 할 수 있도록 하는 장치가 '라우터'
- 예시


```
    # '/'로 접속했을 때, index()가 호출되어 'Index Page'가 응답함
    @app.route('/')
    def index():
        return 'Index Page'  
    # '/hello'로 접속했을 때 hello()가 호출되어 'Hello World'가 응답함
    @app.route('/hello')
    def hello():
        return 'Hello World'
```

## 3. 챗봇/데이터분석 - 메서드 혹은 이론 및 개념
진행중인 프로젝트에서 챗봇 모델(구글 BERT)을 가져와 사용하는데, 이 챗봇 엔진 서버를 flask로 구현한다.  
그래서 간략하게나마 적어본다.  

1. Cosine_Similarity  
코사인 유사도는 두 벡터 간의 코사인 각도를 이용하여 구할 수 있는 두 벡터의 유사도를 의미합니다.   
두 벡터의 방향이 완전히 동일한 경우에는 1의 값을 가지며, 90°의 각을 이루면 0, 180°로 반대의 방향을 가지면 -1의 값을 갖게 됩니다.  
즉, 결국 코사인 유사도는 -1 이상 1 이하의 값을 가지며 값이 1에 가까울수록 유사도가 높다고 판단할 수 있습니다.  
이를 직관적으로 이해하면 두 벡터가 가리키는 방향이 얼마나 유사한가를 의미합니다.  
<img width="520" alt="스크린샷 2023-02-03 오후 2 41 32" src="https://user-images.githubusercontent.com/104587537/216521836-9badbf9b-beed-4458-a0e0-a10bc9c83072.png">
<br>

2. squeeze()  
squeeze함수는 차원이 1인 차원을 제거해준다.  
따로 차원을 설정하지 않으면 1인 차원을 모두 제거한다.  
그리고 차원을 설정해주면 그 차원만 제거한다.  

[예제]  

```python
import torch

x = torch.rand(1, 1, 20, 128)
x = x.squeeze() # [1, 1, 20, 128] -> [20, 128]

# 1번 위치의 차원 1을 삭제
x2 = torch.rand(1, 1, 20, 128)
x2 = x2.squeeze(dim=1) # [1, 1, 20, 128] -> [1, 20, 128]
```  

3. idxmin() & idxmax()  
최대값이나 최소값을 갖는 인덱스를 불러오는 메서드
- idxmin(): 최소값을 갖는 인덱스 레이블 출력
- idxmax(): 최대값을 갖는 인덱스 레이블 출력

## 5. 오류
*개발 혹은 공부하면서 발생하는 오류와 해결법 정리*  

1. 모듈 에러  

```
ModuleNotFoundError: No module named 'flask'
```  

- 원인
  - 현재 플라스크가 설치되어있지 않음.
- 해결방법
  - 터미널에 `pip install flask` 입력


