---
layout  : wiki
title   : 프로그래머스 - 약수의 합
date    : 2022-09-15 16:44:00 +0900
updated : 2023-01-15 22:15:00 +0900
tag     : 
toc     : true
public  : true
parent  : problem-solving
latex   : false
---

* TOC
{:toc}

[약수의 합](https://school.programmers.co.kr/learn/courses/30/lessons/12928)

## 구해야 하는 것
- 주어진 정수의 약수를 모두 더한 값

## 조건
- 0 <= n <= 3,000

## 계획
1. 1부터 n / 2까지 n으로 나누어떨어지는 수를 answer에 더한다.
2. answer에 n을 더한 수를 return 한다.

## 실행
```java
public class Solution {
    public int solution(int n) {
        int answer = 0;

        if(n == 0) {
            return 0;
        }
        
        for(int i = 1; i <= n / 2; i++) {
            if(n % i == 0) {
                answer += i;
            }
        }

        return answer + n;
    }
}
```
