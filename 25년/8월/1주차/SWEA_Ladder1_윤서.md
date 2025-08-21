## Ladder1([SWEA](https://swexpertacademy.com) 1210)

## 풀이

첫 번째 행을 탐색하면서 1이라면 그 1을 기준으로 답(2)에 도달하는지 확인하는 방식입니다.
아래로 내려가면서 좌우로 이동할 수 있는 경우 해당 방향으로 끝까지 이동한 후 다시 아래로 내려갑니다.

## 해답

```java
/*
1210. Ladder1
메모리: 36,732KB
시간: 138ms
아이디어: 첫 번째 행을 탐색하면서 1이라면 그 1을 기준으로 답(2)에 도달하는지 확인
*/
package ssafy_algo;
import java.io.*;
import java.util.*;
public class Solution_1210_남윤서 {

	public static void main(String[] args) throws IOException{
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		for(int t = 1; t <= 10; t++) {
			br.readLine(); // 무시
			int[][] arr = new int [100][100];
			
			for(int i = 0; i < 100; i++) {
				StringTokenizer st = new StringTokenizer(br.readLine());
				for(int j = 0; j < 100; j++) {
					arr[i][j] = Integer.parseInt(st.nextToken());
				}
			}
			
			int answer = -1 ;
			
			outer:
			for(int j = 0; j < 100; j++) {
				if(arr[0][j]!=1) continue;
				
				int ci = 0;
				int cj = j;
				
				while(ci < 100) {
					if(arr[ci][cj] == 2) {
						answer = j;
						break outer;//2 라면 완전 종료
					}
					
					if(cj > 0 && arr[ci][cj-1] == 1) {
						while(cj > 0 && arr[ci][cj-1] == 1) {
							cj--;
						}
					}
					else if(cj < 99 && arr[ci][cj+1] == 1) {
						while(cj < 99 && arr[ci][cj+1] == 1) {
							cj++;
						}
					}
						ci ++;
					
				}
				
				
				
				
			}
			System.out.printf("#%d %d",t,answer);
			System.out.println();
		}

	}

}
```