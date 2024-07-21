---
layout: single
title: "📘[FastAPI] FastAPI와 ChatGPT를 사용한 PDF 관련 질의 응답"
toc: true
toc_sticky: true
toc_label: "목차"
categories: fastapi
excerpt: ""
tag: [python, llm, fastapi]
---

## main.py

```python
from fastapi import FastAPI
from fastapi.staticfiles import StaticFiles
from routers.upload import router as upload_router
from routers.read import router as read_router
from routers.index import router as index_router

from dotenv import load_dotenv
load_dotenv()

app = FastAPI()
app.mount("/static", StaticFiles(directory="static"), name="static")

app.include_router(index_router, prefix="")
app.include_router(upload_router, prefix="/upload")
app.include_router(read_router, prefix="/read")
```

- `app.mount("/static", StaticFiles(directory="static"), name="static")`: 정적 파일 서빙을 위해, static 폴더를 명시해준다.
- `app.include_router(index_router, prefix="")`: `include_router` 를 사용해 main.py에서 모든 로직을 관리하지 않고, 분리해서 관리할 수 있도록 한다.
    - Flask에서 `BluePrint`와 비슷한 역할이라고 생각할 수 있을 듯 함!
    - `prefix=""` 옵션을 설정해, 엔드 포인트를 설정해준다!
        - django의 `urls.py` 와 비슷한 역할

## routers/upload.py

```python
from fastapi import File, UploadFile, Form, APIRouter
from fastapi.responses import JSONResponse
from utils.load_pdf import load_pdf
from utils.save_answer import save_pdf_answer, save_none_pdf_answer
from utils.gpt_config import get_answer
from dotenv import load_dotenv
from utils.save_pdf import save_pdf
from datetime import datetime

load_dotenv()

router = APIRouter()
now = datetime.now().strftime('%Y-%m-%d %H:%M:%S')

@router.post("")
async def upload_file(user_id: str = Form(...), pdf: UploadFile = File(None), question: str = Form(...)):
    pdf_path = await save_pdf(pdf, user_id, now)
    context = await load_pdf(pdf_path, question)
    prompt = f"Context: {context}\n\nQuestion: {question}\n\nAnswer:"
    answer = get_answer(prompt)
    save_pdf_answer(user_id, pdf_path, answer, pdf, now)

    return JSONResponse(content={"answer": answer}) 

@router.post("/nonepdf")
async def upload_file(user_id: str = Form(...), pdf: UploadFile = File(None), question: str = Form(...)):
    prompt = f"Question: {question}\n\nAnswer:"
    answer = get_answer(prompt)
    save_none_pdf_answer(user_id, answer, pdf, now)

    return JSONResponse(content={"answer": answer})
```

위와 같이 파일을 작성하면, pdf를 첨부한 상태에서의 질문과 pdf를 첨부하지 않은 상태에서의 질문을 처리할 수 있다!

- pdf를 첨부한 상태의 엔드 포인트: ~/chatbot
- pdf를 첨부하지 않은 상태의 엔드 포인트: ~/chatbot/nonepdf

여기서 `async` 와 `await` 는 django, flask에서 사용해본 경험이 없는 예약어라서 아래에 정리해본다!

최신 파이썬 버전은 `async` 와 `await` 문법과 함께 `코루틴` 이라는 것을 사용하는 `비동기 프로그래밍`를 지원한다고 한다!

### 비동기 프로그래밍

`비동기 프로그래밍` 이란, 특정 코드의 실행 완료를 기다리지 않고 다음 코드를 실행하는 프로그래밍 패러다임이다.

I/O 작업, 네트워크 요청 등의 블로킹 연산에서 특히 유용하며, `asyncio` 라이브러리를 통해 비동기 프로그래밍을 사용할 수 있다!

### async def & await

`async def` : 비동기 함수를 선언하는 예약어임. 이렇게 선언된 함수는 `코루틴` 이라는 것을 반환한다.

`await` : 코루틴의 실행을 일시 중지하고, 완료될 때까지 기다리는 예약어임. `await`는 `async` 함수 내에서만 사용 가능함!

### 코루틴

그럼 코루틴이 뭘까?

예를 들어, 한 번에 여러 개의 주문이 동시에 들어왔는데 요리사는 한 명이다.

그럼 요리사는 여러 요리를 동시에 만들어야 하는데, 이런 방식을 병렬적으로 처리한다고 말한다.

이런 상황에서 요리사는 `코루틴`과 같이 동작하는 것이라고 생각하면 된다!

- 여러 작업을 번갈아 수행: 하나의 작업이 끝나기를 기다리는 동안, 다른 작업을 수행할 수 있음.
- 작업의 중단과 재개: 진행 중이던 작업에서 다른 작업으로 전환할 때, 현재 상태를 기억해야 한다. 이 처럼 코루틴은 중단된 지점을 기억하고, 나중에 그 지점부터 작업을 재개한다.
- 효율적인 작업 관리: 여러 작업을 병렬적으로 처리함으로써 시간을 절약할 수 있다.

파이썬은 `async` 와 `await` 를 사용해 코루틴을 만들고, `async`로 정의된 함수는 **’할 일 목록’**, `await` 는 **’이 할 일이 끝날 때까지 기다리는 것’** 이라고 생각할 수 있다!

**즉, 코루틴은 여러 일을 동시에 하면서, 각각의 일이 서로 방해받지 않도록 하는 방법이다!**

## utils 폴더

나는 controller에서 사용되는 로직이 길고 복잡한 경우, `utils` 파일을 생성하여 메서드로 관리하는 것을 좋아한다.

AOP 방식이라고 할 수 있을지는 모르겠지만, 우선 자주 사용되는 로직 혹은 프로젝트 전체에 걸쳐 사용되는 로직, 뎁스(Depth)가 깊어 가독성을 저해하는 코드 등을 따로 모듈화하여 관리하는 방식이다.

먼저 ChatGPT API를 사용하여 모델을 선정하는 등의 설정을 정의하는 `git_config.py`, pdf를 load하여 읽는 `load_pdf.py`, 첨부된 pdf를 지정된 경로에 저장하는 `save_pdf.py`, ChatGPT의 답변을 지정된 경로에 저장하는 `save_answer.py` 가 있다.

### gpt_config.py

```python
import os
from openai import OpenAI
from dotenv import load_dotenv

load_dotenv()

OPENAI_KEY = os.getenv("OPENAI_KEY")
MODEL = "gpt-3.5-turbo"

client = OpenAI(
    api_key = OPENAI_KEY
)

def get_answer(prompt: str):
    response = client.chat.completions.create(
            model=MODEL,
            messages=[
                {"role": "system", "content": "You are a helpful assistant."},
                {"role": "user", "content": prompt}
            ]
        )
    
    return response.choices[0].message.content
```

### load_pdf.py

```python
from langchain_community.document_loaders import PyPDFLoader
from langchain_openai import OpenAIEmbeddings
from langchain_community.vectorstores import FAISS
from utils.gpt_config import OPENAI_KEY

async def load_pdf(pdf_path: str, question: str) -> str:
    loader = PyPDFLoader(pdf_path)
    pages = loader.load_and_split()
    embeddings = OpenAIEmbeddings(openai_api_key=OPENAI_KEY)
    faiss_index = FAISS.from_documents(pages, embeddings)
    docs = faiss_index.similarity_search(question, k=4)
    context = "\n\n".join([doc.page_content for doc in docs])

    return context
```

### save_answer.py

```python
import os
from fastapi import File, UploadFile

def save_pdf_answer(user_id: str, pdf_path: str, answer: str, pdf: UploadFile = File(None), time: str = None) -> bool:
    try:
        answer_filename = f"answers/pdfs/{os.path.basename(pdf_path) if pdf else 'no_pdf'}_{time}.txt"
        os.makedirs(os.path.dirname(answer_filename), exist_ok=True)
        with open(answer_filename, "w") as f:
            f.write(answer)
        return True
    except Exception as e:
        print(e)   
        return False
    
    
def save_none_pdf_answer(user_id: str, answer: str, pdf: UploadFile = File(None), time: str = None) -> bool:
    try:
        answer_filename = f"answers/nonepdfs/{user_id}_none_pdf_{time}.txt"
        os.makedirs(os.path.dirname(answer_filename), exist_ok=True)
        with open(answer_filename, "w") as f:
            f.write(answer)
        return True
    except Exception as e:
        print(e)   
        return False
```

### save_pdf.py

```python
from fastapi import UploadFile

async def save_pdf(pdf: UploadFile, user_id: str, time: str) -> str:
    try:
        pdf_path = f"pdfs/{user_id}_{pdf.filename}_{time}"
        with open(pdf_path, "wb") as buffer:
            buffer.write(await pdf.read())
        return pdf_path
    except Exception as e:
        print(e)
        return None
```

## static 폴더

static 폴더에서는 html, css, javascript 파일을 관리한다.

### index.html

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PDF Q&A System</title>
    <link rel="stylesheet" href="/static/styles.css">
</head>
<body>
    <div class="container">
        <h1>PDF Q&A System</h1>
        <form id="uploadForm">
            <input type="text" id="userId" placeholder="User ID" required>
            <input type="file" id="pdfFile" accept=".pdf">
            <textarea id="question" placeholder="Enter your question" required></textarea>
            <button type="submit">Submit</button>
        </form>
        <div id="answer"></div>
    </div>
    <script src="/static/script.js"></script>
</body>
</html>
```

### script.js

```jsx
// seperate situation of pdf and none pdf
document.getElementById('uploadForm').addEventListener('submit', async (e) => {
    e.preventDefault();
    
    const userId = document.getElementById('userId').value;
    const pdfFile = document.getElementById('pdfFile').files[0];
    const question = document.getElementById('question').value;

    const formData = new FormData();
    formData.append('user_id', userId);
    formData.append('question', question);
    if (pdfFile) {
        formData.append('pdf', pdfFile);
        try {
            const response = await fetch('/upload/', {
                method: 'POST',
                body: formData
            });
    
            const data = await response.json();
            document.getElementById('answer').innerHTML = `<h2>Answer:</h2><p>${data.answer}</p>`;
        } catch (error) {
            console.error('Error:', error);
            document.getElementById('answer').innerHTML = '<p>An error occurred. Please try again.</p>';
        }
    }
    else {
        try{
        const response = await fetch('/upload/nonepdf/', {
            method: 'POST',
            body: formData
        });
        
            const data = await response.json();
            document.getElementById('answer').innerHTML = `<h2>Answer:</h2><p>${data.answer}</p>`;
        } catch (error) {
            console.error('Error:', error);
            document.getElementById('answer').innerHTML = '<p>An error occurred. Please try again.</p>';
        }
    }
});
```

- pdf를 첨부했는지 여부에 따라 엔드포인트를 다르게 설정하여 POST 요청을보낸다!

### styles.css

```css
body {
    font-family: Arial, sans-serif;
    line-height: 1.6;
    margin: 0;
    padding: 0;
    background-color: #f4f4f4;
}

.container {
    width: 80%;
    margin: auto;
    overflow: hidden;
    padding: 20px;
}

form {
    background: #fff;
    padding: 20px;
    margin-bottom: 20px;
}

input[type="text"], input[type="file"], textarea {
    width: 100%;
    padding: 10px;
    margin-bottom: 10px;
}

button {
    display: block;
    width: 100%;
    padding: 10px;
    background: #333;
    color: #fff;
    border: none;
    cursor: pointer;
}

button:hover {
    background: #555;
}

#answer {
    background: #fff;
    padding: 20px;
    margin-top: 20px;
}
```

# 참고 자료

[동시성과 async / await - FastAPI](https://fastapi.tiangolo.com/ko/async/)

[파이썬 비동기(async)함수와 코루틴(coroutine) 흐름 이해하기](https://teddylee777.github.io/python/python-async/)