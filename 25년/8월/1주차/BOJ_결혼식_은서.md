## 결혼식(BOJ 5567)

## 풀이

처음에 DFS 탐색을 했지만, 이미 방문처리한 친구에 대한 친구의 친구를 탐색하지 못하는 현상이 발생했다.

따라서 DFS 부적합하고, 깊이 우선이 아닌 내 친구들 먼저 다 탐색하는 너비우선탐색 BFS로 진행해야 한다.

친구의 친구까지만 체크하므로 depth가 2가 되면 탐색을 중단한다.

## 해답

```java
// 2025.08.05
//Main_5567 결혼식
//난이도: 실버2
//메모리: 18964kb
//시간: 168ms
//아이디어: 처음에 dfs 탐색을 했지만, 이미 방문처리한 친구에 대한 친구의 친구를 탐색하지 못하는 현상이 발생
//		따라서 dfs 부적합. 깊이 우선이 아닌 내 친구들 먼저 다 탐색하는 너비우선탐색 bfs로 진행해야 함

// 5567. 결혼식
public class Main_5567 {
	static int N, M;
	static boolean[] visited;
	static List<Integer>[] node;
	static int cnt = 0;
	
	public static void main(String[] args) throws IOException {
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		
		StringTokenizer st = new StringTokenizer(br.readLine());
		N = Integer.parseInt(st.nextToken());
		st = new StringTokenizer(br.readLine());
		M = Integer.parseInt(st.nextToken());
	
		//그래프 노드 문제에서 보통 사용하는 타입
		//List<Integer>[] node = new ArrayList[N + 1];
		node = new ArrayList[N + 1];
		visited = new boolean[N+1];
	
		//각 배열 객체 초기화
		for(int i=0; i<N+1; i++) {
			node[i] = new ArrayList<>();
		}
		
		for(int i=0; i<M; i++) {
			st = new StringTokenizer(br.readLine());
			int f1 = Integer.parseInt(st.nextToken());
			int f2 = Integer.parseInt(st.nextToken());
			
			node[f1].add(f2);
			node[f2].add(f1);
		}

		bfs(1);
		System.out.println(cnt);

	}
	
	public static void bfs(int x) {
		Queue<int[]> queue = new ArrayDeque<>();
		//현재 노드와 depth 배열 offer
		queue.offer(new int[] {x, 0});
		visited[x] = true;
		
		while(!queue.isEmpty()) {
			int[] temp = queue.poll();
			int pos = temp[0];
			int depth = temp[1];
			
			// 친구의 친구까지만 체크
			if (depth >= 2) {
				break;
			}
			
			for(int next : node[pos]) {
				if(!visited[next]) {
					visited[next] = true;	
					//새로운 친구 발견하면 cnt++
					cnt++;
					//새로운 친구 큐에 offer
					queue.offer(new int[] {next, depth+1});
				}
			}
		}
	}
}
```