## 하나로([SWEA](https://swexpertacademy.com/main/code/problem/problemDetail.do?contestProbId=AV15StKqAQkCFAYD) 1251)

## 풀이

서로소집합과 크루스칼 알고리즘을 이용해서 최소신장트리(MST) 문제를 해결했습니다.
- 모든 섬들 사이의 거리를 계산하여 간선 정보로 사용 (nC2개)
- 간선의 가중치는 두 섬 사이의 거리의 제곱으로 설정
- 크루스칼 알고리즘으로 최소신장트리 구성
- Union-Find를 사용하여 사이클 검사 및 집합 관리
- 최종 비용에 세율 E를 곱하여 환경 부담금 계산

## 해답

```java
package ssafy_algo;
/* Swea 1251 하나로 문제
 * 서로소집합과 크루스탈 알고리즘을 이용해서 문제를 풀었음
 * 간선의 가중치 대신 섬 사이의 거리를 넣는다
 * 간선의 갯수는 nC2개로 고정
 * 메모리 : 134,632 kb
 * 시간 : 1,272 ms
 */
import java.io.*;
import java.util.*;
public class Solution_1251_남윤서 {
	static int [] parents;
	static int[][]island ;
	static long[][] distance;
	static double cnt;
	static double E;
	public static void main(String[] args)throws Exception{
		BufferedReader br= new BufferedReader(new InputStreamReader(System.in));
		
		int T = Integer.parseInt(br.readLine());
		for(int t = 1; t <= T; t++) {
			
			int N = Integer.parseInt(br.readLine()); // 섬 갯수
			island = new int[N][2]; // 섬 좌표
			int m = N * N-1 /2;
			distance = new long[m][3]; 
			parents = new int[N];
			
			StringTokenizer st= new StringTokenizer(br.readLine());
			for(int i = 0; i < N ; i++) { 
				island[i][0] = Integer.parseInt(st.nextToken());
			}
			
			st= new StringTokenizer(br.readLine());
			for(int i = 0; i < N ; i++) { 
				island[i][1] = Integer.parseInt(st.nextToken());
			}
			
			
			E = Double.parseDouble(br.readLine()); // 세율 실수 입력받기
			
			int idx = 0;
			for(int i = 0; i < N; i++) {
				for(int j = i + 1; j < N; j++) {
					distance[idx][0] = i;
					distance[idx][1] = j;
					distance[idx][2] = (long)Math.pow(island[i][0] - island[j][0], 2) + (long)Math.pow(island[i][1] - island[j][1], 2);
					idx ++;
				}
			}
			
			Arrays.sort(distance, (a,b) -> Double.compare(a[2],b[2])); // 거리기준 오름차순 정렬
			
			for(int i = 0; i < N; i++) {
				parents[i] = i;
			}
			
			kruskal();
			System.out.printf("#%d %d\n", t, Math.round(cnt));
			
			
			
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
		cnt = 0;
		for(int i = 0; i < distance.length; i++) {
			if(find((int)distance[i][0]) != find((int)distance[i][1])) {
				cnt += distance[i][2] * E;
				union((int)distance[i][0], (int)distance[i][1]);
			}
		}
	}
}
```