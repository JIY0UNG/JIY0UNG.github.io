---
layout  : wiki
title   : 프로그래머스 - 호텔 대실
date    : 2023-02-06 18:15:00 +0900
updated : 2023-02-06 18:15:00 +0900
tag     : 
toc     : true
public  : true
parent  : problem-solving
latex   : false
---

* TOC
{:toc}

[호텔 대실](https://school.programmers.co.kr/learn/courses/30/lessons/155651)

## 구해야 하는 것
- 필요한 최소 객실 수

## 주어진 자료
- 한 번 사용한 객실은 퇴실 시간을 기준으로 10분간 청소를 하고 다음 손님들이 사용 가능
- 예약 시각이 문자열 형태로 담긴 2차원 배열 = book_time

## 조건
- 1 ≤ book_time의 길이 ≤ 1,000
- `book_time[i]`는 ["HH:MM", "HH:MM"]의 형태로 이루어진 배열
- [대실 시작 시각, 대실 종료 시각]
- 시각은 24시간 표기법을 따르며 "00:00" ~ "23:59"
- 예약 시간이 자정을 넘어가는 경우는 없음
- 시작 시각은 항상 종료 시각보다 빠름

## 계획
1. book_time을 먼저 예약한 순으로 정렬한다.
2. 가장 먼저 예약한 손님 이용 종료 후 청소까지 마친 시간을 리스트에 추가한다.
3. 그 다음으로 예약한 손님의 예약 시간이 리스트에 저장된 시간보다 늦으면 해당 인덱스에 청소까지 마친 시간을 저장한다.
4. 리스트에 저장된 시간보다 빠르면 리스트에 청소까지 마친 시간을 새로 추가한다.
5. 2~4번 과정 반복 후 리스트의 크기를 반환한다.

## 실행
```java
public int solution(String[][] book_time) {
    int[][] times = new int[book_time.length][book_time[0].length]; // book_time을 분으로 바꿔서 저장

    for(int i = 0; i < book_time.length; i++) {
        String start = book_time[i][0];
        String[] arr = start.split(":");
        int startNum = Integer.parseInt(arr[0]) * 60 + Integer.parseInt(arr[1]);

        String end = book_time[i][1];
        arr = end.split(":");
        int endNum = Integer.parseInt(arr[0]) * 60 + Integer.parseInt(arr[1]);

        times[i][0] = startNum;
        times[i][1] = endNum;
    }
    // 먼저 예약한 순으로 정렬
    Arrays.sort(times, (o1, o2) -> {
        return o1[0] - o2[0];
    });

    ArrayList<Integer> arr = new ArrayList<>();
    boolean hasRoom = false;
    for(int i = 0; i < book_time.length; i++) {
        hasRoom = false;
        int currentStart = times[i][0];
        int currentEnd = times[i][1] + 10;

        if(arr.size() == 0) {
            arr.add(currentEnd);
        } else {
            for (int j = 0; j < arr.size(); j++) {
                if (arr.get(j) <= currentStart) { // 사용 가능한 방이 있으면 그 방에 배정
                    arr.remove(j);
                    arr.add(j, currentEnd);
                    Collections.sort(arr);
                    hasRoom = true;

                    if(hasRoom) {
                        break;
                    }
                }
            }
            if(hasRoom == false) { // 사용 가능한 방이 없으면 방 새로 배정
                arr.add(currentEnd);
                Collections.sort(arr);
            }
        }
    }

    return arr.size();
}
```
