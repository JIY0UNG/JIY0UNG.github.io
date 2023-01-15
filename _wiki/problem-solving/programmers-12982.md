---
layout  : wiki
title   : 프로그래머스 - 예산
date    : 2022-06-08 16:06:39 +0900
updated : 2023-01-15 22:15:00 +0900
tag     : 
toc     : true
public  : true
parent  : problem-solving
latex   : false
---

* TOC
{:toc}

[예산](https://programmers.co.kr/learn/courses/30/lessons/12982)

## 구해야 하는 것
- 최대 몇 개의 부서에 물품을 지원할 수 있는지 구하라

## 주어진 자료
- 부서별로 신청한 금액이 들어있는 배열 = d
- d의 길이 = 전체 부서의 개수
- 예산 = budget

## 조건
- 각 부서가 신청한 금액보다 적은 금액을 지원해줄 수는 없다
- d의 길이(전체 부서의 개수)는 1 이상 100 이하
- d의 각 원소(부서별로 신청한 금액)은 1 이상 100,000 이하의 자연수
- budget은 1 이상 10,000,000 이하의 자연수

### 주어진 자료로부터 유용한 것 이끌어 내기
- d의 길이가 100이고 d의 각 원소가 모두 100,000이면 총 신청 금액은 10,000,000이니까 int만으로 표현 가능하겠다.

### 문제 풀이에 사용할 수 있는 유용한 지식
- d를 sort()로 정렬한다.
- 반복문으로 d의 원소를 더해주면서 budget과 비교해주면 될 듯

## 계획 1
1. d를 오름차순으로 정렬한다.
2. for문을 돌면서 d의 첫번째 원소부터 sum에 더해간다.
3. sum이 budget보다 작거나 같을 때까지 2번 과정을 하는데, 그 때마다 answer도 1씩 증가시킨다.
4. sum이 budget보다 커지면 for문을 탈출하고 answer를 return한다.

## 실행 1
```c
int solution(vector<int> d, int budget) {
    int answer = 0;
    int sum = 0;

    sort(d.begin(), d.end());
    
    for(int i = 0; i < d.size(); i++) {
        if(sum > budget) {
            break;
        }
        sum += d[i];
        answer++;
    }
    
    return answer;
}
```

## 계획 2
주어진 입출력 예시 중 d = [1, 3, 2, 5, 4], budget = 9 일 때를 가지고 첫 번째 풀이에 적용해 보면,

d 정렬 => [1, 2, 3, 4, 5]  
(for문 첫 번째 반복)
1. 현재 sum = 0
2. sum이 budget보다 작으므로 계속 진행
3. sum += 1;
4. answer++;(answer = 1)

(for문 두 번째 반복)
1. 현재 sum = 1
2. sum이 budget보다 작으므로 계속 진행
2. sum += 2;
4. answer++;(answer = 2)

(for문 세 번째 반복)
1. 현재 sum = 3
2. sum이 budget보다 작으므로 계속 진행
3. sum += 3;
4. answer++;(answer = 3)

(for문 네 번째 반복)
1. 현재 sum = 6
2. sum이 budget보다 작으므로 계속 진행
3. sum += 4
4. answer++;(answer = 4)

(for문 다섯 번째 반복)
1. 현재 sum = 10
2. sum이 budget보다 크므로 탈출

이렇게 해서 answer = 4가 된다.  
하지만 1, 2, 3까지 금액을 지원해주고 나면 남는 예산은 3이고, 더이상 금액을 지원해줄 수 없기 때문에 d[3]일 때 이미 for문을 탈출했어야 한다.  
그러므로 sum += d[i]를 한 이후에 이 sum이 budget보다 큰지 비교하도록 변경해야 한다.

## 실행 2
for문 탈출 조건문의 위치만 바꿈
```c
    for(int i = 0; i < d.size(); i++) {
        sum += d[i];
        if(sum > budget) {
            break;
        }
        answer++;
    }
```

## 반성
- 정렬한 d를 돌면서 budget -= d[i] 를 해주다가 budget이 0보다 작아지면 탈출하는 방식으로도 풀 수 있을 것 같다.
- 초기 코드에 주어진 헤더들만 가지고는 내가 원하는 함수를 쓸 수 없는 경우가 종종 있었다. 오늘도 sort를 쓰려다가 선언되지 않았다면서 오류가 나길래 헤더를 그냥 `#include<bits/stdc++.h>` 이걸로 바꾸고 풀었는데, 문제를 풀고나서 다른 사람의 풀이를 찾아보니 `#include <algorithm>` 이 헤더를 추가해주면 sort를 사용할 수 있다는 걸 알게 되었다. 함수별 헤더도 잘 알아둬야 할 것 같다.
