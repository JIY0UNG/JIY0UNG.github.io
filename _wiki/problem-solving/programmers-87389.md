---
layout  : wiki
title   : 프로그래머스 - 나머지가 1이 되는 수 찾기
date    : 2023-01-30 10:54:00 +0900
updated : 2023-01-30 10:54:00 +0900
tag     : 
toc     : true
public  : true
parent  : problem-solving
latex   : false
---

* TOC
{:toc}

[나머지가 1이 되는 수 찾기](https://school.programmers.co.kr/learn/courses/30/lessons/87389)

## 구해야 하는 것
- n을 x로 나눈 나머지가 1이 되도록 하는 가장 작은 자연수

## 조건
- 3 ≤ n ≤ 1,000,000

## 계획
1. 1부터 돌면서 n을 나눈 나머지가 1이 되는 수를 찾으면 return

## 실행
```java
public int solution(int n) {
    int x = 1;
    while(true) {
        if(n % x == 1) {
            return x;
        }
        x++;
    }
}
```
