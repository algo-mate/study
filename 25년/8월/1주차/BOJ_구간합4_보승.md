## 구간 합 구하기 4(실버 1)

## 풀이

사각형 구간합 공식을 외우면 개념 문제이다. 

변수 관리 때문이라도 외워서 푸는 걸 추천하는 문제.

## 해답

```java
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.util.StringTokenizer;
import java.util.*;
/* Main 11659 구간 합 구하기 4
	메모리: 18260kb
	시간: 152ms
	아이디어: preSum 문제, 사전에 미리 부분의 합을 구해서 시간복잡도를 효율적으로 개선하는게 핵심인 문제
*/ 
public class Main_11660_강보승 {
	static int n, m;
	static int[][] arr, preSum;
    public static void main(String[] args) throws IOException{
    	BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
    	StringTokenizer st = new StringTokenizer(br.readLine());
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
    	// 사각형 preSum 사전작업
    	for(int i = 1; i <= n; i++) {
    		for(int j = 1; j <= n; j++) {
    			preSum[i][j] = preSum[i - 1][j] + preSum[i][j - 1] - preSum[i - 1][j - 1] + arr[i][j];
    		}
    	}
    	for(int i = 0; i < m; i++) {
    		st = new StringTokenizer(br.readLine());
    		int x1 = Integer.parseInt(st.nextToken());
    		int y1 = Integer.parseInt(st.nextToken());
    		int x2 = Integer.parseInt(st.nextToken());
    		int y2 = Integer.parseInt(st.nextToken());
    		System.out.println(getPreSum(x1, y1, x2, y2));
    	}
    }
    // 사각형preSum 출력
    static int getPreSum(int x1, int y1, int x2, int y2) {
    	return preSum[x2][y2] - preSum[x1 - 1][y2] - preSum[x2][y1 - 1] + preSum[x1 - 1][y1 - 1];
    }
}


```