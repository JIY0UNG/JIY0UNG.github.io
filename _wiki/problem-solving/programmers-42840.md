---
layout  : wiki
title   : 프로그래머스 - 모의고사
date    : 2023-02-08 13:57:00 +0900
updated : 2023-02-08 13:57:00 +0900
tag     : 
toc     : true
public  : true
parent  : problem-solving
latex   : false
---

* TOC
{:toc}

[모의고사](https://school.programmers.co.kr/learn/courses/30/lessons/42840)

## 구해야 하는 것
- 가장 많은 문제를 푼 사람

## 주어진 자료
- 수포자는 총 3명
- 1번 수포자가 찍는 방식: 1, 2, 3, 4, 5 반복
- 2번 수포자가 찍는 방식: 2, 1, 2, 3, 2, 4, 2, 5 반복
- 3번 수포자가 찍는 방식: 3, 3, 1, 1, 2, 2, 4, 4, 5, 5 반복

## 조건
- 시험 문제 개수 ≤ 10,000
- 문제의 정답은 1, 2, 3, 4, 5중 하나
- 가장 높은 점수를 받은 사람이 여럿일 경우 오름차순 해서 반환

## 계획
1. 각 수포자가 찍는 패턴을 배열로 저장해놓는다.
2. 각 수포자가 문제를 맞혔으면 각자 맞힌 개수를 증가시킨다.
3. 각 수포자용 인덱스를 증가시킨다.
4. 2 ~ 3 과정을 문제를 다 풀때까지 반복한다.
5. 최대 점수를 구한다.
6. 최대 점수와 같은 점수를 받은 사람들을 구한다.

## 실행
```java
public int[] getHighestScore(int[] count, int max) {
    ArrayList<Integer> arr = new ArrayList<>();
    for(int i = 0; i < 3; i++) {
        if(count[i] == max) {
            arr.add(i + 1);
        }
    }

    return arr.stream()
            .mapToInt(i -> i)
            .toArray();
}

public int getMax(int[] count) {
    int max = 0;
    for(int i = 0; i < 3; i++) {
        if(count[i] >= max) {
            max = count[i];
        }
    }
    return max;
}

public void solveProblem(int[] answers, int[] count) {
    int[] person1 = {1, 2, 3, 4, 5}; // 1번 수포자가 찍는 패턴
    int[] person2 = {2, 1, 2, 3, 2, 4, 2, 5}; // 2번 수포자가 찍는 패턴
    int[] person3 = {3, 3, 1, 1, 2, 2, 4, 4, 5, 5}; // 3번 수포자가 찍는 패턴
    int index1 = 0; // 1번 수포자용 인덱스
    int index2 = 0; // 2번 수포자용 인덱스
    int index3 = 0; // 3번 수포자용 인덱스

    for(int i = 0; i < answers.length; i++) {
        int current = answers[i]; // 현재 문제의 정답

        // 문제 맞혔으면 맞춘 문제 개수 증가
        if(person1[index1] == current) {
            count[0]++;
        }
        if(person2[index2] == current) {
            count[1]++;
        }
        if(person3[index3] == current) {
            count[2]++;
        }

        // 각 수포자용 인덱스 증가
        index1++;
        if(index1 == 5) {
            index1 = 0;
        }
        index2++;
        if(index2 == 8) {
            index2 = 0;
        }
        index3++;
        if(index3 == 10) {
            index3 = 0;
        }
    }
}

public int[] solution(int[] answers) {
    int[] count = new int[3]; // 맞춘 문제 개수 저장할 배열
    
    solveProblem(answers, count); // 문제 풀기
    int max = getMax(count); // 가장 높은 점수 구하기
    int[] answer = getHighestScore(count, max); // 최다 득점자들 구하기

    return answer;
}
```
