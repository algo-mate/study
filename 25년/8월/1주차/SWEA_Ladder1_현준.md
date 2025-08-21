## Ladder1([SWEA](https://swexpertacademy.com) 1210)

## 풀이

도착점 부터 거꾸로 탐색하는 방식으로 접근했습니다.
재귀함수를 사용하여 위쪽으로 올라가면서 좌우로 이동 가능한 경우 해당 방향으로 끝까지 이동한 후 다시 위로 올라갑니다.

## 해답

```java
/*
1210. Ladder1
메모리: 135,828KB
시간: 123ms
아이디어: 도착점 부터 거꾸로 탐색해야겠다라고 생각하고 풀이 시작. dfs 문제인줄 알고 dfs로 풀려고 했는데 그게 아니었던것 같다. 그냥 dfs도 이것도 저것도 아닌 애메하게 재귀함수로 푼게 되어버렸다.
*/
import java.io.BufferedReader;
import java.io.FileInputStream;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

public class Solution_1210_장현준 {

	static int[][] ladder = new int[100][100];
	
	public static void main(String[] args) throws IOException {
//		System.setIn(new FileInputStream("res/input_1210.txt"));
		
		BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(System.in));
		
		for (int testCase = 1; testCase <= 10; testCase++) {
			bufferedReader.readLine(); // 사용 안 하는 값이라 버림
			StringTokenizer stringTokenizer;
			
			int destination = -1;
			int start = -1;
			
			for (int i = 0; i < 99; i++) {
				stringTokenizer = new StringTokenizer(bufferedReader.readLine());
				for (int j = 0; j < 100; j++) {
					ladder[i][j] = Integer.parseInt(stringTokenizer.nextToken());
				}
			}
			
			stringTokenizer = new StringTokenizer(bufferedReader.readLine());
			for (int i = 0; i < 100; i++) { // 마지막 줄에서 도착점 찾기
				ladder[99][i] = Integer.parseInt(stringTokenizer.nextToken());
				if(ladder[99][i] == 2) {
					destination = i;
				}
			}
			
			start = findStart(99, destination);
			
			System.out.println("#" + testCase + " " + start);
		}
	}
	
	static int findStart(int row, int col) {
		if (row == 0) {
			return col;
		}
		
		if (col > 0 && ladder[row][col - 1] == 1) {
			while(col > 0 && ladder[row][col - 1] == 1) {
				col--;
			}
		} else if (col < 99 && ladder[row][col + 1] == 1) {
			while(col < 99 && ladder[row][col + 1] == 1) {
				col++;
			}
		}
		
		return findStart(--row, col);	
	}
}
```