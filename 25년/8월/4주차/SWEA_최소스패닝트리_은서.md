## 최소스패닝트리([SWEA](https://swexpertacademy.com/main/code/problem/problemDetail.do?contestProbId=AV_mSnmKUckDFAWb) 3124)

## 풀이

크루스칼 알고리즘을 사용한 최소 신장 트리(MST) 구현으로, 자료형 오버플로우에 주의한 문제입니다.

주어진 무방향 가중 그래프에서 모든 정점을 연결하면서 간선의 가중치 합이 최소가 되는 트리를 찾습니다.

핵심 주의사항:
- 간선 수 최대 200,000개, 가중치 최대 1,000,000
- 최악의 경우 총 가중치가 int형 범위를 초과할 수 있음
- 따라서 가중치와 합계를 long형으로 처리

알고리즘 단계:
1. 모든 간선을 가중치 기준으로 오름차순 정렬
2. Union-Find로 각 정점을 독립된 집합으로 초기화
3. 정렬된 간선을 순서대로 검사하며 사이클이 없는 간선만 선택
4. V-1개의 간선 선택 시 MST 완성

## 해답

```java
package day_250827;
/*
 * 날짜: 2025.08.26
 * 문제: Solution_3124. 최소 스패닝 트리(MST)
 * 난이도: D4
 * 메모리: 120,016 kb
 * 시간: 1,645 ms
 * 아이디어: 가중치의 데이터 타입을 Long으로 선언해주어야 함. 간선의 최댓값=200,000, 가중치의 최댓값=1,000,000이므로 int형의 최댓값을 훨씬 상회함
 * 
 */
import java.io.*;
import java.util.*;

public class Solution_3124 {
	
	static int V;
	static int E;
	static List<Edge> edgeList;
	static int[] root;
	
	//간선 클래스 (오름차순 정렬)
	static class Edge implements Comparable<Edge>{
		int from;
		int to;
		long weight;
		
		public Edge(int from, int to, long weight) {
			super();
			this.from = from;
			this.to = to;
			this.weight = weight;
		}
		
		public int compareTo(Edge o) {
			return Long.compare(this.weight, o.weight); // 오름차순
		}
		

		public String toString() {
			// TODO Auto-generated method stub
			return"Edge{" + from + " -> " + to + ", w=" + weight + "}";
		}
		
	}
	
	public static void main(String[] args) throws IOException {
		// TODO Auto-generated method stub
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		StringBuilder sb  = new StringBuilder();
		StringTokenizer st;
		
		int T = Integer.parseInt(br.readLine());
		
		for(int t=1; t<=T; t++){
			
			st = new StringTokenizer(br.readLine());
			V = Integer.parseInt(st.nextToken());
			E = Integer.parseInt(st.nextToken());
			edgeList = new ArrayList<Edge>();
			root = new int[V+1];
			
			for(int j=0; j<E; j++) {
				st = new StringTokenizer(br.readLine());
				int from = Integer.parseInt(st.nextToken());
				int to = Integer.parseInt(st.nextToken());
				int weight = Integer.parseInt(st.nextToken());
				edgeList.add(new Edge(from, to, weight));
				
			}
			
			Collections.sort(edgeList);
			
			long weightSum = 0L;
			int count = 0;
			
			make();
			
			for(Edge e : edgeList) {
				if(!union(e.from, e.to)) continue; //사이클 발생
				weightSum += e.weight;
				if(++count == V-1) break; 
			}
			
			System.out.println("#"+t+" "+weightSum);
			
		}
	}
	
	static void make() {
		for(int i=1; i<V+1; i++) {
			root[i] = i;
		}
	}
	
	static int find(int x) {
		if(x==root[x]) return x;
		else return root[x] = find(root[x]);
	}
	
	static boolean union(int x, int y) {
		int rx = find(x);
		int ry = find(y);
		
		if(rx == ry) return false;
		
		if(rx > ry)  root[ry] = rx;
		else root[rx] = ry;
		
		return true;
	}
	
}
```