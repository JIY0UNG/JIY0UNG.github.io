---
layout  : wiki
title   : 프로그래머스 - 할인 행사
date    : 2023-01-04 17:30:00 +0900
updated : 2023-01-15 22:15:00 +0900
tag     : 
toc     : true
public  : true
parent  : problem-solving
latex   : false
---

* TOC
{:toc}

[할인 행사](https://school.programmers.co.kr/learn/courses/30/lessons/131127)

## 구해야 하는 것
- 회원등록 시 정현이가 원하는 제품을 모두 할인 받을 수 있는 회원 등록 날짜의 총 일수

## 주어진 자료
- 회원은 매일 한 가지 제품을 할인받을 수 있음
- 할인 제품은 하루에 한개만 구매 가능
- 정현이는 원하는 제품과 수량이 할인하는 날짜와 10일 연속으로 일치할 경우에 맞춰서 회원가입 하려고 함
- 정현이가 원하는 제품을 나타내는 문자열 배열 = want
- 원하는 제품의 수량을 나타내는 정수 배열 = number
- 마트에서 할인하는 제품을 나타내는 문자열 배열 = discount

## 조건
- 1 <= want의 길이 = number의 길이 <= 10
    - 1 <= number의 원소 <= 10
    - number[i]는 want[i]의 수량을 의미, number의 원소의 합 = 10
- 10 <= discount의 길이 <= 100,000
- want와 discount의 원소들은 알파벳 소문자로 이루어진 문자열
    - 1 <= want의 원소의 길이, discount의 원소의 길이 <= 12

## 계획
1. 정현이가 원하는 제품의 이름을 key로, 개수를 value로 저장해둔다.
2. 첫 번째 날부터 돌면서 해당 key의 value를 하나씩 감소시킨다.
3. 다 돌았을 때 모든 key의 value가 0이 되면 result 1 증가

## 실행
```java
public int solution(String[] want, int[] number, String[] discount) {
    Map<String, Integer> map = new HashMap<>();

    int total = 0; // 정현이가 사려는 물건 총 개수
    for(int i = 0; i < want.length; i++) {
        String key = want[i];
        int value = number[i];
        total += value;

        if(map.get(key) == null) {
            map.put(key, value);
        } else {
            map.put(key, map.get(key) + value);
        }
    }

    int result = 0;
    int start = 0;
    int end;
    int cnt;
    Map<String, Integer> copiedMap = new HashMap<String, Integer>();

    while(start <= discount.length - 10) {
        cnt = total;
        copiedMap.putAll(map);
        end = start + 9; // 회원 자격 10일

        for(int i = start; i <= end; i++) {
            String cur = discount[i];

            if(copiedMap.get(cur) != null && copiedMap.get(cur) > 0) {
                copiedMap.put(cur, copiedMap.get(cur) - 1);
                cnt--;
            } else {
                break;
            }

            if(cnt == 0) {
                result++;
                break;
            }
        }
        start++;
    }

    return result;
}
```
