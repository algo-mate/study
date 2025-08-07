## 햄버거

## 해설

전형적인 조합 문제이다.

재귀함수 인자는 depth와 점수, 칼로리로 설정해서 간단하게 해결했다.

## 해답

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.util.*;
import java.util.StringTokenizer;

/*
 * 5215 햄버거 다이어트
 * 메모리: 26112kb
 * 시간: 129 ms
 * 아이디어
 * 1. 전형적인 조합 문제, 칼로리 합에 따라 탈출 조건과 함께 최대 점수 결정해주면 됨
 */
public class Solution_5215_강보승 {
	static int n, maxCal, maxScore;
	static int[][] ingredients;
    public static void main(String[] args) throws IOException{
    	BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
    	int T = Integer.parseInt(br.readLine());
    	for(int t = 1; t <= T; t++) {
    		StringTokenizer st = new StringTokenizer(br.readLine());
    		n = Integer.parseInt(st.nextToken());
    		maxCal = Integer.parseInt(st.nextToken());
    		ingredients = new int[n][2];
    		for(int i = 0; i < n; i++) {
    			st = new StringTokenizer(br.readLine());
    			ingredients[i][0] = Integer.parseInt(st.nextToken());
    			ingredients[i][1] = Integer.parseInt(st.nextToken());
    		}
    		maxScore = 0;
    		choose(0, 0, 0);
    		System.out.println("#"+ t + " " + maxScore);
    	}
    }
    static void choose(int depth, int score, int cal) {
    	// 마지막 depth 도달
    	if(depth == n) {
    		// 최대 칼로리 이하
    		if(cal <= maxCal) {
    			// 최대 점수 결정
    			maxScore = Math.max(maxScore, score);
    		}
    		return;
    	}
    	choose(depth + 1, score, cal);
    	choose(depth + 1, score + ingredients[depth][0], cal + ingredients[depth][1]);
    }
}

```