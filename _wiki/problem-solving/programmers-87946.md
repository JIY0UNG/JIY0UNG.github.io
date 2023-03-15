---
layout  : wiki
title   : 프로그래머스 - 피로도
date    : 2023-03-15 15:08:00 +0900
updated : 2023-03-15 15:08:00 +0900
tag     : 
toc     : true
public  : true
parent  : problem-solving
latex   : false
---

* TOC
{:toc}

[피로도](https://school.programmers.co.kr/learn/courses/30/lessons/87946)

## 구해야 하는 것
- 유저가 탐험할 수 있는 최대 던전 수

## 주어진 자료
- 최소 필요 피로도: 해당 던전을 탐험하기 위해 가지고 있어야 하는 최소한의 피로도
- 소모 피로도: 던전 탐험 후 소모되는 피로도
- 유저의 현재 피로도 = k
- 각 던전별 최소 필요 피로도, 소모 피로도가 담긴 2차원 배열 = dungeons

## 조건
- 1 ≤ k ≤ 5,000
- 1 ≤ dungeons의 세로(행) 길이 ≤ 8
- dungeons의 가로(열) 길이 = 2
- `dungeons[i]` = `["최소 필요 피로도", "소모 피로도"]`
- 1 ≤ 소모 피로도 ≤ 최소 필요 피로도 ≤ 1,000
- 서로 다른 던전의 최소 필요 피로도와 소모 피로도가 서로 같을 수 있음

## 계획
1. 던전을 탐험할 수 있는 모든 순서를 미리 뽑아놓는다.(순열)
2. 해당 순서대로 탐험했을 때 탐험할 수 있는 최대 던전 수를 구한다.
3. 최대 던전 수의 최대값을 반환한다.

## 실행
```java
List<int[]> orders = new ArrayList<>();

public void permutation(int arr[], int output[], boolean visited[], int depth, int r) {
    if(depth == r) {
        int[] order = new int[output.length];
        for(int i = 0; i < output.length; i++) {
            order[i] = output[i];
        }
        orders.add(order);
        return;
    }

    for(int i = 0; i < arr.length; i++) {
        if(!visited[i]) {
            visited[i] = true;
            output[depth] = arr[i];
            permutation(arr, output, visited, depth + 1, r);
            visited[i] = false;
        }
    }
}

public void getOrders(int[][] dungeons) {
    int[] arr = new int[dungeons.length];
    for(int i = 0; i < dungeons.length; i++) {
        arr[i] = i;
    }

    int[] output = new int[arr.length];
    boolean[] visited = new boolean[arr.length];

    permutation(arr, output, visited, 0, dungeons.length);

}

public int getMax(int max, int k, int[][] dungeons) {
    for(int i = 0; i < orders.size(); i++) {
        int[] currentOrder = orders.get(i);
        int count = 0;
        int currentFatigue = k;

        for(int j = 0; j < currentOrder.length; j++) {
            int currentNum = currentOrder[j];
            int[] currentDungeon = dungeons[currentNum]; // 현재 탐험할 던전
            int minRequiredFatigue = currentDungeon[0]; // 최소 필요 피로도
            int exhaustionFatigue = currentDungeon[1]; // 소모 피로도

            // 현재 피로도가 최소 필요 피로도 이상이고, 피로도가 소모된 이후에 남은 피로도가 0 이상이면 탐험 가능
            if((currentFatigue >= minRequiredFatigue) &&
                    (currentFatigue - exhaustionFatigue >= 0)) {
                currentFatigue -= exhaustionFatigue;
                count++;
            }
        }

        if(count >= max) {
            max = count;
        }
    }

    return max;
}

public int solution(int k, int[][] dungeons) {
    getOrders(dungeons); // 가능한 탐험 순서 구해놓기
    return getMax(-1, k, dungeons); // 탐험 가능한 최대 던전 수 구하기
}
```

## 반성
- dfs로도 풀어볼 수 있을듯
