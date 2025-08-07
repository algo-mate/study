## 파리퇴치

## 해설

preSum 활용문제

사각형 preSum 공식을 외우면 길이 m 조건만 조심하면 간단하게 해결 가능한 문제이다.

## 해답

```java
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.util.StringTokenizer;
import java.util.*;
/* Main 2001 파리 퇴치
	메모리: 25,344 kb
	시간: 89 ms
	아이디어: 
	1. preSum 문제, 사전에 미리 부분의 합을 구해서 시간복잡도를 효율적으로 개선하는게 핵심인 문제
	2. 이후에는 m의 길이에 맞게 완전탐색
*/ 
public class Solution_2001_강보승 {
	static int n, m, maxValue;
	static int[][] arr, preSum;
    public static void main(String[] args) throws IOException{
    	BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
    	StringTokenizer st = new StringTokenizer(br.readLine());
    	int T = Integer.parseInt(st.nextToken());
    	for(int t = 1; t <= T; t++) {
    		st = new StringTokenizer(br.readLine());
    		n = Integer.parseInt(st.nextToken());
        	m = Integer.parseInt(st.nextToken());
        	arr = new int[n + 1][n + 1];
        	preSum = new int[n + 1][n + 1];
        	for(int i = 1; i <= n; i++) {
        		st = new StringTokenizer(br.readLine());
        		for(int j = 1; j <= n; j++) {
        			arr[i][j] = Integer.parseInt(st.nextToken());
        		}
        	}
        	//preSum 사전작업
        	for(int i = 1; i <= n; i++) {
        		for(int j = 1; j <= n; j++) {
        			preSum[i][j] = preSum[i - 1][j] + preSum[i][j - 1] - preSum[i - 1][j - 1] + arr[i][j];
        		}
        	}
        	maxValue = 0;
        	// m의 길이에 맞게 완전탐색
        	for(int i = 1; i <= n - m + 1; i++) {
        		for(int j = 1; j <= n - m + 1; j++) {
        			maxValue = Math.max(maxValue, getPreSum(i, j, i + m - 1, j + m - 1));
        		}
        	}
        	StringBuilder sb = new StringBuilder();
        	sb.append("#").append(t).append(" ").append(maxValue);
        	System.out.println(sb);
    	}
    }
    // 사각형 preSum 출력
    static int getPreSum(int x1, int y1, int x2, int y2) {
    	return preSum[x2][y2] - preSum[x1 - 1][y2] - preSum[x2][y1 - 1] + preSum[x1 - 1][y1 - 1];
    }
}


```