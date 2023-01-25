---
layout  : wiki
title   : 프로그래머스 - 기사단원의 무기
date    : 2023-01-25 17:25:00 +0900
updated : 2023-01-25 17:25:00 +0900
tag     : 
toc     : true
public  : true
parent  : problem-solving
latex   : false
---

* TOC
{:toc}

[기사단원의 무기](https://school.programmers.co.kr/learn/courses/30/lessons/136798)

## 구해야 하는 것
- 무기를 모두 만들기 위해 필요한 철의 무게

## 주어진 자료
- 각 기사는 자신의 기사 번호의 약수 개수에 해당하는 공격력을 가진 무기를 구매함
- 구매해야하는 무기의 공격력이 제한 수치보다 크면 협약 기관에서 정한 공격력을 가진 무기를 구매해야 함
- 기사단원의 수 = number
- 공격력 제한 수치 = limit
- 제한 수치 초과한 기사가 사용할 무기의 공격력 = power

## 조건
- 1 ≤ number ≤ 100,000
- 2 ≤ limit ≤ 100
- 1 ≤ power ≤ limit

## 계획
1. 1부터 number를 돌면서 약수의 개수를 구함
2. 약수의 개수가 limit 이하이면 약수의 개수만큼 result에 더함
3. limit 초과이면 power를 result에 더함

## 실행
```java
public void getStrikingPower(int num, int[] strikingPower, int limit, int power) {
    int cnt = 0;
    for(int i = 1; i * i <= num; i++) {
        if(i * i == num) {
            cnt++;
        } else if(num % i == 0) {
            cnt += 2;
        }
    }

    if(cnt > limit) {
        strikingPower[num - 1] = power;
    } else {
        strikingPower[num - 1] = cnt;
    }
}

public int getIronWeight(int[] strikingPower) {
    int sum = 0;
    for(int i = 0; i < strikingPower.length; i++) {
        sum += strikingPower[i];
    }
    return sum;
}

public int solution(int number, int limit, int power) {
    int[] strikingPower = new int[number];

    // 각 기사가 사용할 수 있는 공격력 구하기
    for(int i = 1; i <= number; i++) {
        getStrikingPower(i, strikingPower, limit, power);
    }

    // 필요한 철의 무게 구하기
    int answer = getIronWeight(strikingPower);
    return answer;
}
```
