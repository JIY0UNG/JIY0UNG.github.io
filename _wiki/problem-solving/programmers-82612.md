---
layout  : wiki
title   : 프로그래머스 - 부족한 금액 계산하기
date    : 2022-06-07 22:06:01 +0900
updated : 2023-01-15 22:15:00 +0900
tag     : 
toc     : true
public  : true
parent  : problem-solving
latex   : false
---

* TOC
{:toc}

[부족한 금액 계산하기](https://programmers.co.kr/learn/courses/30/lessons/82612)

## 구해야 하는 것
- 놀이 기구를 count번 타게 될 경우 현재 자신이 가지고 있는 금액에서 얼마가 모자라는지 구하라

## 주어진 자료
- 원래 이용료 = price
- 놀이 기구를 이용할 횟수 = count
- 처음 가지고 있던 금액 = money

## 조건
- price는 1 이상 2,500 이하의 자연수
- money는 1 이상 1,000,000,000 이하의 자연수
- count는 1 이상 2,500 이하의 자연수
- 놀이 기구를 N 번째 이용할 경우 이용료 = price * N
- 금액이 부족하지 않으면 0을 return 하라

## 계획
일단 주어진 입출력 예시를 살펴보자.
1. price = 3, money = 20, count = 4
2. 처음 탈 때 이용료는 3, 두 번째 탈 때 이용료는 3 * 2 = 6, 세 번째 탈 때 이용료는 3 * 3 = 9, 네 번째 탈 때 이용료는 3 * 4 = 12
3. 그러므로 필요한 금액은 3 + 6 + 9 + 12 = 30
4. 내가 가진 금액은 20이고, 필요한 금액은 30이므로 10이 부족하다
5. 10을 return하면 됨

이제 이걸 가지고 계획을 세워보자
1. 필요한 총 금액을 구해야 한다. 총 금액을 sum에 저장하고, sum의 초기값은 0으로 한다.
2. i가 count가 될 때까지 i를 계속 증가시키면서 for문을 돈다.
3. i와 price를 곱해준 값을 sum에 더해준다.
4. money가 sum보다 크면 금액이 부족하지 않은거니까 0을 return 하고, money가 sum보다 작으면 sum - money를 return 한다.

## 실행 1 → 틀림
```c
using namespace std;

long long solution(int price, int money, int count) {
    int answer = -1;
    int sum = 0;
    
    for(int i = 1; i <= count; i++) {
        sum += (price * i);
    }
    
    if(money >= sum) {
        answer = 0;
    }
    else {
        answer = sum - money;
    }

    return answer;
}
```
이렇게 풀었는데, 채점에서 틀린 테스트 케이스가 나왔다.  
조건을 다시 살펴보니 만약에 price가 2500이고 count가 2500이면,  
sum = 2500 + (2500 * 2) + ... + (2500 * 2500) = 약 7,000,000,000 이상의 숫자가 나올 것 같다.  
그러므로 answer와 sum을 int가 아닌 long long으로 선언해줘야 한다.

## 실행 2
나머지는 같고 변수의 자료형만 long long으로 바꿔줌
```c
long long answer = -1;
long long sum = 0;
```

## 반성
- 필요한 금액 sum을 구할 때 for문으로 돌리지 않고 `sum = 1LL * price * count * (count + 1) / 2;` 이런 공식으로 한번에 구할 수 있다.
- 조건으로 주어진 변수의 범위도 잘 살펴봐야겠다. 범위에서 10억 이상의 수가 보이면 int형으로 표현이 가능할지 생각해보자.
