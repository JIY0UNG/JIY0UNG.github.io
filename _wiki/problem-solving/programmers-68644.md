---
layout  : wiki
title   : 프로그래머스 - 두 개 뽑아서 더하기
date    : 2022-06-20 15:30:04 +0900
updated : 2023-01-15 22:15:00 +0900
tag     : 
toc     : true
public  : true
parent  : problem-solving
latex   : false
---

* TOC
{:toc}

[두 개 뽑아서 더하기](https://programmers.co.kr/learn/courses/30/lessons/68644)

## 구해야 하는 것
- 정수 배열에서 서로 다른 인덱스에 있는 두 개의 수를 뽑아 더해서 만들 수 있는 모든 수를 오름차순으로 정렬한 배열

## 주어진 자료
- 정수 배열 = numbers

## 조건
- numbers의 길이는 2 이상 100 이하
- numbers의 모든 수는 0 이상 100 이하

### 숨겨진 조건이나 자료
- '서로 다른 인덱스에 있는 두 개의 수'라고 했으니, numbers를 내 마음대로 정렬해도 상관 없다.

## 계획
1. numbers를 오름차순으로 정렬한다.
2. 이중 for문을 돌린다.  
바깥쪽 for문은 i가 0부터 시작해서 numbers의 길이보다 작을 때까지 반복하고,  
안쪽 for문은 j가 i + 1부터 시작해서 numbers의 길이보다 작을 때까지 반복한다.
3. numbers[i] + numbers[j] 값을 answer에 넣는다.
4. for문 탈출 후 answer를 오름차순으로 정렬한다.
5. answer의 중복을 제거한다.

## 실행
```c
vector<int> solution(vector<int> numbers) {
    vector<int> answer;
    
    sort(numbers.begin(), numbers.end()); // numbers 오름차순 정렬
    
    for(int i = 0; i < numbers.size(); i++) {
        for(int j = i + 1; j < numbers.size(); j++) {
            answer.push_back(numbers[i] + numbers[j]);
        }
    }
    
    sort(answer.begin(), answer.end()); // answer 오름차순 정렬
    answer.erase(unique(answer.begin(), answer.end()), answer.end()); // answer 중복 제거
    
    return answer;
}
```

## 반성
- answer에 중복이 있으면 안되니까 벡터 말고 set을 사용해도 되겠다.
- `answer.erase(unique(answer.begin(), answer.end()), answer.end());` 이런식으로 벡터의 중복을 제거한다는 건 알고 있었지만, 이게 어떤 과정을 거쳐서 중복을 제거하는지는 잘 몰랐다. 이번 기회에 확실히 알고 가려고 각 함수에 대해 알아보았다.
    - `unique` 메서드: 중복되는 원소들을 벡터의 제일 뒷부분으로 보내고, 쓰레기 값의 첫 번째 인덱스를 반환한다.
    - `erase` 메서드: unique가 반환한 인덱스부터 answer의 마지막 인덱스까지의 원소들을 제거한다.
