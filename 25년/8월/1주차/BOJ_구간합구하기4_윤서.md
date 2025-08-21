## 구간 합 구하기 4([BOJ](https://www.acmicpc.net) 11659)

## 풀이

for문으로 일일히 합을 다 구하다가 시간초과가 나서 누적합으로 답을 구했습니다.
입력과 함께 누적합을 계산하여 배열에 저장하고, 구간합 질의에서는 두 누적합의 차이를 구하여 답을 구합니다.

## 해답

```java
/*
11659. 구간 합 구하기 4
메모리: 89,328KB
시간: 2,232ms
아이디어: for문으로 일일히 합을 다 구하다가 시간초과가 나서 누적합으로 답을 구했습니다
*/
package ssafy_algo;
import java.io.*;
import java.util.*;
public class Main_11659_남윤서 {

	public static void main(String[] args) throws IOException {
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		StringTokenizer st = new StringTokenizer(br.readLine());
		int N = Integer.parseInt(st.nextToken());
		int M = Integer.parseInt(st.nextToken());
		
		long[] nums = new long [N+1];
		st = new StringTokenizer(br.readLine());
		for(int i = 1; i <=N; i++) {
			nums[i] = Integer.parseInt(st.nextToken());
		}
		
		for(int i = 1; i <= N; i ++) {
			nums[i] = nums[i-1] + nums[i];
		}
		
		for(int j = 0; j < M; j++) {
			st = new StringTokenizer(br.readLine());
			int a = Integer.parseInt(st.nextToken());
			int b = Integer.parseInt(st.nextToken());
			
			long sum = 0;
			sum = nums[b] - nums[a-1];
			System.out.println(sum);
		}
		
		
	}

}
```