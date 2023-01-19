---
layout  : wiki
title   : 프로그래머스 - 과일 장수
date    : 2023-01-19 10:43:00 +0900
updated : 2023-01-19 10:43:00 +0900
tag     : 
toc     : true
public  : true
parent  : problem-solving
latex   : false
---

* TOC
{:toc}

[과일 장수](https://school.programmers.co.kr/learn/courses/30/lessons/135808)

## 구해야 하는 것
- 과일 장수가 얻을 수 있는 최대 이익

## 주어진 자료
- 사과 한 상자의 가격 책정
  - 한 상자에 사과를 m개씩 담아 포장
  - 상자에 담긴 사과 중 가장 낮은 점수가 p(1 <= p <= k)인 경우, 사과 한 상자의 가격은 p * m
- 사과는 상자 단위로만 판매하며, 남은 사과는 버림

## 조건
- 3 ≤ k ≤ 9
- 3 ≤ m ≤ 10
- 7 ≤ score의 길이 ≤ 1,000,000
- 1 ≤ score[i] ≤ k
- 이익이 발생하지 않는 경우에는 0 반환

## 계획 1
1. score를 내림차순으로 정렬
2. 0번 인덱스부터 m개만큼 선택(인덱스 m - 1까지)
3. 한 상자 가격 = (m - 1)번째 사과의 점수 * m
4. 한 상자 가격을 result에 더함
5. 그 다음은 (0 + m)번 인덱스부터 m개만큼 선택
6. 위 과정 반복한 후 result 반환

## 실행 1 → 시간 초과
```java
public int solution(int k, int m, int[] score) {
    ArrayList<Integer> arr = new ArrayList<>();
    for(int i = 0; i < score.length; i++) {
        arr.add(score[i]);
    }
    arr.sort(Comparator.reverseOrder());

    int min = k + 1;
    int answer = 0;
    while(arr.size() >= m) {
        int cnt = 0;
        while(cnt < m) {
            int cur = arr.get(0);
            if(cur < min) {
                min = cur;
            }
            arr.remove(0);
            cnt++;
        }
        answer += (min * m);
    }

    return answer;
}
```

## 계획 2
while문이 2개라서 시간 초과가 나는 것 같아, 내부 로직은 거의 유사하고 while문만 1개로 바꿨다.

## 실행 2 → 시간 초과
```java
public int solution(int k, int m, int[] score) {
    ArrayList<Integer> arr = new ArrayList<>();
    for(int i = 0; i < score.length; i++) {
        arr.add(score[i]);
    }
    arr.sort(Comparator.reverseOrder());

    int min = k + 1;
    int answer = 0;
    int cnt = 0;
    while(arr.size() >= 0) {
        if(cnt == m) {
            answer += (min * m);
            cnt = 0;
            min = k + 1;
        }
        if(arr.size() == 0) {
            break;
        }
        int cur = arr.get(0);
        if(cur < min) {
            min = cur;
        }
        arr.remove(0);
        cnt++;
    }

    return answer;
}
```

## 계획 3
접근을 다르게 해봐야할 것 같았다. 그래서 처음에 score를 정렬해놓고, 인덱스만으로 한 상자의 가격을 구할 수 있도록 해봤다.

한 상자의 가격 = (끝에서 m번째 인덱스 점수) * m  
그 다음 상자의 가격 = (끝에서 m - m번째 인덱스 점수) * m  
...

## 실행 3
```java
public int solution(int k, int m, int[] score) {
    Arrays.sort(score);

    int answer = 0;
    int idx = score.length - m; // 한 상자 내에서 점수 가장 작은 사과의 인덱스
    int repeat = (score.length / m); // 반복할 횟수

    for(int i = 0; i < repeat; i++) {
        answer += (score[idx] * m);
        idx -= m;
    }

    return answer;
}
```

## 반성
문제를 풀다 보면 계속 풀리지 않는데도 불구하고 거기에 매몰되어버리는 경우가 종종 생긴다. 내가 그런 상태에 놓였다는 것을 인지했을 때, '좀만 더 해보자'보다 그냥 아예 싹 갈아엎고 처음부터 다시 하는 게 뇌를 환기 시키는 데에 도움이 되는 것 같다.
