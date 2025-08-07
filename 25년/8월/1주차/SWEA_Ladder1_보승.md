
## 풀이

처음에는 dfs로 풀었지만, 사실 dfs로 풀기에는 위험한 문제이다.

dfs로 풀다가 잘못하면 스택이 많이 쌓여서 의문의 오류가 발생하기 쉽다.(실제로 여러번 터짐)

결국 반복문으로 다시 접근했다.

## 처음 해답

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

public class Solution {
    static final int MAX_NUM = 100;
    static int n, answer;
    static boolean flag;
    static int[] dx = new int[] {0, 0, -1};
    static int[] dy = new int[] {-1, 1, 0};
    static boolean[][] visited;
    static int[][] arr;
    public static void main(String[] args) throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        for(int t = 1; t <= 10; t++) {
            int T = Integer.parseInt(br.readLine());
            StringBuffer sb = new StringBuffer();
            sb.append("#").append(t).append(" ");
            arr = new int[MAX_NUM + 1][MAX_NUM + 1];
            visited = new boolean[MAX_NUM + 1][MAX_NUM + 1];
            for(int i = 0; i < MAX_NUM; i++) {
                StringTokenizer st = new StringTokenizer(br.readLine());
                for(int j = 0; j < MAX_NUM; j++) {
                    arr[i][j] = Integer.parseInt(st.nextToken());
                }
            }
            answer = -1;
            int startY = 0;
            for(int i = 0; i < MAX_NUM; i++) {
                if(arr[MAX_NUM - 1][i] == 2) {
                    startY = i;
                    break;
                }
            }
            flag = false;
            dfs(MAX_NUM - 1, startY);
            System.out.println(sb.append(answer));
        }
    }

    static void dfs(int x, int y) {
        if(flag)
            return;
        if(x == 0) {
            answer = y;
            flag = true;
            return;
        }
        for(int i = 0; i < 3; i++) {
            int nx = x + dx[i];
            int ny = y + dy[i];
            if(inRange(nx, ny) && arr[nx][ny] == 1 && !visited[nx][ny]) {
                visited[nx][ny] = true;
                dfs(nx, ny);
                visited[nx][ny] = false;
            }
        }
    }
    static boolean inRange(int x, int y) {
        return 0 <= x && x < MAX_NUM && 0 <= y && y < MAX_NUM;
    }
}
```

## 반복문 해답

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.util.*;
import java.util.StringTokenizer;

public class Solution {
	static final int MAX_NUM = 100;
	static boolean isFindDestination;
	static int n, answer;
	static int[] dy = new int[] {-1, 1};
	static boolean[][] visited;
	static int[][] arr;
    public static void main(String[] args) throws IOException{
    	BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
    	for(int t = 1; t <= 10; t++) {
    		int T = Integer.parseInt(br.readLine());
    		StringBuffer sb = new StringBuffer();
    		sb.append("#").append(t).append(" ");
    		arr = new int[MAX_NUM + 1][MAX_NUM + 1];
    		visited = new boolean[MAX_NUM + 1][MAX_NUM + 1];
    		for(int i = 0; i < MAX_NUM; i++) {
    			StringTokenizer st = new StringTokenizer(br.readLine());
    			for(int j = 0; j < MAX_NUM; j++) {
    				arr[i][j] = Integer.parseInt(st.nextToken());
    			}
    		}
    		int startY = -1;
    		for(int i = 0; i < MAX_NUM; i++) {
    			if(arr[MAX_NUM - 1][i] == 2) {
    				startY = i;
    			}
    		}
    		int startX = MAX_NUM - 1;
    		while(true) {
    			if(startX == 0) {
    				answer = startY;
    				break;
    			}
    			int nx = startX;
    			int ny = startY;
    			if(canGoLeft(nx, ny)) {
        			while(canGoLeft(nx, ny)) {
        				ny = ny - 1;
        			}
    			}else if(canGoRight(nx, ny)) {
    				while(canGoRight(nx, ny)) {
        				ny = ny + 1;
        			}
    			}
    			if(inRange(nx - 1, ny) && arr[nx - 1][ny] == 1) {
    				startX = nx - 1;
    				startY = ny;
    			}
    		}
    		System.out.println(sb.append(answer));
    	}
    }
    static boolean canGoLeft(int x, int y) {
    	return inRange(x, y - 1) && arr[x][y - 1] == 1;
    }
    static boolean canGoRight(int x, int y) {
    	return inRange(x, y + 1) && arr[x][y + 1] == 1;
    }
    static boolean inRange(int x, int y) {
    	return 0 <= x && x < MAX_NUM && 0 <= y && y < MAX_NUM;
    }
}
```