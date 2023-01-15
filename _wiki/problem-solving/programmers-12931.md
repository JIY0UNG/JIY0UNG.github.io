---
layout  : wiki
title   : 프로그래머스 - 자릿수 더하기
date    : 2022-09-15 16:37:00 +0900
updated : 2023-01-15 22:15:00 +0900
tag     : 
toc     : true
public  : true
parent  : problem-solving
latex   : false
---

* TOC
{:toc}

[자릿수 더하기](https://school.programmers.co.kr/learn/courses/30/lessons/12931)

## 구해야 하는 것
- 주어진 수의 각 자릿수의 합

## 주어진 자료
- n = 123이면 1 + 2 + 3 = 6을 반환하면 됨

## 조건
- n <= 100,000,000

## 계획
1. n을 10으로 나눈 나머지를 answer에 더한다.
2. n에 n을 10으로 나눈 몫을 대입한다.
3. n이 0이 될 때까지 반복한다.

## 풀이
```java
public class Solution {
    public int solution(int n) {
        int answer = 0;
        while(n != 0) {
            answer += n % 10;
            n /= 10;
        }
        return answer;
    }
}
```
