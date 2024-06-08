---
layout: single
title: "📘 [Python] Celery에 대해서"
toc: true
toc_sticky: true
toc_label: "목차"
categories: python
excerpt: ""
tag: []
---
# 클래스메소드..?
프로젝트를 진행하다보니, `@classmethod` 를 사용하는 일이 있었다. 

chatgpt api를 사용하면서, 답변을 받아오는데에 필요한 부분들을 `__init__()` 으로 분리하는 과정에서 사용했다. 

# 기존 chatgpt api code

```python
messages = []

class ChatGptApi:
    openai.api_key = CHATGPT_API_KEY # API키 입니다. 외부에 공개하면 안됩니다.

    def get_completion(content): 
        # 사용자 입력을 받습니다.
        # content = input("input content: ")
        print("input message: ", content)

        # 사용자 입력을 messages에 추가해줍니다.
        messages.append({"role": "user", "content": f"{content}"})

        completion = openai.ChatCompletion.create(
            model="gpt-3.5-turbo",	# 모델 지정
            messages=messages,		# 질문 및 답변
            max_tokens=1024,		# 최대 토큰 수 제한
            n=1, 					# 답변 개수
            stop=None, 				# 답변 중단 예약어 설정
            temperature=0.5			# 답변의 창의성 설정
        )

        # gpt로부터 응답을 받습니다.
        # strip() : 응답이 올 때, 빈 공간 제거
        assistance_content = completion.choices[0].message['content'].strip()

        # assistance에 이전 대화 답변을 저장합니다.
        messages.append({"role": "assistant", "content": f"{assistance_content}"})

        print(f'ChatGPT : {assistance_content}')
        return assistance_content
```

# 변경한 chatgpt api code

```python
messages = []

class ChatGptApi:
    openai.api_key = CHATGPT_API_KEY # API키 입니다. 외부에 공개하면 안됩니다.

    def __init__(self):
        self.model = "gpt-3.5-turbo" # 모델 지정
        self.max_tokens = 1024       # 최대 토큰 수 제한
        self.n = 1                   # 답변 개수
        self.stop = None             # 답변 중단 예약어 설정
        self.temperature = 0.5       # 답변의 창의성 설정
        
        self.completion = openai.ChatCompletion.create(
            model = self.model,
            messages = messages,		# 질문 및 답변
            max_tokens = self.max_tokens,
            n = self.n,
            stop = self.stop, 
            temperature = self.temperature
        )

    @classmethod
    def get_completion(cls, content): 
        # 사용자 입력을 받습니다.
        print("input message: ", content)

        # 사용자 입력을 messages에 추가해줍니다.
        messages.append({"role": "user", "content": f"{content}"})

        completion = cls().completion

        # gpt로부터 응답을 받습니다.
        assistance_content = completion.choices[0].message['content'].strip()

        # assistance에 이전 대화 답변을 저장합니다.
        messages.append({"role": "assistant", "content": f"{assistance_content}"})

        print(f'ChatGPT : {assistance_content}')
        return assistance_content
 
```

# 바뀐 부분?

`get_completion()` 을 보면, `@classmethod` 데코레이터와 함께 `cls` 인자와 `cls().completion` 을 볼 수 있다. 

이제 이 것의 의미와 사용 이유에 대해 정리하겠다.

# @classmethod

`@classmethod` 는 클래스로 호출하기 위한 메소드이다.

`cls` 는 class의 준말로 파이썬에서 관례로 사용되는 변수명이고, 이건 `클래스 본인`을 뜻하는 변수다.

보통 class 내부의 method에서 `self` 를 쓰는 경우가 많은데, self는 `인스턴스 본인` 을 뜻한다면, cls는 클래스 자기 자신이다.

## 왜 쓸까?

```python
  def get_completion(cls, content): 
      # 사용자 입력을 받습니다.
      print("input message: ", content)

      # 사용자 입력을 messages에 추가해줍니다.
      messages.append({"role": "user", "content": f"{content}"})

      completion = ChatGptApi().completion

      # gpt로부터 응답을 받습니다.
      assistance_content = completion.choices[0].message['content'].strip()

      # assistance에 이전 대화 답변을 저장합니다.
      messages.append({"role": "assistant", "content": f"{assistance_content}"})

      print(f'ChatGPT : {assistance_content}')
      return assistance_content
```

위 코드는 @classmethod 데코레이터를 사용하지 않은 경우의 코드이다.

물론 정상적으로 동작하는 것은 분명하다.

다만, 클래스 본인의 호출이 필요한 경우에는 `Pythonic(파이썬스러운)` 코드를 위해 데코레이터를 통한 명시가 있으면 좋기 때문에 사용한다. → 맞을까(?)

# @staticmethod

`@staticmethod` 는 `self` 와 `cls` 인자를 필요로 하지 않는 메서드이다.

```python
class Robot:

    ...
    
    # @staticmethod 주석처리
    def 스태틱메서드():
        print('스태틱메서드 호출')
        
        
robot = Robot('다코')

robot.스태틱메서드() # 인스턴스로 호출
Robot.스태틱메서드() # 클래스로 호출

'''
>>>
TypeError: 스태틱메서드() takes 0 positional arguments but 1 was given
'''

```

위 예제를 보면 인스턴스로 스태틱메서드()를 호출했는데 에러가 발생한 것을 볼 수 있다.

인자로 아무 것도 받지 않아야 하는데, 1개의 인자가 들어왔다는 뜻이다.

**여기서 알 수 있는 것은 인스턴스로 메서드를 호출할 때, 인자를 따로 넣지 않아도 자동으로 인자가 들어간다는 뜻이고, 이 인자는 바로 `self` 인 것이다.**

이 문제를 해결하기 위해서는 두 가지 방법이 있다.

1. self 인자를 넣어주는 방법
    
    ```python
    def 스태틱메서드():
            print('스태틱메서드 호출')
    ```
    
    - 이렇게 하면 스태틱메서드는 인스턴스메서드가 되는 것이다. 하지만, 클래스에서 호출을 하게될 경우 인스턴스 인자를 필요로 하게 된다.
2. `@staticmethod` 를 사용하는 방법
    
    ```python
    @staticmethod
    def 스태틱메서드():
        print('스태틱메서드 호출')
    ```
    
    - 이렇게 하면 인자를 받지 않아도 동작하는 클래스 내의 **정적 메서드**를 실행할 수 있다.
    

**그냥 어떤 속성에도 변화를 일으키지 않고, 입력이 들어오면 항상 같은 출력을 반환하는 순수함수** 라고 생각하자.

# 요약

`@classmethod` 와 `@staticmethod` 는 객체의 메서드를 더 직관적으로 설계하고 간편하게 호출하기 위한 데코레이터이다.

앞으로 이 두 녀석을 만나면 **클래스로 호출하기 위한 메서드**, **self, cls 인자가 필요 없는 메서드** 로 이해하면 편하겠다.

# 참고자료

[Python _ @classmethod, @staticmethod 란 무엇인가?](https://daco2020.tistory.com/281)