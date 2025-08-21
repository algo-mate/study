## 농작물 수확하기(SWEA 2805)

## 풀이

BFS를 활용하여 농장 중심에서 시작해서 일정 거리 이내의 농작물들을 수확하는 문제이다.

농장 중심에서 시작하여 상하좌우로 탐색하면서 거리가 N/2 이하인 농작물들의 합을 구한다.

큐를 사용하여 레벨별로 탐색하고, 방문 체크를 통해 중복 방문을 방지한다.

## 해답

```java
// SWEA 2805. 농작물 수확하기
// 난이도: D3
// 메모리 32,204 kb
// 실행시간: 116ms
public class Swea_2805 {

	public static void main(String[] args) throws IOException {
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		int T = Integer.parseInt(br.readLine());
		
		for(int test_case=0; test_case<T; test_case++) {
			// 농장 크기 N*N
			int N = Integer.parseInt(br.readLine());
			int[][] arr = new int[N][N];
			boolean[][] visited = new boolean[N][N];
			
			// 배열 입력 받기
			for(int i=0; i<N; i++) {
				String numLine = br.readLine();
				for(int j=0; j<N; j++){
					arr[i][j] = numLine.charAt(j) - '0'; //공백 없이 입력 받을 때 각 숫자 저장하는 방식
					visited[i][j] = false;
				}
			}
			
			Queue<int[]> dq = new ArrayDeque<>();
			int x = N/2;
			int y = N/2;
			int z = 0;
			
			int[] dx = {0,0,-1,1};
			int[] dy = {-1,1,0,0};
			
			dq.offer(new int[] {x, y, z});
			
			int sum = arr[x][y];
			visited[x][y] = true;

			while(!dq.isEmpty()) {
				int[] pos = dq.poll();
				x = pos[0];
				y = pos[1];
				z = pos[2];
				
				// N*N일 때 테두리 N/2번 탐색 -> 더 바깥의 인덱스 탐색하지 않도록 break
				if(z >= N/2) {
					break;
				}
				
				
				for(int i=0; i<4; i++) {
					// x, y 기준 상하좌우 탐색 
					int nx = x+dx[i];
					int ny = y+dy[i];
					int nz = z+1;
					// 전체 범위 체크, 방문 여부 체크
					if(0<=nx && nx<N && 0<=ny && ny<N && !visited[nx][ny]) {
						visited[nx][ny] = true;
						dq.offer(new int[]{nx, ny, nz}); //이후 탐색 할 인덱스 push 
						sum += arr[nx][ny];
					}
					
				}
				
			}
			
			System.out.printf("#%d %d", test_case+1, sum);
			System.out.println();
		}
	}
}
```