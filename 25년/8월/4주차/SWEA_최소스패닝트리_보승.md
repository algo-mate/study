## 최소스패닝트리([SWEA](https://swexpertacademy.com/main/code/problem/problemDetail.do?contestProbId=AV_mSnmKUckDFAWb) 3124)

## 풀이

크루스칼 알고리즘을 사용한 표준적인 최소 신장 트리(MST) 구현입니다.

주어진 무방향 가중 그래프에서 모든 정점을 연결하면서 간선의 가중치 합이 최소가 되는 트리를 찾는 문제입니다.

1. 모든 간선을 가중치 기준으로 오름차순 정렬
2. Union-Find 자료구조로 각 정점을 독립된 집합으로 초기화
3. 정렬된 간선을 순서대로 검사하며 사이클을 생성하지 않는 간선만 선택
4. V-1개의 간선을 선택할 때까지 반복하여 MST 완성

경로 압축을 통해 Union-Find 연산의 효율성을 높였습니다.

## 해답

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.Arrays;
import java.util.Collections;
import java.util.PriorityQueue;
import java.util.StringTokenizer;
/*
 * 3124. 최소 스패닝 트리
 * 실행시간: 2,596 ms
 * 메모리: 127,044 kb
 * 아이디어: 크루스칼 알고리즘 
 */
public class Solution_3124_강보승 {
	static int T, v, e;
	static int[] uf;
	static int[][] nodes;
	public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringBuilder sb = new StringBuilder();
        T = Integer.parseInt(br.readLine());
        for(int t = 1; t <= T; t++) {
            StringTokenizer st = new StringTokenizer(br.readLine());
        	v = Integer.parseInt(st.nextToken());
            e = Integer.parseInt(st.nextToken());
            nodes = new int[e][3];
            for(int i = 0; i < e; i++) {
            	st = new StringTokenizer(br.readLine());
            	nodes[i][0] = Integer.parseInt(st.nextToken());
            	nodes[i][1] = Integer.parseInt(st.nextToken());
            	nodes[i][2] = Integer.parseInt(st.nextToken());
            }
            uf = new int[v + 1];
            for(int i = 1; i <= v; i++) {
            	uf[i] = i;
            }
            Arrays.sort(nodes, (a, b) -> Integer.compare(a[2], b[2]));
            int count = 0;
            long weightSum = 0;
            for(int[] node : nodes) {
            	if(find(node[0]) != find(node[1])) {
            		union(node[0], node[1]);
            		count++;
            		weightSum += node[2];
            	}
            	if(v - 1 == count) break;
            }
            System.out.println("#" + t + " " + weightSum);
        }
    }
	private static int find(int x) {
		if(uf[x] == x) {
			return x;
		}
		uf[x] = find(uf[x]);
		return uf[x];
	}
	private static void union(int x, int y) {
		int X = find(x);
		int Y = find(y);
		if(X < Y) {
			uf[Y] = X;
		}else{
			uf[X] = Y;
		}
	}
}
```