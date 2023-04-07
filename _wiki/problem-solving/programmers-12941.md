---
layout  : wiki
title   : 프로그래머스 - 최솟값 만들기
date    : 2023-04-08 00:51:00 +0900
updated : 2023-04-08 00:51:00 +0900
tag     : 
toc     : true
public  : true
parent  : problem-solving
latex   : false
---

* TOC
{:toc}

[최솟값 만들기](https://school.programmers.co.kr/learn/courses/30/lessons/12941)

## 구해야 하는 것
- 배열 A, B가 주어질 때 최종적으로 누적된 최솟값을 return

## 자료
- 배열 A, B에서 각각 한 개의 숫자를 뽑아 두 수를 곱한다.
- 위 과정을 배열의 길이만큼 반복한다.
- 두 수를 곱한 값을 누적하여 더한다.
- 최종적으로 누적된 값이 최소가 되도록 만들어야 한다.
- 한번 뽑은 숫자는 또 뽑을 수 없다.

## 조건
- A의 길이, B의 길이 ≤ 1,000
- `A[i]`, `B[i]` ≤ 1,000

## 계획
- 배열들을 정렬한다.
- A는 맨 앞부터, B는 맨 뒤부터 돌면서 숫자들을 곱한다.
- 곱한 값을 결과에 누적한다.

## 실행
```java
public class Solution {
    public int solution(int[] a, int[] b) {
        Arrays.sort(a);
        Arrays.sort(b);

        int answer = 0;
        for(int i = 0; i < a.length; i++) {
            answer += a[i] * b[b.length - i - 1];
        }

        return answer;
    }
}
```
