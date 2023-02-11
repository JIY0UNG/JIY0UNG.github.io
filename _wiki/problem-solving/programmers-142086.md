---
layout  : wiki
title   : 프로그래머스 - 가장 가까운 같은 글자
date    : 2023-02-11 15:20:00 +0900
updated : 2023-02-11 15:20:00 +0900
tag     : 
toc     : true
public  : true
parent  : problem-solving
latex   : false
---

* TOC
{:toc}

[가장 가까운 같은 글자](https://school.programmers.co.kr/learn/courses/30/lessons/142086)

## 구해야 하는 것
- s의 각 위치마다 자신보다 앞에 나왔으면서, 자신과 가장 가까운 곳에 있는 같은 글자의 위치

## 주어진 자료
- s = "banana"라고 할 때
  - b는 처음 나왔으므로 자신의 앞에 같은 글자가 없음. ∴ -1
  - a는 처음 나왔으므로 자신의 앞에 같은 글자가 없음. ∴ -1
  - n은 처음 나왔으므로 자신의 앞에 같은 글자가 없음. ∴ -1
  - a는 자신보다 두 칸 앞에 a가 있음. ∴ 2
  - n은 자신보다 두 칸 앞에 n이 있음. ∴ 2
  - a는 자신보다 두 칸, 네 칸 앞에 a가 있음. 이 중 가까운 것은 두 칸 앞. ∴ 2

## 조건
- 1 ≤ s의 길이 ≤ 10,000
- s는 영어 소문자로만 이루어져 있음

## 계획
1. `s[i]`가 처음 나왔는지 검사
2. `map.get(s[i])`의 값이 null이면,
  - `answer[i]` = -1
  - key = `s[i]`, value = 현재 인덱스로 map에 추가한다.
3. `map.get(s[i])`의 값이 null이 아니면,
  - `answer[i]` = (현재 인덱스 - `map.get(s[i])`)
  - map의 value를 현재 인덱스로 교체한다.
4. answer를 반환한다.

## 실행
```java
public int[] solution(String s) {
    Map<Character, Integer> map = new HashMap<>();
    int[] answer = new int[s.length()];

    for(int i = 0; i < s.length(); i++) {
        char current = s.charAt(i);
        if(map.get(current) == null) {
            answer[i] = -1;
            map.put(current, i);
        } else {
            int value = map.get(current);
            answer[i] = i - value;
            map.put(current, i);
        }
    }
    return answer;
}
```
