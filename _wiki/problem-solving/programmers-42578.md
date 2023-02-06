---
layout  : wiki
title   : 프로그래머스 - 위장
date    : 2023-02-06 16:16:00 +0900
updated : 2023-02-06 16:16:00 +0900
tag     : 
toc     : true
public  : true
parent  : problem-solving
latex   : false
---

* TOC
{:toc}

[위장](https://school.programmers.co.kr/learn/courses/30/lessons/42578)

## 구해야 하는 것
- 서로 다른 옷의 조합의 수

## 주어진 자료
- 다음 날은 오늘 입은 옷과 하나라도 겹치지 않게 해야 함

## 조건
- clothes의 각 행은 [의상의 이름, 의상의 종류]로 이루어져 있음
- 1 ≤ 스파이가 가진 의상의 수 ≤ 30
- 같은 이름을 가진 의상은 존재하지 않음
- clothes의 모든 원소는 문자열로 이루어져 있음
- 1 ≤ 모든 문자열의 길이 ≤ 20
- 모든 문자열은 알파벳 소문자 또는 '_'로만 이루어져 있음
- 스파이는 하루에 최소 한 개의 의상은 입음

## 계획
문제를 보고 예전에 풀었던 백준 - 패션왕 신해빈 문제가 떠올랐다.
1. 의상 종류가 key, 종류의 개수가 value인 map을 만든다.
2. `((headgear 중 1개를 선택하는 경우의 수 + headgear를 아예 선택하지 않는 경우의 수) * (eyewear 중 1개를 선택하는 경우의 수 + eyewear를 아예 선택하지 않는 경우의 수)) - 아무것도 입지 않는 경우의 수`  
  = `(2C1 + 1) * (1C1 + 1) - 1`

## 실행
```java
public int solution(String[][] clothes) {
    Map<String, Integer> map = new HashMap<>();

    for(int i = 0; i < clothes.length; i++) {
        String key = clothes[i][1]; // 의상 종류

        if(map.get(key) == null) {
            map.put(clothes[i][1], 1);
        } else {
            map.put(clothes[i][1], map.get(key) + 1);
        }
    }

    int sum = 1; // 서로 다른 옷 조합의 수
    for(String key : map.keySet()){
        sum *= map.get(key) + 1; // 해당 의상 종류 중 1개를 선택하는 경우의 수 + 아무것도 선택하지 않는 경우의 수
    }
    sum -= 1; // 아무것도 입지 않는 경우의 수 빼줌

    return sum;
}
```
