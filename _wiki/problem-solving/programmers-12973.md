---
layout  : wiki
title   : 프로그래머스 - 짝지어 제거하기
date    : 2023-04-08 15:49:42 +0900
updated : 2023-04-08 15:49:42 +0900
tag     : 
toc     : true
public  : true
parent  : problem-solving
latex   : false
---

* TOC
{:toc}

[짝지어 제거하기](https://school.programmers.co.kr/learn/courses/30/lessons/12973)

## 구해야 하는 것
- 문자열 S가 주어졌을 때, 짝지어 제거하기를 성공적으로 수행할 수 있는지 반환

## 자료
- 짝지어 제거하기 과정
  - 문자열에서 같은 알파벳이 2개 붙어 있는 짝을 찾음
  - 그 둘을 제거한 뒤, 앞뒤로 문자열을 이어 붙임
  - 이 과정을 반복해서 문자열을 모두 제거하면 종료
- 성공적으로 수행할 수 있으면 1을, 아닐 경우 0을 리턴

## 조건
- 문자열 s 길이 ≤ 1,000,000

## 계획
1. 문자열을 돌면서 문자를 하나씩 스택에 넣음
2. 스택 비어있으면 무조건 넣음
3. 넣어야 하는 문자와 스택 맨 위에 있는 문자가 같으면, 스택에서 꺼냄
4. 문자열을 다 돌았는데 스택이 비어있지 않으면 1 반환, 비어있으면 0 반환

## 실행
```java
public class Solution {
    public int solution(String s) {
        Stack<Character> stack = new Stack<>();
        stack.push(s.charAt(0));

        for (int i = 1; i < s.length(); i++) {
            char current = s.charAt(i);
            if(stack.isEmpty()) {
                stack.push(current);
                continue;
            }
            
            char top = stack.peek();
            if(current == top) {
                stack.pop();
            } else {
                stack.push(current);
            }
        }

        if(stack.isEmpty()) {
            return 1;
        }
        return 0;
    }
}
```
