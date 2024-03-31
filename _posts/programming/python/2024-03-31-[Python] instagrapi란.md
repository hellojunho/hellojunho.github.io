---
layout: single
title: "📘[Python] instagrapi란?"
toc: true
toc_sticky: true
toc_label: "목차"
categories: python
excerpt: ""
tag: [python, instagram]
---

# instagrapi?
현재 인스타그램에서 제공하는 api는 [Instagram 그래프 API](https://developers.facebook.com/docs/instagram-api)와 [Instagram 기본 디스플레이 API](https://developers.facebook.com/docs/instagram-basic-display-api)가 존재한다.  

이 중, `일반 사용자`가 사용가능한 API와 Meta에 앱과 계정을 검수 및 인증받은 `비즈니스 및 크리에이터 계정` API로 나뉘어져있다.  

일반 사용자가 사용 가능한 API는  `Instagram 기본 디스플레이 API`이고, 비즈니스 및 크리에이어 계정만 사용 가능한 API는 `Instagram 그래프 API`이다.  

Instagram 기본 디스플레이 API로는 게시물의 정보와 사용자 정보를 `읽어오기만` 가능하다.  
즉, Instagram 그래프 API만 사용자의 계정에 게시물을 업로드할 수 있는 것이다.  

*Instagram 그래프 API = Instagram 기본 디스플레이 API + 게시물 업로드 기능 정도로 알고있으면 될듯*  
<br>

이런 불편함을 해소하고자 Python을 사용하는 세계 여러 개발자들이 인스타그램에 게시물을 업로드 및 다운로드와 같은 각종 기능을 구현해놓은 라이브러리들이 존재한다.  
그 중 하나가 바로 `instagrapi`이다.  

내가 사용해본 라이브러리는 다음과 같다.  
- instagrapi
- instabot

그 중, instagrapi가 더 다양한 기능을 제공하고, 조금 더 사용하기 쉬웠어서 instagrapi에 대해 작성한다.  
[instagrapi 공식 docs](https://subzeroid.github.io/instagrapi/)  
*instagrapi는 인스타그램에서 공식적으로 지원하는 라이브러리가 아닌 비공식 라이브러리이니 주의하자. (버전 호환에 문제가 있을 수도..)*

## instagrapi 설치

```
pip install instagrapi
```  
위 명령어로 먼저 instagrapi를 설치한다.  

그리고, 아래와 같이 모듈을 import 해주기만 하면된다.  

```
from instagrapi import Client

client = Client()
```

그러면 사용할 준비는 끝났다. **개꿀**  

## 계정 로그인
```
from instagrapi import Client

client = Client()

client.login(username, password)
```  
- username: 인스타그램에서 사용하는 사용자 아이디 - @hellojunho 에서 @를 제외한 부분  
- password: 인스타그램 계정의 비밀번호  

## 계정 로그아웃
```
from instagrapi import Client

client = Client()

client.logout()
```

## 게시물 업로드
게시물에도 여러 경우가 존재한다.  
이미지 1장, 이미지 2장 이상, 비디오 1장, 비디오 2장 이상, 이미지 1장 + 비디오 1장 등...  

각 경우에 대해 예제 코드를 작성했다.  

### 이미지 1장 업로드
```
from instagrapi import Client

client = Client()

client.login(username, password)

media_path = "/Users/hellojunho/instagrapi_test/image/media1.jpeg"
caption = "hellojunho의 이미지 업로드 예제1"

client.photo_upload(media_path, caption)
```  
이미지가 1장인 경우에는 `photo_upload(media_path, caption)`을 사용해주면 된다.  

### 이미지 2장 이상 업로드
```
from instagrapi import Client

client = Client()

client.login(username, password)

media_path = ["/Users/hellojunho/instagrapi_test/image/media1.jpeg", ""/Users/hellojunho/instagrapi_test/image/media2.jpeg", ...]
caption = "hellojunho의 이미지 업로드 예제2"

client.album_upload(media_path, caption)
```  
이미지가 2장 이상인 경우에는 `album_upload(media_path, caption)`을 사용하면 된다.  
여기서 주의할 점은 `media_path`가 **list** 로 바뀌었다는 점이다.  

### 비디오 1개 업로드
```
from instagrapi import Client

client = Client()

client.login(username, password)

media_path = "/Users/hellojunho/instagrapi_test/video/media3.mp4"
caption = "hellojunho의 이미지 업로드 예제3"

client.video_upload(media_path, caption)
```  
비디오의 경우에는 이미지 1장과 비슷하게 `media_path`가 **str**이고, `video_upload(media_path, caption)`을 사용하면 된다.  

> 만약 비디오 업로드에 문제가 생겼다면 MacOS의 경우에는 `brew install ffmpeg`을 해보기... 

### 비디오 2개 이상 업로드
```
from instagrapi import Client

client = Client()

client.login(username, password)

media_path = ["/Users/hellojunho/instagrapi_test/video/media3.mp4", "/Users/hellojunho/instagrapi_test/video/media4.mp4", ... ]
caption = "hellojunho의 이미지 업로드 예제4"

client.album_upload(media_path, caption)
```  
비디오 여러 개는 `video_upload()` 대신에 `album_upload(media_path, caption)`을 사용한다.  

### 혼합 미디어 (사진 + 비디오)
```
from instagrapi import Client

client = Client()

client.login(username, password)

media_path = ["/Users/hellojunho/instagrapi_test/image/media1.jpeg", "/Users/hellojunho/instagrapi_test/image/media2.jpeg", "/Users/hellojunho/instagrapi_test/video/media3.mp4", "/Users/hellojunho/instagrapi_test/video/media4.mp4", ... ]
caption = "hellojunho의 이미지 업로드 예제5"

client.album_upload(media_path, caption)
```  
혼합 미디어의 경우에는 `album_upload(media_path, capion)`을 사용한다.  
이 때, 주의할 점은 `media_path`가 **list**라는 점과 media_path에 입력된 미디어의 순서대로 게시된다는 점이다.  
현재 예시에는 [이미지1, 이미지2, 비디오1, 비디오2]의 순으로 지정되어있으니, 인스타그램 게시물에도 이 순서와 동일하게 업로드 된다는 것이다.  

## 내가 사용한 예제
> 나는 instagrapi를 불러와 사용하는 부분을 class로 정의하고, 실행할 python 파일에서는 함수로 구분해서 단위 기능 별로 테스트를 진행했다. 모두 테스트에 성공했다. 

### insta_post.py
```
from instagrapi import Client
import os
from dotenv import load_dotenv
load_dotenv()

class InstaPost():
    
    def __init__(self) -> None:
        self.client = Client()
        self.isLogined = False

    def instaLogin(self, username, password):
        try:
            self.client.login(username, password)
            self.isLogined = True
            message = "로그인에 성공했습니다."
            print(message)
            return "로그인에 성공했습니다."
        
        except Exception as e:
            if "We couldn't find an account with the username" in str(e):
                print("아이디 혹은 비밀번호를 확인하세요.")
            else:
                print("로그인에 문제가 있습니다:", e)
                
    def get_profile_image(self):
        if self.isLogined:
            profile_img = self.client.user_info_by_username(self.client.username).profile_pic_url
            return profile_img
                
    # 이미지 한 장 업로드            
    def image_upload_one(self, media_path, caption):
        if self.isLogined:
            try:
                self.client.photo_upload(media_path, caption)
                message = "게시물 업로드를 완료했습니다."
                return message
            except Exception as e:
                print("게시물 업로드 중 문제가 생겼습니다.", e)
        else:
            message = "로그인 먼저 해주세요."
            return message          
        
    # 비디오 한 개 업로드
    def video_upload_one(self, media_path, caption):
        if self.isLogined:
            try:
                self.client.video_upload(media_path, caption)
                message = "게시물 업로드를 완료했습니다."
                return message
            except Exception as e:
                print("게시물 업로드 중 문제가 생겼습니다.", e)
        else:
            message = "로그인 먼저 해주세요."
            return message
        
    # 여러 개의 미디어 업로드(이미지 여러 장, 비디오 여러 개, 사진 + 비디오 등...)
    def album_upload(self, media_path, caption):
        if self.isLogined:
            try:
                self.client.album_upload(media_path, caption)
                message = "게시물 업로드를 완료했습니다."
                return message
            except Exception as e:
                print("게시물 업로드 중 문제가 생겼습니다.", e)
        else:
            message = "로그인 먼저 해주세요."
            return message        


    # 미디어 파일을 불러와 저장 (media_path가 str인 경우)
    def save_media(self, media_path):
        folder_path = os.path.join(os.path.dirname(__file__), "", "media")
        media_name = os.path.basename(media_path)
        destination_path = os.path.join(folder_path, media_name)

        try:
            with open(media_path, "rb") as f_source:
                with open(destination_path, "wb") as f_dest:
                    f_dest.write(f_source.read())
            print(f"Media saved to: {destination_path}")
            return destination_path
        except Exception as e:
            print("Error saving media:", e)

            
    def save_media_by_list(self, media_path):
        folder_path = os.path.join(os.path.dirname(__file__), "media")
        
        try:
            for m_path in media_path:
                media_name = os.path.basename(m_path)
                destination_path = os.path.join(folder_path, media_name)
                
                with open(m_path, "rb") as f_source:
                    with open(destination_path, "wb") as f_dest:
                        f_dest.write(f_source.read())
                print(f"Media saved to: {destination_path}")
            return folder_path  # 저장된 폴더 경로를 반환할 수도 있습니다.
        except Exception as e:
            print("Error saving media:", e)

```

### insta_post_ex.py
```
from insta_post import InstaPost
from datetime import datetime
import os
from dotenv import load_dotenv
load_dotenv()

username = os.getenv("INSTAGRAM_USER_ID")
password = os.getenv("INSTAGRAM_USER_PASSWORD")
contents_path = os.getenv("CONTENTS_PATH")

client = InstaPost()
client.instaLogin(username, password)


def photo_upload():
    media_filename = "photo1.jpeg"
    media_path = contents_path + media_filename
    caption = f"현재 시간: {datetime.now()}"

    if client.isLogined:
        try:
            saved_media_path = client.save_media(media_path)
        except Exception as e:
            print("이미지 저장 실패", e)
    else:
        print("Login first.")
    client.image_upload_one(media_path, caption)


def video_upload():
    media_filename = "video1.mp4"
    media_path = contents_path + media_filename
    caption = f"{datetime.now()} upload test"

    if client.isLogined:
        try:
            saved_media_path = client.save_media(media_path)
        except Exception as e:
            print("이미지 저장 실패", e)
    else:
        print("Login first.")

    client.video_upload_one(media_path, caption)


def album_upload():
    media_filename = ["video1.mp4", "video2.mp4", "photo1.jpeg", "photo2.jpeg"]
    prefix_path = contents_path
    media_path =  [prefix_path + i for i in media_filename]
    caption = f"현재 시간: {datetime.now()}"
    # print(media_path)

    if client.isLogined:
        try:
            saved_media_path = client.save_media_by_list(media_path)
        except Exception as e:
            print("이미지 저장 실패", e)
    else:
        print("Login first.")
    client.album_upload(media_path, caption)

album_upload()
```