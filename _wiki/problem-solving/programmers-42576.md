---
layout  : wiki
title   : 프로그래머스 - 완주하지 못한 선수
date    : 2022-07-07 21:03:00 +0900
updated : 2023-01-15 22:15:00 +0900
tag     : 
toc     : true
public  : true
parent  : problem-solving
latex   : false
---

* TOC
{:toc}

[완주하지 못한 선수](https://school.programmers.co.kr/learn/courses/30/lessons/42576)

## 구해야 하는 것
- 마라톤을 완주하지 못한 선수의 이름

## 주어진 자료
- 마라톤에 참여한 선수들의 이름이 담긴 배열 = participant
- 완주한 선수들의 이름이 담긴 배열 = completion

## 조건
- 경기에 참여한 선수의 수는 1 이상 100,000 이하
- completion의 길이 == (participant 길이 - 1)
- 참가자의 이름은 1개 이상 20개 이하의 알파벳 소문자로 이루어져 있다
- 참가자 중에는 동명이인이 있을 수 있다.

## 계획 1
1. participant의 원소와 일치하는 이름이 있는지 completion을 탐색한다.
2. 만약 일치하면 completion[현재 인덱스]에 빈 문자열을 넣고 break
3. completion을 끝까지 탐색했는데도 일치하는 이름이 없으면 해당 원소를 answer에 넣고 break
4. answer를 return 한다.

## 실행 1 → 효율성 테스트 실패
```c
string solution(vector<string> participant, vector<string> completion) {
    string answer = "";
    bool completed = false; // 완주 여부

    for(int i = 0; i < participant.size(); i++) {
        completed = false;

        for(int j = 0; j < completion.size(); j++) {
            if(participant[i] == completion[j]) { // 완주 명단에 있으면 명단에서 제거
                completion[j] = "";
                completed = true;
                break;
            }
        }

        if(completed == false) {
            answer = participant[i]; // 완주하지 못한 선수
            break;
        }
    }

    return answer;
}
```

## 계획 2
첫 번째 풀이로 채점을 하니 답은 모두 통과했지만 효율성 테스트에서 모두 실패했다. 중첩 반복문이 사용돼서 시간복잡도가 O(N^2)인데, 이를 O(N)으로 만들어야 하는 것 같았다. 그래서 map을 써보기로 했다.

1. participant를 돌면서 map[participant[i]]을 1씩 증가시킨다.
2. completion을 돌면서 map[completion[i]]를 1씩 감소시킨다.
3. map을 탐색하면서 값이 1 이상이면 해당 key값을 answer에 넣는다.
4. answer를 return 한다.

## 실행 2
```c
string solution(vector<string> participant, vector<string> completion) {
    string answer = "";
    map<string, int> _map;

    for(int i = 0; i < participant.size(); i++) {
        _map[participant[i]]++;
    }
    for(int i = 0; i < completion.size(); i++) {
        _map[completion[i]]--;
    }

    for(auto element : _map) {
        if(element.second >= 1) { // 값이 1 이상이면 완주하지 못한 선수
            answer = element.first;
            break;
        }
    }

    return answer;
}
```

## 반성
- map으로 푸니까 효율성도 모두 통과되었다! 이렇게 중첩 반복문을 사용해야할 것 같은 문제를 보면 map을 써서 효율성을 높일 수 있을지 생각해봐야겠다.
