## 서로소집합([SWEA](https://swexpertacademy.com/main/code/problem/problemDetail.do?contestProbId=AV7nu4fcZh8CFAXW) 3289)

## 풀이

Union-Find 자료구조를 사용하여 해결합니다. make 함수로 각 원소를 자신의 부모로 초기화하고, find 함수에서는 경로 압축을 통해 루트를 찾습니다. union 함수에서는 두 집합의 루트끼리 연결하여 하나의 집합으로 만듭니다. 연산 0은 합집합, 연산 1은 같은 집합에 속하는지 확인하는 연산입니다.

## 해답

```java
package swea;
/* 날짜: 2025.08.25
 * 문제: Solution_3289. 서로소 집합
 * 메모리: 105,992 kb
 * 시간: 331 ms
 * 아이디어: 원소 x, y 가 어느 집합(루트)에 속해있는지 찾고 (find)
 * 		그 두 집합의 루트끼리 연결해서 하나의 집합으로 만든다. (union)
 * 		꼭 루트끼리 연결!
 * 		find 함수 내에서 return parents[x] = find(parents[x])로 경로압축 꼭 해주기
 */
import java.io.*;
import java.util.*;

public class Solution_3289 {
	static int N, M;
	static int[] parents;
//	static int[] rank;
	
	public static void main(String[] args) throws IOException {
		// TODO Auto-generated method stub
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		StringBuilder sb  = new StringBuilder();
		StringTokenizer st;
		
		int T = Integer.parseInt(br.readLine());
		
		for(int t=1; t<=T; t++) {
			st = new StringTokenizer(br.readLine());
            sb  = new StringBuilder();
			N = Integer.parseInt(st.nextToken());
			M = Integer.parseInt(st.nextToken());
			parents = new int[N+1];
//			rank = new int[N+1]; //높이
			make();
			
			for(int i=0; i<M; i++) {
				st = new StringTokenizer(br.readLine());
				int op = Integer.parseInt(st.nextToken());
				int x = Integer.parseInt(st.nextToken());
				int y = Integer.parseInt(st.nextToken());
				
				if(op == 0) { //합집합
					union(x, y);
				}
				else {
					if(find(x) == find(y)) {
						sb.append(1);
					}else {
						sb.append(0);
					}
				}
			}
			
			System.out.println("#"+t+" " +sb.toString());
		}
		
		
		
	}
	 // 자신을 자신의 부모로 초기화(자신이 곧 루트)
	private static void make() {
		for (int i = 0; i < N+1; i++) {
			parents[i] = i; 
//			rank[i] = 0; //초기 랭크 0
		}
	}

	// 합집합 만들기 -> 꼭 루트끼리 연결 !!
	private static void union(int x, int y) {
		int xRoot = find(x);
		int yRoot = find(y);
		
		
		if(xRoot == yRoot) return; //같은 집합
		
		//트리 균형을 고려하여 집합의 크기를 기준으로 나눠서 붙임(O(logN))
		//단순 parents[yRoot] = xRoot 은 한쪽으로 치우진 긴 사슬 -> 최악 O(N)
		if(xRoot > yRoot) parents[yRoot] = xRoot; 
		else parents[yRoot] = xRoot;
		
		
		//by rank(트리 높이 비교해서 붙이기)
//		if(rank[xRoot] > rank[yRoot]) {
//			parents[yRoot] = xRoot;
//		}
//		else if(rank[xRoot] < rank[yRoot]) {
//			parents[xRoot] = yRoot; 
//		}
//		else { // 두 높이가 같으면
//			parents[xRoot] = yRoot;  //한쪽에 붙이고
//			rank[yRoot]++; //높이+1
//		}
		
	}
	
	private static int find(int x) {
		if(parents[x] == x) return x;
		return parents[x] = find(parents[x]); //경로 압축

	}

}
```