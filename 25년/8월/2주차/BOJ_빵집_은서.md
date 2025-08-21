## 빵집(BOJ 3109)

## 풀이

앞 단계의 최적 선택이 다음 단계, 전체 최적에도 영향을 주는 그리디 알고리즘 문제이다.

갈 수 있는 방향을 오른쪽 위 -> 오른쪽 -> 오른쪽 아래 순서로 탐색하여 아래 행들의 경로 확보를 위한 최적의 순서를 유지한다.

가장 최적의 순서대로 탐색했으므로, 목표지점에 도달한 순간이 가장 best 경로이다. 파이프라인 확보 후 즉시 종료한다.

## 해답

```java
/* 날짜: 2025.08.13
 * 문제 : Main_3109. 빵집 
 * 난이도 : 골드2
 * 메모리 : 49260kb
 * 시간 : 412ms 
 * 아이디어 : 앞 단계의 최적 선택이 다음 단계, 전체 최적에도 영향을 준다  -> 그리디. 최적 경로 탐색 
 */
public class Main_3109 {

	static int R, C, maxCnt;
	static char[][] map;
	static int[] dx, dy;
	static boolean [][] visited;
	static int pipelineCnt;
	public static void main(String[] args) throws IOException {
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		StringTokenizer st = new StringTokenizer(br.readLine());
		
		R = Integer.parseInt(st.nextToken());		
		C = Integer.parseInt(st.nextToken());
		map = new char[R][C];
		maxCnt = R; 
		visited = new boolean[R][C];
		pipelineCnt = 0;
		
		//갈 수 있는 방향: 오른쪽 위 -> 오른쪽 -> 오른쪽 아래 (아래 행들의 경로 확보를 위한 최적의 순서)
		dx = new int[]{-1, 0, 1}; 
		dy = new int[]{1, 1, 1};
		
		for(int i=0; i<R; i++) {
			char[] arr = br.readLine().toCharArray();
			for(int j=0; j<C; j++) {
				map[i][j] = arr[j]; 
			}
		}

		for(int i=0; i<R; i++) {
			visited[i][0] = true;
			getPipeline(i,0);
		}
		
		System.out.println(pipelineCnt);
		
	}

	public static boolean getPipeline(int x, int y) {
		//기저조건: 목표지점인 빵집까지 도달했을 때 -> 파이프라인 추가 -> true 반환 -> for문 즉시 종료
		if(y == C-1) {
			pipelineCnt++;
			return true;
		}
		//오른쪽 위 -> 오른쪽 -> 오른쪽 아래
		for(int i=0; i<3; i++) {
			int nx = x+dx[i];
			int ny = y+dy[i];
			
			if(0<=nx && nx<R && 0<=ny && ny<C && map[nx][ny]!='x' && !visited[nx][ny]) {
				visited[nx][ny] = true;
				//가장 최적의 순서대로 탐색했으므로, 목표지점에 도달한 순간이 가장 best 경로 -> 파이프라인 확보 후 즉시 종료
				if(getPipeline(nx, ny)) return true;
			}
		}
		
		return false;
		
	}

}
```