---
layout  : wiki
title   : 프로그래머스 - 서울에서 김서방 찾기
date    : 2022-06-04 01:06:00 +0900
updated : 2023-01-15 22:15:00 +0900
tag     : 
toc     : true
public  : true
parent  : problem-solving
latex   : false
---

* TOC
{:toc}

[서울에서 김서방 찾기](https://programmers.co.kr/learn/courses/30/lessons/12919)

## 구해야 하는 것
- String 형 배열의 element 중 “Kim”의 위치

## 주어진 자료
- String형 배열 seoul의 길이는 1 이상 1000 이하
- seoul의 원소는 1 이상 20 이하인 문자열

## 조건
- seoul의 element 중 “Kim”의 위치 x를 찾아서 “김서방은 x에 있다”라는 String을 반환해라

## 계획
### 문제 풀이에 사용할 수 있는 유용한 지식
- 벡터의 find() 함수를 사용하여 배열 안에서의 “Kim”의 위치를 알 수 있을 것 같다

1. int x에 find()로 구한 김서방의 위치를 저장한다.
2. answer에 "김서방은 " + x + "에 있다" 라는 출력문을 넣어준다.
3. answer를 반환한다.

## 실행
```c
string solution(vector<string> seoul) {
    string answer = "";
    
    int x = find(seoul.begin(), seoul.end(), "Kim") - seoul.begin();
    string strX = to_string(x);
    
    answer = "김서방은 " + strX + "에 있다";
    return answer;
}
```

## 반성
- answer에 출력문을 넣어줄 때 `answer = "김서방은 " + x + "에 있다";` 이렇게 했더니 문자열에 숫자는 더해줄 수 없다는 오류가 났다. java랑 헷갈렸던 것 같다. 그래서 x를 string으로 다시 바꾼 후 넣어줬다.
