---
layout  : wiki
title   : 프로그래머스 - 카드 뭉치
date    : 2023-03-08 19:09:00 +0900
updated : 2023-03-08 19:09:00 +0900
tag     : 
toc     : true
public  : true
parent  : problem-solving
latex   : false
---

* TOC
{:toc}

[카드 뭉치](https://school.programmers.co.kr/learn/courses/30/lessons/159994)

## 구해야 하는 것
- 규칙에 따라 카드에 적힌 단어들을 사용해 원하는 순서의 단어 배열을 만들 수 있는지 여부

## 주어진 자료
- 규칙
  - 원하는 카드 뭉치에서 카드를 순서대로 한 장씩 사용
  - 한 번 사용한 카드는 다시 사용 불가
  - 카드를 사용하지 않고 다음 카드로 넘어갈 수 없음
  - 기존에 주어진 카드 뭉치의 단어 순서는 바꿀 수 없음
- 문자열로 이루어진 배열 = cards1, cards2
- 원하는 단어 배열 = goal
- 원하는 단어 배열 만들 수 있으면 "Yes", 만들 수 없으면 "No" 반환

## 조건
- 1 ≤ cards1의 길이, cards2의 길이 ≤ 10
- 1 ≤ `cards1[i]`의 길이, `cards2[i]`의 길이 ≤ 10
- cards1과 cards2는 서로 다른 단어만 존재
- 2 ≤ goal의 길이 ≤ cards1의 길이 + cards2의 길이
- 1 ≤ `goal[i]`의 길이 ≤ 10
- goal의 원소는 cards1과 cards2의 원소들로만 이루어져 있음
- 문자열들은 모두 알파벳 소문자로만 이루어져 있음

## 계획
1. cards1과 cards2의 초기 인덱스 = 0
2. 각 인덱스가 배열 길이보다 작고, `cards1[idx1]`과 `cards2[idx2]` 중 `goal[idx]`과 일치하는 것이 있으면 goal의 인덱스 증가, 일치하는 card의 인덱스 증가
3. 현재 `goal[idx]`와 일치하는 것이 없으면 "No" return

## 실행
```java
public String solution(String[] cards1, String[] cards2, String[] goal) {
    int idx1 = 0;
    int idx2 = 0;

    for(int i = 0; i < goal.length; i++) {
        String cur = goal[i];

        if((idx1 < cards1.length) && (cards1[idx1].equals(cur))) {
            idx1++;
        } else if((idx2 < cards2.length) && (cards2[idx2].equals(cur))) {
            idx2++;
        } else {
            return "No";
        }
    }
    return "Yes";
}
```
