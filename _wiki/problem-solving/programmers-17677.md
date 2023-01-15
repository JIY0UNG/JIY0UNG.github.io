---
layout  : wiki
title   : 프로그래머스 - 뉴스 클러스터링
date    : 2022-08-24 12:26:06 +0900
updated : 2023-01-15 22:15:00 +0900
tag     : 
toc     : true
public  : true
parent  : problem-solving
latex   : false
---

* TOC
{:toc}

[뉴스 클러스터링](https://school.programmers.co.kr/learn/courses/30/lessons/17677)

## 구해야 하는 것
- 두 문자열의 자카드 유사도

## 주어진 자료
- 두 문자열 = str1, str2
- 자카드 유사도 = 교집합 / 합집합
- 다중집합 A = {1, 1, 2, 2, 3}, B = {1, 2, 2, 4, 5}일 때 교집합은 {1, 2, 2}, 합집합은 {1, 1, 2, 2, 3, 4, 5}이다.  
즉 교집합에는 둘 중 더 적은 개수인 것이 포함되고, 합집합은 둘 중 더 많은 개수인 것이 포함된다.

## 조건
- 각 문자열의 길이는 2 이상 1,000 이하
- 문자열은 두 글자씩 끊어서 다중집합의 원소로 만드는데, 이때 영문자로 된 글자 쌍이 아니면 버린다.
- 대소문자를 구별하지 않는다.
- 자카드 유사도 값에 65536을 곱한 후 소수점 아래를 버린 값을 반환한다.

## 계획
1. 두 문자열 중 대문자를 모두 소문자로 바꾼다.
2. 두 글자씩 자른 것을 키로 갖는 맵을 각각 만든다.  
이때 두 글자씩 자른 문자열에 영문자 외의 문자가 포함되어 있으면 맵에 추가하지 않는다.
3. 교집합과 합집합 개수를 구한다.
두 개의 맵에 같은 키가 존재하면, 둘 중 값이 더 작은 것을 교집합 개수에 더한다.  
그리고 둘 중 값이 더 큰 것을 합집합 개수에 더한다.  
한쪽에만 존재하는 키라면(교집합이 아니라면), 그 값을 합집합 개수에 더한다.
4. (교집합 / 합집합 * 65536)의 결과에서 소수점 아래를 버린 것을 return 한다.

## 실행
```java
class Solution {
    public double intersection = 0;
    public double union = 0;
    
    // 소문자 외의 문자를 포함하면 true 반환
    public boolean hasSpecialChar(String s) {
        if (s.matches("[a-z]{2}")){
             return false;
        }
        return true;
    }
    
    // 두 글자씩 자른 것을 키로 갖는 맵 만들기
    public void makeMap(String s, Map<String, Integer> m) {
        for (int i = 0; i < s.length() - 1; i++) {
            String s1 = s.substring(i, i + 2); // 두 글자씩 자르기

            if(hasSpecialChar(s1)) { // 특수문자 포함 여부 확인
                continue;
            }

            if(!m.containsKey(s1)) { // 맵에 키 존재하지 않으면 값에 1 넣어줌
                m.put(s1, 1);
            } else {
                m.put(s1, m.get(s1) + 1); // 맵에 이미 키 존재하면 값 1 증가
            }
        }
    }
    
    // 교집합, 합집합 개수 구하기
    public void getCnt(Map<String, Integer> m1, Map<String, Integer> m2) {
        Set<String> keySet = m1.keySet();
        for (String key : keySet) {
            if(m2.containsKey(key)) { // m2에도 같은 키가 존재하면(교집합이면)
                if(m1.get(key) < m2.get(key)) {
                    intersection += m1.get(key); // 개수가 적은 쪽을 교집합에 더해줌
                    union += m2.get(key); // 개수가 많은 쪽을 합집합에 더해줌
                }
                else {
                    intersection += m2.get(key);
                    union += m1.get(key);
                }
            }
            else { // 교집합이 아니면 합집합에 더해줌
                union += m1.get(key);
            }
        }
        
        // 위에서 교집합을 모두 구해놨으므로 m2에서는 합집합만 구하면 됨
        keySet = m2.keySet();
        for (String key : keySet) {            
            if(!m1.containsKey(key)) { // 교집합이 아니면 합집합에 더해줌
                union += m2.get(key);
            }
        }
    }
    
    // (자카드 유사도 * 65536) 한 후 정수부만 반환
    public int calc() {
        if(intersection == 0 && union == 0) {
            return 65536;
        }
        else {
            double ans = (double)(intersection / union) * 65536;
            return (int)Math.floor(ans);
        }
    }
    
    public int solution(String str1, String str2) {
        // 1. str1, str2에서 대문자를 모두 소문자로 변경
        str1 = str1.toLowerCase();
        str2 = str2.toLowerCase();
        
        // 2. str1, str2를 두 글자씩 자른 것을 키로 갖는 map을 만든다.
        Map<String, Integer> m1 = new HashMap<>();
        Map<String, Integer> m2 = new HashMap<>();
        makeMap(str1, m1);
        makeMap(str2, m2);
        
        // 3. 교집합, 합집합 개수 구하기
        getCnt(m1, m2);
        
        // 4. (자카드 유사도 * 65536) 한 후 정수부만 return
        int answer = calc();        
        return answer;
    }
}
```

## 반성
- 처음에는 일반적으로 알고있는 개념의 교집합과 합집합이라고 생각하고 문제를 제대로 읽지 않았었다. 문제가 좀 길더라도 꼼꼼히 읽고 정확히 해석할 수 있도록 하자
- 교집합, 합집합 구할 때 크기 비교 후 더해주는 코드를
```java
intersection += (m1.get(key) < m2.get(key)) ? m1.get(key) : m2.get(key);
union += (m1.get(key) < m2.get(key)) ? m2.get(key) : m1.get(key);
```
이렇게 삼항연산자로 처리해줄 수도 있겠다.(더 헷갈리나?)
- 두 글자씩 자를 때 처음에는 while로 처리했었는데, 언제 끝나는지 정확하게 아는 경우에는 for문을 사용하도록 하자
- 정규식을 좀 알아둬야겠다. "[a-z]" 는 a-z까지의 문자가 포함되어 있는지, "[^a-z]"는 a-z까지의 문자가 제외되어 있는지를 뜻한다.
- 자바는 call by reference가 안된다.. 다시 자바에 적응하자..!
