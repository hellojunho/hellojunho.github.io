---
layout: single
title: "📘 [Python] self 짚고 넘어가기!"
toc: true
toc_sticky: true
toc_label: "목차"
categories: python
excerpt: ""
tag: []
---

> 항상 쓰는 `self` 제대로 알고 쓰자
> 

# self의 역할?

`self` 는 클래스의 메소드에서 해당 메소드가 속한 객체를 가리키는 변수이다.

객체를 생성할 때 클래스의 인스턴스를 **자동으로** 전달하므로, 클래스의 모든 메소드는 첫 번째 매개변수로 `self` 를 가져야 한다.

즉, `self` 를 통해 메소드는 객체의 속성에 접근하고, 상태를 변경할 수 있는 것이다.

## 예시 코드

```python
class Car:
    def __init__(self, brand, model):
        self.brand = brand
        self.model = model
        self.mileage = 0  # 초기 주행 마일리지는 0으로 설정합니다.
    
    def drive(self, distance):
        self.mileage += distance
        print(f"{self.brand} {self.model}이(가) {distance}마일 운전하여 총 주행 마일리지는 {self.mileage}마일입니다.")
    
    def honk(self):
        print(f"{self.brand} {self.model}이(가) 층층 하고 경적을 울립니다.")

# Car 클래스의 인스턴스 생성
my_car = Car("Toyota", "Corolla")

# drive 메소드 호출
my_car.drive(50)
my_car.drive(30)

# honk 메소드 호출
my_car.honk()

##### 출력 #####
# Toyota Corolla이(가) 50마일 운전하여 총 주행 마일리지는 50마일입니다.
# Toyota Corolla이(가) 30마일 운전하여 총 주행 마일리지는 80마일입니다.
# Toyota Corolla이(가) 층층 하고 경적을 울립니다.
```

- Car 클래스 정의 → brand, model, mileage 속성
- `__init__()` → 객체가 생성될 때 호출되는 초기화 메소드 → 객체의 속성을 초기화함
- `drive()` → 주행 거리를 누적 & 마일리지 업데이트 → `self` 를 사용해 인스턴스 변수에 접근함
- `honk()` → 경적 울리기

# self 역할 요약

- `self` 는 클래스의 인스턴스를 가리키는 특별한 키워드
- 클래스의 메소드는 첫 번째 매개변수로 `self` 를 가져야 함(정적 메소드 제외)
- `self` 를 통해 객체의 속성에 접근하고 강태를 변경할 수 있음