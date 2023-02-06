---
layout  : wiki
title   : 프로그래머스 - 전화번호 목록
date    : 2023-02-06 14:11:00 +0900
updated : 2023-02-06 14:11:00 +0900
tag     : 
toc     : true
public  : true
parent  : problem-solving
latex   : false
---

* TOC
{:toc}

[전화번호 목록](https://school.programmers.co.kr/learn/courses/30/lessons/42577)

## 구해야 하는 것
- 어떤 전화번호가 다른 번호의 접두어인지 구하라

## 주어진 자료
- 한 번호가 다른 번호의 접두어이면 false, 그렇지 않으면 true를 반환

## 조건
- 1 ≤ phone_book의 길이 ≤ 1,000,000
- 1 ≤ 각 전화번호의 길이 ≤ 20
- 같은 전화번호가 중복해서 들어있지 않음

## 계획 1
1. 전화번호가 key, 전화번호의 길이가 value인 map을 만든다.
2. 전화번호의 길이를 기준으로 오름차순 정렬한다.
3. 길이가 가장 짧은 전화번호부터 다른 전화번호의 접두어인지 확인한다.
4. 접두어인 경우가 있으면 false 반환, 끝까지 다 돌았는데 없으면 true 반환

## 실행 1 → 시간 초과
```java
public boolean solution(String[] phone_book) {
    Map<String, Integer> map = new HashMap<>();

    for(int i = 0; i < phone_book.length; i++) {
        map.put(phone_book[i], phone_book[i].length());
    }

    List<Map.Entry<String, Integer>> entryList = new LinkedList<>(map.entrySet());
    entryList.sort(Map.Entry.comparingByValue());

    for(Map.Entry<String, Integer> entry : entryList){
        String current = entry.getKey();

        for(Map.Entry<String, Integer> entry2 : entryList) {
            String current2 = entry2.getKey();
            if(current != current2 && current2.startsWith(current)) {
                return false;
            }
        }
    }
    return true;
}
```

## 계획 2
이중 for문때문에 시간 초과가 난 것 같았다. '이미 정렬 되어 있으므로 바로 뒤에 것만 보면 된다'라는 다른 사람의 힌트를 살짝 보고 계획을 다시 세웠다.  
1. phone_book을 오름차순 정렬한다.
3. `phone_book[i]`가 `phone_book[i - 1]`로 시작하면 false를 반환하고, phone_book을 끝까지 돌았는데도 없으면 true를 반환한다.

## 실행 2
```java
public boolean solution(String[] phone_book) {
    Arrays.sort(phone_book);

    for(int i = 1; i < phone_book.length; i++) {
        if(phone_book[i].startsWith(phone_book[i - 1])) {
            return false;
        }
    }
    return true;
}
```
