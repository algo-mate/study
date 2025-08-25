## 디저트 카페([SWEA](https://swexpertacademy.com/main/code/problem/problemDetail.do?contestProbId=AV5VwAr6APYDFAWu) 2105)

## 풀이

DFS로 구현한 문제이다. 반복문 범위와 조건을 생각하는데 시간이 소요된다.

시작점의 범위는 양쪽 테두리를 제외한 가운데 부분의 모든 점으로 지정한다. dfs 안에서는 모든 대각선 방향을 탐색하지 않고, 현재 방향과 이 다음 방향만 탐색한다.

## 해답

```java
package swea;

/* 날짜: 2025.08.21
 * 문제: Solution_2105. 디저트 카페
 * 메모리: 29,440 kb
 * 시간: 222 ms
 * 아이디어:  dfs로 구현. 반복문 범위와 조건을 생각하는데 시간이 소요됨
 * 		-. 시작점의 범위는 양쪽 테두리를 제외한 가운데 부분의 모든 점으로 지정
 * 		-. dfs 안에서는 모든 대각선 방향을 탐색하지 않고, 현재 방향과 이 다음 방향만 탐색 
 */

import java.io.*;
import java.util.*;

public class Solution_2105 {
    
    static int[] dx = {1, 1, -1, -1}; //오른쪽 아래, 왼쪽 아래, 왼쪽 위 , 오른쪽 위
    static int[] dy = {1, -1, -1, 1};
    static int N, maxLen;
    static List<Integer> desert;
    static int[][] cafe;
    static int initX, initY;
    static boolean[][] visited;
    public static void main(String[] args) throws IOException {
        // TODO Auto-generated method stub
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringBuilder sb = new StringBuilder();
        StringTokenizer st;
        
        int T =  Integer.parseInt(br.readLine());
        
        for(int t=1; t<=T; t++) {
        	st = new StringTokenizer(br.readLine());
            
            N = Integer.parseInt(st.nextToken());
            cafe = new int[N][N];
            visited = new boolean[N][N];
            desert = new ArrayList<>(); //먹은 디저트 목록
            
            //카페 지도 입력받기 
            for (int i = 0; i < N; i++) {
                st = new StringTokenizer(br.readLine());
                for (int j = 0; j < N; j++) {
                    cafe[i][j] = Integer.parseInt(st.nextToken());
                }
            }
            
            maxLen = -1;
            //마름모 사각형을 만들 수 있는 시작점 범위 설정
            for (int i = 0; i <= N-3; i++) {
                for (int j = 1; j <= N-2; j++) {
                	//시작 위치 고정시키기(비교 위해)
                    initX = i; initY = j;
                    getPath(i, j, 0, 0);
                }
                
            }
            
            System.out.println("#"+t+" "+maxLen);
        }
        
    }
    
    static void getPath(int x, int y, int len, int d) {
    	//기저조건. 현재 x, y값이 시작 위치인지 + 사각형이 만들어졌는지
        if(x==initX && y==initY && len>=4) {
            maxLen = Math.max(maxLen, len);
            return;
        }
        
        for(int i=d; i<=d+1; i++) { //현재 방향과, 그 다음 방향까지만 돌리기
            //종료조건이 위에 없으니, 방향 배열 벗어나는 걸 따로 체크해줘야 함
        	if(i>3) return;
        	
        	//현재 방향 or 이 다음 방향
            int nx = x+dx[i];
            int ny = y+dy[i];
            
            if(0<=nx && nx<N && 0<=ny && ny<N) {
                //먹지 않은 디저트만 탐색
                if(!desert.contains(cafe[nx][ny])) {
                    desert.add(cafe[nx][ny]);
                    getPath(nx, ny, len+1, i);
                    desert.remove(desert.size()-1);
                }
            }

        }
    }

}
```