---
layout  : wiki
title   : 프로그래머스 - 비밀지도
date    : 2022-06-14 19:06:00 +0900
updated : 2023-01-15 22:15:00 +0900
tag     : 
toc     : true
public  : true
parent  : problem-solving
latex   : false
---

* TOC
{:toc}

[비밀지도](https://programmers.co.kr/learn/courses/30/lessons/17681)

## 구해야 하는 것
- 두 지도를 겹친 전체 지도

## 주어진 자료
- 지도는 한 변의 길이가 n인 정사각형 배열이며, 각 칸은 공백(" ") 또는 벽("#") 두 종류로 이루어져 있다.
- 지도1과 지도2 중 어느 하나라도 벽인 부분은 전체 지도에서도 벽이고, 두 지도 모두 공백인 부분은 전체 지도에서도 공백이다.
- 지도1과 지도2는 정수 배열로 암호화되어 있다.
- 암호화된 배열은 지도의 각 가로줄에서 벽 부분을 1, 공백 부분을 0으로 부호화했을 때 얻어지는 이진수에 해당하는 값의 배열이다.

## 조건
- 지도의 한 변 크기 n은 1 이상 16 이하
- 두 지도는 각각 arr1, arr2이며, 길이 n인 정수 배열
- 정수 배열의 각 원소 x를 이진수로 변환했을 때의 길이는 n이하. 즉 x는 0 이상, 2의 n제곱 - 1
- 비밀지도를 해독하여 '#', ' '으로 구성된 문자열 배열로 출력하라

### 문제를 더 단순하게 하기 위해서 주어진 조건 버리기
- 조건 중 정수 배열의 각 원소 x의 범위는 굳이 고려할 필요가 없는 것 같다.

### 문제 풀이에 사용할 수 있는 유용한 지식
- 10진수를 2진수로 변환할 때 10진수가 0이 될 때까지 2로 나누고, 나눌 때마다 나오는 나머지를 배열에 저장한 후 그 배열을 뒤집으면 이진수가 된다.

## 계획 1
1. for문을 n번만큼 돌면서 map1과 map2의 원소를 이진수로 변환한다.
2. 2진수로 변환하기
숫자가 0이 될 때까지 2로 나누고, 그 때의 나머지를 binary 배열에 저장한다.  
ex) 11을 이진수로 변환 ⇒ binary1에 1 0 0 1 0 저장됨  
map1의 원소와 map2의 원소에 대해 변환 후 각각 binary1과 binary2에 저장한다.
3. 현재 2진수가 거꾸로 저장되어 있으므로 binary1과 binary2를 뒤집어준다.
4. 지도의 가로줄은 line에 저장한다. for문을 n번만큼 돌면서 binary1과 binary2의 원소 중 어느 하나라도 1이면 line에 "#"를 더해주고, binary1과 binary2의 원소가 모두 0이면 line에 " "를 더해준다.
5. 완성된 line을 전체 지도인 combinedMap에 넣어준다.
6. combinedMap을 return 한다.

## 실행 1
```c
void toBinary(int decimal, vector<int> &binary) {
    while(decimal != 0) {
        binary.push_back(decimal % 2);
        decimal /= 2;
    }
}

vector<string> solution(int n, vector<int> map1, vector<int> map2) {
    vector<string> combinedMap; // 전체 지도
    
    for(int i = 0; i < n; i++) {
        vector<int> binary1;
        vector<int> binary2;
        string line = "";
        
        // map1, map2의 각 원소를 이진수로 변환 후 binary에 저장
        toBinary(map1[i], binary1);
        toBinary(map2[i], binary2);
        
        // binary 뒤집기
        reverse(binary1.begin(), binary1.end());
        reverse(binary2.begin(), binary2.end());
        
        // binary1, 2의 각 원소를 탐색하면서 combinedMap에 벽과 공백 표시
        for(int j = 0; j < n; j++) {
            // 두 지도 중 어느 하나라도 벽이면 전체 지도에서도 벽
            if(binary1[j] == 1 || binary2[j] == 1) {
                line += '#';
            }
            
            // 두 지도 모두 공백이면 전체 지도에서도 공백
            if(binary1[j] == 0 && binary2[j] == 0) {
                line += ' ';
            }
        }

        // 완성된 line을 combinedMap에 넣어줌
        combinedMap.push_back(line);
    }
    
    return combinedMap;
}
```

## 계획 2
만약 1을 2진수로 변환하면 binary에 0 0 0 0 1이 저장돼야 하는데, 첫 번째 계획대로 변환하면 binary에는 1만 저장된다.  
그래서 2진수로 변환할 때 2로 나누는 것을 무조건 n번 하도록 바꿔주었다.

1.과정 동일

2.n이 0이 될 때까지 10진수를 2로 나눠주고, 그 때의 나머지를 binary 배열에 저장한다. 그리고 n을 1 감소시킨다.

3.~ 6.과정 동일

## 실행 2
toBinary 부분 변경됨
```c
void toBinary(int decimal, vector<int> &binary, int n) {
    while(n > 0) {
        binary.push_back(decimal % 2);
        decimal /= 2;
        n--;
    }
}

vector<string> solution(int n, vector<int> map1, vector<int> map2) {
    vector<string> combinedMap; // 전체 지도
    
    for(int i = 0; i < n; i++) {
        vector<int> binary1;
        vector<int> binary2;
        string line = "";
        
        // map1, map2의 각 원소를 이진수로 변환 후 binary에 저장
        toBinary(map1[i], binary1, n);
        toBinary(map2[i], binary2, n);

        // binary 뒤집기
        reverse(binary1.begin(), binary1.end());
        reverse(binary2.begin(), binary2.end());
        
        // binary의 각 원소를 탐색하면서 combinedMap에 벽과 공백 표시
        for(int j = 0; j < n; j++) {
            // 두 지도 중 어느 하나라도 벽이면 전체 지도에서도 벽
            if(binary1[j] == 1 || binary2[j] == 1) {
                line += '#';
            }
            
            // 두 지도 모두 공백이면 전체 지도에서도 공백
            if(binary1[j] == 0 && binary2[j] == 0) {
                line += ' ';
            }
        }
        
        // 완성된 line을 combinedMap에 넣어줌
        combinedMap.push_back(line);
    }
    
    return combinedMap;
}
```

## 반성
- 다른 사람의 풀이를 보니 나처럼 2진수로 바꿔주는 과정 없이 비트 연산만 활용한 풀이가 있었다. binary를 뒤집는 과정도 없었다. 그 풀이를 이해해 봐야겠다.
- 2진수가 나오는 문제에서 비트 연산을 활용해볼 수 있을 것 같다.
- 처음에 toBinary 메서드를 `void toBinary(int decimal, vector<int> binary, int n)` 이렇게 선언했었는데 main에 돌아오니 binary에 값이 저장되지 않았다. 요새 거의 하루종일 자바만 쓰다보니까 이런 부분에서 혼동이 온다. binary를 참조변수로 수정하니(`&binary`) 원하던대로 이진수가 잘 저장되었다.  
C에서는 주소값으로 호출하고 포인터로 매개변수를 받았었는데, C++에서는 참조변수로 매개변수를 받는다는 걸 알게 됐다.
