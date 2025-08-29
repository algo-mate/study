## 게리맨더링([BOJ](https://www.acmicpc.net/problem/17471) 17471)

## 풀이

비트 마스킹을 활용한 부분집합 생성과 BFS를 통한 연결성 검사를 결합한 문제입니다.

선거구를 두 개의 연결된 집합으로 나누어 인구 차이를 최소화하는 문제로, 다음과 같이 해결합니다:

1. 비트 마스킹으로 모든 가능한 부분집합 생성 (2^N - 2가지)
   - 공집합과 전체집합 제외
   - 각 비트는 해당 구역의 포함 여부를 나타냄

2. 각 부분집합에 대해 BFS로 연결성 검사
   - 같은 집합 내의 구역들이 모두 연결되어 있는지 확인
   - 두 집합 모두 연결되어야 유효한 분할

3. 유효한 분할에 대해 인구 수 차이 계산 후 최솟값 갱신

시간 복잡도는 O(2^N × N)이지만 N≤10 조건에서 효율적으로 동작합니다.

## 해답

```java
package baekjoon;
/* 날짜: 2025.08.26
 * 문제 : Main_17471. 게리맨더링
 * 난이도 : 골드3
 * 메모리 : 15616 kb
 * 시간 : 136 ms
 * 아이디어 : 인접하는 구역 기준으로 확장해나가려고 했지만, 모든 경우의 수 탐색 실패 및 contains 이용으로 인한 시간 초과
 * 		N의 크기가 크지 않으니 모든 부분 집합 구하는게 정석 풀이(dfs 또는 비트 마스킹)
 */

import java.io.*;
import java.util.*;

public class Main_17471 {

	static int N, minDiff;
	static int[] peopleSize;
	static List<Integer>[] adjList;
	static List<Integer> section1;
	static List<Integer> section2;
	static boolean[] visited;
	
	public static void main(String[] args) throws IOException {
		// TODO Auto-generated method stub
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		StringBuilder sb  = new StringBuilder();
		StringTokenizer st;

		N = Integer.parseInt(br.readLine());
		peopleSize = new int[N];
		adjList = new ArrayList[N];
		visited = new boolean[N];
		minDiff = Integer.MAX_VALUE;
				
		st = new StringTokenizer(br.readLine());
		
		//인구 수 담기
		for(int i=0; i<N; i++) {
			peopleSize[i] = Integer.parseInt(st.nextToken());
		}
		
		for(int i=0; i<N; i++) {
			adjList[i] = new ArrayList<>();
		}
		
		for(int i=0; i<N; i++) {
			st = new StringTokenizer(br.readLine());
			int adjNum = Integer.parseInt(st.nextToken());
			for(int j=0; j<adjNum; j++) {
				adjList[i].add(Integer.parseInt(st.nextToken())-1); //인접하는 노드 저장
			}
			
		}
		// 1. 비트 마스킹 부분집합 구하기
		for(int mask = 1; mask < (1<<N)-1; mask++) { // 1<<N : 2의 N승
		    section1 = new ArrayList<>();
		    section2 = new ArrayList<>();
		    
		    for(int i=0; i<N; i++) {
		    	if((mask&(1<<i)) != 0) section1.add(i); //0이 아니면 포함되어 있는 것
		    	else section2.add(i);
		    }
		    
		    if(isConnected(section1) && isConnected(section2)) {
				//두 구역 인구수의 차이 구하기
				int sum1 = 0, sum2 = 0;
				for(int s : section1) {
					sum1 += peopleSize[s];
				}
				for(int s : section2) {
					sum2 += peopleSize[s];
				}
				
				minDiff = Math.min(minDiff, Math.abs(sum1-sum2));

			}
		    
		}
		
		System.out.println(minDiff == Integer.MAX_VALUE ? -1 : minDiff);

	}

	static boolean isConnected(List<Integer> section) {
		Queue<Integer> q = new ArrayDeque<>();
		boolean[] check = new boolean[N+1];
		int start = section.get(0);
		check[start] = true;
				
		q.offer(start);
		int cnt = 1;
		
		while(!q.isEmpty()) {
			start = q.poll();
			for(int nx : adjList[start]) {	
				if(!check[nx] && section.contains(nx)) {
					check[nx] = true;
					cnt++;
					q.offer(nx);
				}
			}
			
		}
		
		return cnt == section.size();
		
	}

}
```