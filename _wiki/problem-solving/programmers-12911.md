---
layout  : wiki
title   : 프로그래머스 - 다음 큰 숫자
date    : 2023-04-08 15:38:49 +0900
updated : 2023-04-08 15:38:49 +0900
tag     : 
toc     : true
public  : true
parent  : problem-solving
latex   : false
---

* TOC
{:toc}

[다음 큰 숫자](https://school.programmers.co.kr/learn/courses/30/lessons/12911)

## 구해야 하는 것
- n의 다음 큰 숫자를 return

## 자료
- n의 다음 큰 숫자는 n보다 큰 자연수
- n의 다음 큰 숫자와 n은 2진수로 변환했을 때 1의 개수가 같음
- n의 다음 큰 숫자는 조건 1, 2를 만족하는 수 중 가장 작은 수

## 조건
- n ≤ 1,000,000

## 계획
1. n을 2진수로 변환하고 1의 개수를 센다.
  - 숫자를 2로 나눈 나머지가 1이면 count++;
  - 숫자 /= 2
  - n이 0이될 때까지 반복
2. n보다 큰 자연수들을 탐색한다.
3. 다음 큰 숫자를 2진수로 변환하고 1의 개수를 센다.
4. 2진수로 변환했을 때 1의 개수가 같은 수를 return

## 실행
```java
public class Solution {
    public int getOneNumber(int n) {
        int count = 0;
        while(n != 0) {
            if(n % 2 == 1) {
                count++;
            }
            n /= 2;
        }
        return count;
    }

    public int solution(int n) {
        int countN = getOneNumber(n);

        for (int i = n + 1; i <= 1_000_000; i++) {
            int countBigger = getOneNumber(i);
            if(countN == countBigger) {
                return i;
            }
        }
        return 0;
    }
}
```
