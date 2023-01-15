---
layout  : wiki
title   : 프로그래머스 - 귤 고르기
date    : 2023-01-03 17:35:00 +0900
updated : 2023-01-15 22:15:00 +0900
tag     : 
toc     : true
public  : true
parent  : problem-solving
latex   : false
---

* TOC
{:toc}

[귤 고르기](https://school.programmers.co.kr/learn/courses/30/lessons/138476)

## 구해야 하는 것
- 귤 k개를 고를 때 크기가 서로 다른 종류의 최솟값

## 주어진 자료
- 귤 8개의 크기가 1, 3, 2, 5, 4, 5, 2, 3일 때 6개를 고르고 싶다면 3, 2, 5, 5, 2, 3 이렇게 고르면 서로 다른 종류가 3개로 최소임
- 한 상자에 담으려는 귤의 개수 = k
- 귤의 크기를 담은 배열 = tangerine

## 조건
- 1 <= k <= tangerine의 길이 <= 100,000
- 1 <= tangerine의 원소 <= 10,000,000

## 계획
1. 귤의 크기를 key로 하고, 개수를 value에 저장한다.
2. 개수가 많은 순으로 정렬한다.
3. k가 0이 될 때까지 value를 뺀다.
4. 다음 key로 넘어가면 result를 1 더해준다.
5. result를 return한다.

## 실행
```java
public int solution(int k, int[] tangerine) {
    Map<Integer, Integer> map = new HashMap<>();
    
    for(int i = 0; i < tangerine.length; i++) {
        int key = tangerine[i];

        if(map.get(key) == null) {
            map.put(key, 1);
        } else {
            map.put(key, map.get(key) + 1);
        }
    }

    List<Map.Entry<Integer, Integer>> entryList = new LinkedList<>(map.entrySet());
    entryList.sort((o1, o2) -> o2.getValue() - o1.getValue()); // 내림차순 정렬

    int result = 0;
    for(Map.Entry<Integer, Integer> entry : entryList){
        if(k <= 0) {
            break;
        }
        k -= entry.getValue();
        result++;
    }

    return result;
}
```
