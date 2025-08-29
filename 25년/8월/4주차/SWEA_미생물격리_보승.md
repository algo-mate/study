## 미생물격리([SWEA](https://swexpertacademy.com/main/code/problem/problemDetail.do?contestProbId=AV597vbqAH0DFAVl) 2382)

## 풀이

미생물 군집의 이동과 합체를 시뮬레이션하는 격자 문제입니다.

핵심 규칙:
1. 모든 군집이 주어진 방향으로 동시에 이동
2. 약품 구역(가장자리)에 도달하면 미생물 수가 절반으로 감소하고 방향 반전
3. 같은 위치에 여러 군집이 모이면 합체 (가장 큰 군집의 방향을 따름)

구현 전략:
1. 임시 격자를 사용해 모든 군집의 이동을 동시에 처리
2. 각 격자 위치에 총 미생물 수와 최대 군집의 방향을 저장
3. 임시 격자의 결과를 원본 격자에 반영

이 방식으로 동시 이동과 합체 규칙을 정확하게 구현할 수 있습니다.

## 해답

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.*;
/*
 * 2382. 미생물 격리
 * 메모리: 118,144 kb
 * 실행시간: 3,011ms
 * 아이디어: 격자 시뮬레이션
 *       1. 처음에 초기 격자에서 시뮬레이션 하다가 문제 발생
 *       2. 임시 격자 만들어서 해당 격자에서 시뮬레이션 후 초기 격자로 옮기기
 */
public class Solution_2382_강보승 {
	static final int VIRUS_NUM = 0;
	static final int DIRECTION = 1;
	static final int MAX_VIRUS_NUM = 2;
    static int T, n, m, k;
    static int[] dx = {0, -1, 1, 0, 0};
    static int[] dy = {0, 0, 0, -1, 1};
    static int[][] arr;
    static int[][][] graph;
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        T = Integer.parseInt(br.readLine());
        for (int t = 1; t <= T; t++) {
        	StringTokenizer st = new StringTokenizer(br.readLine());
        	n = Integer.parseInt(st.nextToken());
        	m = Integer.parseInt(st.nextToken());
        	k = Integer.parseInt(st.nextToken());
        	graph = new int[n][n][2];
        	arr = new int[k][4];
        	for(int i = 0; i < k; i++) {
        		st = new StringTokenizer(br.readLine());
        		// 세로위치, 가로위치, 미생물 수, 이동방향
        		int x = Integer.parseInt(st.nextToken());
        		int y = Integer.parseInt(st.nextToken());
        		int v = Integer.parseInt(st.nextToken());
        		int d = Integer.parseInt(st.nextToken());
        		graph[x][y][0] = v;
        		graph[x][y][1] = d;
        	}
            simulate();
            System.out.println("#" + t + " " + countVirus());
        }
    }
    static void simulate() {
    	for(int t = 0; t < m; t++) {
        	int[][][] tmpGraph = new int[n][n][3];
    		for(int i = 0; i < n; i++) {
    			for(int j = 0; j < n; j++) {
    				if(graph[i][j][VIRUS_NUM] == 0) continue;
					int virusNum = graph[i][j][VIRUS_NUM];
					int dir = graph[i][j][DIRECTION];
					int nx = i + dx[dir];
					int ny = j + dy[dir];
					// 약품 칠해진 셀이면 방향, 수 변경
					if(!inRange(nx, ny)) {
						virusNum = (virusNum / 2);
						dir = change(dir);
					}
					if(virusNum == 0) continue;
					// 임시 그래프에서 격자에서 가장 최대 수의 미생물 군집 방향 찾기
					if(tmpGraph[nx][ny][MAX_VIRUS_NUM] < virusNum) {
						tmpGraph[nx][ny][MAX_VIRUS_NUM] = virusNum;
						tmpGraph[nx][ny][DIRECTION] = dir;
					}
					tmpGraph[nx][ny][VIRUS_NUM] += virusNum;
    			}
    		}
        	for(int i = 0; i < n; i++) {
    			for(int j = 0; j < n; j++) {
    				graph[i][j][0] = tmpGraph[i][j][0];
    				graph[i][j][1] = tmpGraph[i][j][1];
    			}
        	}
    	}
    }
    static int countVirus() {
    	int count = 0;
    	for(int i = 0; i < n; i++) {
			for(int j = 0; j < n; j++) {
				count += graph[i][j][VIRUS_NUM];
			}
    	}
    	return count;
    }
    static int change(int dir) {
    	if(dir == 1) return 2;
    	else if(dir == 2) return 1;
    	else if(dir == 3) return 4;
    	else return 3;
    }
    static boolean inRange(int x, int y) {
    	return 0 < x && x < n - 1 && 0 < y && y < n - 1;
    }
}
```