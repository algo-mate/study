## 서로소집합([SWEA](https://swexpertacademy.com/main/code/problem/problemDetail.do?contestProbId=AV7nu4fcZh8CFAXW) 3289)

## 풀이

Union-Find (Disjoint Set Union) 자료구조를 사용하여 해결합니다. 연산 0은 두 원소를 같은 집합으로 만드는 union 연산이고, 연산 1은 두 원소가 같은 집합에 속하는지 확인하는 find 연산입니다. find 함수에서는 재귀를 통해 부모 노드를 찾고, union 함수에서는 두 집합의 루트를 연결합니다.

## 해답

```java
/* Swea 3289 서로소 집합 문제
 * 메모리 : 112,180 kb
 * 실행시간 : 420 ms
 * 오늘 수업시간에 배웠던 내용을 그대로 이용했습니다
 * 같은 집합 -> 같은 부모를 바라보게끔 만들기
 * find함수는 재귀형식을 통해서 부모노드를 찾는다
 * 두 집합의 합은 두 부모노드중 하나의 부모가 다른 부모를 가리키도록 한다
 */
package ssafy_algo;
import java.io.*;
import java.util.*;
public class Solution_3289_남윤서 {
	static int [] parent;
	public static void main(String[] args)throws Exception {
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		int T = Integer.parseInt(br.readLine());
		for(int t= 1; t <= T; t++) {
			StringTokenizer st = new StringTokenizer(br.readLine());
			int n = Integer.parseInt(st.nextToken());
			int m = Integer.parseInt(st.nextToken());
			
			parent = new int [n+1];
			for(int i = 0; i <= n ; i++) {
				parent[i] = i;
			}
			
			StringBuilder sb = new StringBuilder();
			for(int i = 0; i < m; i++) {
				st = new StringTokenizer(br.readLine());
				int op = Integer.parseInt(st.nextToken());
				int a = Integer.parseInt(st.nextToken());
				int b = Integer.parseInt(st.nextToken());
				
				
				if(op == 0) {
					union(a,b);
				}else {
					if(find(a) == find(b)) {
						sb.append(1);
					}
					else {
						sb.append(0);
					}
				}
			}
			System.out.printf("#%d %s\n",t,sb.toString());
			
		}

	}
	
	static int find(int x) { // 재귀로 부모가 누구인지 파악(어떤 집합에 포함되어있는지)
		if(parent[x] == x) return x;
		else return find(parent[x]);
	}
	
	static void union(int x, int y) {
		int xRoot = find(x);
		int yRoot = find(y);
		if(xRoot > yRoot) parent[xRoot] = yRoot;
		else parent[yRoot] = xRoot;
	}
}
```