---
layout  : wiki
title   : 프로그래머스 - 둘만의 암호
date    : 2023-02-07 23:31:00 +0900
updated : 2023-02-07 23:31:00 +0900
tag     : 
toc     : true
public  : true
parent  : problem-solving
latex   : false
---

* TOC
{:toc}

[둘만의 암호](https://school.programmers.co.kr/learn/courses/30/lessons/155652)

## 구해야 하는 것
- 규칙대로 s를 변환한 결과

## 주어진 자료
- 두 문자열 s, skip 그리고 자연수 index가 주어짐
- 암호의 규칙
  - 문자열 s의 각 알파벳을 index만큼 뒤의 알파벳으로 바꿔줌
  - index 만큼의 뒤의 알파벳이 z를 넘어갈 경우 다시 a로 돌아감
  - skip에 있는 알파벳은 제외하고 건너뜀

## 조건
- 5 ≤ s의 길이 ≤ 50
- 1 ≤ skip의 길이 ≤ 10
- s와 skip은 알파벳 소문자로만 이루어져 있음
- skip에 포함되는 알파벳은 s에 포함되지 않음
- 1 ≤ index ≤ 20

## 계획 1
1. skip 알파벳을 제외한 전체 알파벳을 map에 넣어놓는다.
  - 알파벳이 key, 해당 알파벳의 인덱스가 value
2. s의 첫 번째 알파벳의 인덱스를 찾고, 해당 인덱스 + index에 해당하는 key를 찾아서 result에 넣는다.

## 실행 1 → 런타임 에러
```java
// 암호화
public String encode(
        String original,
        int index,
        Map<Character, Integer> keyMap,
        Map<Integer, Character> valueMap
) {
    StringBuilder sb = new StringBuilder();

    for(int i = 0; i < original.length(); i++) {
        char current = original.charAt(i); // 암호화 해야 하는 문자

        int targetIndex = keyMap.get(current) + index;
        if(targetIndex >= valueMap.size()) {
            targetIndex = valueMap.size() - targetIndex;
        }
        char encodedChar = valueMap.get(targetIndex);
        sb.append(encodedChar);
    }

    return sb.toString();
}

// 현재 알파벳이 skip 문자인지 확인
public boolean isSkip(char[] sortedSkip, char alpha) {
    for(int j = 0; j < sortedSkip.length; j++) {
        if(sortedSkip[j] == alpha) {
            return true;
        }
    }
    return false;
}

// skip 문자를 제외한 알파벳 맵 만들기
public void makeAlphaWithoutSkip(
        char[] sortedSkip,
        Map<Character, Integer> keyMap,
        Map<Integer, Character> valueMap
) {
    int number = 0;
    for(char i = 'a'; i <= 'z'; i++) {
        char alpha = i;
        if(!isSkip(sortedSkip, alpha)) {
            keyMap.put(alpha, number);
            valueMap.put(number, alpha);
            number++;
        }
    }
}

public char[] sortSkip(String skip) {
    char[] c = new char[skip.length()];
    for(int i = 0; i < skip.length(); i++) {
        c[i] = skip.charAt(i);
    }
    Arrays.sort(c);
    return c;
}

public String solution(String s, String skip, int index) {
    // skip 정렬하기
    char[] sortedSkip = sortSkip(skip);

    // skip 제외한 알파벳 맵 만들기
    Map<Character, Integer> keyMap = new HashMap<>();
    Map<Integer, Character> valueMap = new HashMap<>();
    makeAlphaWithoutSkip(sortedSkip, keyMap, valueMap);

    // s 암호화
    String answer = encode(s, index, keyMap, valueMap);
    return answer;
}
```

## 계획 2
1. 암호화 해야 하는 문자로부터 index 만큼 뒤의 문자를 구해야 한다.
2. 현재 문자의 다음 문자가 skip에 해당하면 index를 감소시키지 않는다.
3. 현재 문자의 다음 문자가 skip에 해당하지 않으면 index를 감소시킨다.
4. 만약 현재 문자의 다음 문자가 'z'를 넘어가면 현재 문자의 다음 문자를 'a'로 한다.
5. 위 과정을 index가 0이 될 때까지 반복한다.

## 실행 2 → 실패
```java
public char encode(char c, String skip, int index) {
    if(index == 0) {
        if(c > 'z') {
            return 'a';
        }
        return c;
    }

    if(skip.contains(String.valueOf(c))) {
        return encode((char) (c + 1) > 'z' ? 'a' : (char) (c + 1), skip, index);
    }
    return encode((char) (c + 1) > 'z' ? 'a' : (char) (c + 1), skip, index - 1);
}

public String solution(String s, String skip, int index) {
    String answer = "";
    for(int i = 0; i < s.length(); i++) {
        answer += encode(s.charAt(i), skip, index);
    }
    return answer;
}
```

## 계획 3
contains에서 문제가 있는건가 싶어서 그냥 skip 문자 여부를 for문으로 돌려서 확인했다.

## 실행 3
```java
public char encode(char c, String skip, int index) {
    if(index == 0) {
        return c;
    }

    char next = (char) (c + 1);
    if(next > 'z') { // 다음 문자가 z를 넘어가면 a로 돌아감
        next = 'a';
    }

    for(int i = 0; i < skip.length(); i++) {
        if(skip.charAt(i) == next) { // skip 문자인 경우
            return encode(next, skip, index);
        }
    }
    return encode(next, skip, index - 1);
}

public String solution(String s, String skip, int index) {
    String answer = "";
    for(int i = 0; i < s.length(); i++) {
        answer += encode(s.charAt(i), skip, index);
    }
    return answer;
}
```

## 반성
- `index만큼 뒤에 있는 문자를 구한다, 해당 문자가 z를 넘어가면 a로 돌아간다, skip 문자는 건너뛴다` 이 세 과정만 있으면 되는걸 처음에 너무 복잡하게 생각했던 것 같다. 처음부터 재귀적으로 생각했으면 좀더 쉽게 풀었을듯
