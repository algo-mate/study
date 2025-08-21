## 숫자 만들기([SWEA](https://swexpertacademy.com) 4008)

## 풀이

dfs를 이용해서 풀었습니다.
백트래킹에서 복구를 안해서 처음에 문제를 틀렸습니다. 복구를 꼭 잊지말기!

## 해답

```java
/*
4008. 숫자 만들기
메모리: 29,824KB
시간: 131ms
아이디어: dfs를 이용해서 풀었습니다. 복구를 안해서 처음에 문제를 틀렸습니다 꼭 잊지말기 !!!!!!
*/
package ssafy_algo;

import java.util.Scanner;

public class Solution_4008_남윤서 {
	static int n, min, max;
	static int [] nums, oper;
	public static void main(String[] args) {
		
		Scanner sc= new Scanner(System.in);
		oper = new int[4];
		int T = sc.nextInt();
		
		for(int t = 1; t <= T; t++) {
			min = Integer.MAX_VALUE; 
			max = Integer.MIN_VALUE;
			n = sc.nextInt();
			nums = new int [n];
			for(int i = 0; i < 4; i++) {
				oper[i] = sc.nextInt();
			}
			
			for(int i = 0; i < n; i++) {
				nums[i] = sc.nextInt();
			}
			
			dfs(0, nums[0]);
			System.out.printf("#%d %d\n",t,Math.abs(min-max));
		}
	}
	
	static void dfs(int start, int rand) {
		if(start == n-1) {
			max = Math.max(max, rand);
			min = Math.min(min, rand);
		}
		
		for(int i = 0; i < 4 ; i++) {
			int next = rand;
			if(oper[i] > 0) {
				oper[i] -= 1;
				
				if(i == 0) next = rand + nums[start + 1];
				if(i == 1) next = rand - nums[start + 1];
				if(i == 2) next = rand * nums[start + 1];
				if(i == 3) {
					if(rand < 0 ) next = -(Math.abs(rand)/ nums[start+1]);
					else next = rand / nums[start + 1];
				}
				
				dfs(start+1, next);
				oper[i]++; // 복구 필수 !@@@!!!
			}
		}
	}

}
```