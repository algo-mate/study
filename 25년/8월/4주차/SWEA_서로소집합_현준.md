## 서로소집합([SWEA](https://swexpertacademy.com/main/code/problem/problemDetail.do?contestProbId=AV7nu4fcZh8CFAXW) 3289)

## 풀이

Union-Find (Disjoint Set Union) 자료구조를 사용하여 해결합니다. makeSet 함수로 각 원소를 자신의 부모로 초기화하고, findSet 함수에서는 path compression을 통해 루트를 찾습니다. union 함수에서는 두 원소가 속한 집합을 합치어 하나의 집합으로 만듭니다. 명령어 0은 합집합, 명령어 1은 두 원소가 같은 집합에 속하는지 확인하는 연산입니다.

## 해답

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

/*
 * Solution 3289 서로소 집합
 * 
 * 메모리: 107,332 KB / 실행시간: 370 ms
 * 
 * 서로소 집합.
 */
public class Solution_3289_장현준 {

	private static int[] parents; // 부모 인덱스 관리

	public static void main(String[] args) throws IOException {
		BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(System.in));
		
		int T = Integer.parseInt(bufferedReader.readLine()); // 테스트 케이스의 수
		
		for (int t = 1; t <= T; t++) {
			StringTokenizer stringTokenizer = new StringTokenizer(bufferedReader.readLine());
			
			int N = Integer.parseInt(stringTokenizer.nextToken()); // 원소의 개수
			int M = Integer.parseInt(stringTokenizer.nextToken()); // 연산의 개수
			
			// 초기화
			parents = new int[N + 1];
			makeSet(N);
			
			StringBuilder stringBuilder = new StringBuilder();
			
			for (int i = 0; i < M; i++) {
				stringTokenizer = new StringTokenizer(bufferedReader.readLine());
				
				int cmd = Integer.parseInt(stringTokenizer.nextToken()); // 연산
				int a = Integer.parseInt(stringTokenizer.nextToken());
				int b = Integer.parseInt(stringTokenizer.nextToken());
				
				if (cmd == 0) { // 합집합
					union(a, b);
				} else if (cmd == 1) { // 두 원소가 같은 집합에 포함되어 있는지를 확인하는 연산
					int aRoot = findSet(a);
					int bRoot = findSet(b);
					if (aRoot == bRoot) {
						stringBuilder.append(1);
					} else {
						stringBuilder.append(0);
					}
				}
			}
			
			// 출력
			System.out.println("#" + t + " " + stringBuilder);
		}
	}
	
	private static void makeSet(int N) { 
		for (int i = 1; i <= N; i++) {
			parents[i] = i; // 자신을 자신의 부모로 초기화(자신이 곧 루트 대표자가 됨)
		}
	}
	
	private static int findSet(int a) { // a가 속한 집합(집합의 대표자) 찾기
		if (parents[a] == a) {
			return a;
		}
		return parents[a] = findSet(parents[a]); // path 압축
	}
	
	private static void union(int a, int b) { // a가 속한 집합과 b가 속한 집합 합치기
		int aRoot = findSet(a);
		int bRoot = findSet(b);
		if (aRoot == bRoot) { //  이미 같은 집합이므로 합치기 필요 없음
			return;
		}
		parents[bRoot] = aRoot;
	}
}
```