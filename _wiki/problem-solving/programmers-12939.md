---
layout  : wiki
title   : 프로그래머스 - 최댓값과 최솟값
date    : 2023-03-31 00:03:00 +0900
updated : 2023-03-31 00:03:00 +0900
tag     : 
toc     : true
public  : true
parent  : problem-solving
latex   : false
---

* TOC
{:toc}

[최댓값과 최솟값](https://school.programmers.co.kr/learn/courses/30/lessons/12939)

## 구해야 하는 것
- str에 나타나는 숫자 중 최소값과 최대값을 찾아 이를 `"(최소값) (최대값)"`형태의 문자열을 반환

## 조건
- s에는 둘 이상의 정수가 공백으로 구분되어 있음

## 계획
1. 공백을 기준으로 구분한 숫자들을 배열에 넣는다.
2. 배열을 오름차순으로 정렬한다.
3. 배열의 첫 번째 숫자와 마지막 숫자를 문자열에 넣어서 반환한다.

## 실행
```java
public class Solution {
    public String solution(String s) {
        StringTokenizer st = new StringTokenizer(s, " ");
        List<Integer> list = new ArrayList<>();
        while(st.hasMoreTokens()) {
            list.add(Integer.parseInt(st.nextToken()));
        }

        Collections.sort(list);
        return list.get(0) + " " + list.get(list.size() - 1);
    }
}
```
