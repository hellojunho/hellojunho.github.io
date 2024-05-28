---
layout: single
title: "📘[Django] Ubuntu & Django?"
toc: true
toc_sticky: true
toc_label: "목차"
categories: django
excerpt: ""
tag: [python, linux]
---

# 우분투??

`우분투(Ubuntu)` 는 **데비안(Debian) 기반의 리눅스 배포판**으로, 개인 사용자부터 기업에 이르기까지 다양한 용도로 사용되는 버전 정도로 이해할 수 있다!

## 데비안은 또 뭔데?

`데비안` 은 **이안 머독(Ian Murdock)**에 의해 설립된 자유 소프트웨어 공동체임.

데비안 프로젝트는 `GNU/Linux` 운영 체제의 한 종류인 `데비안 GNU/Linux` 를 개발하고 유지보수한다.

데비안 프로젝트는 **자유 소프트웨어를 지지하고, 사용자가 자유롭게 소프트웨어를 사용할 수 있도록 하는 것을 목표**로 한다

> 일단 이정도만 알고 넘어가자..!
> 

## 우분투의 주요 용도

- 개인용 데스크탑
    - 사용자 친화적 인터페이스와 광범위한 소프트웨어 호환성으로 개인 사용자에게 적합함!
- 서버 환경
    - 안정성과 보안성을 바탕으로 웹 서버, 데이터베이스 서버 등 다양한 서버 용도로 사용됨!
- 클라우드 컴퓨팅
    - 우분투 서버는 AWS, Azure, Google Cloud 등의 주요 클라우드 플랫폼에서 지원되고, 클라우드 환경에 최적화된 성능을 제공함!
- 사물 인터넷
    - 우분투 코어를 통해 IoT 장치에도 최적화된 운영체제로 사용된다고 한다.

# 우분투 설치 및 설정

## 우분투 ISO 파일 다운로드

[우분투 공식 웹사이트](https://ubuntu.com/download/desktop)에서 최신 우분투 데스크탑 버전의 ISO 파일을 다운로드할 수 있다!

## 부팅 가능한 USB 드라이브

우분투 ISO 파일을 사용할 부팅 가능한 USB 드라이브(최소 4GB 이상)를 준비해야함!

## Rufus

부팅 가능한 USB 드라이브를 만드는 도구임.

[Rufus 공식 웹사이트](https://rufus.ie/)에서 다운로드 가능~!

# 우분투 접속하기

## 로그인

AWS에서 우분투 서버에 접속하려면 SSH 프로토콜을 사용함.

이를 위해 AWS 관리 콘솔에서 제공한 `키 페어` 를 사용해 보안 연결을 설정해야한다!

## AWS 기준 우분투 서버 접속 방법

AWS 인스턴스에 SSH로 들어가자!

```
chmod 400 /path/to/your-key-file.pem 
```

```
ssh -i "your-key-file.pem" ubuntu@ec2-xx-xxx-xxx-xx.ap-northeast-2.compute.amazonaws.com
```

- ec2-xx-xxx-xxx-xx는 EC2의 퍼블릭 IP 주소임!
- aws ec2 대시보드 들어가서 인스턴스 연결 → SSH 연결 누르면 아래에 SSH 접속 명령어 제공함

## 배포는 어떻게?

예를 들어, Django 애플리케이션을 배포하려면 `Gunicorn` 과 `Nginx` 를 결합하여 배포를 진행한다!

Django에서 개발 및 테스트에 사용하는 `runserver` 명령어를 통한 서버는 고성능을 위한 최적화가 되어있지 않은 서버임!!

심지어 `멀티 스레드` 및 `멀티 프로세스` 를 지원하지 않아, 고부하 환경에서 안정적으로 동작하지 않는다는 단점도 있다..

게다가 보안적인 측면에서도 약점이 많다고 한다..

### 그럼 Gunicorn이 뭔데?

`Gunicorn(Green Unicorn)` 은 Python WSGI 애플리케이션을 실행하기 위한 `HTTP 서버`임!

- WSGI(Web Server Gateway Interface) [[WSGI 공식문서](https://wsgi.readthedocs.io/en/latest/what.html)]
    - 쉽게 생각하면 Python Application이 웹 서버와 통신하기 위한 표준 인터페이스로, 파이썬 프레임워크로 이해할 수 있다.

### Gunicorn 특징

- WSGI 서버
- 멀티 프로세스 지원
    - 여러 Worker 프로세스를 실행해 동시 요청을 처리할 수 있다!
- 간단한 설정
- 확장성
    - Worker 프로세스 수를 조절하여 애플리케이션의 처리 능력을 쉽게 확장할 수 있다!

### Nginx는?

`Nginx` 는 **웹 서버 & 리버스 프록시 서버이다.**

클라이언트의 요청을 받아 Gunicorn 서버로 전달하고, 정적 파일을 제공하며, 여러 가지 중요한 기능을 수행한다!

### Nginx 특징

- 정적 파일 서빙
    - 정적 파일(CSS, JavaScript, 이미지 등)을 고속으로 제공할 수 있다.
    - Gunicorn은 동적 컨텐츠 처리를 위한 것이기 때문에, 정적 파일 서빙을 Nginx가 담당하여 효율 UP!
- 리버스 프록시
    - Nginx는 클라이언트의 요청을 받아 Gunicorn에 전달하고, Gunicorn으로부터의 응답을 클라이언트에게 전달한다.
    - 이를 통해 Guicorn의 뒤에서 로드 밸런싱, 캐싱, SSL 종료 등의 역할을 수행할 수 있다!
- 보안
    - Nginx는 다양한 보안 기능을 제공하고, Dos 공격 방어와 SSL/TLS 설정 등을 통해 보안성을 강화할 수 있다!
- 로드 밸런싱
    - 여러 Gunicorn 인스턴스에 트래픽을 분산시켜 고가용성을 제공할 수 있다!
- 고성능
    - `비동기 이벤트 기반 아키텍처` 를 사용하여 매우 높은 성능을 제공한다!

## 서버 환경 설정을 해보자!

### 시스템 업데이트 및 패키지 설치

```
sudo apt update
sudo apt upgrade -y
```

- 시스템 패키지 목록을 업데이트하고 모든 패키지를 최신 버전으로 업그레이드함.

```
sudo apt install python3-pip python3-dev libpq-dev nginx curl -y
```

- Python 패키지 관리 도구와 개발 헤더 파일, PostgreSQL 데이터베이스 라이브러리, 웹 서버 Nginx, 그리고 curl을 설치함.

### 서버 안에서도 가상환경!

```
curl -L -O "https://github.com/conda-forge/miniforge/releases/latest/download/Miniforge3-$(uname)-$(uname -m).sh"
bash Miniforge3-$(uname)-$(uname -m).sh

source ~/.bashrc
conda create -n env python=3.10
conda activate env
pip install django
```

- conda 가상환경으로 진행함

### Django 프로젝트를 생성하거나 클론 받자!

[`settings.py`](http://settings.py) 에서 DB 설정을 내가 사용하려는 DB로 바꾸자!

그리고 `ALLOWED_HOTS` 를 만져줘야된다.

```
ALLOWED_HOSTS = ['your-ec2-public-dns', 'your-domain.com']
```

- EC2 인스턴스의 퍼블릭 DNS 또는 IP를 추가

```
STATIC_URL = '/static/'
STATIC_ROOT = os.path.join(BASE_DIR, 'static/')
```

- 정적 파일 설정도 해주고

```
python manage.py makemigrations
python manage.py migrate
```

- 다 했으면 마이그레이션

```
python manage.py createsuperuser
python manage.py runserver 0:8000
```

- 관리자 계정도 만들어주고 실행해보자!

## Gunicorn 사용

먼저 gunicorn 설치부터 해야지!

```
pip install gunicorn
gunicorn --bind 0.0.0.0:8000 myproject.wsgi:application
```

## Nginx도 같이 쓴다 했으니까!

```
sudo apt update
sudo apt install nginx -y
nginx -V # 버젼 확인
```

### Nginx 설정

```
sudo /etc/init.d/nginx start
```

- Nginx 시작해보기!

```
# nano /etc/nginx/nginx.conf

http {
	....
	# /etc/nginx/conf.d 디렉토리 아래 있는 .conf 파일을 모두 불러옴
	include /etc/nginx/conf.d/*.conf;
}
```

**default.conf 생성**

<aside>
💡 pub_ip:80(nginx 서버주소)으로 요청하면 127.0.0.1:8000(gunicorn 서버주소)로 리다이렉션

</aside>

```jsx
# nano /etc/nginx/conf.d/default.conf

server {
	listen 80;
    server_name pub_ip; 
    
    location /{
    	proxy_pass http://127.0.0.1:8000; 
    }
}
```

- `nano /etc/nginx/conf.d/default.conf` 명령어로 파일을 열고, 수정 후 저장하려니 자꾸 오류 발생함
- 파일을 여는 명령어를 `sudo nano /etc/nginx/conf.d/default.conf` 로 하면 해결됨!

```
sudo service nginx restar
```

- Nginx 재시작 해보기!