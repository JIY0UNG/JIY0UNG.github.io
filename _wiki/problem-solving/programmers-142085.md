---
layout  : wiki
title   : 프로그래머스 - 디펜스 게임
date    : 2023-01-17 17:45:00 +0900
updated : 2023-01-17 17:45:00 +0900
tag     : 
toc     : true
public  : true
parent  : problem-solving
latex   : false
---

* TOC
{:toc}

[디펜스 게임](https://school.programmers.co.kr/learn/courses/30/lessons/142085)

## 구해야 하는 것
- 준호가 몇 라운드까지 막을 수 있는지

## 주어진 자료
- 준호가 처음에 갖고 있는 병사 = n
- 매 라운드마다 `enemy[i]` 마리의 적이 등장
- 남은 병사 중 `enemy[i]`명 만큼 소모하여 `enemy[i]` 마리의 적을 막을 수 있음 → 남은 병사 = n - `enemy[i]`
- 남은 병사 수보다 현재 라운드의 적의 수가 더 많으면 게임 종료
- 무적권 쓰면 병사 소모 없이 한 라운드 공격 막을 수 있음
- 무적권은 최대 k번 사용 가능

## 조건
- 1 <= n <= 1,000,000,000
- 1 <= k <= 500,000
- 1 <= enemy의 길이 <= 1,000,000
- 1 <= enemy[i] <= 1,000,000
- enemy[i]에는 i + 1 라운드에서 공격해오는 적의 수가 담겨있음
- 모든 라운드를 막을 수 있는 경우에는 enemy[i]의 길이를 return

## 계획 1
1. 만약 k >= enemy의 길이이면 enemy의 길이를 반환
2. key = 라운드 수, value = 해당 라운드 적의 수인 map을 만듦
3. value를 내림차순으로 정렬.
4. 정렬된 map을 돌면서 k번째까지 적 제거
5. n이 0보다 작아질 때까지 map 돌면서 병사 소모하고 result++
6. result 반환

## 실행 1 → 틀림
```java
public int solution(int n, int k, int[] enemy) {
    // 만약 k >= enemy의 길이이면 enemy의 길이 반환
    if(k >= enemy.length) {
        return enemy.length;
    }

    // key = 라운드 수, value = 해당 라운드 적의 수인 map을 만듦
    HashMap<Integer, Integer> map = new HashMap<>();
    for(int i = 1; i <= enemy.length; i++) {
        map.put(i, enemy[i - 1]);
    }

    // value를 내림차순으로 정렬
    List<Integer> keySet = new ArrayList<>(map.keySet());
    keySet.sort((o1, o2) -> map.get(o2).compareTo(map.get(o1)));

    // 정렬된 map을 돌면서 k번째까지 적 제거
    for(int i = 0; i < k; i++) {
        map.put(keySet.get(i), 0);
    }

    int round = 1;
    while(n >= 0) {
        n = n - map.get(round);
        if(n <= 0) {
            return round - 1;
        }
        round++;
    }
    return round;
}
```
