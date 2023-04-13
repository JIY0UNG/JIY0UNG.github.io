---
layout  : wiki
title   : 프로그래머스 - 타겟 넘버
date    : 2023-04-13 23:16:04 +0900
updated : 2023-04-13 23:16:04 +0900
tag     : 
toc     : true
public  : true
parent  : problem-solving
latex   : false
---

* TOC
{:toc}

[타겟 넘버](https://school.programmers.co.kr/learn/courses/30/lessons/43165)

## 구해야 하는 것
- 숫자를 적절히 더하고 빼서 타겟 넘버를 만드는 방법의 수를 return

## 자료
- 주어진 정수들의 순서를 바꾸면 안 됨
- 각 정수는 더하거나 뺄 수 있음

## 계획
1. 각 숫자를 더하거나 뺀 값을 누적한다.
2. 마지막 숫자까지 계산한 값이 타겟 넘버와 같으면 result + 1

## 실행
```java
class Solution {
    int result = 0;

    public void calculate(int[] numbers, int index, int sum, int target) {
        if(index == numbers.length) {
            if(sum == target) {
                result++;
            }
            return;
        }

        calculate(numbers, index + 1, sum + numbers[index], target);
        calculate(numbers, index + 1, sum - numbers[index], target);
    }

    public int solution(int[] numbers, int target) {
        calculate(numbers, 0, 0, target);
        return result;
    }
}
```
