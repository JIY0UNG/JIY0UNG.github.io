---
layout  : wiki
title   : 프로그래머스 - 크기가 작은 부분 문자열
date    : 2023-02-14 17:15:00 +0900
updated : 2023-02-14 17:15:00 +0900
tag     : 
toc     : true
public  : true
parent  : problem-solving
latex   : false
---

* TOC
{:toc}

[크기가 작은 부분 문자열](https://school.programmers.co.kr/learn/courses/30/lessons/147355)

## 구해야 하는 것
- 부분문자열이 나타내는 수가 p가 나타내는 수보다 작거나 같은 것이 나오는 횟수

## 주어진 자료
- t="3141592"이고 p="271"인 경우 t의 길이가 3인 부분 문자열은 314, 141, 415, 159, 592

## 조건
- 1 ≤ p의 길이 ≤ 18
- p의 길이 ≤ t의 길이 ≤ 10,000
- t와 p는 숫자로만 이루어진 문자열이며, 0으로 시작하지 않음

## 계획 1
1. p의 길이를 구한다.
2. 만약 p의 길이가 3이면 t를 p의 길이만큼 자른다.
  - t의 0번째 인덱스부터 2번째 인덱스까지
3. 자른 문자열이 p보다 작거나 같으면 result를 증가시킨다.

## 실행 1 → 런타임 에러
```java
public int solution(String t, String p) {
    int length = p.length();
    int pNum = Integer.parseInt(p);
    int answer = 0;

    for(int i = length; i <= t.length(); i++) {
        String s = t.substring(i - length, i);
        int sNum = Integer.parseInt(s);

        if(sNum <= pNum) {
            answer++;
        }
    }
    return answer;
}
```

## 계획 2
질문하기에 런타임 오류가 나는 경우 int 타입을 long 타입으로 바꿔보라는 말이 있어서 그렇게 해봤다.

## 실행 2
```java
public int solution(String t, String p) {
    int length = p.length();
    long pNum = Long.parseLong(p);
    int answer = 0;

    for(int i = length; i <= t.length(); i++) {
        String s = t.substring(i - length, i);
        long sNum = Long.parseLong(s);

        if(sNum <= pNum) {
            answer++;
        }
    }
    return answer;
}
```
