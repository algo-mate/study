## 작업순서([SWEA](https://swexpertacademy.com/main/code/problem/problemDetail.do?contestProbId=AV2-AAkqAzkDFAUu) 1267)

## 풀이

위상정렬 문제로, BFS를 사용하여 구현했다. 진입 차수가 0인 정점부터 시작하여 선행 작업을 끝내면서 다음 작업의 우선순위를 변경한다.

각 정점의 진입 차수를 관리하고, 진입 차수가 0이 되면 큐에 추가하여 탐색을 진행한다.

## 해답

```java
import java.io.BufferedReader;
import java.io.FileInputStream;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.ArrayDeque;
import java.util.ArrayList;
import java.util.Queue;
import java.util.StringTokenizer;

/*
 * Solution 1267 작업순서
 * 
 * 메모리: 30,184 KB / 실행 시간: 167 ms
 * 
 * 위상정렬 문제. bfs 사용하여 구현.
 */
public class Solution_1267_장현준 {

	private static final int T = 10; // 테스트케이스 수
	
	private static ArrayList<ArrayList<Integer>> task; // 그래프
	private static int [] indgree; // 진입 차수
	private static ArrayList<Integer> taskOrder; // 출력 결과. 올바른 작업 순서들 중 하나.
	
	public static void main(String[] args) throws IOException {
//		System.setIn(new FileInputStream("res/input_1267.txt"));
		
		BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(System.in));
		
		for (int t = 1; t <= T; t++) {
			StringTokenizer stringTokenizer = new StringTokenizer(bufferedReader.readLine());
			
			int V = Integer.parseInt(stringTokenizer.nextToken()); // 그래프의 정점의 개수
			int E = Integer.parseInt(stringTokenizer.nextToken()); // 그래프의 간선의 개수
			
			task = new ArrayList<>(); // 그래프 생성
			for (int i = 0; i <= V; i++) {
				task.add(new ArrayList<>());
			}
			
			indgree = new int[V + 1]; // 인덱스에 해당하는 각 정점 번호의 진입 차수 저장
			
			stringTokenizer = new StringTokenizer(bufferedReader.readLine());
			
			for (int i = 0; i < E; i++) {
				int from = Integer.parseInt(stringTokenizer.nextToken());
				int to = Integer.parseInt(stringTokenizer.nextToken());
				
				task.get(from).add(to);
				indgree[to]++;
			}
			
			taskOrder = new ArrayList<>(); // 출력할 올바른 작업순서 1개
			Queue<Integer> queue = new ArrayDeque<>();
			boolean[] visited = new boolean[V + 1];
			
			for (int i = 1; i <= V; i++) {
				if (indgree[i] == 0) { // 진입차수 0 == 부모 노드가 0인 정점 == 작업순서 1순위 중 하나
					queue.offer(i); // 탐색 순서 관리
				}
			}
			
			bfs(queue, visited); // 탐색 시작
			
			StringBuilder stringBuilder = new StringBuilder(); // 출력 결과 작성
			stringBuilder.append("#" + t + " ");
			for (int i = 0; i < V; i++) {
				stringBuilder.append(taskOrder.get(i));
				stringBuilder.append(" ");
			}
			
			System.out.println(stringBuilder); // 출력
		}
	}
	
	private static void bfs(Queue<Integer> queue, boolean[] visited) {
		
		visited[queue.peek()] = true; // 진입차수 0 정점 == 작업순서  1순위 정점 부터 탐색 시작
		
		while (!queue.isEmpty()) {
			int current = queue.poll();
			taskOrder.add(current); // 작업
			
			for (int next : task.get(current)) {
				indgree[next]--; // 선행 작업 끝났으니 다음 작업을 우선 순위로 변경 == 다음 정점 진입 차수 -1
				if(indgree[next] == 0 && !visited[next]) { // 변경된 작업 우선 순위가 0 && 아직 방문안했을 경우 
					visited[next] = true;
					queue.offer(next);  // 작업 우선 순위순으로 큐에 추가
				}
			}	
		}
	}
}
```