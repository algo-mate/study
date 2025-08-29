## 하나로([SWEA](https://swexpertacademy.com/main/code/problem/problemDetail.do?contestProbId=AV15StKqAQkCFAYD) 1251)

## 풀이

크루스칼 알고리즘을 사용한 최소 신장 트리(MST) 문제입니다.

모든 섬을 최소 비용으로 연결하는 해저터널 건설 문제로, 환경 부담금이 E × 거리² 로 계산됩니다.

1. 모든 섬 쌍에 대해 간선 생성 (환경 부담금 = E × 유클리드 거리의 제곱)
2. 간선들을 가중치 기준으로 오름차순 정렬
3. Union-Find 자료구조로 사이클을 방지하며 간선 선택
4. N-1개의 간선으로 모든 섬을 연결하는 MST 완성

Union-Find의 경로 압축 기법을 사용하여 효율성을 높였습니다.

## 해답

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.*;

/*
    1251. 하나로
    실행시간: 1146ms
    메모리: 122,596 kb
    아이디어: 크루스칼 알고리즘
 */
public class Solution_1251_강보승 {
    static int T, N;
    static Double E;
    static int[] uf;
    static int[][] arr;
    static Point[] points;

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        T = Integer.parseInt(br.readLine());
        for (int t = 1; t <= T; t++) {
            N = Integer.parseInt(br.readLine());
            arr = new int[N][2];
            for (int i = 0; i < 2; i++) {
                StringTokenizer st = new StringTokenizer(br.readLine());
                for (int j = 0; j < N; j++) {
                    arr[j][i] = Integer.parseInt(st.nextToken());
                }
            }
            E = Double.parseDouble(br.readLine());
            int index = 0;
            points = new Point[N * N];
            // 노드랑 가중치 만들어주기
            for (int i = 0; i < N; i++) {
                for (int j = 0; j < N; j++) {
                    if (i == j) continue;
                    points[index++] = new Point(i, j, E * getDistance(i, j));
                }
            }
            List<Point> queue = new ArrayList<>();
            uf = new int[N];
            for (int i = 0; i < N; i++) {
                uf[i] = i;
            }
            for (int i = 0; i < N * N; i++) {
                if (points[i] == null) continue;
                queue.add(points[i]);
            }
            Collections.sort(queue);
            double weightSum = 0;
            int linkedCount = 0;
            for (Point p : queue) {
                // 같은 집합 아니면 추가
                if (find(p.x) != find(p.y)) {
                    union(p.x, p.y);
                    weightSum += p.w;
                    linkedCount++;
                }
                if (N - 1 == linkedCount) break;
            }
            System.out.println("#" + t + " " + Math.round(weightSum));
        }
    }

    private static void union(int x, int y) {
        int X = find(x);
        int Y = find(y);
        if (X < Y) {
            uf[X] = Y;
        } else {
            uf[Y] = X;
        }
    }

    private static int find(int x) {
        if (uf[x] == x) {
            return x;
        }
        uf[x] = find(uf[x]);
        return uf[x];
    }
    // 유클리드 거리
    private static double getDistance(int i, int j) {
        long dx = ((long) arr[i][0] - arr[j][0]);
        long dy = ((long) arr[i][1] - arr[j][1]);
        return dx * dx + dy * dy;
    }
}

class Point implements Comparable<Point> {
    int x;
    int y;
    double w;

    Point(int x, int y, double w) {
        this.x = x;
        this.y = y;
        this.w = w;
    }
    // Double 비교
    @Override
    public int compareTo(Point o) {
        return Double.compare(this.w, o.w);
    }
}
```