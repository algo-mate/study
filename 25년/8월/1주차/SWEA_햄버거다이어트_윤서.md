## 햄버거 다이어트([SWEA](https://swexpertacademy.com) 5215)

## 풀이

부분집합을 만들고 칼로리 제한을 넘기지 않는 부분집합 중 맛 점수가 최대인 것을 찾는 문제입니다.
재귀를 사용하여 각 재료를 포함하거나 포함하지 않는 메방식으로 진행합니다.

## 해답

```java
/*
5215. 햄버거 다이어트
메모리: 29,824KB
시간: 162ms
아이디어: 부분집합 만들고 칼로리 제한을 넘기지 않는 부분집합 중 맛 점수가 최대인 것을 찾음
*/
package ssafy_algo;

import java.util.*;
import java.io.*;

public class Solution_5215_남윤서 {
	static int N, L , maxScore;
	static int[] scores, cal;
	static boolean[] isSelected;
	public static void main(String[] args)  {
		Scanner sc = new Scanner(System.in);
		int T = sc.nextInt();
		
		
		for(int t = 1; t <= T; t++) {
			N = sc.nextInt(); // N개의 재료
			L = sc.nextInt(); // 칼로리 제한
			
			scores = new int [N];
			cal = new int[N];
			
			for(int i = 0; i < N; i++) {
				scores[i] = sc.nextInt();
				cal[i] = sc.nextInt();
			}
			
			maxScore = 0;
			subSet(0,0,0);
			
			System.out.printf("#%d %d\n",t,maxScore);
			
		}
	}
	
	
	static void subSet(int depth, int calSum, int scoreSum) {
		if(calSum > L) return; 
		
		if(depth == N) {
			maxScore = Math.max(scoreSum, maxScore);
			return;
		}
		
		subSet(depth + 1, calSum + cal[depth], scoreSum + scores[depth]); // 해당 재료를 넣었을 때
			
		subSet(depth + 1, calSum , scoreSum ); // 해당 재료를 안넣었을 때
	}
	
		
}
```