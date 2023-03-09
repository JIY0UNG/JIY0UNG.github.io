---
layout  : wiki
title   : 프로그래머스 - 실패율
date    : 2023-03-09 18:16:00 +0900
updated : 2023-03-09 18:16:00 +0900
tag     : 
toc     : true
public  : true
parent  : problem-solving
latex   : false
---

* TOC
{:toc}

[실패율](https://school.programmers.co.kr/learn/courses/30/lessons/42889)

## 구해야 하는 것
- 실패율이 높은 스테이지부터 내림차순으로 스테이지의 번호가 담겨있는 배열

## 주어진 자료
- 실패율 = 스테이지에 도달했으나 아직 클리어하지 못한 플레이어의 수 / 스테이지에 도달한 플레이어 수
- 전체 스테이지 개수 = N
- 게임을 이용하는 사용자가 현재 멈춰있는 스테이지의 번호가 담긴 배열 = stages

## 조건
- 1 ≤ N ≤ 500
- 1 ≤ stages의 길이 ≤ 200,000
- 1 ≤ `stages[i]` ≤ N + 1
  - `stages[i]` = 사용자가 현재 도전 중인 스테이지 번호
  - N + 1은 N번째 스테이지까지 클리어한 사용자
- 실패율이 같은 스테이지가 있으면 작은 번호의 스테이지가 먼저 오도록
- 스테이지에 도달한 유저가 없는 경우 해당 스테이지의 실패율은 0

## 계획
N = 5, stages = [2, 1, 2, 6, 2, 4, 3, 3]인 경우

stages의 길이 = 사용자 수  
현재 1번 스테이지에 머물러 있는 사람은 1명.  
1번 스테이지에 도달한 사람 수 = 8, 아직 클리어하지 못한 사람 수 = 1  
따라서 1번 스테이지의 실패율 = 1/8

그러면 이제 고려할 사용자 수는 7명이 되었다.  
2번 스테이지에 머물러 있는 사람은 3명.  
2번 스테이지에 도달한 사람 수 = 7, 아직 클리어하지 못한 사람 수 = 3  
따라서 2번 스테이지의 실패율 = 3/7

이런식으로 각 스테이지의 실패율을 모두 구하고, 실패율의 내림차순으로 정렬하면 될 것 같다.

1. key = 스테이지의 번호, value = 스테이지에 머물러있는 사람 수인 맵을 만든다.
2. stages의 길이로 사용자 수를 구한다.
3. 1번 스테이지의 실패율을 구한다. 만약 스테이지에 도달한 유저가 없으면 실패율은 0.
4. 실패율을 key = 스테이지 번호, value = 실패율인 맵에 넣는다.
5. 사용자 수 =- 해당 스테이지에 머물러있는 사람 수
6. 3 ~ 4 과정을 N번 반복한다.
7. 각 실패율이 저장돼있는 맵을 내림차순으로 정렬한다.
8. 정렬된 맵의 key를 순서대로 배열에 넣고 반환한다.

## 실행
```java
public void initMap(Map<Integer, Double> map, int[] stages) {
    for(int i = 1; i <= stages.length; i++) {
        int key = stages[i - 1];
        if(map.get(key) == null) {
            map.put(key, 1.0);
        } else {
            map.put(key, map.get(key) + 1);
        }
    }
}

public void getFailure(int N, double userCount, Map<Integer, Double> stagesMap, Map<Integer, Double> failureMap) {
    for(int i = 0; i < N; i++) {
        double failure = 0;
        if(stagesMap.get(i + 1) != null) {
            failure = stagesMap.get(i + 1) / userCount;
            userCount -= stagesMap.get(i + 1);
        }
        failureMap.put(i + 1, failure);
    }
}

public int[] solution(int N, int[] stages) {
    Map<Integer, Double> stagesMap = new HashMap<>();
    initMap(stagesMap, stages);

    double userCount = stages.length;
    Map<Integer, Double> failureMap = new HashMap<>();
    getFailure(N, userCount, stagesMap, failureMap);

    List<Integer> keySet = new ArrayList<>(failureMap.keySet());

    keySet.sort(new Comparator<Integer>() {
        @Override
        public int compare(Integer o1, Integer o2) {
            return failureMap.get(o2).compareTo(failureMap.get(o1));
        }
    });

    int[] answer = new int[keySet.size()];
    for(int i = 0; i < keySet.size(); i++) {
        answer[i] = keySet.get(i);
    }
    return answer;
}
```
