## 치즈 도둑([SWEA](https://swexpertacademy.com) 7733)

## 풀이

BFS를 활용한 문제입니다.
먹은 치즈를 먼저 처리하고, BFS 탐색하면서 덩어리 개수를 세는 방식으로 풀었습니다.

## 해답

```java
/*
7733. 치즈 도둑
메모리: 103,984KB
시간: 411ms
아이디어: BFS
        1. 먹은 치즈 먼저 처리
        2. BFS 탐색하면서 덩어리 + 1
*/
package algorithm;

import java.io.*;
import java.util.*;

public class Solution_7733_강보승 {
    static int[] dx = {-1, 1, 0, 0};
    static int[] dy = {0, 0, -1, 1};
    static int T, n;
    static int[][] arr;
    static boolean[][] ate;
    static boolean[][] visited;

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        T = Integer.parseInt(br.readLine());
        for (int t = 1; t <= T; t++) {
            StringTokenizer st = new StringTokenizer(br.readLine());
            n = Integer.parseInt(st.nextToken());
            arr = new int[n][n];
            ate = new boolean[n][n];
            visited = new boolean[n][n];
            int maxCount = 1;
            for (int i = 0; i < n; i++) {
                st = new StringTokenizer(br.readLine());
                for (int j = 0; j < n; j++) {
                    arr[i][j] = Integer.parseInt(st.nextToken());
                }
            }
            for (int d = 1; d <= 100; d++) {
                for (int i = 0; i < n; i++) {
                    for (int j = 0; j < n; j++) {
                        if (arr[i][j] == d) {
                            ate[i][j] = true;
                        }
                    }
                }
                int count = 0;
                visited = new boolean[n][n];
                for (int i = 0; i < n; i++) {
                    for (int j = 0; j < n; j++) {
                        if (!ate[i][j] && !visited[i][j]) {
                            bfs(i, j);
                            count++;
                        }
                    }
                }
                maxCount = Math.max(count, maxCount);
            }
            System.out.println("#" + t + " " + maxCount);
        }
    }
    
    static void bfs(int i, int j) {
        Queue<int[]> queue = new LinkedList<>();
        queue.add(new int[]{i, j});
        visited[i][j] = true;
        while (!queue.isEmpty()) {
            int[] cur = queue.poll();
            int x = cur[0];
            int y = cur[1];
            for(int dir = 0; dir < 4; dir++){
                int nx = x + dx[dir];
                int ny = y + dy[dir];
                if(inRange(nx, ny) && !visited[nx][ny] && !ate[nx][ny]){
                    visited[nx][ny] = true;
                    queue.add(new int[]{nx, ny});
                }
            }
        }
    }
    
    static boolean inRange(int x, int y) {
        return x >= 0 && x < n && y >= 0 && y < n;
    }
}
```