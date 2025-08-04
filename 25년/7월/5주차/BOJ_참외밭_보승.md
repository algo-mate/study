## 문제(실버 2) 

[참외밭](https://www.acmicpc.net/problem/2477) 

## 해설

**밭의 전체 넓이**보다 **작은 밭의 너비**와 **높이**를 구하는 것이 핵심인 문제이다.

이를 위해 **최댓값**을 통해 **밭의 전체 너비**와 **높이**를 확정한 후 

**변수**처럼 보이던 **작은 밭과의 관계**를 살피기 시작했다.

그리고 **동서남북 방향** 정보와 **반시계 방향**으로 값이 주어진다는 정보를 활용한 결과, **작은 밭의 너비와 높이**를 확정할 수 있었다.

## 풀이

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

public class Main {
    public static void main(String[] args) throws IOException{
    	BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
    	int c = Integer.parseInt(br.readLine());
    	int maxWidthIdx = 0;
    	int maxHeightIdx = 0;
    	int maxWidth = 0;
    	int maxHeight = 0;
    	int[][] arr = new int[6][2];
    	for(int i = 0; i < 6; i++) {
    		StringTokenizer st = new StringTokenizer(br.readLine());
    		arr[i][0] = Integer.parseInt(st.nextToken());
    		arr[i][1] = Integer.parseInt(st.nextToken());
            //최대 너비
    		if((arr[i][0] == 1 || arr[i][0] == 2) && maxWidth < arr[i][1]) {
    			maxWidth = Math.max(maxWidth, arr[i][1]);
    			maxWidthIdx = i;
            //최대 높이
    		}else if((arr[i][0] == 3 || arr[i][0] == 4) && maxHeight < arr[i][1]) {
    			maxHeight = Math.max(maxHeight, arr[i][1]);
    			maxHeightIdx = i;
    		}
    	}
    	int house = 0;
        // 최대 너비와 작은 밭의 관계 case 1
    	if(maxHeightIdx == (maxWidthIdx + 1) % 6) {
    		house = maxWidth * maxHeight - arr[(maxWidthIdx + 3) % 6][1] * arr[(maxWidthIdx + 4) % 6][1]; 
        // 최대 너비와 작은 밭의 관계 case 2
    	}else if(maxHeightIdx == (maxWidthIdx + 5) % 6) {
    		house = maxWidth * maxHeight - arr[(maxWidthIdx + 2) % 6][1] * arr[(maxWidthIdx + 3) % 6][1];
    	}
    	System.out.println(house * c);
    	
    }
}
```


