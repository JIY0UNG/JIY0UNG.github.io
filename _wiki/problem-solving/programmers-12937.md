---
layout  : wiki
title   : 프로그래머스 - 짝수와 홀수
date    : 2022-09-15 16:33:00 +0900
updated : 2023-01-15 22:15:00 +0900
tag     : 
toc     : true
public  : true
parent  : problem-solving
latex   : false
---

* TOC
{:toc}

[짝수와 홀수](https://school.programmers.co.kr/learn/courses/30/lessons/12937)

## 구해야 하는 것
- 주어진 정수가 짝수인지 홀수인지 구하라

## 조건
- num은 int 범위의 정수
- 0은 짝수
- 짝수이면 Even 반환, 홀수이면 Odd 반환

## 계획
1. num이 2로 나누어떨어지면 Even을 반환한다.
2. 2로 나누어떨어지지 않으면 Odd를 반환한다.

## 실행
```java
public class Solution {
    public String solution(int num) {
        if(num % 2 == 0) {
            return "Even";
        } else {
            return "Odd";
        }
    }
}
```
