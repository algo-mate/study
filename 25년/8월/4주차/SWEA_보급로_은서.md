## 보급로([SWEA](https://swexpertacademy.com/main/code/problem/problemDetail.do?contestProbId=AV15QRX6APsCFAYD) 1249)

## 풀이

다익스트라 알고리즘을 사용하여 해결합니다. 양수 가중치를 가진 그래프에서의 최단 경로 탐색 문제입니다. 우선순위 큐를 사용하여 현재까지의 최소 비용이 가장 작은 경로부터 탐색하며, 각 위치까지의 최소 비용을 times 배열에 저장합니다. 시작점 (0,0)에서 도착점 (N-1,N-1)까지의 최소 복구 시간을 구합니다.

## 해답

```java
package day_250825;
/*
 * 날짜: 2025.08.25
 * 문제: Solution_1249. 보급로
 * 난이도: D4
 * 메모리: 41,452 kb
 * 시간: 191ms
 * 아이디어: 다 익 스 트 라 (양수 가중치 를 가진 그래프에서의 최소 경로 탐색)
 * 		** 최단 경로 알고리즘 종류 **
 * 		- 무가중치 or 0/1가중치 : BFS
 * 		- 양수 가중치: 다익스트라
 * 		- 음수 가중치 : 벨만-포드
 * 		- 모든 정점 쌍 간의 최단 거리 : 플로이드-워셜
 * 		- 최소 비용으로 전체 연결(네트워크 연결 비용): MST 알고리즘(크루스칼/프림)
 */

import java.util.*;
import java.io.*;

public class Solution_1249 {
	static int N;
	static int[][] roads;
	static int[][] times;
	static int[] dx = {0,1,0,-1}; 
	static int[] dy = {1,0,-1,0}; 
	
	public static void main(String[] args) throws IOException {
		// TODO Auto-generated method stub
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		StringTokenizer st;
		
		int T = Integer.parseInt(br.readLine());
		
		for(int t=1; t<=T; t++) {
			
			N = Integer.parseInt(br.readLine());
			roads = new int[N][N];
			times = new int[N][N];
			
			//도로 가중치 입력받기(파여진 도로 깊이)
			for(int i=0; i<N; i++) {
				String str = br.readLine(); //띄어쓰기 없는 한줄 입력 받기
				for(int j=0; j<N; j++) {
					roads[i][j] = str.charAt(j) - '0'; //문자 하나씩 읽고 '0' 빼줘서 숫자 읽기
					times[i][j] = Integer.MAX_VALUE;   //모든 가중치를 최댓값으로 설정
				}
			}
			
			dijkstra(0, 0, 0);
			System.out.println("#"+t+" "+times[N-1][N-1]); //도착지의 가중치 출력
		}
		
		
		
	}
	// 다익스트라 다시 작성
	static void dijkstra(int x, int y, int nowTime) {
		//최소 힙, 우선순위 큐 : 정렬은 가중치 기준으로(마지막 인자)
		Queue<int[]> pq = new PriorityQueue<>(Comparator.comparing(arr -> arr[2]));
		
		pq.offer(new int[] {x, y, nowTime}); // 현재 경로의 좌표, 가중치 큐에 넣기
		
		while(!pq.isEmpty()) {
			int[] current = pq.poll();
			x = current[0];
			y = current[1];
			nowTime = current[2];
			
			if(nowTime > times[x][y]) continue; //꺼낸 경로의 가중치가 times에 저장되어있는 가중치보다 클 때? -> 이 경로는 최소 경로가 아님 -> 돌아가
			
			for(int i=0; i<4; i++) { //상하좌우 경로 탐색
				int nx = x+dx[i];
				int ny = y+dy[i];
				
				if(checkRange(nx, ny)) { //범위 체크
					int nTime = nowTime + roads[nx][ny]; //지금까지의 경로 가중치와 다음 위치의 가중치의 합
					
					if(nTime < times[nx][ny]) { //합이 저장되어있던 이 위치의 가중치보다 클 때? -> 이 경로는 최소 경로가 아님. 작을 때만 보기
						times[nx][ny] = nTime; //최소 가중치 업데이트
						pq.offer(new int[] {nx, ny, nTime}); //최소 경로 업데이트 후 큐에 담기 (이 경로는 계속해서 탐색하겠다)
					}
					
				}
				
			}
			
		}
		
	}
	
	
	//범위 체크
	static boolean checkRange(int x, int y) {
		return (0 <= x && x < N && 0 <= y && y < N);
	}

}
```