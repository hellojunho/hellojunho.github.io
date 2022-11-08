---
layout: single
title: "Radix_Sort (기수 정렬)"
toc: true
toc_sticky: true
toc_label: "목차"
categories: algorithms, sort
excerpt: "기수 정렬이란?"
tag: [algorithms, sort]
---

# 📘 Radix_Sort (기수 정렬)

## 1. 기수 정렬이란?
주어진 배열의 요소들의 가장 낮은 자릿수부터 비교하며 정렬하는 알고리즘   

- stable counting sort를 기반으로 동작함
- 장점: 비교연산을 하지 않아 정렬 속도가 빠름
- 단점: 데이터 전체 크기에 기수 테이블의 크기만한 메모리가 필요

---

## 2. 기수 정렬의 동작 과정
1. 1~10 까지의 배열을 선언한다. (A라고 하자)
2. 입력받은 데이터의 요소들에 대하여 가장 낮은 자릿수를 기준으로 배열-A에 차례대로 저장한다. (이전 데이터의 요소 순서를 지킨다. (stable counting sort를 기반으로 하기 때문))
3. 다음으로 낮은 자릿수를 기준으로 다시 배열-A에 저장한다.
4. 2, 3번 과정을 반복한다.

## 3. 예제코드
<pre><code>def radixSort(nums:List[int])->List[int]:  
largest_num = max(nums)  
digits = int(math.log10(largest_num))+1  
sorted = nums  
for digit in range(digits):  
    sorted = countingSortByDigit(nums=sorted,digit=digit)  
return sorted  

print(radixSort(nums=[391,582,50,924,8,192]))</code></pre>
