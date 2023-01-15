---
layout  : wiki
title   : 프로그래머스 - 다트 게임
date    : 2022-06-15 21:06:17 +0900
updated : 2023-01-15 22:15:00 +0900
tag     : 
toc     : true
public  : true
parent  : problem-solving
latex   : false
---

* TOC
{:toc}

[다트 게임](https://programmers.co.kr/learn/courses/30/lessons/17682)

## 구해야 하는 것
- 다트 게임의 총점수

## 주어진 자료
- `점수|보너스|옵션`으로 이루어진 문자열 = dartResult

## 조건
- 다트 게임은 총 3번의 기회로 구성되며, 각 기회마다 얻을 수 있는 점수는 0 이상 10 이하의 정수이다.
- 영역
    - Single(S): 점수에서 1제곱
    - Double(D): 점수에서 2제곱
    - Triple(T): 점수에서 3제곱
    - 점수마다 존재
- 옵션
    - 스타상(*)
        - 해당 점수와 바로 전에 얻은 점수를 2배로 만듦.
        - 첫 번째 기회에서 당첨 시 첫 번째 점수만 2배.
        - 다른 스타상 효과와 중첩 가능(중첩 시 4배), 아차상 효과와 중첩 가능(중첩 시 -2배)
    - 아차상(#): 해당 점수는 마이너스
    - 스타상과 아차상 둘 중 하나만 존재. 존재하지 않을 수도 있다.

### 문제 풀이에 사용할 수 있는 유용한 지식
- n제곱을 구할 때 pow() 함수를 사용하면 좀 더 간편하게 구할 수 있겠다.
- dartResult의 현재 원소가 숫자인지 판단할 때 isdigit() 함수를 써야겠다.

## 계획
1. 각 시도에 대한 점수를 배열 score에 저장한다. score의 인덱스는 idx에 저장한다. idx는 몇 번째 시도인지를 뜻하기도 한다.
2. dartResult의 크기만큼 for문을 돌면서 탐색한다.
3. 현재 원소 dartResult[i]를 current에 저장한다.
4. current가 숫자형태의 문자이면, 이를 숫자로 변환해서 score[idx]에 저장한다.
그런데 이렇게 하면 점수가 10인 경우에는 dartResult[i]가 1일 때 score[idx]에 1이 저장됐다가, 반복자 i가 1 증가했을 때는 score[idx]에 다시 0이 저장돼서 결국 0점이 되어버린다.
따라서 dartResult[i]가 1이면 dartResult[i + 1]이 0인지 체크해서 0이면 score[idx]에 10을 저장한 후 반복자 i를 1 증가시키는 방법으로 10점인 경우를 처리해 준다.
5. current가 'S'인 경우 score[idx]를 1제곱 하면 되므로 'S'에 대한 조건문은 생략한다.
6. current가 'D'인 경우 score[idx]를 2제곱 한다.
7. current가 'T'인 경우 score[idx]를 3제곱 한다.
8. current가 '*'인 경우 score[idx]에 2를 곱한다.
만약 idx가 0이 아닌 경우(첫 번째 기회가 아닌 경우)에는 score[idx - 1]에도 2를 곱한다.
9. current가 '#'인 경우 score[idx]에 -1을 곱한다.
10. 만약 dartResult[i + 1]이 숫자이면 dartResult[i + 1]부터는 새로운 시도에 대한 점수이므로 idx를 1 증가시킨다.
11. 3.~10.반복, i가 dartResult의 크기와 같아지면 for문을 탈출한다.
12. answer에 score[0] + score[1] + score[2]의 값을 저장한 후 return 한다.

## 실행
```c
int solution(string dartResult) {
    int answer = 0;
    int score[3];
    int idx = 0;
    char current;
    
    for(int i = 0; i < dartResult.size(); i++) {
        current = dartResult[i];

        if(isdigit(current)) {
            score[idx] = current - 48; // 숫자로 변환해서 저장
            
            // 점수가 10인 경우
            if(dartResult[i] == '1' && dartResult[i + 1] == '0') {
                score[idx] = 10;
                i++;                
            }
        }
        
        if(current == 'D') {
            // 점수 2제곱
            score[idx] = pow(score[idx], 2);
        }
        if(current == 'T') {
            // 점수 3제곱
            score[idx] = pow(score[idx], 3);
        }
        
        if(current == '*') {
            // 해당 점수 2배
            score[idx] = score[idx] * 2;
            
            // 첫 번째 기회가 아닌 경우 바로 전에 얻은 점수도 2배
            if(idx != 0) {
                score[idx - 1] = score[idx - 1] * 2;
            }
        }
        if(current == '#') {
            // 해당 점수는 마이너스
            score[idx] = score[idx] * -1;
        }
        
        if(isdigit(dartResult[i + 1])) {
            idx++;
        }
    }
    
    answer = score[0] + score[1] + score[2];
    return answer;
}
```

## 반성
- dartResult 문자열을 세 부분으로 나눌 때 어떤 기준으로 나눠야 할지 좀 오래 고민했었다. 처음에는 스타상이 앞의 점수까지 영향을 미치는데, 앞의 점수는 그 뒤의 점수가 스타상에 당첨될지 모르니까 뒤에서부터 탐색해볼까 생각했었다. 근데 그렇게 하면 점수를 계산하는 순서가 잘못되므로 다른 방법을 찾아봤고, 각 시도에 대한 점수를 score 배열에 저장하고 현재 점수가 스타상에 당첨된 점수면 이전 score[i - 1]의 점수도 2배 해주는 방법을 택하게 되었다.  
이 부분을 고민하는 데에 시간이 좀 걸렸고, 나머지는 주어진 조건대로 점수 계산만 하면 돼서 쉽게 풀렸다.
- 다른 사람의 풀이 중 stringstream이라는 걸 사용한 풀이가 있었는데, 여태까지 코테 문제들을 풀면서 stringstream을 사용해 본 적이 없기도 하고 굳이 이걸 쓸 필요는 없는 것 같아서, 그냥 이런 것도 있구나~ 하고 넘어갔다.
