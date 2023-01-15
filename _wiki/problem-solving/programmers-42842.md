---
layout  : wiki
title   : 프로그래머스 - 카펫
date    : 2022-06-27 16:20:04 +0900
updated : 2023-01-15 22:15:00 +0900
tag     : 
toc     : true
public  : true
parent  : problem-solving
latex   : false
---

* TOC
{:toc}

[카펫](https://programmers.co.kr/learn/courses/30/lessons/42842)

## 구해야 하는 것
- 갈색 격자의 수, 노란색 격자의 수가 주어졌을 때 카펫의 가로, 세로 크기

## 주어진 자료
- 갈색 격자의 수 = brown
- 노란색 격자의 수 = yellow

## 조건
- brown은 8 이상 5,000 이하의 자연수
- yellow는 1 이상 2,000,000 이하의 자연수
- 카펫의 가로 길이는 세로 길이와 같거나, 세로 길이보다 길다.

## 계획 1
두 수의 곱이 (brown + yellow)이면서 차이가 가장 작은 두 수를 구해야 한다.
1. brown과 yellow의 합을 sum에 저장한다.
2. 곱했을 때 12가 되는 두 수의 차의 절대값을 sub에 저장하기로 한다. 초기값은 2,005,000으로 잡아놓는다.
3. i = 1부터 sum / 2까지 for문을 돌면서 i와 곱했을 때 sum이 되는 두 수 num1, num2를 구한다.
4. 이때 두 수의 차의 절대값이 sub보다 작으면 sub에 (num1 - num2)의 절대값을 저장한다.  
그리고 answer에 num1, num2를 저장한다.(이때 더 큰 숫자를 먼저 저장)
5. for문 탈출 후 answer를 return 한다.

## 실행 1 → 틀림
```c
vector<int> solution(int brown, int yellow) {
    vector<int> answer = {0, 0};
    int sub = 2005000;
    int sum = brown + yellow;

    for(int num1 = 1; num1 <= sum / 2; num1++) {        
        if(sum % num1 == 0) {
            int num2 = sum / num1;

            if(num1 > num2) { 
                break;
            }

            // 두 수의 차이가 가장 적어야 함
            if(num2 - num1 < sub) {
                sub = num2 - num1;
                
                answer[0] = num2;
                answer[1] = num1;
            }
        }
    }

    return answer;
}
```

## 계획 2
첫 번째 계획에서는 중앙에 노란색이 칠해져있다는 것을 고려하지 않았었다.  
그래서 만약 broan = 18, yellow = 6인 경우 첫 번째 풀이에 따르면 가로 = 6, 세로 = 4가 나오게 되는데, 이는 노란색이 중앙에 칠해져있지 않는 모양이다. 따라서 가로 = 8, 세로 = 3이어야 바른 모양이 된다.

그래서 answer에 값을 넣는 조건을 '가로와 세로의 차이가 가장 적을 때'가 아니라 '노란색이 중앙에 칠해져있는 모양일 때'로 수정했다.
1. brown과 yellow의 합을 sum에 저장한다.
2. i = 1부터 sum / 2까지 for문을 돌면서 i와 곱했을 때 sum이 되는 두 수 num1, num2를 구한다.
3. 이때 (num1 - 2) * (num2 - 2) == yellow이면 answer에 num1, num2를 저장한다.  
num2가 가로이므로 num2를 먼저 저장
4. for문 탈출 후 answer를 return 한다.

## 실행 2
```c
vector<int> solution(int brown, int yellow) {
    vector<int> answer = {0, 0};
    int sum = brown + yellow;
    
    for(int num1 = 1; num1 <= sum / 2; num1++) {        
        if(sum % num1 == 0) {
            int num2 = sum / num1;

            if(num1 > num2) { 
                break;
            }

            // yellow가 중앙에 있어야 함
            if((num1 - 2) * (num2 - 2) == yellow) {
                answer[0] = num2;
                answer[1] = num1;
            }
        }
    }
    
    return answer;
}
```

## 반성
- 그림이 주어진 문제는 그림에서 숨겨진 조건을 찾을 수 있도록 주의해야겠다.
