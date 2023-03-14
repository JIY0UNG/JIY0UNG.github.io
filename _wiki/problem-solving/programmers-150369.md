---
layout  : wiki
title   : 프로그래머스 - 택배 배달과 수거하기
date    : 2023-03-14 18:31:00 +0900
updated : 2023-03-14 18:31:00 +0900
tag     : 
toc     : true
public  : true
parent  : problem-solving
latex   : false
---

* TOC
{:toc}

[택배 배달과 수거하기](https://school.programmers.co.kr/learn/courses/30/lessons/150369)

## 구해야 하는 것
- 모든 배달과 수거를 마치고 물류창고까지 돌아올 수 있는 최소 이동 거리

## 주어진 자료
- i번째 집은 물류창고에서 거리 i만큼 떨어져 있음
- i번째 집은 j번째 집과 거리 j - i만큼 떨어져 있음 (1 ≤ i ≤ j ≤ n)
- 트럭에는 재활용 택배 상자를 최대 cap개 실을 수 있음
- 트럭은 배달할 재활용 택배 상자들을 실어 물류창고에서 출발해 각 집에 배달하면서, 빈 재활용 택배 상자들을 수거해 물류 창고에 내림
- 각 집에 배달 및 수거할 때 원하는 개수마늠 택배를 배달 및 수거 가능
- 트럭에 실을 수 있는 재활용 택배 상자의 최대 개수 = cap
- 배달할 집의 개수 = n
- 각 집에 배달할 재활용 택배 상자의 개수를 담은 1차원 정수 배열 = deliveries
- 각 집에서 수거할 빈 재활용 택배 상자의 개수를 담은 1차원 정수 배열 = pickups

## 조건
- 1 ≤ cap ≤ 50
- 1 ≤ n ≤ 100,000
- deliveries의 길이 = pickups의 길이 = n
  - `deliveries[i]` = i + 1번째 집에 배달할 재활용 택배 상자의 개수
  - `pickups[i]` = i + 1번째 집에서 수거할 빈 재활용 택배 상자의 개수
  - 0 ≤ `deliveries[i]` ≤ 50
  - 0 ≤ `pickups[i]` ≤ 50
- 트럭의 초기 위치는 물류창고

## 계획
1. cap이 0이 될 때까지 제일 마지막 집부터 배달해야 하는 상자 개수를 감소시킨다.
2. 이때 이동 거리는 제일 마지막 집의 번호와 같다.
3. cap이 0이 될 때까지 제일 마지막 집부터 수거해야 하는 상자 개수를 감소시킨다.
4. 이때 이동 거리는 제일 마지막 집의 번호와 같다.
5. 배달해야 하는 상자 개수와 수거해야 하는 상자 개수가 모두 0이 되면, 다음 번에 (해당 집 - 1)번 집까지 방문해야 한다.

이런식으로 풀어보려고 했다가 도저히 안풀려서, 카카오에서 제공하는 문제 해설을 참고해서 풀었다.

1. 배달, 수거할 상자 개수를 집 번호 작은 순으로 스택에 담는다.
2. 스택 크기가 더 큰걸 골라서 (더 큰 크기 * 2) 한 값을 distance에 더한다.
3. 스택 맨 위의 값을 pop한다.
4. pop한 값이 cap보다 작으면 cap에서 pop한 값을 감소시킨다.
5. pop한 값이 cap보다 크면 해당 값에서 cap만큼 감소시킨 값을 다시 push한 후 cap을 0으로 바꾼다.
6. cap이 0이 되면 종료한다.
7. 두 스택이 빌 때까지 2.~6. 과정을 반복한다.

## 실행
```java
public Stack<Integer> initStack(int[] arr) {
    Stack<Integer> s = new Stack<>();

    for(int e : arr) {
        s.push(e);
    }

    return removeZero(s); // 맨 위에 0인 것들 없애기
}

public int getDistance(int deliverSize, int pickupSize) {
    if(deliverSize > pickupSize) {
        return deliverSize * 2;
    } else {
        return pickupSize * 2;
    }
}

public Stack<Integer> go(int cap, Stack<Integer> s) {
    while(cap > 0) {
        if(s.isEmpty()) {
            return s;
        }

        int current = s.pop();

        if(current == 0) {
            continue;
        }

        if(current <= cap) {
            cap -= current;
        } else if(current > cap) {
            s.push(current - cap);
            cap = 0;
        }
    }

    return removeZero(s); // 맨 위에 0인 것들 없애기
}

public Stack<Integer> removeZero(Stack<Integer> s) {
    while(true) {
        if(!s.isEmpty() && s.peek() == 0) {
            s.pop();
        } else {
            break;
        }
    }
    return s;
}

public long solution(int cap, int n, int[] deliveries, int[] pickups) {
    Stack<Integer> deliverStack = initStack(deliveries);
    Stack<Integer> pickupStack = initStack(pickups);

    long distance = 0;
    while(true) {
        // 이동 거리 구하기
        distance += getDistance(deliverStack.size(), pickupStack.size());

        // 배달하기
        deliverStack = go(cap, deliverStack);

        // 수거하기
        pickupStack = go(cap, pickupStack);

        // 배달 및 수거 종료
        if(deliverStack.isEmpty() && pickupStack.isEmpty()) {
            return distance;
        }
    }
}
```

## 반성
- 처음에 `removeZero()`에서 Stack이 비어있는 경우 peek()을 하면 `EmptyStackException`이 발생할 수 있다는 것을 고려하지 않아서 틀렸었다. 스택 사용 시 비어있는 경우에 주의하자.
