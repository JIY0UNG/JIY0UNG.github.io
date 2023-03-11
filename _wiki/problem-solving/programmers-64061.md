---
layout  : wiki
title   : 프로그래머스 - 크레인 인형뽑기 게임
date    : 2023-03-12 03:00:00 +0900
updated : 2023-03-12 03:00:00 +0900
tag     : 
toc     : true
public  : true
parent  : problem-solving
latex   : false
---

* TOC
{:toc}

[크레인 인형뽑기 게임](https://school.programmers.co.kr/learn/courses/30/lessons/64061)

## 구해야 하는 것
- 크레인을 모두 작동시킨 후 터트려져 사라진 인형의 개수

## 주어진 자료
- 같은 모양의 인형 두 개가 바구니에 연속해서 쌓이면 두 인형은 모두 바구니에서 사라짐
- 인형이 없는 곳에서 크레인 작동시키면 아무 일도 일어나지 않음
- 게임 화면의 격자 상태가 담긴 2차원 배열 = board
- 인형을 집기 위해 크레인을 작동시킨 위치가 담긴 배열 = moves

## 조건
- 5 * 5 ≤ 2차원 배열 board의 크기 ≤ 30 * 30
- 0 ≤ `board[i][j]` ≤ 100
  - 0은 빈칸
  - 1 ~ 100의 각 숫자는 각기 다른 인형의 모양을 의미하며 같은 숫자는 같은 모양의 인형을 나타냄
- 1 ≤ moves 배열 크기 ≤ 1,000
- 1 ≤ `moves[i]` ≤ board 배열 가로 크기

## 계획
1. 만약 바구니의 맨 위에 있는 인형과 `moves[i]`번의 맨 위에 있는 인형이 같으면, 바구니의 맨 위에 있는 인형을 없애고 터진 인형 개수에 2를 더한다.
2. 만약 다르면 `moves[i]`번의 맨 위에 있는 인형을 바구니에 쌓는다.

## 실행
```java
public void init(int[][] board, List<Stack<Integer>> l) {
    for(int i = 0; i < board.length; i++) {
        Stack<Integer> s = new Stack<>();
        for(int j = board[i].length - 1; j >= 0; j--) {
            int current = board[j][i];
            if(current == 0) {
                break;
            }
            s.push(current);
        }
        l.add(s);
    }
}

public int pullDolls(List<Stack<Integer>> l, int[] moves) {
    int answer = 0; // 터진 인형 개수
    Stack<Integer> basket = new Stack<>();

    for(int i = 0; i < moves.length; i++) {
        int current = moves[i];
        Stack s = l.get(current - 1);
        if(!s.isEmpty()) {
            int top = (int) s.pop();
            if(!basket.isEmpty() && basket.peek() == top) {
                basket.pop();
                answer += 2;
            } else {
                basket.push(top);
            }
        }
    }

    return answer;
}

public int solution(int[][] board, int[] moves) {
    List<Stack<Integer>> l = new ArrayList<>();

    init(board, l); // 인형들 스택에 쌓아놓기
    return pullDolls(l, moves); // 인형 뽑기
}
```
