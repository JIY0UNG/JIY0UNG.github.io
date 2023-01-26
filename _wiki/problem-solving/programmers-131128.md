---
layout  : wiki
title   : 프로그래머스 - 숫자 짝꿍
date    : 2023-01-26 17:43:00 +0900
updated : 2023-01-26 17:43:00 +0900
tag     : 
toc     : true
public  : true
parent  : problem-solving
latex   : false
---

* TOC
{:toc}

[숫자 짝꿍](https://school.programmers.co.kr/learn/courses/30/lessons/131128)

## 구해야 하는 것
- X, Y의 짝꿍

## 주어진 자료
- 두 수의 짝꿍 = 두 정수 X, Y의 임의의 자리에서 공통으로 나타나는 정수 k들을 이용하여 만들 수 있는 가장 큰 정수(단, 공통으로 나타나는 정수 중 서로 짝지을 수 있는 숫자만 사용)
- X, Y의 짝꿍이 존재하지 않으면 짝꿍은 -1
- X, Y의 짝꿍이 0으로만 구성되어 있으면 짝꿍은 0

## 조건
- 3 ≤ X, Y의 길이(자릿수) ≤ 3,000,000
- X, Y는 0으로 시작하지 않음
- X, Y의 짝꿍은 상당히 큰 정수일 수 있으므로, 문자열로 반환

## 계획
1. 배열 xNum과 yNum에 X와 Y를 구성하고 있는 숫자 개수를 저장한다.
2. 배열 common에 공통 정수의 개수를 저장한다.
  - `xNum[i] == yNum[i]`이면 `xNum[i]`를 `common[i]`에 저장
  - `xNum[i] != yNum[i]`이면 더 작은 수를 `common[i]`에 저장
3. 공통 정수로 가장 큰 수를 만든다.
  - 배열 common을 뒤에서부터 돌면서 해당 인덱스의 값이 0이 될 때까지 문자열에 인덱스값을 넣는다.
  - 인덱스 값이 0이 됐으면 인덱스를 감소시킨다.

## 실행
```java
public String solution(String x, String y) {
    int[] xNum = new int[10];
    int[] yNum = new int[10];
    int[] common = new int[10];

    // X와 Y를 구성하고 있는 숫자 개수 저장하기
    for(int i = 0; i < x.length(); i++) {
        int cur = Character.getNumericValue(x.charAt(i));
        xNum[cur]++;
    }
    for(int i = 0; i < y.length(); i++) {
        int cur = Character.getNumericValue(y.charAt(i));
        yNum[cur]++;
    }

    // 공통 정수의 개수 저장하기
    for(int i = 0; i < 10; i++) {
        if(xNum[i] > 0 && yNum[i] > 0) {
            if(xNum[i] == yNum[i]) {
                common[i] = xNum[i];
            } else {
                if(xNum[i] > yNum[i]) {
                    common[i] = yNum[i];
                } else {
                    common[i] = xNum[i];
                }
            }
        }
    }

    // 공통 정수로 가장 큰 수 만들기
    StringBuilder sb = new StringBuilder();
    for(int i = 9; i >= 0; i--) {
        while(common[i] > 0) {
            sb.append(i);
            common[i]--;
        }
    }

    if(sb.toString().equals("")) {
        return "-1";
    } else if(sb.toString().matches("^[^1-9]*$")) {
        return "0";
    } else {
        return sb.toString();
    }
}
```
