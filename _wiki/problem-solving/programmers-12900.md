---
layout  : wiki
title   : 프로그래머스 - 2 x n 타일링
date    : 2023-01-05 17:40:00 +0900
updated : 2023-01-15 22:15:00 +0900
tag     : 
toc     : true
public  : true
parent  : problem-solving
latex   : true
---

* TOC
{:toc}

[2 x n 타일링](https://school.programmers.co.kr/learn/courses/30/lessons/12900)

## 구해야 하는 것
- 바닥을 타일로 채우는 방법의 수

## 주어진 자료
- 타일의 가로 = 2, 세로 = 1
- 바닥의 세로 = 2, 가로 = n
- 타일 배치 방법
  - 타일을 가로로 배치
  - 타일을 세로로 배치

## 조건
- n <= 60,000
- 경우의 수를 1,000,000,007로 나눈 나머지를 return

## 계획
바닥이 2 x 1인 경우의 수 = 1  
바닥이 2 x 2인 경우의 수 = 2  
바닥이 2 x 3인 경우의 수 = 3  
바닥이 2 x 4인 경우의 수 = 5  
바닥이 2 x 5인 경우의 수 = 8  
...  
$$\begin{pmatrix}
 a_1 = 1\\
 a_2 = 2\\
 a_3 = 3\\
 a_4 = 5\\
 a_5 = 8\\
 \vdots
 \end{pmatrix}$$

따라서 `a[n] = a[n - 2] + a[n - 1]` 이라는 점화식이 세워진다.

## 실행
```java
public int solution(int n) {
    int[] dp = new int[n + 1];

    dp[1] = 1;
    dp[2] = 2;

    for(int i = 3; i <= n; i++) {
        dp[i] = (dp[i - 2] + dp[i - 1]) % 1000000007;
    }

    return dp[n];
}
```
