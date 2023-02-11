---
layout  : wiki
title   : 프로그래머스 - 나누어 떨어지는 숫자 배열
date    : 2023-02-11 14:16:00 +0900
updated : 2023-02-11 14:16:00 +0900
tag     : 
toc     : true
public  : true
parent  : problem-solving
latex   : false
---

* TOC
{:toc}

[나누어 떨어지는 숫자 배열](https://school.programmers.co.kr/learn/courses/30/lessons/12910)

## 구해야 하는 것
- array의 각 element 중 divisor로 나누어 떨어지는 값을 오름차순으로 정렬한 배열

## 주어진 자료
- divisor로 나누어 떨어지는 원소가 하나도 없으면 배열에 -1을 담아 반환

## 조건
- i ≠ j이면 `arr[i]` ≠ `arr[j]`
- 1 ≤ array의 길이

## 계획
1. array를 순서대로 돈다.
2. 원소가 divisor로 나누어 떨어지면 arrayList에 넣는다.
3. array를 끝까지 돌았는데 arrayList의 크기가 0이면 -1을 넣는다.
4. arrayList를 정렬된 배열로 만들어서 반환한다.

## 실행
```java
public int[] solution(int[] arr, int divisor) {
    List<Integer> answer = new ArrayList<>();
    for(int i = 0; i < arr.length; i++) {
        int current = arr[i];
        if(current % divisor == 0) {
            answer.add(current);
        }
    }

    if(answer.size() == 0) {
        answer.add(-1);
    }

    return answer.stream()
            .mapToInt(i -> i)
            .sorted()
            .toArray();
}
```
