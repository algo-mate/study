## ⚾([BOJ](https://www.acmicpc.net/problem/17281) 17281)

## 풀이

순열과 야구 규칙을 구현하는 문제이다. 3아웃일 때 이닝이 종료되고, 아니면 계속 정해진 순서로 선수들이 플레이한다.

안타 칠 때마다 한 칸씩 밀어주는데, 배열을 오른쪽으로 밀어줄 때는 역순으로 해야 정보 누락이 없다. 홈런일 때는 안타친 선수들부터 돌린 후 +1을 추가한다.

## 해답

```java
import java.io.*;
import java.util.*;

/*
    17281. 야구
    메모리: 63168kb
    실행시간: 508ms
    아이디어: 순열, 야구 규칙
            1. 3아웃일 때 이닝 종료, 아니면 계속 정해진 순서로 선수들 플레이
            2. 안타 칠 때마다 한 칸씩 밀어주기 -> 배열 오른쪽으로 밀어주기(역순으로 해야 정보 누락 x)
            3. 홈런일 때는 안타친 선수들부터 돌린 후 + 1 추가
 */

public class Main_17281_강보승 {
    static int n, maxScore;
    static int[] order;
	static int[][] arr;
	static boolean[] visited;
	static StringTokenizer st;
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        n = Integer.parseInt(br.readLine());
        arr = new int[n][9];
        order = new int[9];
        visited = new boolean[9];
        for(int i = 0; i < n; i++) {
        	st = new StringTokenizer(br.readLine());
        	for(int j = 0; j < 9; j++) {
        		arr[i][j] = Integer.parseInt(st.nextToken());
        	}
        }
        order[3] = 0;
        visited[0] = true;
        permutation(0);
        System.out.println(maxScore);
    }
    static void permutation(int depth) {
    	if(depth == 9) {
    		playGame();
    		return;
    	}
    	if(depth == 3) {
    		permutation(depth + 1);
    		return;
    	}
		for(int i = 1; i < 9; i++) {
    		if(!visited[i]) {
    			visited[i] = true;
    			order[depth] = i;
    			permutation(depth + 1);
    			visited[i] = false;
    		}
    	}
    }
    static void playGame() {
        int currentScore = 0;
        int index = 0;

        for (int turn = 0; turn < n; turn++) {
            // 한 이닝 끝나면 아웃 정보 초기화...
            int outCount = 0;
            boolean[] bases = new boolean[4];

            while (outCount < 3) {
                int player = order[index];
                int hit = arr[turn][player]; 

                if (hit == 0) {
                    outCount++;
                } else {
                    currentScore = getCurrentScore(bases, hit, currentScore);
                    // 주자 다 돌린 후 홈런인 경우 + 1
                    if (hit == 4) {
                        currentScore++;
                    } else {
                        bases[hit] = true;
                    }
                }
                index = (index + 1) % 9;
            }
        }
        maxScore = Math.max(maxScore, currentScore);
    }

    private static int getCurrentScore(boolean[] bases, int hit, int currentScore) {
        // 3루부터 홈으로 돌려야 정보 누락 없음
        for (int i = 3; i >= 1; i--) {
            if (bases[i]) {
                if (i + hit >= 4) {
                    currentScore++;
                } else {
                    bases[i + hit] = true;
                }
                bases[i] = false;
            }
        }
        return currentScore;
    }
}
```