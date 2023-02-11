---
layout  : wiki
title   : 프로그래머스 - 두 정수 사이의 합
date    : 2023-02-11 14:50:00 +0900
updated : 2023-02-11 14:50:00 +0900
tag     : 
toc     : true
public  : true
parent  : problem-solving
latex   : false
---

* TOC
{:toc}

[두 정수 사이의 합](https://school.programmers.co.kr/learn/courses/30/lessons/12912)

## 구해야 하는 것
- a와 b사이에 속한 모든 정수의 합

## 주어진 자료
- a = 3, b = 5이면, 3 + 4 + 5 = 12이므로 12를 반환

## 조건
- a와 b가 같으면 둘 중 아무 수나 반환
- -10,000,000 ≤ a, b ≤ 10,000,000
- a와 b의 대소관계는 정해져있지 않음

## 계획 1
1. a와 b가 같으면 a를 반환한다.
2. a를 answer에 더한다.
3. a를 1 증가시킨다.
4. a가 b보다 커지면 answer를 반환한다.

## 실행 1 → 실패
```java
public long solution(int a, int b) {
    if(a == b) {
        return a;
    }
    long answer = 0;
    while(a <= b) {
        answer += a;
        a++;
    }
    return answer;
}
```

## 계획 2
a가 b보다 무조건 작다고 생각하고 계획을 세웠었는데, 그게 아니라는걸 깨달았다. 그래서 a와 b 중 더 작은 것을 start로 두고, 큰 것을 end로 두는 로직을 추가했다.
1. a와 b가 같으면 a를 반환한다.
2. a가 b보다 크면 b가 start, a가 end
3. a가 b보다 작으면 a가 start, b가 end
4. start를 answer에 더한다.
5. start를 1 증가시킨다.
6. start가 end보다 커지면 answer를 반환한다.

## 실행 2
```java
public long solution(int a, int b) {
    int start = 0;
    int end = 0;
    if(a > b) {
        start = b;
        end = a;
    } else if(a < b) {
        start = a;
        end = b;
    } else if(a == b) {
        return a;
    }

    long answer = 0;
    while(start <= end) {
        answer += start;
        start++;
    }
    return answer;
}
```
