## 최적경로([SWEA](https://swexpertacademy.com/main/code/problem/problemDetail.do?contestProbId=AV15OZ4qAPICFAYD) 1247)

## 풀이

순열을 이용한 완전탐색으로 TSP(외판원 순회 문제)를 해결하는 문제입니다.

회사에서 출발하여 모든 고객을 방문하고 집으로 돌아가는 최단 경로를 찾습니다.

핵심 아이디어:
1. 고객 방문 순서의 모든 순열을 생성 (N! 가지)
2. 각 순열에 대해 총 이동 거리를 계산
   - 회사 → 첫 번째 고객
   - 고객들 간의 이동
   - 마지막 고객 → 집
3. 맨하탄 거리(|x1-x2| + |y1-y2|) 사용
4. 모든 경우 중 최솟값 선택

시간 복잡도는 O(N! × N)이지만 N≤10 조건에서 충분히 해결 가능합니다.

## 해답

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.*;
/*
 * 1247. 최적 경로
 * 실행시간: 2,282 ms
 * 메모리: 30,100 kb
 * 아이디어: 순열 -> 계산
 */
public class Solution_1247_강보승 {
	static int T, n, sx, sy, ex, ey, minDistance;
	static boolean[] selected;
	static int[][] arr;
	static List<Integer> list;
	public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringBuilder sb = new StringBuilder();
        T = Integer.parseInt(br.readLine());
        for(int t = 1; t <= T; t++) {
            n = Integer.parseInt(br.readLine());
            StringTokenizer st = new StringTokenizer(br.readLine());
            sx = Integer.parseInt(st.nextToken());
            sy = Integer.parseInt(st.nextToken());
            ex = Integer.parseInt(st.nextToken());
            ey = Integer.parseInt(st.nextToken());
            arr = new int[n][2];
            selected = new boolean[n];
            list = new ArrayList<>();
            minDistance = Integer.MAX_VALUE;
            for(int i = 0; i < n; i++) {
            	arr[i][0] = Integer.parseInt(st.nextToken());
            	arr[i][1] = Integer.parseInt(st.nextToken());
            }
            permutation(0);
            System.out.println("#" + t + " " + minDistance);
        }
    }
	// 순열 대신 dfs로 가치지기로 하면 더 효율적이지만.. 시간이 넉넉하기 때문에..
	static void permutation(int depth) {
		if(depth == n) {
			calculate();
			return;
		}
		for(int i = 0; i < n; i++) {
			if(!selected[i]) {
				selected[i] = true;
				list.add(i);
				permutation(depth + 1);
				selected[i] = false;
				list.remove(list.size() - 1);
			}
		}
	}
	static void calculate() {
		 int startIndex = 0;
		 int peopleNum = list.get(startIndex);
		 int distanceSum = getDistance(sx, sy, arr[peopleNum][0], arr[peopleNum][1]);
		 for(int i = 1; i < list.size(); i++) {
			 int peopleNum1 = list.get(startIndex);
			 int peopleNum2 = list.get(i);
			 distanceSum += getDistance(arr[peopleNum1][0], arr[peopleNum1][1], arr[peopleNum2][0], arr[peopleNum2][1]);
			 startIndex = i;
		 }
		 peopleNum = list.get(startIndex);
		 distanceSum += getDistance(arr[peopleNum][0], arr[peopleNum][1], ex, ey);
		 minDistance = Math.min(distanceSum, minDistance);
	}
	static int getDistance(int x1, int y1, int x2, int y2) {
		return Math.abs(x1 - x2) + Math.abs(y1 - y2);
	}
}
```