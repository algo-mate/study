## 파핑파핑지뢰찾기([SWEA](https://swexpertacademy.com/main/code/problem/problemDetail.do?contestProbId=AV5LqsqDApIDFAXc) 1868)

## 풀이

지뢰 찾기 게임을 시뮬레이션하는 BFS 탐색 문제입니다.

클릭 횟수를 최소화하기 위한 전략:
1. 주변 8방향에 지뢰가 없는 칸(0인 칸)을 우선적으로 클릭
2. 0인 칸을 클릭하면 BFS로 연쇄적으로 확장되어 여러 칸이 동시에 열림
3. 남은 빈 칸들은 개별적으로 클릭해야 함

핵심 아이디어:
- 지뢰 개수가 0인 모든 칸에서 BFS 수행하여 최대한 많은 영역을 한 번에 처리
- BFS는 지뢰 개수가 0인 칸에서만 계속 확장
- 지뢰 개수가 0이 아닌 칸에 도달하면 해당 칸만 처리하고 확장 중단

## 해답

```java
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.util.LinkedList;
import java.util.Queue;

/*
    1868. 파핑파핑 지뢰찾기
    메모리: 120,120 kb
    실행시간: 547ms
    아이디어: 지뢰 개수 세기 + bfs
 */

public class Solution_1868_강보승 {
    static int T, n;
    static int[] dx = new int[]{1, 1, -1, -1, 0, 0, 1, -1};
    static int[] dy = new int[]{1, -1, -1, 1, -1, 1, 0, 0};
    static char[][] map;
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
        StringBuilder sb = new StringBuilder();
        T = Integer.parseInt(br.readLine());
        for (int t = 1; t <= T; t++) {
            n = Integer.parseInt(br.readLine());
            map = new char[n][n];
            for(int i = 0; i < n; i++){
                String str = br.readLine();
                for(int j = 0; j < n; j++){
                    map[i][j] = str.charAt(j);
                }
            }
            int count = 0;
            for(int i = 0; i < n; i++){
                for(int j = 0; j < n; j++){
                    if(map[i][j] == '.'){
                        // 지뢰개수 세고 0이면 bfs 탐색
                        if(countBomb(i, j) == 0){
                            bfs(i, j);
                            count++;
                        }
                    }
                }
            }
            for(int i = 0; i < n; i++){
                for(int j = 0; j < n; j++){
                    // 나머지 개수 세기
                    if(map[i][j] == '.'){
                        count++;
                    }
                }
            }
            System.out.println("#" + t + " " + count);
        }
    }
    static int countBomb(int x, int y){
        int count = 0;
        for(int dir = 0; dir < 8; dir++){
            int nx = x + dx[dir];
            int ny = y + dy[dir];
            if(inRange(nx, ny) && map[nx][ny] == '*'){
                count++;
            }
        }
        return count;
    }
    static void bfs(int x, int y){
        boolean[][] visited = new boolean[n][n];
        Queue<int[]> queue = new LinkedList<>();
        queue.add(new int[]{x, y});
        visited[x][y] = true;
        while(!queue.isEmpty()){
            int[] curr = queue.poll();
            x = curr[0];
            y = curr[1];
            int count = countBomb(x, y);
            if(count == 0){
                for(int i = 0; i < 8; i++){
                    int nx = x + dx[i];
                    int ny = y + dy[i];
                    if(inRange(nx, ny) && !visited[nx][ny] && map[nx][ny] == '.'){
                        queue.add(new int[]{nx, ny});
                        visited[nx][ny] = true;
                    }
                }
            }
            map[x][y] = (char)(count + '0');
        }
    }
    static boolean inRange(int x, int y){
        return 0 <= x && x < n && 0 <= y && y < n;
    }
}
```