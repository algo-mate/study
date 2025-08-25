## 상호의 배틀필드([SWEA](https://swexpertacademy.com) 1873)

## 풀이

시뮬레이션 문제입니다.
각 문자에 맞게 방향을 조절하고, 포탄 발사 시 벽돌('*')은 파괴하고 평지('.')나 물('-')은 통과하며 재귀 호출하는 방식으로 구현했습니다.

## 해답

```java
/*
1873. 상호의 배틀필드
메모리: 27,264KB
시간: 94ms
아이디어: 1. 시뮬레이션
        2. 각 문자에 맞게 방향 조절
*/
package com.example.algorithm.test;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

public class Solution_1873_강보승 {
    static int n, m;
    static char[][] graph;
    static int[] dx = {-1, 1, 0, 0}; // 상, 하, 좌, 우
    static int[] dy = {0, 0, -1, 1};

    public static boolean inRange(int x, int y) {
        return x >= 0 && x < n && y >= 0 && y < m;
    }

    public static void shoot(int x, int y, int dir) {
        int nx = x + dx[dir];
        int ny = y + dy[dir];

        if (inRange(nx, ny)) {
            if (graph[nx][ny] == '*') {
                graph[nx][ny] = '.';
            } else if (graph[nx][ny] == '.' || graph[nx][ny] == '-') {
                shoot(nx, ny, dir);
            }
        }
    }

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st;
        StringBuilder sb = new StringBuilder();

        int t = Integer.parseInt(br.readLine());

        for (int v = 1; v <= t; v++) {
            st = new StringTokenizer(br.readLine());
            n = Integer.parseInt(st.nextToken());
            m = Integer.parseInt(st.nextToken());

            graph = new char[n][m];
            int tankX = -1, tankY = -1;

            String tankDirections = "^v<>";

            for (int i = 0; i < n; i++) {
                String line = br.readLine();
                for (int j = 0; j < m; j++) {
                    graph[i][j] = line.charAt(j);
                    if (tankDirections.indexOf(graph[i][j]) != -1) {
                        tankX = i;
                        tankY = j;
                    }
                }
            }

            int l = Integer.parseInt(br.readLine());
            String inputs = br.readLine();

            for (char command : inputs.toCharArray()) {
                if (command == 'S') {
                    int shootDir = tankDirections.indexOf(graph[tankX][tankY]);
                    shoot(tankX, tankY, shootDir);
                    continue;
                }

                int currDir = -1;
                if (command == 'U') currDir = 0;
                else if (command == 'D') currDir = 1;
                else if (command == 'L') currDir = 2;
                else if (command == 'R') currDir = 3;

                graph[tankX][tankY] = tankDirections.charAt(currDir);

                int nx = tankX + dx[currDir];
                int ny = tankY + dy[currDir];

                if (inRange(nx, ny) && graph[nx][ny] == '.') {
                    graph[nx][ny] = graph[tankX][tankY];
                    graph[tankX][tankY] = '.';
                    tankX = nx;
                    tankY = ny;
                }
            }

            sb.append("#").append(v).append(" ");
            for (int i = 0; i < n; i++) {
                sb.append(new String(graph[i])).append("\n");
            }
        }
        System.out.print(sb.toString());
    }
}
```