---
layout  : wiki
title   : 프로그래머스 - 이진 변환 반복하기
date    : 2022-10-13 15:57:00 +0900
updated : 2023-01-15 22:15:00 +0900
tag     : 
toc     : true
public  : true
parent  : problem-solving
latex   : false
---

* TOC
{:toc}

[이진 변환 반복하기](https://school.programmers.co.kr/learn/courses/30/lessons/70129)

## 구해야 하는 것
- 주어진 문자열 s가 "1"이 될 때까지 이진 변환을 했을 때 변환 횟수와 그 과정에서 제거된 0의 개수

## 주어진 자료
- 이진 변환 과정
    - x의 모든 0 제거
    - x의 길이를 c라고 하면, x를 "c를 2진법으로 표현한 문자열"로 바꿈

## 조건
- 1 <= s의 길이 <= 150,000
- s에는 1이 최소 하나 이상 포함되어 있음

## 실행
```java
public class Solution {
    int deleted = 0; // 제거된 0 개수
    
    // 모든 0 제거
    public String deleteAllZero(String s) {
        int length = s.length(); // 0 제거하기 전 길이
        s = s.replaceAll("0", ""); // 제거 후 길이
        deleted += length - s.length();
        
        return s;
    }
    
    // x의 길이를 2진법으로 변환
    public String lengthToBinary(int length) {
        StringBuilder sb = new StringBuilder(); 
        
        // 이진수로 변환
        while(length != 1) {
            int remain = length % 2;
            sb.append(remain);
            length /= 2;
        }
        sb.append(length);
        
        return sb.reverse().toString();
    }
    
    public int[] solution(String s) {
        int count = 0;
        
        // s가 1이 될 때까지 반복
        while(!s.equals("1")) {
            count++; // 변환 횟수 증가

            s = deleteAllZero(s);
            if(s.equals("1")) {
                break;
            }

            s = lengthToBinary(s.length());
            if(s.equals("1")) {
                break;
            }
        }
        
        int answer[] = new int[2];
        answer[0] = count;
        answer[1] = deleted;
        return answer;
    }
}
```
