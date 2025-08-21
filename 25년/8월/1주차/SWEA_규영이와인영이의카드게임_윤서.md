## 규영이와 인영이의 카드게임([SWEA](https://swexpertacademy.com) 6808)

## 풀이

순열을 이용해 인영이 카드 조합을 만들고 output으로 전달합니다.
카드 조합을 받아서 play하는 메서드를 만들어 이긴 횟수와 진 횟수를 구합니다.

## 해답

```java
/*
6808. 규영이와 인영이의 카드게임
메모리: 30,844KB
시간: 2,208ms
아이디어: 순열을 이용해 인영이 카드 조합을 만들고 (output). 카드조합을 받아서 play하는 메서드를 만들어 이긴 횟수와 진 횟수를 구한다
*/
package ssafy_algo;
import java.io.*;
import java.util.*;
public class Solution_6808_남윤서 {

	static int[] gyu = new int[9];
	static int[] iny = new int[9];
	static boolean [] used = new boolean[19];
	static int win,lose;
	
	public static void main(String[] args){
		Scanner sc = new Scanner(System.in);
		int T = sc.nextInt();
		for(int t = 1; t <= T; t++) {
			
			Arrays.fill(used, false);
            Arrays.fill(used, false);
            win = 0;
            lose = 0;
            
			for(int i = 0; i < 9; i++) {
				gyu[i] = sc.nextInt();
				used[gyu[i]] = true;
			}
			
			int idx = 0;
			for(int i = 1; i <= 18; i++) {
				if(!used[i]) iny[idx++] = i;
			}
			
			permute(0, new boolean[9], new int[9]);
			System.out.printf("#%d %d %d\n", t, win, lose);

		}
	}
		
		
	
	
	static void permute(int depth, boolean[] visited, int[] output) {
		if(depth == 9) {
			play(output);
			return;
		}
		for(int i = 0; i < 9; i++) {
			if(!visited[i]) {
				visited[i] = true;
				output[depth] = iny[i];
				permute(depth + 1, visited, output);
				visited[i] = false;
			}
		}
	}
	
	static void play(int[] output) {
		int gyuScore = 0;
		int inyScore = 0;
		
		for(int i = 0; i < 9; i++) {
			if(gyu[i] > output[i]) {
				gyuScore += gyu[i]+output[i];
			}
			else {
				inyScore += gyu[i]+output[i];
			}
		}
		
		if (gyuScore > inyScore) win++;
		else lose++;
	}
	

}
```