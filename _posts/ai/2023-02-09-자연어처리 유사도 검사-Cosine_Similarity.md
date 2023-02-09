---
layout: single
title: "Cosine_similarity 대해서..."
toc: true
toc_sticky: true
toc_label: "목차"
categories: ai
excerpt:
tag: []
---

# 📘Cosine_similarity

## 1. Cosine_similarity(코사인 유사도)
코사인 유사도는 두 벡터 간의 코사인 각도를 이용하여 구할 수 있는 두 벡터의 유사도를 의미합니다.   
두 벡터의 방향이 완전히 동일한 경우에는 1의 값을 가지며, 90°의 각을 이루면 0, 180°로 반대의 방향을 가지면 -1의 값을 갖게 됩니다.  
즉, 결국 코사인 유사도는 -1 이상 1 이하의 값을 가지며 값이 1에 가까울수록 유사도가 높다고 판단할 수 있습니다.  
이를 직관적으로 이해하면 두 벡터가 가리키는 방향이 얼마나 유사한가를 의미합니다.  
<img width="520" alt="스크린샷 2023-02-03 오후 2 41 32" src="https://user-images.githubusercontent.com/104587537/216521836-9badbf9b-beed-4458-a0e0-a10bc9c83072.png">
<br>

## 2. squeeze()  
squeeze함수는 차원이 1인 차원을 제거해준다.  
따로 차원을 설정하지 않으면 1인 차원을 모두 제거한다.  
그리고 차원을 설정해주면 그 차원만 제거한다.
<br>
[예제]
```python
import torch

x = torch.rand(1, 1, 20, 128)
x = x.squeeze() # [1, 1, 20, 128] -> [20, 128]

# 1번 위치의 차원 1을 삭제
x2 = torch.rand(1, 1, 20, 128)
x2 = x2.squeeze(dim=1) # [1, 1, 20, 128] -> [1, 20, 128]
```

## 3. idxmin() & idxmax()  
최대값이나 최소값을 갖는 인덱스를 불러오는 메서드
- idxmin(): 최소값을 갖는 인덱스 레이블 출력
- idxmax(): 최대값을 갖는 인덱스 레이블 출력
