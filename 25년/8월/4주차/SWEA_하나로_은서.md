## 하나로([SWEA](https://swexpertacademy.com/main/code/problem/problemDetail.do?contestProbId=AV15StKqAQkCFAYD) 1251)

## 풀이

크루스칼 알고리즘을 사용한 최소 신장 트리(MST) 문제입니다.

모든 섬을 최소 환경 부담금으로 연결하는 해저터널 건설 문제로, 환경 부담금은 E × 거리²로 계산됩니다.

핵심 아이디어:
1. 모든 섬 쌍에 대해 간선 생성 (비용 = E × 유클리드 거리의 제곱)
2. 간선들을 가중치 기준으로 오름차순 정렬
3. Union-Find를 이용해 사이클을 방지하면서 최소 비용 간선들을 선택
4. N-1개의 간선으로 모든 섬을 연결하는 MST 완성

Union-Find에서 경로 압축을 통해 시간 복잡도를 최적화했습니다.

## 해답

```java
package swea;

import java.io.*;
import java.util.*;
/*
 * 날짜: 2025.08.26
 * 문제: Solution_1251. 하나로
 * 난이도: D4
 * 메모리: 96,344 kb
 * 시간: 644 ms
 * 아이디어: 최소 비용으로 전체 연결(네트워크! 연결 비용): MST 알고리즘(크루스칼/프림)
 * 
 */
public class Solution_1251 {
	
	static int N;
	static double E;
	static int[][] island;
	static int[] parents;
	static List<Edge> edgeList;
	
	static class Edge implements Comparable<Edge>{
		int from; 
		int to;
		double length;
		
		public Edge(int a, int b, double length) {
			super();
			this.from = a;
			this.to = b;
			this.length = length;
		}
		
		public int compareTo(Edge e) {
			return Double.compare(this.length, e.length); //오름차순
		}
	}
	
	
	public static void main(String[] args) throws IOException {
		// TODO Auto-generated method stub
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		StringBuilder sb  = new StringBuilder();
		StringTokenizer st;
		
		int T = Integer.parseInt(br.readLine());
		
		for(int t=1; t<=T; t++) {
			//섬의 개수
			N = Integer.parseInt(br.readLine());
			island = new int[N][2];
			parents = new int[N];
			edgeList = new ArrayList<>();
			//x좌표 입력 받기
			st = new StringTokenizer(br.readLine());
			for(int i=0; i<N; i++) {
				island[i][0] = Integer.parseInt(st.nextToken());
			}
			
			//y좌표 입력 받기
			st = new StringTokenizer(br.readLine());
			for(int i=0; i<N; i++) {
				island[i][1] = Integer.parseInt(st.nextToken());
			}
		
			//환경 부담 세율
			E = Double.parseDouble(br.readLine());
			
			//간선 리스트 만들기
			for(int i=0; i<N; i++) {
				for(int j=i+1; j<N; j++) {
					long dx = island[i][0] - island[j][0];
					long dy = island[i][1] - island[j][1];
					double len = E*(dx*dx+dy*dy);    //두 좌표의 길이(가중치)
					edgeList.add(new Edge(i,j, len));
				}
			}
			
			make();
			Collections.sort(edgeList);
			
			double totalLen = 0;
			int cnt = 0;
			for(Edge e : edgeList) {
				if(!union(e.from, e.to)) continue; //사이클
				totalLen += e.length;
				if(++cnt == N-1) break;
			}
			
			System.out.println("#"+t+" "+Math.round(totalLen));
		}
		
		
	}
	
	static void make() {
		for(int i=0; i<N; i++) {
			parents[i] = i;
		}
	}
	
	static int find(int a) {
		if(parents[a] == a) return a;
		return	parents[a] = find(parents[a]);
	}
	
	static boolean union(int a, int b) {
		int aRoot = find(a);
		int bRoot = find(b);
		
		if(aRoot == bRoot) return false; // 이미 같은 집합이면 return  
		
		if(aRoot > bRoot) {
			parents[bRoot] = aRoot;
		}else {
			parents[aRoot] = bRoot;
		}
		
		return true;
	}

}
```