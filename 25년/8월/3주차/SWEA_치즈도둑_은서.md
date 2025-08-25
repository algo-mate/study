## 치즈도둑([SWEA](https://swexpertacademy.com/main/code/problem/problemDetail.do?contestProbId=AWuSgKpqmooDFASy) 7733)

## 풀이

BFS를 이용한 덩어리 세는 문제이다. 먹은 표시를 위해 yumyum map을 추가해서 해결했다.

각 날짜마다 해당 맛의 치즈를 먹고, 남은 치즈 덩어리의 개수를 세어 최댓값을 갱신한다. 처음 덩어리는 1개부터 시작한다.

## 해답

```java
package day_250820;
/*
 * 날짜: 2025.08.20
 * 문제: Solution_7733. 치즈도둑
 * 메모리: 96,852 kb
 * 시간: 331 ms
 * 아이디어: bfs 덩어리 세는 로직에서, 먹은 표시  yumyum map 추가 해서 풀기
 */
import java.util.*;
import java.io.*;

public class Solution_7733 {
	static int N, maxSectionCnt;
	static int[][] cheeze;
	static boolean[][] visited; 
	static boolean[][] yumyum; 
	static int[] dx = {-1,1,0,0};
	static int[] dy = {0,0,-1,1};
	static List<int[]>[] tasteScore;
	
	public static void main(String[] args) throws IOException {
		// TODO Auto-generated method stub
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		StringTokenizer st; 
		
		int T = Integer.parseInt(br.readLine());
		
		for(int t=1; t<=T; t++) {
			N = Integer.parseInt(br.readLine());
			cheeze = new int[N][N];
			tasteScore = new ArrayList[101];
			
			for(int i=0; i<101; i++) {
				tasteScore[i] = new ArrayList<>();
			}
			
			for(int i=0; i<N; i++) {
				st = new StringTokenizer(br.readLine());
				for(int j=0; j<N; j++) {
					cheeze[i][j] = Integer.parseInt(st.nextToken());
					 //각 맛의 위치를 List애 담기
					tasteScore[cheeze[i][j]].add(new int[] {i,j});
				}
			}

			yumyum = new boolean[N][N];
			//처음 덩어리 1!!!!!!!!!
			maxSectionCnt = 1;

			for(int i=1; i<101; i++) { //100일 세기 
				//X번째 날 -> 맛이 X인  모든 위치 먹음 표시
				
				for(int[] li : tasteScore[i]) {
					yumyum[li[0]][li[1]] = true;
				}
				
				visited = new boolean[N][N];
				
				int nowCnt = getSectionCnt();

				maxSectionCnt = Math.max(maxSectionCnt, nowCnt);
				
				if(nowCnt == 0) { // 남은 치즈 덩어리가 없을 때
					break;
				}
			}
			
			System.out.println("#"+t+" "+maxSectionCnt);
		}
		
	}
	
	static int getSectionCnt() {
		int sectionCnt = 0;
		for(int i=0; i<N; i++) {
			for(int j=0; j<N; j++) {
				if(!yumyum[i][j] && !visited[i][j]) { //안 먹은거 체크해야지+안 센거
					bfs(i, j);
					sectionCnt++;
				}
			}
		}
		
		return sectionCnt;
	}
	
	static void bfs(int x, int y) {
		Queue<int[]> dq = new ArrayDeque<>();
		
		visited[x][y] = true;
		dq.offer(new int[] {x, y});
		
		while(!dq.isEmpty()) {
			int[] temp = dq.poll();
			x = temp[0]; y = temp[1];
			
			for(int i=0; i<4; i++) {
				int nx = x+dx[i]; 
				int ny = y+dy[i];

				if(nx>=0 && nx<N && ny>=0 && ny<N && !visited[nx][ny] && !yumyum[nx][ny]) {
					visited[nx][ny] = true;
					dq.offer(new int[] {nx, ny});
				}

			}
			
		}
		
	}

}
```