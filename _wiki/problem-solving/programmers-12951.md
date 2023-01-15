---
layout  : wiki
title   : 프로그래머스 - JadenCase 문자열 만들기
date    : 2022-10-11 16:10:00 +0900
updated : 2023-01-15 22:15:00 +0900
tag     : 
toc     : true
public  : true
parent  : problem-solving
latex   : false
---

* TOC
{:toc}

[JadenCase 문자열 만들기](https://school.programmers.co.kr/learn/courses/30/lessons/12951)

## 구해야 하는 것
- 주어진 문자열 s를 JadenCase로 바꾼 문자열

## 주어진 자료
- JadenCase란 모든 단어의 첫 문자가 대문자이고, 그 외의 알파벳은 소문자인 문자열
- 첫 문자가 알파벳이 아닐 때에는 이어지는 알파벳은 소문자로 쓰면 됨

## 조건
- 1 <= s의 길이 <= 200
- s는 알파벳과 숫자, 공백문자(" ")로 이루어져 있음
    - 숫자는 단어의 첫 문자로만 나옴
    - 숫자로만 이루어진 단어는 없음
    - 공백문자가 연속해서 나올 수 있음

## 계획
1. s의 문자를 전부 소문자로 바꾼다.
2. 공백 다음에 위치한 문자들 중 알파벳인 것들의 인덱스들을 저장해놓는다.
3. 저장해놓은 인덱스들에 위치한 문자들을 대문자로 바꾼다.

## 실행
```java
class Solution {
    public String solution(String s) {
        s = s.toLowerCase();
        
        char[] c = s.toCharArray();
        ArrayList<Integer> index = new ArrayList<>();
        
        if(isAlphabetic(c[0])) {
            index.add(0);
        }
        
        for(int i = 1; i < c.length; i++) {
            if(c[i - 1] == ' ' && isAlphabetic(c[i])) {
                index.add(i);
            }
        }
        
        for(int i = 0; i < index.size(); i++) {
            c[index.get(i)] = toUpperCase(c[index.get(i)]);
        }
        
        String answer = String.valueOf(c);
        return answer;
    }
}
```
