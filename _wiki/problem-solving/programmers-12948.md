---
layout  : wiki
title   : 프로그래머스 - 핸드폰 번호 가리기
date    : 2022-06-04 01:06:57 +0900
updated : 2023-01-15 22:15:00 +0900
tag     : 
toc     : true
public  : true
parent  : problem-solving
latex   : false
---

* TOC
{:toc}

[핸드폰 번호 가리기](https://programmers.co.kr/learn/courses/30/lessons/12948)

## 구해야 하는 것
- 전화번호 뒷자리 4자리를 제외한 나머지 숫자를 전부 *로 바꿔라

## 주어진 자료
- 전화번호가 문자열 phone_number로 주어진다

## 조건
- phone_number의 뒷 4자리를 제외한 나머지를 전부 *로 바꾼 문자열을 반환해라

### 주어진 자료로부터 유용한 것 이끌어내기
- 문자열의 끝에서부터 돌면서 끝에서 5번째 자리 숫자부터 *로 바꿔주면 될 것 같다.

## 계획
1. for문을 돈다. i는 phone_number.size()이고 i가 0 이상인동안 i를 감소시킨다.
2. i가 phone_number.size() - 4보다 작으면, 해당 숫자를 *로 바꾼다.
3. 핸드폰 번호를 다 가렸으면 다 가려진 핸드폰 번호가 저장돼있는 phone_number를 앞에서부터 돌면서 answer에 넣어준다.
4. answer를 출력한다.

## 실행
```c
string solution(string phone_number) {
    string answer = "";
    
    int numberSize = phone_number.size();
    for(int i = numberSize; i >= 0; i--) {
        if(i < numberSize - 4) {
            phone_number[i] = '*';
        }
    }
    
    for(int i = 0; i < numberSize; i++) {
        answer += phone_number[i];
    }
    return answer;
}
```

## 반성
- 숫자를 *로 바꿔주는 코드를 아래와 같이 바꿔주면 코드가 훨씬 간단해진다.
```c
for(int i = 0; i < phone_number.size() - 4; i++) {
    // 숫자를 *로 바꿔주기
}
```
- 마지막에 다 가려진 핸드폰 번호를 answer에 넣어줄 때 for문을 돌면서 하나하나 넣어줬는데, 그것보다는 그냥 아래와 같이 써주면 같은 동작을 한줄로 해결할 수 있다.
```c
answer = phone_number;
```
