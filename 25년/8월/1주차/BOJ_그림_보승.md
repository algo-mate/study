## 그림(실버 1)

## 해설

아이디어: dfs, bfs 어느 것으로도 풀어도 가능한 문제
1. 처음 방문 시작하는 경우 세는 방식으로 그림의 개수 구하기
2. 연속으로 방문할 경우 그림의 넓이 구하기

## 정답

```java
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.util.StringTokenizer;
import java.util.*;
/* Main 1926 그림
	메모리: 57580kb
	시간: 336ms
*/ 
public class Main_1926_강보승 {
	static int n, m, maxPictureSize;
	static int[] dx, dy;
	static int[][] arr;
	static boolean[][] visited;
	static Queue<int[]> queue = new LinkedList<>();
    public static void main(String[] args) throws IOException{
    	BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
    	StringTokenizer st = new StringTokenizer(br.readLine());
    	n = Integer.parseInt(st.nextToken());
    	m = Integer.parseInt(st.nextToken());
    	dx = new int[] {0, 1, 0, -1};
    	dy = new int[] {1, 0, -1, 0};
    	arr = new int[n][m];
    	visited = new boolean[n][m];
    	for(int i = 0; i < n; i++) {
    		st = new StringTokenizer(br.readLine());
    		for(int j = 0; j < m; j++) {
    			arr[i][j] = Integer.parseInt(st.nextToken());
    		}
    	}
    	int count = 0;
    	for(int i = 0; i < n; i++) {
    		for(int j = 0; j < m; j++) {
    			if(!visited[i][j] && arr[i][j] == 1) {
    				//처음 방문시 카운트
    				count++;
    				visited[i][j] = true;
    				maxPictureSize = Math.max(maxPictureSize, dfs(i, j));
    			}
    		}
    	}
    	System.out.println(count);
    	System.out.println(maxPictureSize);
    }
    static int dfs(int x, int y) {
    	int count = 1;
    	for(int i = 0; i < 4; i++) {
    		int nx = x + dx[i];
    		int ny = y + dy[i];
    		// 재귀적으로 연속 방문할 경우 세주기
    		if(inRange(nx, ny) && !visited[nx][ny] && arr[nx][ny] == 1) {
    			visited[nx][ny] = true;
        		count += dfs(nx, ny);
        	}
    	}
    	return count;
    }
    static boolean inRange(int x, int y) {
    	return 0 <= x && x < n && 0 <= y && y < m;
    }
}

```