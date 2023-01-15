---
layout  : wiki
title   : 프로그래머스 - 행렬 테두리 회전하기
date    : 2022-08-10 17:19:06 +0900
updated : 2023-01-15 22:15:00 +0900
tag     : 
toc     : true
public  : true
parent  : problem-solving
latex   : false
---

* TOC
{:toc}

[행렬 테두리 회전하기](https://school.programmers.co.kr/learn/courses/30/lessons/77485)

## 구해야 하는 것
- 회전에 의해 위치가 바뀐 숫자들 중 가장 작은 숫자들을 순서대로 구하라

## 주어진 자료
- 행렬의 세로 길이 = 행 개수 = rows
- 행렬의 가로 길이 = 열 개수 = columns
- 회전 목록 = queries

## 조건
- rows와 columns는 2 이상 100 이하의 자연수
- queries의 행 개수는 1 이상 10,000 이하

## 계획
1. rows * columns 크기의 배열 arr을 1, 2, 3...으로 초기화한다.
2. minNum을 10000으로 초기화해놓는다.
3. 첫 번째 입출력 예시로 생각해보면,  
queries[1] = [2, 2, 5, 4]인 경우  
x1 = 2, y1 = 2, x2 = 5, y2 = 4  
v[1][1], v[1][2]는 오른쪽으로 이동,  
v[1][3], v[2][3], v[3][3]은 아래쪽으로 이동,  
v[4][3], v[4][2]는 왼쪽으로 이동,  
v[4][1], v[3][1], v[2][1]은 위쪽으로 이동해야 한다.
4. 회전된 배열은 rotated에 따로 저장해놓는다.
5. 이동 시키면서 가장 작은 수를 minNum에 넣어놓는다.
6. 첫 번째 회전이 끝났으면 answer에 minNum을 push
7. 두 번째 회전 하기 전에 minNum을 다시 10000으로 초기화
8. 회전된 배열 rotated를 arr에 복사
9. queries의 길이만큼 위 과정을 반복 후 answer를 return 한다.

## 실행
```c
int arr[100][100];
int rotated[100][100];
int x1, y1, x2, y2;
int minNum = 10000;

// 옮긴 배열을 기존 배열에 복사
void copy(int rows, int columns) {
    for(int i = 0; i < rows; i++) {
        for(int j = 0; j < columns; j++) {
            arr[i][j] = rotated[i][j];
        }
    }    
}

// 배열 회전
void rotate(int x, int y) {
    // 오른쪽으로 이동
    while(y <= y2) {
        rotated[x][y] = arr[x][y - 1];
        if(rotated[x][y] < minNum) {
            minNum = rotated[x][y];
        }
        y++;
    }
    y--;
    x++;

    // 아래쪽으로 이동
    while(x <= x2) {
        rotated[x][y] = arr[x - 1][y];
        if(rotated[x][y] < minNum) {
            minNum = rotated[x][y];
        }
        x++;
    }
    x--;
    y--;
    
    // 왼쪽으로 이동
    while(y >= y1) {
        rotated[x][y] = arr[x][y + 1];
        if(rotated[x][y] < minNum) {
            
            minNum = rotated[x][y];
        }
        y--;
    }
    y++;
    x--;
    
    // 위쪽으로 이동
    while(x >= x1) {
        rotated[x][y] = arr[x + 1][y];
        if(rotated[x][y] < minNum) {
            minNum = rotated[x][y];
        }
        x--;
    }
}

vector<int> solution(int rows, int columns, vector<vector<int>> queries) {
    vector<int> answer;
    
    // 배열 초기화
    int num = 1;
    for(int i = 0; i < rows; i++) {
        for(int j = 0; j < columns; j++) {
            arr[i][j] = num;
            rotated[i][j] = num;
            num++;
        }
    }
    
    // queries에 담겨있는 만큼 회전 수행
    for(int i = 0; i < queries.size(); i++) {
        x1 = queries[i][0] - 1;
        y1 = queries[i][1] - 1;
        x2 = queries[i][2] - 1;
        y2 = queries[i][3] - 1;
        
        rotate(x1, y1 + 1); // 회전
        answer.push_back(minNum); // 회전하면서 찾은 가장 작은 수를 answer에 push
        minNum = 10000;
        copy(rows, columns); // 그 다음 회전을 위해 arr에 rotated를 복사
    }
    
    return answer;
}
```

## 반성
- 문제에서 이렇게 행렬 초기화 조건을 친절하게 알려줬음에도
> 아무 회전도 하지 않았을 때, i 행 j 열에 있는 숫자는 ((i-1) x columns + j)입니다.

  이 부분을 놓쳐서, num을 행렬에 넣어준 후 1씩 증가시키는 방식으로 초기화를 했었다.  
  문제에서 제공하는 조건을 더 꼼꼼히 읽어야겠다.
- 회전한 결과를 rotate 배열에 저장한 후 그걸 arr로 다시 복사하는 과정이 불필요한 것 같다. arr에서 바로 회전시키는게 더 깔끔할듯
