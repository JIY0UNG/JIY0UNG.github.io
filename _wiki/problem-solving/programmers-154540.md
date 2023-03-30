---
layout  : wiki
title   : 프로그래머스 - 무인도 여행
date    : 2023-03-30 23:34:00 +0900
updated : 2023-03-30 23:34:00 +0900
tag     : 
toc     : true
public  : true
parent  : problem-solving
latex   : false
---

* TOC
{:toc}

[무인도 여행](https://school.programmers.co.kr/learn/courses/30/lessons/154540)

## 구해야 하는 것
- 각 섬에서 최대 며칠씩 머무를 수 있는지 배열에 오름차순으로 담아 return

## 주어진 자료
- 지도는 1 x 1 크기의 사각형들로 이루어진 직사각형 격자 형태
- 격자의 각 칸에는 'X' 또는 1에서 9 사이의 자연수가 적혀있음
- 'X'는 바다, 숫자는 무인도를 나타냄
- 상, 하, 좌, 우로 연결되는 땅들은 하나의 무인도를 이룸
- 지도의 각 칸에 적힌 숫자는 식량을 나타냄
- 상, 하, 좌, 우로 연결되는 칸에 적힌 숫자를 모두 합한 값은 해당 무인도에서 최대 며칠동안 머물 수 있는지를 나타냄

## 조건
- 3 ≤ maps의 길이 ≤ 100
- 3 ≤ `maps[i]`의 길이 ≤ 100
- `maps[i]`는 'X' 또는 1 과 9 사이의 자연수로 이루어진 문자열

## 계획
1. 바다가 아닌 땅을 찾는다.
2. 땅을 찾았으면 연결된 땅들을 방문하면서 식량을 더한다.
3. 더이상 연결된 땅이 없으면 총 식량을 배열에 담는다.
4. 1 ~ 3번 과정을 반복한 후 더이상 방문할 곳이 없으면 배열에 담긴 식량을 오름차순 정렬 후 반환한다.
5. 만약 배열이 비어있으면, 배열에 -1을 담아서 반환한다.

## 실행
```java
public class Solution {
    int[] dx = {0, 0, -1, 1}; // 상, 하, 좌, 우
    int[] dy = {-1, 1, 0, 0};

    public int bfs(int y, int x, int sum, char[][] map, boolean[][] visited) {
        Queue<int[]> q = new LinkedList<>();
        q.add(new int[]{y, x});
        visited[y][x] = true;

        while(true) {
            if(q.isEmpty()) {
                return sum;
            }

            int[] next = q.poll();
            int curY = next[0];
            int curX = next[1];
            sum += map[curY][curX] - 48;

            for(int i = 0; i < 4; i++) {
                int ny = curY + dy[i];
                int nx = curX + dx[i];

                if(ny < 0 || ny >= visited.length) {
                    continue;
                }
                if(nx < 0 || nx >= visited[0].length) {
                    continue;
                }
                if(map[ny][nx] == 'X') {
                    continue;
                }
                if(visited[ny][nx] == true) {
                    continue;
                }
                q.add(new int[]{ny, nx});
                visited[ny][nx] = true;
            }
        }
    }

    public int[] solution(String[] maps) {
        // 지도를 2차원 배열로 옮기기
        char[][] map = new char[maps.length][maps[0].length()];
        for(int i = 0; i < maps.length; i++) {
            String current = maps[i];

            for(int j = 0; j < maps[0].length(); j++) {
                map[i][j] = current.charAt(j);
            }
        }

        boolean[][] visited = new boolean[maps.length][maps[0].length()];
        List<Integer> answer = new ArrayList<>();
        // 무인도 찾기
        for(int y = 0; y < maps.length; y++) {
            for(int x = 0; x < maps[0].length(); x++) {
                if(visited[y][x] == true) {
                    continue;
                }
                visited[y][x] = true;
                if(map[y][x] == 'X') {
                    continue;
                }

                // 무인도 찾았으면 연결된 땅의 숫자 더하기
                int sum = bfs(y, x, 0, map, visited);
                answer.add(sum);
            }
        }

        // 무인도가 없으면 배열에 -1 담아서 return
        if(answer.size() == 0) {
            return new int[]{-1};
        }

        Collections.sort(answer);
        return answer.stream()
                .mapToInt(i -> i)
                .toArray();
    }
}
```

## 반성
- 자바 스트림을 잘 활용하면 코드를 좀더 깔끔하게 작성할 수 있을 것 같다. 공부하자
