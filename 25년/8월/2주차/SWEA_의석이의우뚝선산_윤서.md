## 의석이의 우뚝 선 산([SWEA](https://swexpertacademy.com) 4796)

## 풀이

산을 하나 찾고 산의 오르막 갯수 * 내리막 갯수로 경우의 수를 모두 카운트하는 방식입니다.
연속된 오르막과 내리막을 찾아서 각각의 경우의 수를 계산합니다.

## 해답

```java
/*
4796. 의석이의 우뚝 선 산
메모리: 102,492KB
시간: 652ms
아이디어: 산을 하나 찾고 산의 오르막 갯수 * 내리막 갯수로 경우의 수를 모두 카운트
*/
import java.util.Scanner;

public class Solution_4796_남윤서 {
	    static int N;
	    static int[] mountain;
	    static int count;
	    public static void main(String[] args) {
	        Scanner sc = new Scanner(System.in);
	        int T = sc.nextInt();
	        for(int t = 1; t <= T; t++) {
	            N = sc.nextInt();
	            mountain = new int [N];
	            for(int i = 0; i < N; i++) {
	                mountain[i] = sc.nextInt();
	            }

	            count = 0;
	            int up = 0;
	            int down = 0;
	            for(int i = 1; i < N; i++) {
	            	if(mountain[i-1] < mountain[i]) {
	            		if(down > 0) {
	            			count += up * down;
	            			up = 1;
	            			down = 0;
	            		}else {
	            			up++;
	            		}
	            	}else if(mountain[i-1] > mountain[i]) {
	            		if(i == N-1 && up > 0) {
	            			down++;
	            			count += up * down; 
	            		}else {
	            			down++;
	            		}
	            	}
	            }
	            System.out.printf("#%d %d\n",t,count);

	        }
	        sc.close();

	}

}
```