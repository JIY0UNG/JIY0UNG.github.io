---
layout  : wiki
title   : 프로그래머스 - 올바른 괄호
date    : 2023-04-08 15:26:33 +0900
updated : 2023-04-08 15:26:33 +0900
tag     : 
toc     : true
public  : true
parent  : problem-solving
latex   : false
---

* TOC
{:toc}

[올바른 괄호](https://school.programmers.co.kr/learn/courses/30/lessons/12909)

## 구해야 하는 것
- 문자열 s가 올바른 괄호이면 true를 return 하고, 올바르지 않은 괄호이면 false를 return 

## 자료
- 올바르게 짝지어진 괄호 =  '(' 문자로 열렸으면 반드시 짝지어서 ')' 문자로 닫힘

## 조건
- 문자열 s의 길이 ≤ 100,000

## 계획
1. 문자열을 돌다가 `(`를 만나면 스택에 넣는다.
2. `)`를 만나면 스택에서 뺀다.
3. 만약 `)`를 만났는데 스택이 비어있거나, 문자열을 끝까지 돌았는데 스택이 비어있지 않으면 false 반환

## 실행
```java
public class Solution {
    boolean solution(String s) {
        Stack<Character> stack = new Stack();
        for (int i = 0; i < s.length(); i++) {
            char current = s.charAt(i);
            if(current == '(') {
                stack.push(current);
            } else if(current == ')') {
                if(stack.isEmpty()) {
                    return false;
                }
                stack.pop();
            }
        }

        if(!stack.isEmpty()) {
            return false;
        }
        return true;
    }
}
```
