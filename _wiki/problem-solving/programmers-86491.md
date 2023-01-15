---
layout  : wiki
title   : 프로그래머스 - 최소직사각형
date    : 2022-06-13 19:06:23 +0900
updated : 2023-01-15 22:15:00 +0900
tag     : 
toc     : true
public  : true
parent  : problem-solving
latex   : false
---

* TOC
{:toc}

[최소직사각형](https://programmers.co.kr/learn/courses/30/lessons/86491)

## 구해야 하는 것
- 모든 명함을 수납할 수 있는 가장 작은 지갑의 크기

## 주어진 자료
- 모든 명함의 가로 길이(w)와 세로 길이(h)를 나타내는 2차원 배열 = sizes

## 조건
- sizes의 원소는 [w, h] 형식
- w와 h는 1 이상 1,000 이하인 자연수
- 지갑의 크기 = w * h
- 명함은 회전해서 수납할 수도 있다. 즉 명함의 가로 길이가 세로 길이일 수도 있고, 세로 길이가 가로 길이일 수도 있다.

## 계획 1
1. 가로 길이 중 가장 큰 것을 구해서 maxWidth, 세로 길이 중 가장 큰 것을 구해서 maxHeight을 저장한다.
2. answer에 maxWidth * maxHeight을 저장한다.
3. 세로 길이가 maxHeight와 같은 명함들의 가로와 세로 길이를 바꿔준다.
4. 다시 가로 길이 중 가장 큰 것을 구해서 maxWidth, 세로 길이 중 가장 큰 것을 구해서 maxHeight을 저장한다.
5. 기존의 answer와 4.에서 구한 maxWidth * maxHeight을 값을 비교한 후 더 작은 것을 answer에 저장한다.
6. answer를 return한다.

## 실행 1
```c
int solution(vector<vector<int>> sizes) {
    int maxWidth, maxHeight;
    int answer = 0;
    
    // 가로 길이 중 가장 큰 것 구하기
    maxWidth = sizes[0][0];
    for(int i = 1; i < sizes.size(); i++) {
        if(maxWidth <= sizes[i][0]) {
            maxWidth = sizes[i][0];
        }
    }
    
    // 세로 길이 중 가장 큰 것 구하기
    maxHeight = sizes[0][1];
    for(int i = 1; i < sizes.size(); i++) {
        if(maxHeight <= sizes[i][1]) {
            maxHeight = sizes[i][1];
        }
    }
    
    answer = maxWidth * maxHeight; // 제일 클 때의 지갑 크기
    
    // 세로 길이가 maxHeight와 같은 명함들의 가로와 세로 길이 바꿔주기
    int tmp;
    for(int i = 0; i < sizes.size(); i++) {
        if(sizes[i][1] == maxHeight) {
            tmp = sizes[i][1];
            sizes[i][1] = sizes[i][0];
            sizes[i][0] = tmp;
        }
    }
    
    // 다시 가로 길이 중 가장 큰 것 구하기
    maxWidth = sizes[0][0];
    for(int i = 1; i < sizes.size(); i++) {
        if(maxWidth <= sizes[i][0]) {
            maxWidth = sizes[i][0];
        }
    }
    
    // 세로 길이 중 가장 큰 것 구하기
    maxHeight = sizes[0][1];
    for(int i = 1; i < sizes.size(); i++) {
        if(maxHeight <= sizes[i][1]) {
            maxHeight = sizes[i][1];
        }
    }
    
    // 기존의 answer와 와 현재 width에 * height의 값을 비교한 후 더 작은 것을 answer에 저장한다.
    if(answer >= maxWidth * maxHeight) {
        answer = maxWidth * maxHeight;
    }
    return answer;
}
```

## 계획 2
첫 번째 계획으로 짠 코드가 세 번째 예시에서 실패했다. 가로와 세로의 길이를 바꿔주는 조건이 잘못된 것 같았고, 계획을 다시 세웠다.
1. 각 명함의 가로 길이와 세로 길이를 비교해서 가로 길이가 더 짧으면 세로 길이와 맞바꾼다.
2. 가로 길이 중 가장 큰 것을 구해서 maxWidth에 저장하고, 세로 길이 중 가장 큰 것을 구해서 maxHeight에 저장한다.
3. answer에 maxWidth * maxHeight을 저장한다.
4. answer을 return한다.

## 실행 2
```c
int solution(vector<vector<int>> sizes) {
    int maxWidth, maxHeight, tmp;
    int answer = 0;
    
    // 각 명함의 가로 길이와 세로 길이를 비교해서 가로 길이가 더 짧으면 세로 길이와 맞바꾼다.
    for(int i = 0; i < sizes.size(); i++) {
        if(sizes[i][0] < sizes[i][1]) {
            tmp = sizes[i][0];
            sizes[i][0] = sizes[i][1];
            sizes[i][1] = tmp;
        }
    }
    
    // 가로 길이 중 가장 큰 것 구하기
    maxWidth = sizes[0][0];
    for(int i = 1; i < sizes.size(); i++) {
        if(maxWidth <= sizes[i][0]) {
            maxWidth = sizes[i][0];
        }
    }
    
    // 세로 길이 중 가장 큰 것 구하기
    maxHeight = sizes[0][1];
    for(int i = 1; i < sizes.size(); i++) {
        if(maxHeight <= sizes[i][1]) {
            maxHeight = sizes[i][1];
        }
    }
    
    answer = maxWidth * maxHeight;
    return answer;
}
```

## 반성
- 나는 for문을 돌면서 하나하나 비교해 보는 방식으로 maxWidth나 maxHeight를 구했는데, 아래 코드와 같이 max()함수를 사용하면 훨씬 간단하게 구할 수 있다는 것을 알게 되었다.
    ```c
    for(int i = 0; i < sizes.size(); i++) {
        maxWidth = max(maxWidth, min(sizes[i][0],sizes[i][1]));
        maxHeight = max(maxHeight, max(sizes[i][0],sizes[i][1]));
    }
    ```
    이렇게 코드의 양을 줄여주는 편리한 함수들은 좀 알고 있어야겠다.
- 주어진 입출력 예시를 첫 번째와 두 번째까지만 보고 바로 계획을 세우기 시작해서 첫 번째 계획이 실패한 것 같다. 주어진 예시들을 모두 꼼꼼히 살펴보도록 해야겠다.
- '세로 길이가 maxHeight와 같은 명함들의 가로와 세로 길이를 바꿔준다.'라고 생각했었는데, 세 번째 예시를 보니 그 조건이 아닌 것 같았다.  
그래서 세 번째 예시에서는 어떤 조건에 따라서 가로와 세로 길이를 바꿔준 건지 생각하다가, 그냥 단순하게 둘 중 긴 건 왼쪽으로, 짧은 건 오른쪽으로 놓고 그중에서 제일 긴 것들을 가지고 answer를 만들어보자! 했는데 성공했다. 문제를 단순하게 생각해 본 게 도움이 된 것 같다.
