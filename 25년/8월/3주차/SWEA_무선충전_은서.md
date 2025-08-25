## 무선 충전([SWEA](https://swexpertacademy.com/main/code/problem/problemDetail.do?contestProbId=AWXRDL1aeugDFAUo) 5644)

## 풀이

초 마다 모든 지점, 모든 사용자에 대해 각 BC의 범위 내에 있는지 확인하고, 저장한다. 각 BC에서의 최대 충전량을 이중 반복문을 통해 계산하여 최댓값을 갱신한다.

주의할 점은 x,y 좌표가 반대라는 것이다. 핵심은 최대 충전량을 계산하는 이중 포문 작성이 까다로운 점이다.

## 해답

```java
package swea;

import java.util.*;
import java.io.*;

/*
 * 날짜: 2025.08.19
 * 문제: Solution_5644. 무선 충전
 * 난이도: -
 * 메모리: 28,328 kb
 * 시간: 107ms
 * 아이디어: 초 마다 모든 지점, 모든 사용자에 대해 각 BC의 범위 내에 있는지 확인하고, 저장 
 * 		각 BC에서의 최대 충전량을 이중 반복문을 통해 계산 -> 최댓값 갱신
 * 		- 주의할 점: x,y 좌표 반대
 * 		- 핵심: 최대 충전량을 계산하는 이중 포문 작성이 까다로웠음. 다시 공부
 */

public class Solution_5644 {
	
	static int M, A;
	static int[] dx = {0, -1, 0, 1, 0};
	static int[] dy = {0, 0, 1, 0, -1};
	static int[][] matrix = new int[11][11];
	static int[][] BCData;
	static int[][] userMove;

 	
	public static void main(String[] args) throws IOException {
		// TODO Auto-generated method stub
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
		
		int T = Integer.parseInt(br.readLine());
		
		for(int t=1; t<=T; t++) {
			StringTokenizer st =  new StringTokenizer(br.readLine());
			
			M = Integer.parseInt(st.nextToken()); // 이동 시간
			A = Integer.parseInt(st.nextToken()); // BC의 개수 (Battery Charger)
			 
			
			userMove = new int[2][M]; //사용자들 이동 경로 배열
			
			//사용자의 이동 정보 입력 받기
			for(int i=0; i<2; i++) {
				st =  new StringTokenizer(br.readLine());
				for(int j=0; j<M; j++) {
					userMove[i][j] = Integer.parseInt(st.nextToken()); 
				}
			}
			
			BCData = new int[A][4];
			//BC 정보 입력받기
			for(int i=0; i<A; i++) {
				 st =  new StringTokenizer(br.readLine());
				 for(int j=0; j<4; j++) {
					 // X, Y 좌표, 충전 범위, 처리량 입력 받기
					 BCData[i][j] = Integer.parseInt(st.nextToken());
				 }
	            // 이 문제에서는 행과열이 반대 -> 바꿔서 저장
	            int tempX = BCData[i][0];
	            BCData[i][0] = BCData[i][1]; // Y
	            BCData[i][1] = tempX;        // X
			}
			
			
			int time = 0;
			int[] userA = {1, 1}; //userA 위치
			int[] userB = {10, 10}; //userB 위치
			
			int sum = getMaxCharge(userA, userB); //0초부터 계산
			
			//M초 동안, 초 마다 각 BC의 사용자 여부를 보고 최대 충전량 합산
			while(time < M) {
				userA[0] += dx[userMove[0][time]];
				userA[1] += dy[userMove[0][time]];
				userB[0] += dx[userMove[1][time]];
				userB[1] += dy[userMove[1][time]];
				
				sum += getMaxCharge(userA, userB);
				time++;
			}
			
			System.out.println("#"+t+" "+sum);
		}
		
		
	}
	
	public static int getMaxCharge(int[] userA, int[] userB) {
		int userA_x = userA[0], userA_y = userA[1];
		int userB_x = userB[0], userB_y = userB[1];
		
		int maxCharge  = 0;
		boolean[][] BCShareUser = new boolean[A][2];
				
		//사용자 A, B가 각 충전기 범위 안에 포함되어있나?
		for(int bcNum=0; bcNum<A; bcNum++) {
			BCShareUser[bcNum][0] =  getDistance(userA_x, userA_y, bcNum);
			BCShareUser[bcNum][1] =  getDistance(userB_x, userB_y, bcNum);
		}
		
		//****최대 충전량 계산!!! 이중 for문으로 조합 -> i는 사용자A, j는 사용자B 확인****
		for(int i=0; i<A; i++) {
			for(int j=0; j<A; j++) {
				int charge = 0;
				if(!BCShareUser[i][0] && !BCShareUser[j][1]) continue; // 두 BC에 아무도 없을 때 -> continue
				if (BCShareUser[i][0])  charge += BCData[i][3];        // 사용자 A가 포함되어 있을 때 -> 충전량 더하기
				if (BCShareUser[j][1])  charge += BCData[j][3];        // 사용자 B가 포함되어 있을 때 -> 충전량 더하기
                // i와 j가 같은 BC이고 A, B 둘 다 포함되어 있을 때 : 충전량 절반
				if(i==j && BCShareUser[i][0] && BCShareUser[j][1]) charge/= 2;
				maxCharge = Math.max(maxCharge, charge); //1초 동안의 모든 BC 조합 탐색하며 max값 갱신하기
			}
		}

		
		return maxCharge;
	}
	
	public static boolean getDistance(int x, int y, int bcNum) {
		return (Math.abs(BCData[bcNum][0]-x)+Math.abs(BCData[bcNum][1]-y)) <= BCData[bcNum][2];
	}

}
```