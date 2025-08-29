## 미로탐색([BOJ](https://www.acmicpc.net/problem/2178) 2178)

## 풀이

BFS 최단거리 탐색을 사용하여 해결합니다. 시작점 (0,0)에서 출발하여 목적지 (n-1, m-1)까지의 최단 경로를 찾습니다. visited 배열에 각 칸까지의 최단 거리를 저장하며, 큐를 이용한 BFS로 탐색을 진행합니다.

## 해답

```java
import java.io.*;
import java.util.LinkedList;
import java.util.Queue;
import java.util.StringTokenizer;

/*
    2178. 미로탐색
    메모리: 14868kb
    시간: 140ms
    아이디어: BFS 최단거리 탐색
 */

public class Main_2178_강보승 {
    static int T, n, m, answer;
    static int[] dx = {-1, 1, 0, 0};
    static int[] dy = {0, 0, -1, 1};
    static int[][] visited, map;

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
        StringBuilder sb = new StringBuilder();
        StringTokenizer st = new StringTokenizer(br.readLine());
        n = Integer.parseInt(st.nextToken());
        m = Integer.parseInt(st.nextToken());
        map = new int[n][m];
        visited = new int[n][m];
        for (int i = 0; i < n; i++) {
            String str = br.readLine();
            for (int j = 0; j < m; j++) {
                map[i][j] = str.charAt(j) - '0';
            }
        }
        bfs(0, 0);
        System.out.println(answer);
    }

    static void bfs(int x, int y) {
        visited = new int[n][m];
        Queue<int[]> queue = new LinkedList<>();
        queue.add(new int[]{x, y});
        visited[x][y] = 1;
        while (!queue.isEmpty()) {
            int[] curr = queue.poll();
            x = curr[0];
            y = curr[1];
            for (int i = 0; i < 4; i++) {
                int nx = x + dx[i];
                int ny = y + dy[i];
                if (inRange(nx, ny) && map[nx][ny] == 1 && visited[nx][ny] == 0) {
                    visited[nx][ny] = visited[x][y] + 1;
                    queue.add(new int[]{nx, ny});
                    if (nx == n - 1 && ny == m - 1) {
                        answer = visited[nx][ny];
                    }
                }
            }
        }
    }

    static boolean inRange(int x, int y) {
        return 0 <= x && x < n && 0 <= y && y < m;
    }
}
```