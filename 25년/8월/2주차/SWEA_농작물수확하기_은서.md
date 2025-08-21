## 농작물 수확하기(SWEA 2805) - 규칙 재풀이

## 풀이

숫자 규칙을 찾아서 center를 중심으로 양쪽 패딩의 크기를 구하는 방법으로 재풀이한 문제이다.

중심 좌표를 기준으로 각 행에서 양쪽으로 넣을 패딩의 크기를 규칙으로 계산한다.

패딩 크기는 center - Math.abs(i-center)로 구할 수 있다.

이전 BFS 풀이보다 시간, 메모리가 더 낮았으나 큰 차이는 없다.

## 해답

```java
/* 날짜: 2025.08.13
 * 문제 : Solution_2805. 농작물 수확하기(규칙으로 재풀이)
 * 난이도 : D3
 * 메모리 : 26,880 kb
 * 시간 : 89 ms
 * 아이디어 : 숫자 규칙을 찾아서 center를 중심으로 양쪽 패딩의 크기를 구함 -> 합산 
 * 		이전 bfs 풀이보다 시간, 메모리가 더 낮았으나 큰 차이는 없음 
 */

public class Solution_2805 {

	public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringBuilder sb = new StringBuilder();
        int T = Integer.parseInt(br.readLine());
        
        for(int t=1; t<=T; t++) {
            int N = Integer.parseInt(br.readLine());
            int[][] arr = new int[N][N];
            
            for(int i=0; i<N; i++) {
            	String s = br.readLine();
            	for(int j=0; j<N; j++) {
            		arr[i][j] = s.charAt(j)- '0';
            	}
            }
            //중심 좌표
            int center = N/2;
            int sum = 0;
            for(int i=0; i<N; i++) {
            	//양쪽으로 넣을 패딩의 크기 구하기(규칙 적용)
            	int padding = center - Math.abs(i-center);
            	for(int j= center-padding; j<=center+padding; j++) {
            		sum += arr[i][j];
            	}
            }
            System.out.println("#" + t + " " + sum);
        }

	}

}
```