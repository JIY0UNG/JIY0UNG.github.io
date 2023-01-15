---
layout  : wiki
title   : 프로그래머스 - 삼총사
date    : 2023-01-16 01:01:00 +0900
updated : 2023-01-16 01:01:00 +0900
tag     : 
toc     : true
public  : true
parent  : problem-solving
latex   : false
---

* TOC
{:toc}

[삼총사](https://school.programmers.co.kr/learn/courses/30/lessons/131705)

## 구해야 하는 것
- 삼총사를 만들 수 있는 방법의 수

## 주어진 자료
- 학생들은 각자 정수 번호를 가지고 있음
- 3명의 정수 번호의 합이 0이 되면 삼총사

## 조건
- 3 <= number의 길이 <= 13
- -1,000 <= number의 각 원소 <= 1,000
- 서로 다른 학생의 정수 번호가 같을 수 있음

## 계획
1. 주어진 number에서 무작위로 3개를 뽑아서 더한다.
2. 합이 0이면 result++

## 실행
```java
public class Solution {
    int rslt = 0;
    int getSum(int[] arr, boolean[] visited, int n) {
        int sum = 0;
        for (int i = 0; i < n; i++) {
            if (visited[i]) {
                sum += arr[i];
            }
        }
        return sum;
    }

    void comb(int[] arr, boolean[] visited, int start, int n, int r) {
        if(r == 0) {
            int sum = getSum(arr, visited, n);
            if(sum == 0) {
                rslt++;
            }
            return;
        }

        for(int i=start; i<n; i++) {
            visited[i] = true;
            comb(arr, visited, i + 1, n, r - 1);
            visited[i] = false;
        }
    }

    public int solution(int[] number) {
        comb(number, new boolean[number.length], 0, number.length, 3);
        return rslt;
    }
}
```
