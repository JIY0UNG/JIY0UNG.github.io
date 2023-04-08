---
layout  : wiki
title   : 프로그래머스 - 영어 끝말잇기
date    : 2023-04-08 17:05:00 +0900
updated : 2023-04-08 17:05:00 +0900
tag     : 
toc     : true
public  : true
parent  : problem-solving
latex   : false
---

* TOC
{:toc}

[영어 끝말잇기](https://school.programmers.co.kr/learn/courses/30/lessons/12981)

## 구해야 하는 것
- 가장 먼저 탈락하는 사람의 번호와 그 사람이 자신의 몇 번째 차례에 탈락하는지 구해서 return

## 자료
- 사람의 번호는 1부터 n까지
- 영어 끝말잇기 규칙
  - 1번부터 번호 순서대로 진행되고, 마지막 사람 다음에는 1번부터 다시 시작
  - 앞사람이 말한 단어의 마지막 문자로 시작하는 단어를 말해야 함
  - 이전에 등장했던 단어는 사용 불가
  - 단어의 길이는 2글자 이상이어야 함

## 조건
- 정답은 [ 번호, 차례 ] 형태로 return
- 탈락자가 생기지 않는다면, [0, 0]을 return

## 계획
1. words를 돌면서 등장한 단어를 map에 저장한다.
2. 이미 map에 존재하는 단어이거나, 앞사람 단어 마지막 문자로 시작하지 않는 단어면 탈락
  - 사람 번호는 (index % n) + 1
  - 차례는 (index / n) + 1
3. words를 끝까지 돌았는데 탈락한 사람 없었으면 [0, 0] return

## 실행
```java
public class Solution {
    public int[] solution(int n, String[] words) {
        Map<String, Integer> map = new HashMap<>();

        int[] answer = {0, 0};

        map.put(words[0], 1);
        for (int i = 1; i < words.length; i++) {
            String pre = words[i - 1];
            String current = words[i];

            if(map.get(current) != null
                    || pre.charAt(pre.length() - 1) != current.charAt(0)) {
                answer[0] = (i % n) + 1;
                answer[1] = (i / n) + 1;
                return answer;
            }

            map.put(current, map.getOrDefault(current, 0) + 1);
        }

        return answer;
    }
}
```
