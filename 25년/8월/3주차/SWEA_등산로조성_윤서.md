## 등산로조성([SWEA](https://swexpertacademy.com/main/code/problem/problemDetail.do?contestProbId=AV5PoOKKAPIDFAUq) 1949)

## 풀이

DFS를 이용한 백트래킹 문제이다. 가장 높은 봉우리에서 시작하여 최장 등산로를 만든다.

K만큼 봉우리를 깎을 수 있는 기회가 한 번 있으며, 깎은 후에는 원상복구를 해야 한다. DFS 탐색 시 방문 체크와 백트래킹을 통해 모든 경우를 탐색한다.

## 해답

```java
/*Swea 1949 등산로 조성
 * 메모리 : 27,892 kb 실행시간 : 105 ms
 * 등산로 깎고 복구해줘야함 !!!!
 */
import java.io.*;
import java.util.*;

public class Solution_1949_남윤서 {
    static int N, K;
    static int[][] map;
    static boolean[][] visited;
    static int[][] dr = {{-1,0},{1,0},{0,-1},{0,1}};
    static int answer;

    public static void main(String[] args)throws Exception {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int T = Integer.parseInt(br.readLine());

        for(int t = 1; t <= T; t++) {
            StringTokenizer st = new StringTokenizer(br.readLine());
            N = Integer.parseInt(st.nextToken());
            K = Integer.parseInt(st.nextToken());

            map = new int[N][N];
            int peak = 0; // 가장 높은 봉우리
            for(int i = 0; i < N; i++) {
                st = new StringTokenizer(br.readLine());
                for(int j = 0; j < N; j++) {
                    map[i][j] = Integer.parseInt(st.nextToken());
                    peak = Math.max(peak, map[i][j]);
                }
            }

            answer = 0;
            // 가장 높은 봉우리에서 DFS 시작
            for(int i = 0; i < N; i++) {
                for(int j = 0; j < N; j++) {
                    if(map[i][j] == peak) {
                        visited = new boolean[N][N];
                        dfs(i, j, 1, false); 
                    }
                }
            }

            System.out.printf("#%d %d\n", t, answer);
        }
    }

    static void dfs(int i, int j, int len, boolean digUsed) {
        visited[i][j] = true;
        answer = Math.max(answer, len);

        for(int d = 0; d < 4; d++) {
            int ni = i + dr[d][0];
            int nj = j + dr[d][1];
            if(ni < 0 || nj < 0 || ni >= N || nj >= N) continue;
            if(visited[ni][nj]) continue;

            if(map[ni][nj] < map[i][j]) { 
                // 그냥 이동 가능
                dfs(ni, nj, len+1, digUsed);
            } else if(!digUsed) { 
                // 깎아서 갈 수 있는 경우
                for(int k = 1; k <= K; k++) {
                    if(map[ni][nj] - k < map[i][j]) {
                        map[ni][nj] -= k;
                        dfs(ni, nj, len+1, true);
                        map[ni][nj] += k; // 원상복구
                    }
                }
            }
        }

        visited[i][j] = false; // 백트래킹
    }
}
```