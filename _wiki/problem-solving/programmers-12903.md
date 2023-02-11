---
layout  : wiki
title   : 프로그래머스 - 가운데 글자 가져오기
date    : 2023-02-11 13:37:00 +0900
updated : 2023-02-11 13:37:00 +0900
tag     : 
toc     : true
public  : true
parent  : problem-solving
latex   : false
---

* TOC
{:toc}

[가운데 글자 가져오기](https://school.programmers.co.kr/learn/courses/30/lessons/12903)

## 구해야 하는 것
- 단어의 가운데 글자

## 주어진 자료
- 단어의 길이가 짝수면 가운데 두글자를 반환

## 조건
- 1 ≤ s의 길이 ≤ 100

## 계획
1. 주어진 단어의 길이를 구한다.
2. 단어의 길이가 짝수면 (길이 / 2 - 1), (길이 / 2) 인덱스 위치의 문자열을 반환한다.
3. 단어의 길이가 홀수면 (길이 / 2) 인덱스 위치의 문자를 반환한다.

## 실행
```java
public String solution(String s) {
    int length = s.length();
    String answer = "";

    if(length % 2 == 0) {
        answer += s.charAt((length / 2) - 1);
        answer += s.charAt((length / 2));
    } else {
        answer += s.charAt(length / 2);
    }
    return answer;
}
```
