---
layout  : wiki
title   : 프로그래머스 - 같은 숫자는 싫어
date    : 2023-02-11 14:06:00 +0900
updated : 2023-02-11 14:06:00 +0900
tag     : 
toc     : true
public  : true
parent  : problem-solving
latex   : false
---

* TOC
{:toc}

[같은 숫자는 싫어](https://school.programmers.co.kr/learn/courses/30/lessons/12906)

## 구해야 하는 것
- 배열 arr에서 연속적으로 나타나는 숫자를 제거하고 남은 수들

## 주어진 자료
- 배열 arr의 각 원소는 숫자 0부터 9까지로 이루어져 있음
- 배열 arr에서 연속적으로 나타나는 숫자는 하나만 남기고 전부 제거하려고 함
- 제거된 후 남을 수들을 반환할 때는 배열 arr의 원소들의 순서를 유지해야 함

## 조건
- 배열 arr의 크기 ≤ 1,000,000 이하의 자연수
- 0 ≤ 배열 arr의 원소의 크기 ≤ 9

## 계획
1. 숫자를 스택에 쌓는다.
2. 앞으로 쌓으려는 숫자가 스택 맨 위의 숫자와 동일하면 쌓지 않는다.
3. 스택 크기와 동일한 배열을 만든다.
4. 스택에서 숫자를 하나씩 꺼내서 배열 끝에서부터 채워넣는다.
5. 배열을 반환한다.

## 실행
```java
public int[] solution(int []arr) {
    Stack<Integer> stack = new Stack<>();
    stack.push(arr[0]);
    for(int i = 1; i < arr.length; i++) {
        int pre = stack.peek();
        int current = arr[i];
        if(pre != current) {
            stack.push(arr[i]);
        }
    }

    int[] answer = new int[stack.size()];
    for(int i = answer.length - 1; i >= 0; i--) {
        answer[i] = stack.pop();
    }

    return answer;
}
```
