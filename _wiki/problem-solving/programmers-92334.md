---
layout  : wiki
title   : 프로그래머스 - 신고 결과 받기
date    : 2022-09-20 06:20:00 +0900
updated : 2023-01-15 22:15:00 +0900
tag     : 
toc     : true
public  : true
parent  : problem-solving
latex   : false
---

* TOC
{:toc}

[신고 결과 받기](https://school.programmers.co.kr/learn/courses/30/lessons/92334)

## 구해야 하는 것
- 각 유저가 받은 신고 결과 메일 개수

## 주어진 자료
- 한 사람이 같은 유저를 여러 번 신고한 것은 1회로 처리
- k번 이상 신고당하면 정지
- 정지 당한 유저를 신고했던 유저들에게 메일 발송
- report의 원소는 "이용자id 신고한id" 형태의 문자열

## 조건
- 2 <= id_list의 길이 <= 1,000
- 1 <= id_list의 원소 길이 <= 10
- 1 <= report의 길이 <= 200,000
- 3 <= report의 원소 길이 <= 21
- 1 <= k <= 200
- id_list에 담긴 id 순서대로 각 유저가 받은 결과 메일 수를 담을 것

## 계획
1. 각 유저가 신고한 유저의 id 목록을 저장한다.
2. 각 유저가 신고 당한 횟수를 저장한다.
3. 1.에서 만든 맵의 value를 돌면서 각 유저가 신고 당한 횟수가 k 이상이면 key 유저가 받을 메일 수를 증가시킨다.

## 실행
```java
public class Solution {
    public int[] solution(String[] id_list, String[] report, int k) {
        int[] answer = new int[id_list.length];
        
        LinkedHashMap<String, ArrayList<String>> reportlist = new LinkedHashMap<>(); // 유저 : 신고한 유저 목록
        LinkedHashMap<String, Integer> cnt = new LinkedHashMap<>(); // 유저 : 신고 당한 횟수
        
        // 맵 초기화
        for(int i = 0; i < id_list.length; i++) {
            ArrayList<String> arr = new ArrayList<>();
            reportlist.put(id_list[i], arr);
            cnt.put(id_list[i], 0);
        }
            
        for(int i = 0; i < report.length; i++) {
            String[] id = report[i].split("\\s");
            
            String str1 = id[0]; // 신고 한 사람
            String str2 = id[1]; // 신고 당한 사람
            
            ArrayList<String> arr = reportlist.get(str1);
            if(!arr.contains(str2)) { // 이전에 신고한 적 없는 경우에만 추가
                arr.add(str2);
                cnt.put(str2, cnt.get(str2) + 1);
            }
        }

        int userIdx = 0;
        for(String key : reportlist.keySet()){
            ArrayList<String> arr = reportlist.get(key); // key: 유저, arr: key가 신고한 사람 목록
            
            for(int i = 0; i < arr.size(); i++) {
                String id = arr.get(i);
                if(cnt.get(id) >= k) { // 신고 당한 횟수가 k 번 이상인 경우
                    answer[userIdx]++;
                }
            }
            userIdx++;
        }
        
        return answer;
    }
}
```
