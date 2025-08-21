## 결혼식([BOJ](https://www.acmicpc.net) 5567)

## 풀이

친구와 친구의 친구까지만 결혼식에 초대하는 문제입니다.
양방향 그래프를 구성하고 BFS를 사용하여 depth가 2 이하인 노드들만 탐색합니다.
Queue에 노드와 depth를 함께 저장하여 깊이를 제한합니다.

## 해답

```java
/*
5567. 결혼식
메모리: 6,748KB
시간: 120ms
아이디어: list를 배열로 저장해 그래프(양방향)을 만들고 Queue에 해당 노드와 depth를 저장. depth가 2이상이면 그 이상 depth는 탐색을 멈춤
*/
package ssafy_algo;

import java.io.*;
import java.util.*;
public class Main_5567_남윤서 {

	public static void main(String[] args)throws IOException {
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		int n = Integer.parseInt(br.readLine()); // 동기의 수
		int m = Integer.parseInt(br.readLine()); // 친구 관계 수
		
		List<Integer>[] graph = new ArrayList[n+1];
		for(int i = 1; i <= n; i++) {
			graph[i] = new ArrayList<>();
		}
		
		for(int i = 0; i < m; i++) {
			StringTokenizer st = new StringTokenizer(br.readLine());
			int a = Integer.parseInt(st.nextToken());
			int b = Integer.parseInt(st.nextToken());
			graph[a].add(b);
			graph[b].add(a);
			
		}
		
		boolean[] visited = new boolean[n+1];
		Queue<int[]> q = new LinkedList<>();
		
		q.add(new int[] {1,0});
		visited[1] = true;// 상근이에서 BFS 시작
		
		int count = 0;// 친구 수 세기
		while(!q.isEmpty()) { 
			int[] cur = q.poll();
			int node = cur[0];
			int depth = cur[1];
			
			if(depth >= 2) continue; // 친구의 친구까지만
			
			for(int next: graph[node]) { // 해당 노드의 친구를 찾고 큐에 저장
				if(!visited[next]) {
					visited[next] = true;
					count++;
					q.add(new int[]{next, depth+1});
				}
			}
		}
		System.out.println(count);
	}

}
```