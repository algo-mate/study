## 최소스패닝트리([SWEA](https://swexpertacademy.com/main/code/problem/problemDetail.do?contestProbId=AV_mSnmKUckDFAWb) 3124)

## 풀이

크루스칼 알고리즘을 사용하여 최소스패닝트리(MST) 문제를 해결했습니다.
- 모든 간선을 가중치 기준으로 오름차순 정렬
- Union-Find 자료구조를 사용하여 사이클 검사
- 가중치가 작은 간선부터 선택하여 MST 구성
- cost를 long으로 처리하여 가중치 합이 커질 경우 대비
- 정확히 V-1개의 간선을 선택하여 트리 완성

## 해답

```java
package ssafy_algo;

import java.io.*;
import java.util.*;
public class Solution_3124_남윤서 { // 크루스칼 알고리즘
	static int[] parents;
	static int[][] graph;
	static long cost; // cost를 long으로 잡아줘야함 가중치의 합 매우 커질 수 있음!!!!!!!!!!!!!!!!!
	public static void main(String[] args) throws Exception{
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		int T = Integer.parseInt(br.readLine());
		for(int t = 1; t <= T; t++) {
			StringTokenizer st = new StringTokenizer(br.readLine());
			int V = Integer.parseInt(st.nextToken());
			int E = Integer.parseInt(st.nextToken());
			
			graph = new int[E][3];
			for(int i = 0; i < E; i++) {
				st = new StringTokenizer(br.readLine());
				graph[i][0] = Integer.parseInt(st.nextToken());
				graph[i][1] = Integer.parseInt(st.nextToken());
				graph[i][2] = Integer.parseInt(st.nextToken()); 
			}
			
			// 간선 정렬 !!!!
			Arrays.sort(graph, (a, b) -> a[2] - b[2]); // graph[2]를 기준으로 오름차순 정렬
			//Arrays.sort(graph, Comparator.comparingInt(a -> a[2])); 이것도 가능
			parents = new int[V+1];
			for(int i = 1; i <= V ; i++) {
				parents[i] = i;
			}
			
			kruskal();
			System.out.printf("#%d %d\n",t, cost);
		}
	}
	static int find(int x) {
		if(parents[x] == x) return x;
		else return find(parents[x]);
	}
	
	static void union(int x, int y) {
		int xRoot = find(x);
		int yRoot = find(y);
		if(xRoot > yRoot) parents[xRoot] = yRoot;
		else parents[yRoot] = xRoot;
	}
	
	static void kruskal() {
		cost = 0; 
		for(int i = 0; i < graph.length; i++) {
			if(find(graph[i][0]) != find(graph[i][1])) { // 같은 집합이 아니라면
				cost += graph[i][2]; // 간선 가중치 더해주기
				union(graph[i][0], graph[i][1]); // 연결됨
			}
		}
	}

}
```