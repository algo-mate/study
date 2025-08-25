## 디저트 카페([SWEA](https://swexpertacademy.com) 2105)

## 풀이

DFS로 완전탐색으로 접근했는데 처음에는 타임아웃이 발생했습니다.
DFS 가지치기를 통해 4^n에서 n^m으로 시간복잡도를 단축하여 해결했습니다.
방향을 넘겨줘서 사각형 길이로만 탐색하게 가지치기해주면 대폭 단축됩니다.

## 해답

```java
/*
2105. 디저트 카페
메모리: 30,156KB
시간: 237ms
아이디어: DFS
        1. 처음에는 DFS로 완전탐색으로 접근하다가 타임아웃
        2. DFS 가지치기를 통해 4^n -> n^m으로 시간복잡도 단축
*/
import java.util.*;
import java.io.*;

public class Solution_2105_강보승 {
	static int[] dx = new int[] {1, 1, -1, -1};
	static int[] dy = new int[] {1, -1, -1, 1};
	static int T, n, answer;
	static int[][] arr;
	static boolean[][] visited;
	static List<Integer> list;
	
	public static void main(String[] args) throws Exception{
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		T = Integer.parseInt(br.readLine());
		for(int t = 1; t <= T; t++) {
			n = Integer.parseInt(br.readLine());
			arr = new int[n][n];
			visited = new boolean[n][n];
			for(int i = 0; i < n; i++) {
				StringTokenizer st = new StringTokenizer(br.readLine());
				for(int j = 0; j < n; j++) {
					arr[i][j] = Integer.parseInt(st.nextToken());
				}
			}
			list = new ArrayList<>();
			answer = -1;
			for(int i = 0; i < n - 2; i++) {
				for(int j = 1; j < n - 1; j++) {
					visited[i][j] = true;
					list.add(arr[i][j]);
					dfs(i, j, i, j, 1, 0);
					list.remove(list.size() - 1);
					visited[i][j] = false;
				}
			}
			System.out.println("#" + t + " " + answer);
		}
	}
	
	static void dfs(int x, int y, int rx, int ry, int count, int dir) {
        // 방향을 넘겨줘서 사각형 길이로만 탐색하게 가지치기해주면 대폭 단축
		for(int i = dir; i < 4; i++) {
			int nx = x + dx[i];
			int ny = y + dy[i];
			if(inRange(nx, ny)) {
				if(!list.contains(arr[nx][ny]) && !visited[nx][ny]) {
					visited[nx][ny] = true;
					list.add(arr[nx][ny]);
					dfs(nx, ny, rx, ry, count + 1, i);
					visited[nx][ny] = false;
					list.remove(list.size() - 1);
				}
				if(nx == rx && ny == ry && count >= 4 && count > answer) {
					answer = Math.max(answer, count);
				}
			}
		}
	}
	
	static boolean inRange(int x, int y) {
		return 0 <= x && x < n && 0 <= y && y < n;
	}
}
```