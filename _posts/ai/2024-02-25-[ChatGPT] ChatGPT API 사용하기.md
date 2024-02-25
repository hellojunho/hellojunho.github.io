---
layout: single
title: "📘[ChatGPT] ChatGPT API 사용하기 - Python"
toc: true
toc_sticky: true
toc_label: "목차"
categories: ai
excerpt:
tag: [chatgpt, api]
---

# Chat GPT API

# Chat GPT API

일반적인 `api` 는 `key` 를 발급받아야 사용할 수 있다. 

https://platform.openai.com/docs/overview 에서 `key` 발급 가능함.

*key를 발급받으면 복사할 수 있는 모달이 나오는데, 반드시 복사하고 어딘가에 저장해놔야 함. 그러지 않고 모달을 끄면 다시는 key를 확인할 수 없음* 

## API 사용하기 (Python 기준)

먼저 openai 라이브러리를 설치해야 api를 불러와 사용할 수 있다. 

물론 가상환경에서 진행하는 편이 좋을 것 같다.

### 가상환경 실행 방법

### 가상환경 실행하기[macOs]

```python
python -m venv venv
source venv/bin/activate
```

### 가상환경 실행하기[windows]

```python
python -m venv venv
cd venv/Scripts
./activate
```

```python
pip install openai
```

### 예시 코드 (OpenAI 문서에서 가져온 코드)

```python
import openai

openai.api_key = "YOUR_API_KEY"

response = openai.ChatCompletion.create(
  model="gpt-3.5-turbo",
  messages=[
        {"role": "system", "content": "You are a helpful assistant."},
        {"role": "user", "content": "Who won the world series in 2020?"},
        {"role": "assistant", "content": "The Los Angeles Dodgers won the World Series in 2020."},
        {"role": "user", "content": "Where was it played?"}
    ]
)

output_text = response["choices"][0]["message"]["content"]
print(output_text)
```

- `openai.api_key` : 여기에 아까 발급받은 api key를 붙여넣어준다.
- `response`
    - model : 사용할 수 있는 모델은 많지만, 3.5 터보가 가장 자주 사용되는 모델
    - messages : `role` 부분에 보면 `system`, `user`, `assistance` 가 있다.
        - 시스템 : gpt에게 어떻게 행동을 할지 지정해놓는 기능 (gpt의 act as ___ 와 유사한 명령)
        - 사용자 : 사용자 질문, 이전 대화를 저장하고 연속성 유지 → 이어지는 답변에 영향을 줄 수 있음
        - 보조자 : 질문을 하기보단, 이 전 대화의 내용을 `저장` 하고, 연속성을 유지하기 위해 사용되는 옵션 → 이어지는 답변에 영향을 줄 수 있음

## 중요한 입력 값

- temperature
    - 0 ~ 무한대의 값으로 이루어짐, 기본 값은 `0.7`
    - 값이 높을 수록  모델이 생성하는 문장이 다양해짐 → 다양한 선택지를 통한 답변
    - 값이 낮을 수록 일관성 있는 문장이 생성됨 → 가능한 선택지 중, 가장 확률이 높은 답변
    - 일반적으로 0.5 ~ 1.0 사이의 값을 주로 사용함
    - 정보가 중요한 경우 : 낮은 값
    - 창의성이 중요한 경우 : 높은 값
- max_tokens
    - 생성되는 텍스트의 최대 길이 지정
    - 기본 값은 `256`
    - 최대 값은 `2048`

## 내가 사용할 코드

```python
import openai

 # API KEY 를 넣어주세요.
openai.api_key = ""

# 사용자 입력을 받을 message 리스트를 생성합니다.
messages = []

while True:
  # 사용자 입력을 받습니다.
  content = input("input content : ") 
  print("input content : ", content)

  # 사용자 입력을 messages에 추가해줍니다.
  messages.append({"role": "user", "content": f"{content}"})

  completion = openai.ChatCompletion .create(
    model="gpt-3.5-turbo",
    messages=messages
  )

  # gpt로부터 응답을 받습니다.
  # strip() : 응답이 올 때, 빈 공간 제거
  assistance_content = completion.choices[0].message['content'].strip()

  # assistance에 이전 대화 답변을 저장합니다.
  messages.append({"role": "assistance", "content": f"{assistance_content}"})

  print(f'ChatGPT : {assistance_content}')
```  
- messages : 사용자 입력과 응답에 관한 리스트
- completion :  사용할 모델을 정하고, `messages` 할당해서 gpt에게 전송
- assistance_content : `completion.choices[0].message['content']`로 받아온 응답을 저장. 
    - strip() : 보기 편하게 하려고 사용.  
    - 응답이 올 때, 애매한 공백이 생겨서 지우기 위해 사용함.  
    - 이전 사용자의 질문의 답변을 저장하면, gpt가 이걸 기억해 다음 질문에 영향을 줄 수 있음
- messages.append({"role": "assistance", "content": f {assistance_content}"}) : `assiatance_content`를 추가함 -> 방금 받은 답변을 다음 질문에 유효하게 하기 위해서

# 참고자료
[OpenAI API 공식 문서](https://platform.openai.com/docs/guides/text-generation/chat-completions-api)  

[챗GPT API 살펴보기](https://aifactory.space/task/2289/discussion/186)  