---
layout  : wiki
title   : 프로그래머스 - 평균 구하기
date    : 2022-09-15 16:53:00 +0900
updated : 2023-01-15 22:15:00 +0900
tag     : 
toc     : true
public  : true
parent  : problem-solving
latex   : false
---

* TOC
{:toc}

[평균 구하기](https://school.programmers.co.kr/learn/courses/30/lessons/12944)

## 구해야 하는 것
- 주어진 배열의 평균

## 조건
- 1 <= arr의 길이 <= 100
- -10,000 <= arr의 원소 <= 10,000

### 문제를 더 단순하게 하기 위해서 주어진 조건 버리기
- 조건 중 정수 배열의 각 원소 x의 범위는 굳이 고려할 필요가 없는 것 같다.

## 계획
1. 배열을 처음부터 돌면서 전부 합한다.
2. 합을 배열의 길이로 나눈 값을 return 한다.

## 실행
```java
public class Solution {
    public double solution(int[] arr) {
        double answer = 0;
        for(int i = 0; i < arr.length; i++) {
            answer += arr[i];
        }
        return answer / arr.length;
    }
}
```
