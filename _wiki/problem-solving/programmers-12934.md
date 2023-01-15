---
layout  : wiki
title   : 프로그래머스 - 정수 제곱근 판별
date    : 2022-09-15 17:11:00 +0900
updated : 2023-01-15 22:15:00 +0900
tag     : 
toc     : true
public  : true
parent  : problem-solving
latex   : false
---

* TOC
{:toc}

[정수 제곱근 판별](https://school.programmers.co.kr/learn/courses/30/lessons/12934)

## 구해야 하는 것
- 주어진 정수 n이 x의 제곱근인지 아닌지 구하라

## 주어진 자료
- 1 <= n <= 50,000,000,000,000
- n이 x의 제곱근이면 x + 1의 제곱 리턴, 그렇지 않으면 -1 리턴

## 계획
1. 1부터 n까지의 수 중 제곱해서 n과 같아지는 수가 있으면 (x + 1)의 제곱을 반환한다.
2. 제곱한 수가 n보다 커지면 -1을 반환한다.

## 실행
```java
public class Solution {
    public long solution(long n) {
        for(long i = 1; i <= n; i++) {
            if(i * i > n) {
                return -1;
            }
            if(i * i == n) {
                return (i + 1) * (i + 1);
            }
        }
        return -1;
    }
}
```
