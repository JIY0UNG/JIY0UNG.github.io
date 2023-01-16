---
layout  : wiki
title   : 프로그래머스 - 명예의 전당 (1)
date    : 2023-01-16 20:36:00 +0900
updated : 2023-01-16 20:36:00 +0900
tag     : 
toc     : true
public  : true
parent  : problem-solving
latex   : false
---

* TOC
{:toc}

[명예의 전당 (1)](https://school.programmers.co.kr/learn/courses/30/lessons/138477)

## 구해야 하는 것
- 명예의 전당의 최하위 점수

## 주어진 자료
- 출연한 가수의 점수가 지금까지 출연한 가수들의 점수 중 상위 k번째 이내이면 해당 가수의 점수를 명예의 전당에 올림
- 프로그램 시작 이후 초기에 k일까지는 모든 출연 가수의 점수가 명예의 전당에 오름
- k일 다음부터는 출연 가수의 점수가 기존의 명예의 전당 목록의 k번째 순위의 가수 점수보다 더 높으면 출연 가수의 점수가 명예의 전당에 오르게 되고 기존의 k번째 순위의 점수는 명예의 전당에서 내려오게 됨
- 명예의 전당 목록의 점수 개수 = k
- 1일부터 마지막 날까지 출연한 가수들의 점수 = score

## 조건
- 3 <= k <= 100
- 7 <= score의 길이 <= 1,000
- 0 <= score[i] <= 2,000

## 계획
1. 현재 출연 가수의 점수를 명예의 전당 리스트에 넣는다.
2. k일이 지난 후에는 명예의 전당 목록 중 k번째 순위 점수와 비교한다.
3. 현재 출연 가수의 점수가 더 높으면 k번째 순위 점수를 제거한 후 현재 출연 가수의 점수를 넣는다.
4. 리스트를 정렬해서 현재 최하위 점수를 answer에 넣는다.
5. answer를 반환한다.

## 실행
```java
public int[] solution(int k, int[] score) {
    int[] answer = new int[score.length];
    ArrayList<Integer> l = new ArrayList<>(); // 명예의 전당

    for(int i = 0; i < score.length; i++) {
        int cur = score[i];

        if(l.size() < k) {
            l.add(cur); // k일까지는 모두 명예의 전당에 오름
        } else {
            int min = l.get(0); // 현재 명예의 전당 최하위 점수

            if(cur > min) {
                l.remove(0); // 기존의 k번째 순위의 점수 제거
                l.add(cur); // 현재 출연 가수의 점수가 명예의 전당에 오름
            }
        }
        Collections.sort(l); // 명예의 전당 순위 정렬
        answer[i] = l.get(0);
    }

    return answer;
}
```

## 반성
처음에는 k일 이후부터 명예의 전당 자리를 교체할 때, 당일 출연자의 점수가 명예의 전당 최상위 점수보다 큰지도 비교했었다. 그런데 위에 내가 정리해놓은 '주어진 자료'를 다시 살펴본 후 해당 로직은 필요 없다는 걸 깨달았다. 주어진 자료를 정리하는 이유는 많은 자료들 중 문제 풀이에 활용할 자료만 뽑아내기 위함이라는 것을 잊지 말자
