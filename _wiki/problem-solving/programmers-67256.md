---
layout  : wiki
title   : 프로그래머스 - 키패드 누르기
date    : 2023-03-10 14:50:00 +0900
updated : 2023-03-10 14:50:00 +0900
tag     : 
toc     : true
public  : true
parent  : problem-solving
latex   : false
---

* TOC
{:toc}

[키패드 누르기](https://school.programmers.co.kr/learn/courses/30/lessons/67256)

## 구해야 하는 것
- 각 번호를 누른 엄지손가락이 왼손인 지 오른손인 지 나타내는 연속된 문자열

## 주어진 자료
- 맨 처음 왼손 엄지손가락은 * 키패드에, 오른손 엄지손가락은 # 키패드 위치에서 시작
- 엄지손가락을 사용하는 규칙
  - 엄지손가락은 상하좌우로만 이동가능
  - 키패드 이동 한 칸은 거리로 1에 해당
  - 1, 4, 7은 왼손 엄지손가락 사용
  - 3, 6, 9는 오른손 엄지손가락 사용
  - 2, 5, 8, 0은 두 엄지손가락 중 더 가까운 엄지손가락 사용
    - 만약 두 엄지손가락 거리 같으면 오른손잡이는 오른손, 왼손잡이는 왼손 엄지손가락 사용
- 순서대로 누를 번호가 담긴 배열 = numbers
- 왼손잡이인지 오른손잡이인지 나타내는 문자열 = hand

## 조건
- 1 ≤ numbers 배열 크기 ≤ 1,000
- 0 ≤ `numbers[i]` ≤ 9
- 왼손 엄지손가락을 사용한 경우는 L, 오른손 엄지손가락을 사용한 경우는 R을 순서대로 이어붙여 문자열 형태로 반환

## 계획
numbers = [1, 3, 4, 5, 8, 2, 1, 4, 5, 9, 5], hand = "right"인 경우

1. 왼손 위치: *, 오른손 위치: #
2. 현재 눌러야 하는 숫자 = 1 → 왼손 사용
3. 현재 눌러야 하는 숫자 = 3 → 오른손 사용
4. 현재 눌러야 하는 숫자 = 4 → 왼손 사용
5. 현재 눌러야 하는 숫자 = 5  
  현재 왼손 위치: 4, 오른손 위치: 3  
  왼손으로부터 거리 = 1  
  오른손으로부터 거리 = 2  
  따라서 왼손으로 누름  
6. 현재 눌러야 하는 숫자 = 8  
  현재 왼손 위치: 5, 오른손 위치: 3  
  왼손으로부터 거리 = 1  
  오른손으로부터 거리 = 3  
  따라서 왼손으로 누름  
7. 현재 눌러야 하는 숫자 = 2  
  현재 왼손 위치: 8, 오른손 위치: 3  
  왼손으로부터 거리 = 2  
  오른손으로부터 거리 = 1  
  따라서 오른손으로 누름  
...

근데 현재 손 위치로부터 눌러야 하는 번호까지 거리를 어떻게 구해야할지 모르겠다..  
그래서 다른 분의 풀이를 참고해서 계획을 세웠다.

1. `numbers[i]`가 1, 4, 7이면 문자열에 L 이어붙인다.
2. `numbers[i]`가 3, 6, 9면 문자열에 R 이어붙인다.
3. `numbers[i]`가 2, 5, 8, 0이면 현재 두 손으로부터 거리를 구한다.  
  `|(눌러야 하는 번호 - 현재 손 위치)| / 3 + |(눌러야 하는 번호 - 현재 손 위치)| % 3`
4. 거리가 더 짧은 손이 왼손이면 L을 이어붙이고, 오른손이면 R을 이어붙인다.
5. 만약 두 손의 거리가 같으면, hand가 left면 L, right면 R을 이어붙인다.
6. 누른 번호로 손의 위치를 이동시킨다.

## 실행
```java
public String solution(int[] numbers, String hand) {
    String answer = "";
    int left = 10; // *
    int right = 12; // #

    for(int i = 0; i < numbers.length; i++) {
        int current = numbers[i];
        if(current == 0) {
            current = 11;
        }

        if(current == 1 || current == 4 || current == 7) {
            answer += "L";
            left = current;
        } else if(current == 3 || current == 6 || current == 9) {
            answer += "R";
            right = current;
        } else if(current == 2 || current == 5 || current == 8 || current == 11) {
            int leftDis = Math.abs(current - left) / 3 + Math.abs(current - left) % 3;
            int rightDis = Math.abs(current - right) / 3 + Math.abs(current - right) % 3;

            if(leftDis == rightDis) {
                if(hand.equals("left")) {
                    answer += "L";
                    left = current;
                } else if(hand.equals("right")){
                    answer += "R";
                    right = current;
                }
            } else if(leftDis < rightDis) {
                answer += "L";
                left = current;
            } else if(leftDis > rightDis) {
                answer += "R";
                right = current;
            }
        }
    }

    return answer;
}
```

## 반성
- 예전부터 몇 번씩 시도해 봤지만 결국 못 풀었던 문제다. 거리 구하는 식을 생각해내는 게 너무 어려웠다. 나중에 다시 한번 풀어봐야 할 것 같다.
