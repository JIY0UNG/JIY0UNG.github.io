---
layout  : wiki
title   : 프로그래머스 - 햄버거 만들기
date    : 2023-02-07 22:43:00 +0900
updated : 2023-02-07 22:43:00 +0900
tag     : 
toc     : true
public  : true
parent  : problem-solving
latex   : false
---

* TOC
{:toc}

[햄버거 만들기](https://school.programmers.co.kr/learn/courses/30/lessons/133502)

## 구해야 하는 것
- 포장하는 햄버거 개수

## 주어진 자료
- 아래에서부터 빵 - 야채 - 고기 - 빵으로 쌓인 햄버거만 포장 가능
- 재료의 정보를 나타내는 정수 배열 = ingredient

## 조건
- 1 ≤ ingredient의 길이 ≤ 1,000,000
- 1 = 빵, 2 = 야채, 3 = 고기

## 계획 1
1. 재료를 4개씩 합쳐보고 빵 - 야채 - 고기 - 빵 순서인지 확인한다.
  - ingredient의 원소를 하나의 문자열로 전부 합쳐놓는다.
  - 문자열의 0번째 인덱스부터 돌면서 4개씩 합쳐서 또 다른 문자열을 만든다.
2. 합친 재료가 4개가 되면 포장 가능한지 검사한다.
  - 합친 문자열이 "1231"과 같은지 확인한다.
3. 맞으면 해당 재료들을 빼고, 포장한 햄버거 개수를 증가시킨다.
  - 같으면 전체 재료 문자열에서 해당 인덱스 범위만큼 제거한다.
  - 햄버거 개수를 증가시킨다.
4. 더이상 포장할 수 없을 때까지 반복한다.
  - 다시 전체 재료 문자열의 0번째 인덱스부터 돌면서 포장 가능한지 확인한다.
5. 햄버거 개수를 반환한다.

## 실행 1 → 실패, 시간 초과
```java
// 전체 재료에서 포장된 햄버거의 재료 제외하기
public void removeIngredients(StringBuilder ingredients, int startIndex, int endIndex) {
    ingredients.delete(startIndex, endIndex);
}

// 빵-야채-고기-빵 순서인지 확인
public boolean isHamburger(String s) {
    return s.equals("1231");
}

// 포장 가능한지 확인
public boolean canWrap(StringBuilder ingredients) {
    for(int i = 3; i < ingredients.length(); i++) {
        String s = "";
        for(int j = i - 3; j <= i; j++) {
            s += ingredients.charAt(j);
        }

        if(isHamburger(s)) {
            removeIngredients(ingredients, i - 3, i);
            return true;
        }
    }
    return false;
}

// 재료를 문자열로 바꾸기
public StringBuilder makeString(int[] ingredient) {
    StringBuilder sb = new StringBuilder();
    for(int i = 0; i < ingredient.length; i++) {
        sb.append(ingredient[i]);
    }
    return sb;
}

public int solution(int[] ingredient) {
    StringBuilder ingredients = makeString(ingredient);

    int hamburger = 0;
    int idx = 0;
    while(idx < ingredients.length()) {
        if(canWrap(ingredients)) {
            hamburger++;
            idx = 0;
            continue;
        }
        idx++;
    }

    return hamburger;
}
```

## 계획 2
하다보니 예전에 풀었던 백준 - 문자열 폭발 문제가 생각났고, 그때 풀이를 좀 참고해서 풀어봤다.
1. 재료를 쌓다가 쌓인 재료 개수가 4개가 되면 포장 가능한지 검사한다.
  - 스택에 ingredients를 쌓는다
  - 스택 크기가 4가 되면 포장 가능한지 검사
2. 포장이 가능하면 4개만큼 재료를 제거한다.
  - 스택에서 4개 제거

## 실행 2
```java
// 재료를 문자열로 바꾸기
public String makeString(int[] ingredient) {
    StringBuilder sb = new StringBuilder();
    for(int i = 0; i < ingredient.length; i++) {
        sb.append(ingredient[i]);
    }
    return sb.toString();
}

public int solution(int[] ingredient) {
    String ingredients = makeString(ingredient);

    Stack<Character> stack = new Stack<>();
    String order = "1231";

    int hamburger = 0;
    for(int i = 0; i < ingredients.length(); i++) {
        stack.push(ingredients.charAt(i));

        if(stack.size() >= 4) {
            Boolean canWrap = true;

            for(int j = 0; j < 4; j++) {
                char currentIngredient = stack.get(stack.size() - 4 + j);

                if(currentIngredient != order.charAt(j)) {
                    canWrap = false;
                    break;
                }
            }

            if(canWrap) {
                hamburger++;
                for(int j = 0; j < 4; j++) {
                    stack.pop();
                }
            }
        }
    }
    return hamburger;
}
```

## 반성
- 예전에 비슷한 문제를 풀었던 기억은 나는데, 그걸 어떻게 풀었는지는 머릿속에 잘 안 남아있는 것 같다. '이런 유형은 이렇게 푸는 거다'가 딱 나올 정도로 비슷한 문제를 많이 풀어봐야 할 것 같다.
