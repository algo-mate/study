## 등산로 조성([SWEA](https://swexpertacademy.com) 1949)

## 풀이

DFS로 풀어야 하는 문제입니다. 처음에 BFS로 접근했는데, 디버깅 하다가 DFS로 접근해야 한다는 것을 발견했습니다.
BFS는 최단거리 구할 때, DFS는 한 경로를 깊게, 길게 구할 때 사용합니다.
배열 값을 바꿀 때, 그대로 사용하는 게 아니라 원래 값을 변수에 담아서 사용해야 서로 영향을 안 받습니다.

## 해답

```java
/*
1949. 등산로 조성
메모리: 26,112KB
시간: 83ms
아이디어: DFS
        1. 처음에 BFS로 접근했는데, 디버깅 하다가 DFS로 접근해야 한다는 것을 발견
        2. BFS는 최단거리 구할 때, DFS는 한 경로를 깊게, 길게 구할 때 사용
        3. 배열 값을 바꿀 때, 그대로 사용하는 게 아니라 원래 값을 변수에 담아서 사용해야 서로 영향을 안 받음
*/
import java.io.*;
import java.util.*;

public class Solution_1949_강보승 {
    static int[] dx = {-1, 1, 0, 0};
    static int[] dy = {0, 0, -1, 1};
    static int T, n, k, maxHeight, maxDepth;
    static int[][] arr;
    static boolean[][] visited;
    static Queue<int[]> queue;
    
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        T = Integer.parseInt(br.readLine());
        for (int t = 1; t <= T; t++) {
            StringTokenizer st = new StringTokenizer(br.readLine());
            n = Integer.parseInt(st.nextToken());
            k = Integer.parseInt(st.nextToken());
            maxHeight = 0;
            maxDepth = 0;
            arr = new int[n][n];
            for(int i = 0; i < n; i++){
                st = new StringTokenizer(br.readLine());
                for(int j = 0; j < n; j++){
                    arr[i][j] = Integer.parseInt(st.nextToken());
                    maxHeight = Math.max(maxHeight, arr[i][j]);
                }
            }
            visited = new boolean[n][n];
            for(int i = 0; i < n; i++){
                for(int j = 0; j < n; j++){
                    if(arr[i][j] == maxHeight){
                        visited[i][j] = true;
                        dfs(i, j, 1, true);
                        visited[i][j] = false;
                    }
                }
            }
            System.out.println("#" + t + " " + maxDepth);
        }
    }
    
    static void dfs(int i, int j, int depth, boolean canCut) {
    	for(int dir = 0; dir < 4; dir++) {
    		int nx = i + dx[dir];
    		int ny = j + dy[dir];
    		if(inRange(nx, ny) && !visited[nx][ny] && arr[i][j] > arr[nx][ny]) {
    			visited[nx][ny] = true;
    			dfs(nx, ny, depth + 1, canCut);
    			visited[nx][ny] = false;
    			maxDepth = Math.max(depth + 1, maxDepth);
    		}else if(inRange(nx, ny) && !visited[nx][ny] && canCut && arr[i][j] <= arr[nx][ny] && arr[i][j] > arr[nx][ny] - k){
    			visited[nx][ny] = true;
                // 꼭 변수에 저장해야 원래 값 유지됨... 배열로 할 경우 값이 영향을 받아서 이상하게 나옴
    			int originalValue = arr[nx][ny] - arr[i][j] + 1;
    			arr[nx][ny] -= originalValue;
    			maxDepth = Math.max(depth + 1, maxDepth);
    			dfs(nx, ny, depth + 1, false);
    			arr[nx][ny] += originalValue;
    			visited[nx][ny] = false;
    		}
    	}
    }
    
    static boolean inRange(int x, int y){
        return 0 <= x && x < n && 0 <= y && y < n;
    }
}
```