## 디저트 카페([SWEA](https://swexpertacademy.com/main/code/problem/problemDetail.do?contestProbId=AV5VwAr6APYDFAWu) 2105)

## 풀이

DFS를 이용한 백트래킹 문제이다. 사각형이 만들어질 수 있는 시작점을 탐색하고, 대각선 방향으로만 이동하면서 사각형을 만든다.

시작점으로 돌아오면서 길이가 4 이상인 경우에만 사각형이 완성된다. 디저트 종류는 중복되지 않아야 하므로 List를 사용하여 관리한다.

## 해답

```java
/*Swea 2105 디저트카페
 * 메모리 :29,156 kb
 * 시간: 208 ms
 */
import java.io.*;
import java.util.*;

public class Solution_2105_남윤서 {
	static int [][] map;
	static int max;
	static int N;
	static List<Integer> desert;
	static int [][]dr = {{1,1},{1,-1},{-1, -1},{-1,1}}; // 대각선 이동 방향
	public static void main(String[] args)throws Exception{
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		int T = Integer.parseInt(br.readLine());
		for(int t = 1; t <= T; t++) {
			N = Integer.parseInt(br.readLine());
			map = new int[N][N];
			desert = new ArrayList<>(); // 그냥 시작점마다 갱신하는게 나을까 하나를 만들고 비우는게 나을까?
			max = -1;
			for(int i = 0; i < N; i++) {
				StringTokenizer st = new StringTokenizer(br.readLine());
				for(int j = 0; j < N; j++) {
					map[i][j] = Integer.parseInt(st.nextToken());
				}
			}
			
			for(int i = 0; i < N -1; i++) {
				for(int j = 1; j < N -1; j++) {//사각형이 만들어질 수 있는 시작점 탐색 
					dfs(i,j,i,j,0,0); // 시작점은 고정
				}
			}
			System.out.printf("#%d %d\n",t,max);
		}

	}
	
	static void dfs(int si,int sj, int i, int j,int d, int len) {
			if(si == i && sj == j && len >= 4) { // 시작점으로 돌아오면 사각형 완성(길이 4이상)
				max = Math.max(max, len); 
				return;
			}
			for(int di = d; di <= d+1; di++) { 
				if (di > 3) return;
				int ni = i + dr[di][0];
				int nj = j + dr[di][1];
				if(ni >=0 && nj >= 0 && ni < N && nj < N) {
					if(!desert.contains(map[ni][nj])) {
						desert.add(map[ni][nj]);
						dfs(si,sj,ni,nj,di,len+1);
						desert.remove(desert.size()-1); // 탐색이 끝나면 디저트 비우기
					}
				}
			}
		}
}
```