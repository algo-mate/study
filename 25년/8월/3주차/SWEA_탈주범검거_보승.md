## 탈주범 검거([SWEA](https://swexpertacademy.com) 1953)

## 풀이

구현 문제입니다.
터널을 깔끔하게 처리하는 것이 포인트입니다. List contains를 활용하기 위해 Arrays.asList로 리스트를 생성한 후 사용했습니다.
현재 위치 터널에서 허용하는 방향과 다음 위치 터널에서 허용하는 이전 방향을 모두 체크해야 합니다.

## 해답

```java
/*
1953. 탈주범 검거
메모리: 31,872KB
시간: 124ms
아이디어: 구현
        1. 터널을 깔끔하게 처리하는게 포인트
        2. List contains를 활용하기 위해 Arrays.asList로 리스트 생성 후 사용
*/
package com.example.algorithm.test;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.*;

public class Solution_1953_강보승 {
    static int T, n, m, r, c, l;
    static int[] dx = {-1, 1, 0, 0};
    static int[] dy = {0, 0, -1, 1};
    static List<List<Integer>> tunnels;
    static List<List<Integer>> nextTunnels;
    static boolean[][] visited;
    static int[][] arr;

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        T = Integer.parseInt(br.readLine());
        for (int t = 1; t <= T; t++) {
            StringTokenizer st = new StringTokenizer(br.readLine());
            n = Integer.parseInt(st.nextToken());
            m = Integer.parseInt(st.nextToken());
            r = Integer.parseInt(st.nextToken());
            c = Integer.parseInt(st.nextToken());
            l = Integer.parseInt(st.nextToken());
            arr = new int[n][m];
            visited = new boolean[n][m];
            for (int i = 0; i < n; i++) {
                st = new StringTokenizer(br.readLine());
                for (int j = 0; j < m; j++) {
                    arr[i][j] = Integer.parseInt(st.nextToken());
                }
            }
            makeTunnel();
            bfs(r, c);
            int count = 0;
            for (int i = 0; i < n; i++) {
                for (int j = 0; j < m; j++) {
                    if (visited[i][j])
                        count++;
                }
            }
            System.out.println("#" + t + " " + count);
        }
    }

    static void bfs(int x, int y) {
        Queue<int[]> queue = new LinkedList<>();
        queue.add(new int[]{x, y, 1});
        visited[x][y] = true;
        while (!queue.isEmpty()) {
            int[] node = queue.poll();
            x = node[0];
            y = node[1];
            int time = node[2];
            if(time >= l) continue;
            for (int dir = 0; dir < 4; dir++) {
                int nx = x + dx[dir];
                int ny = y + dy[dir];
                if (inRange(nx, ny) && arr[nx][ny] != 0) {
                    if (canGo(x, y, dir) && !visited[nx][ny]) {
                        queue.add(new int[]{nx, ny, time + 1});
                        visited[nx][ny] = true;
                    }
                }
            }
        }
    }

    static boolean canGo(int x, int y, int dir) {
        int nx = x + dx[dir];
        int ny = y + dy[dir];
        int currentTunnelType = arr[x][y];
        int nextTunnelType = arr[nx][ny];
        // 현재 위치 터널에서 허용하는 방향
        if (tunnels.get(currentTunnelType).contains(dir)
                // 다음 위치 터널에서 허용하는 '이전' 방향
                && nextTunnels.get(nextTunnelType).contains(dir))
            return true;
        return false;
    }

    static void makeTunnel() {
        tunnels = new ArrayList<>();
        tunnels.add(new ArrayList<>()); // 더미
        tunnels.add(Arrays.asList(0, 1, 2, 3));
        tunnels.add(Arrays.asList(0, 1));
        tunnels.add(Arrays.asList(2, 3));
        tunnels.add(Arrays.asList(0, 3));
        tunnels.add(Arrays.asList(1, 3));
        tunnels.add(Arrays.asList(1, 2));
        tunnels.add(Arrays.asList(0, 2));

        nextTunnels = new ArrayList<>();
        nextTunnels.add(new ArrayList<>()); // 더미
        nextTunnels.add(Arrays.asList(0, 1, 2, 3));
        nextTunnels.add(Arrays.asList(0, 1));
        nextTunnels.add(Arrays.asList(2, 3));
        //여기가 이전 방향이라서 반대
        nextTunnels.add(Arrays.asList(1, 2));
        nextTunnels.add(Arrays.asList(0, 2));
        nextTunnels.add(Arrays.asList(0, 3));
        nextTunnels.add(Arrays.asList(1, 3));
    }

    static boolean inRange(int x, int y) {
        return 0 <= x && x < n && 0 <= y && y < m;
    }
}
```