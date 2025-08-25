## 치즈도둑([SWEA](https://swexpertacademy.com/main/code/problem/problemDetail.do?contestProbId=AWuSgKpqmooDFASy) 7733)

## 풀이

BFS를 이용한 덩어리 세기 문제이다. 매일 특정 맛의 치즈가 먹히고, 남은 치즈 덩어리의 개수를 세어 최댓값을 구한다.

각 날짜마다 해당 맛의 치즈를 0으로 만들어 먹힌 것으로 처리하고, BFS로 연결된 치즈 덩어리를 세어 최댓값을 갱신한다. 처음 덩어리는 1개부터 시작한다.

## 해답

```java
/*Swea 7733 치즈도둑
 * 메모리: 95,720 kb 실행시간 : 323 ms
 * 큐를 이용하여 덩어리 세기 !
 */
import java.io.*;
import java.util.*;

public class Solution_7733_남윤서 {
	static int [][] taste;
	static int N;
	static int[][] dr = {{-1,0},{1,0},{0,-1},{0,1}};
	static boolean [][] visited;
	static int max ;
	public static void main(String[] args)throws Exception {
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		int T = Integer.parseInt(br.readLine());
		for(int t = 1; t <= T; t++) {
			max = 1; // 
			N = Integer.parseInt(br.readLine());
			taste = new int[N][N];
			for(int i = 0; i < N; i++) {
				StringTokenizer st = new StringTokenizer(br.readLine());
				for(int j = 0; j < N; j++) {
					taste[i][j] = Integer.parseInt(st.nextToken());
				}
			}
			
			for(int day = 1; day <= 100; day++) {
				visited = new boolean[N][N];
				for(int i = 0; i < N; i++) {
					for(int j = 0; j < N; j++) {
						if(taste[i][j] == day) {
							taste[i][j] = 0; // 먹음
							visited[i][j] = true;
						}
					}
				}
				// 덩어리 세기
				max = Math.max(max, count());
			}
		
			System.out.printf("#%d %d\n",t,max);
			
		}
	}
	
	static int count() {
		int sum = 0;
		Queue<int[]>q = new LinkedList<>();
		for(int i = 0; i < N; i++) {
			for(int j = 0; j < N; j++) {
				if(taste[i][j] > 0 && !visited[i][j]) {
					sum ++;
					visited[i][j] = true;
					q.add(new int[] {i,j});
					while(!q.isEmpty()) {
						int [] cur = q.poll();
						int si = cur[0]; 
						int sj = cur[1];
						for(int d = 0; d < 4; d++) {
							int ni = si + dr[d][0];
							int nj = sj + dr[d][1];
							if(ni < 0 || nj < 0 || ni >=N || nj >=N) continue;
							if(visited[ni][nj] == true) continue;
							if(taste[ni][nj] == 0) continue;//
							visited[ni][nj] = true;
							q.add(new int[] {ni,nj});
						}
					}
					
				}
			}
		}
		return sum;
	}

}
```