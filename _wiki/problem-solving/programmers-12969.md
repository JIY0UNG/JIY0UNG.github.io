---
layout  : wiki
title   : 프로그래머스 - 직사각형 별찍기
date    : 2023-02-14 17:21:00 +0900
updated : 2023-02-14 17:21:00 +0900
tag     : 
toc     : true
public  : true
parent  : problem-solving
latex   : false
---

* TOC
{:toc}

[직사각형 별찍기](https://school.programmers.co.kr/learn/courses/30/lessons/12969)

## 구해야 하는 것
- 별(*) 문자를 이용해 가로의 길이가 n, 세로의 길이가 m인 직사각형 형태를 출력

## 주어진 자료
- 가로 = n, 세로 = m

## 조건
- n, m ≤ 1,000

## 계획
1. n과 m을 입력받는다.
2. n개의 별을 찍는 것을 m번 반복한다.

## 실행
```java
public static void main(String[] args) {
    Scanner sc = new Scanner(System.in);
    int n = sc.nextInt();
    int m = sc.nextInt();

    for(int i = 0; i < m; i++) {
        for(int j = 0; j < n; j++) {
            System.out.print("*");
        }
        System.out.println();
    }
}
```
