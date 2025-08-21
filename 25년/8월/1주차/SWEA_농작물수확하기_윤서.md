## 농작물 수확하기([SWEA](https://swexpertacademy.com) 2805)

## 풀이

다이아몬드 모양으로 농작물을 수확하는 문제입니다.
행별로 시작 인덱스와 끝 인덱스의 규칙을 찾아 수식을 만들어 적용했습니다.
중앙을 기준으로 거리에 따라 시작과 끝 위치를 계산합니다.

## 해답

```java
/*
2805. 농작물 수확하기
메모리: 28,544KB
시간: 118ms
아이디어: 행별로 시작인덱스와 끌인텍스의 규칙을 찾고 식을 만든 후 적용
*/
package ssafy_algo;
import java.io.*;
import java.util.*;
public class Solution_2805_남윤서 {

	public static void main(String[] args) throws IOException {
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		int T = Integer.parseInt(br.readLine());
		 
		for(int t = 1; t <= T; t++) {
			int n = Integer.parseInt(br.readLine());
			int arr[][] = new int[n][n];
			
			for(int i = 0; i < n; i++) {
				String s = br.readLine();
				for(int j = 0; j < n; j++) {
					arr[i][j] =s.charAt(j)-'0';
				}
			}
			
			int mid = n / 2;
			int count = 0;
			for(int i = 0; i < n; i++) {
				int start = Math.abs(mid-i);
				int end = n-start-1;
				for(int j = start; j <= end; j++) {
					count += arr[i][j] ;
				}
			}
			
			System.out.printf("#%d %d",t,count);
			System.out.println();
			
		}
		

		
		
	}

}
```