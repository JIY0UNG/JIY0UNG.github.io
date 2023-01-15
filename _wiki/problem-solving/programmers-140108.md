---
layout  : wiki
title   : 프로그래머스 - 문자열 나누기
date    : 2023-01-02 17:00:00 +0900
updated : 2023-01-15 22:15:00 +0900
tag     : 
toc     : true
public  : true
parent  : problem-solving
latex   : false
---

* TOC
{:toc}

[문자열 나누기](https://school.programmers.co.kr/learn/courses/30/lessons/140108)

## 구해야 하는 것
- 규칙에 따라 분해한 문자열의 개수

## 주어진 자료
- 주어진 문자열 = s
- 규칙
  1. 문자열을 왼쪽에서 오른쪽으로 읽으면서 첫 글자와 같은 글자가 나온 횟수와 다른 글자들이 나온 횟수를 센다.
  2. 두 횟수가 같아지면 멈추고 분리
  3. 위 과정에서 분리한 문자열을 빼고 남은 부분에 대해서 1.~2.과정 반복
  4. 만약 두 횟수가 다른 상태에서 더 이상 읽을 글자가 없으면 지금까지 읽은 문자열을 분리하고 종료

## 조건
- 1 <= s의 길이 <= 10,000
- s는 영어 소문자로만 이루어져 있음

## 계획
만약 주어진 문자열 s가 banana라면,
1. 첫 글자 = b, 두 번째 글자부터 읽기 시작. b와 같은 글자 개수를 same, 다른 글자 개수를 diff라고 하자.
2. s[1]일 때 same = 1, diff = 1
3. same == diff이므로 분리. 이제 s = nana
4. 첫 글자 = n
5. s[1]일 때 same = 1, diff = 1
6. same == diff이므로 분리. 이제 s = na
7. 첫 글자 = n
8. s[1]일 때 same = 1, diff = 1
9. same == diff이므로 분리. 이제 s = na
10. 총 3번 분리했으므로 3 return

이제 계획을 세워보자
1. 첫 글자를 x에 저장하고 same = 1. 그 다음 인덱스부터 돌기 시작한다.
2. 현재 글자가 x와 다르면 diff 1 증가, x와 같으면 same 1 증가
3. same == diff이면 result 1 증가 후 문자열 분리
4. 만약 인덱스를 끝까지 돌았으면 result 1 증가 후 종료
5. result 반환

## 실행
```java
public int solution(String s) {
    StringBuffer str = new StringBuffer(s);
    int result = 0;
    int same = 0;
    int diff = 0;
    char x = str.charAt(i);
    int i = 0;

    while(i <= str.length() - 1) {
        if(i == str.length() - 1) {
            result++;
            break;
        }

        if(str.charAt(i) == x) {
            same++;
        } else {
            diff++;
        }

        if(same == diff) {
            result++;
            str.delete(0, i + 1); // 분리한 문자열 제외
            if(str.length() == 0) {
                break;
            }
            i = 0;
            x = str.charAt(i); // 분리된 문자열의 맨 앞글자
            same = diff = 0;
            continue;
        }
        i++;
    }
    return result;
}
```

## 반성
- 재귀로도 풀어볼 수 있을 것 같다.
