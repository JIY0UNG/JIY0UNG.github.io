---
layout  : wiki
title   : 프로그래머스 - 푸드 파이트 대회
date    : 2023-01-04 11:42:00 +0900
updated : 2023-01-15 22:15:00 +0900
tag     : 
toc     : true
public  : true
parent  : problem-solving
latex   : false
---

* TOC
{:toc}

[푸드 파이트 대회](https://school.programmers.co.kr/learn/courses/30/lessons/134240)

## 구해야 하는 것
- 대회의 조건을 고려한 음식의 배치를 나타내는 문자열

## 주어진 자료
- 각 선수는 양쪽 끝에서부터 순서대로 음식을 먹음. 이때 두 선수가 먹는 음식의 종류와 양, 순서가 모두 같아야 함
- 중앙에는 물을 배치하고 물을 먼저 먹는 사람이 승리
- 칼로리가 낮은 음식부터 배치함. 즉 안쪽에 있는 음식일수록 칼로리가 높아짐
- 위 대회의 조건에 따라 배치한 후 남은 음식은 그냥 버림

## 조건
- 2 <= food의 길이 <= 9
- 1 <= food의 각 원소 <= 1,000
- food에는 칼로리가 적은 순서대로 음식의 양이 담겨 있음
- `food[i]`는 i번 음식의 수
- `food[0]`은 수웅이가 준비한 물의 양이며, 항상 1
- 정답의 길이가 3 이상인 경우만 입력으로 주어짐

## 실행
```java
public String solution(int[] food) {
    int cnt = 1;
    // 홀수면 1개 빼줌
    for(int i = 1; i < food.length; i++) {
        if(food[i] % 2 != 0) {
            food[i]--;
        }
        cnt += food[i];
    }

    int[] answer = new int[cnt];
    // answer 배열의 양끝에서부터 food[i] 채워줌
    int start = 1;
    for(int i = 1; i < food.length; i++) {
        for(int j = 0; j < food[i] / 2; j++) {
            answer[start - 1] = i;
            answer[answer.length - start] = i;
            start++;
        }
    }

    StringBuilder sb = new StringBuilder();
    for(int i = 0; i < answer.length; i++) {
        sb.append(answer[i]);
    }
    return sb.toString();
}
```
