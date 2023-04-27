---
layout  : wiki
title   : 프로그래머스 - 네트워크
date    : 2023-04-28 01:30:06 +0900
updated : 2023-04-28 01:30:06 +0900
tag     : 
toc     : true
public  : true
parent  : problem-solving
latex   : false
---

* TOC
{:toc}

[네트워크](https://school.programmers.co.kr/learn/courses/30/lessons/43162)

## 구해야 하는 것
- 네트워크의 개수를 return

## 자료
- 간접적으로 연결되어 있으면 같은 네트워크 상에 있는 것
- i번 컴퓨터와 j번 컴퓨터가 연결되어 있으면 `computers[i][j]`를 1로 표현

## 조건
- 1 ≤ n ≤ 200
- 각 컴퓨터는 0부터 n - 1인 정수

## 계획
1. 컴퓨터끼리 연결시킨다.
2. 0번 컴퓨터부터 탐색 시작, 0번 컴퓨터 방문 처리
3. 연결돼있는 노드들을 큐에 넣는다.
4. 큐에서 하나를 뺀다.
5. 현재 노드를 큐에서 뺀 노드로 교체하고 방문처리 한다.
6. 현재 노드와 연결돼있는 노드들을 큐에 넣는다.
7. 큐가 비면 네트워크 개수 + 1

## 실행 1 → 실패

```java
public class Solution {
    class Computer {
        int num;
        List<Computer> connectedComputers;

        public Computer(int num) {
            this.num = num;
            this.connectedComputers = new ArrayList<>();
        }

        public void connect(Computer c) {
            this.connectedComputers.add(c);
        }
    }

    public List<Computer> connect(int n, int[][] computers) {
        List<Computer> list = new ArrayList<>();

        for (int i = 0; i < n; i++) {
            Computer c = new Computer(i);
            list.add(c);
        }

        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                if(i != j && computers[i][j] == 1) {
                    list.get(i).connect(list.get(j));
                }
            }
        }

        return list;
    }

    public int go(List<Computer> list) {
        Queue<Computer> queue = new LinkedList<>();
        boolean[] visited = new boolean[list.size()];

        int answer = 0;
        for (int i = 0; i < list.size(); i++) {
            Computer current = list.get(i);
            if(visited[current.num] == false) {
                visited[current.num] = true;
                List<Computer> connectedList = current.connectedComputers;
                if(connectedList.size() == 0) {
                    answer++;
                    break;
                }

                for (int j = 0; j < connectedList.size(); j++) {
                    Computer connected = connectedList.get(j);
                    if(visited[connected.num] == false) {
                        queue.add(connected);
                    }
                }

                while(true) {
                    if(queue.isEmpty()) {
                        answer++;
                        break;
                    }

                    current = queue.poll();
                    visited[current.num] = true;
                    connectedList = current.connectedComputers;

                    for (int k = 0; k < connectedList.size(); k++) {
                        Computer connected = connectedList.get(k);
                        if(visited[connected.num] == false) {
                            queue.add(connected);
                        }
                    }
                }
            }
        }

        return answer;
    }

    public int solution(int n, int[][] computers) {
        List<Computer> list = connect(n, computers);
        return go(list);
    }
}
```

## 실행 2

```java
public int go(List<Computer> list) {
    Queue<Computer> queue = new LinkedList<>();
    boolean[] visited = new boolean[list.size()];

    int answer = 0;
    for (int i = 0; i < list.size(); i++) {
        Computer current = list.get(i);
        if(visited[current.num] == false) {
            visited[current.num] = true;
            List<Computer> connectedList = current.connectedComputers;
            for (int j = 0; j < connectedList.size(); j++) {
                Computer connected = connectedList.get(j);
                if(visited[connected.num] == false) {
                    queue.add(connected);
                }
            }

            while(true) {
                if(queue.isEmpty()) {
                    answer++;
                    break;
                }

                // 큐에서 하나 빼고 해당 노드 방문처리
                current = queue.poll();
                visited[current.num] = true;

                // 현재 노드와 연결돼있는 노드들을 큐에 넣는다.
                connectedList = current.connectedComputers;
                for (int k = 0; k < connectedList.size(); k++) {
                    Computer connected = connectedList.get(k);
                    if(visited[connected.num] == false){
                        queue.add(connected);
                    }
                }
            }
        }
    }

    return answer;
}
```

## 반성
- 실행 1에서는 탐색 시작 노드와 연결된 노드가 없는 경우에도 answer를 증가시키고 있는데, 아마 연결된 노드가 없을 때 한 번 증가하고 큐가 비었을 때 또 한 번 증가해서 틀린 것 같다. 언제 answer를 증가시켜야 하는지 확실히 파악하지 않고 대충 짐작해서 조건문을 넣어주었었는데, 어떤 조건을 추가할 때에는 그에 따른 결과를 다 따져보는 습관을 들여야겠다.
