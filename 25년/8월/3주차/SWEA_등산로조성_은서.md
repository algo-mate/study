## 등산로 조성([SWEA](https://swexpertacademy.com/main/code/problem/problemDetail.do?contestProbId=AV5PoOKKAPIDFAUq) 1949)

## 풀이

DFS를 이용한 백트래킹 문제이다. 최장 내리막길 경로를 만들어야 하므로, 이전 봉우리의 높이에서 딱 1만 깎는게 최선이다.

딱 한 봉우리만 깎을 수 있으므로 mowed 매개변수를 넣어서 false일 때만 깎도록 해야 한다. 임시 변수를 이용해서, 깎기 전 높이로 그대로 원복해 놓아야 한다.

## 해답

```java
package day_250820;
/*
 * 날짜: 2025.08.20
 * 문제: Solution_1949. 등산로 조성
 * 메모리: 26,240 kb
 * 시간: 94 ms
 * 아이디어: dfs 함수 내에서, 깎을 수 있는 조건일 때의 로직이 핵심
 * 		-. 최장 내리막길 경로를 만들어야 하므로, 이전 봉우리의 높이에서 딱 1만 깎는게 최선 (가능한 높게 깎기)
 * 		-. 딱 한 봉우리만 깎을 수 있으므로 mowed 매개변수를 넣어서 false일 때만 깎도록 해야 함
 * 		-. 임시 변수를 이용해서, 깎기 전 높이로 그대로 원복해 놓아야 함
 * 		-. 그냥 내리막길일 때 mowed 매개 변수는 false, true 조작하지 말고 변수 그대로 넣어주기
 */
import java.io.*;
import java.util.*;

public class Solution_1949 {

	static int N, K;
	static int[][] mountain;
	static int[] dx = {-1,1,0,0};
	static int[] dy = {0,0,-1,1};
	static int longest ;
	static boolean[][] visited;
	
	public static void main(String[] args) throws IOException {
		// TODO Auto-generated method stub
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringBuilder sb = new StringBuilder();
		StringTokenizer st;
		int T =  Integer.parseInt(br.readLine());
		
		for(int t=1; t<=T; t++) {
			st = new StringTokenizer(br.readLine());
			N = Integer.parseInt(st.nextToken()); // 숲 크기
			K = Integer.parseInt(st.nextToken()); // 깎을 수 있는 최대 깊이
		
			mountain = new int[N][N]; //산 배열 선언
			
			
			//산 배열 입력받기
			List<int[]> topArr = new ArrayList<>(); 
			int max = 0;
			
			for(int i=0; i<N; i++) {
				st = new StringTokenizer(br.readLine());
				for(int j=0; j<N; j++) {
					mountain[i][j] = Integer.parseInt(st.nextToken());
					max = Math.max(max, mountain[i][j]);
					
				}
			}
			
			//최대 높이 봉우리의 위치를 담는다
			for(int i=0; i<N; i++) {
				for(int j=0; j<N; j++) {
					if(max == mountain[i][j]) {
						topArr.add(new int[] {i,j});
					}
				}
			}
			
			visited = new boolean[N][N];
			longest = 0;
			//모든 꼭대기 봉우리에서 시작하는 최장 경로를 계산한다. 
			for(int i=0; i<topArr.size(); i++) {
				int[] tempLi = topArr.get(i);
				visited[tempLi[0]][tempLi[1]] = true;  // i번째 꼭대기 봉우리 방문 처리 
				dfs(tempLi[0], tempLi[1], false, 1);   // i번째 꼭대기 봉우리에서 시작하는 최장 경로 탐색
				visited[tempLi[0]][tempLi[1]] = false; // 다음 꼭대기 봉우리 영향을 주면 안돼 -> false로 원복 
			}
			
			System.out.println("#" + t + " " + longest);

		}
		
	}
	
	static void dfs(int x, int y, boolean mowed, int len) {

		for(int i=0; i<4; i++) {
			int nx = x+dx[i];
			int ny = y+dy[i];
			
			//범위 안이고, 현재 len에서의 방문 체크
			if(0<=nx && nx<N && 0<=ny && ny<N && !visited[nx][ny]) {
				//다음 봉우리 높이가 지금 보다 작을 때 -> 평범하게 경로 진행
				if(mountain[x][y] > mountain[nx][ny]) {
					visited[nx][ny] = true;
					dfs(nx, ny, mowed, len+1);
					visited[nx][ny] = false;
				}
			
				//가능한 최대 크기인 K를 깎았을 때 이전보다 작다면, mountain[x][y]-1로 만드는게 가장 최선!
				//K만큼 깎아도 mountain[x][y] 보다 크면? 볼 필요도 없음
				else if(!mowed &&  mountain[nx][ny]-K < mountain[x][y]) {  
						int origin = mountain[nx][ny];
				        mountain[nx][ny] = mountain[x][y]-1; // 깎기
				        visited[nx][ny] = true;
				        dfs(nx, ny, true, len+1);
				        visited[nx][ny] = false;
				        mountain[nx][ny] = origin; //복원
				
				}
				
			}
		}

		if(longest < len) longest = len;

	}
		
	
}
```