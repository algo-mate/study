## 핀볼 게임(SWEA 5650)

## 풀이

시뮬레이션 + 구현 문제이다.

벽을 맞고, 움직이는 방향을 배열로 미리 만들거나 메서드로 구현하는 것이 핵심이다.

시뮬레이션의 경우 예외처리 -> 이동 -> 상황에 따른 케이스 처리 순으로 예외적인 상황들을 앞에서 우선 처리하는 것이 중요하다.

## 해답

```java
/*
5650. 핀볼 게임
    메모리: 103,320 kb
    시간: 1,530 ms
    아이디어: 시뮬레이션 + 구현
    1. 벽을 맞고, 움직이는 방향을 배열로 미리 만들거나 메서드로 구현하는 것이 핵심
    2. 시뮬레이션의 경우 예외처리 -> 이동 -> 상황에 따른 케이스 처리 순으로 예외적인 상황들을 앞에서 우선 처리하는 것이 중요.
*/
public class Solution_5650_강보승 {

    static int[][] dirChange = {
        {0, 0, 0, 0},
        {2, 0, 3, 1},
        {2, 3, 1, 0},
        {1, 3, 0, 2},
        {3, 2, 0, 1},
        {2, 3, 0, 1}
    };
    static int[] dx = {0, 1, 0, -1};
    static int[] dy = {1, 0, -1, 0};

    static int[][] map;
    static ArrayList<Pos>[] wormhole;

    static int N;
    static int answer;

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        int T = sc.nextInt();

        for (int tc = 1; tc <= T; tc++) {
            N = sc.nextInt();
            map = new int[N][N];
            wormhole = new ArrayList[5];

            for (int i = 0; i < 5; i++) {
                wormhole[i] = new ArrayList<>();
            }

            for (int i = 0; i < N; i++) {
                for (int j = 0; j < N; j++) {
                    map[i][j] = sc.nextInt();
                    if (map[i][j] >= 6) {
                        wormhole[map[i][j] - 6].add(new Pos(i, j));
                    }
                }
            }

            answer = 0;

            for (int i = 0; i < N; i++) {
                for (int j = 0; j < N; j++) {
                    if (map[i][j] != 0)
                        continue;
                    for (int dir = 0; dir < 4; dir++) {

                        int score = check(i, j, dir);
                        if (score > answer)
                            answer = score;

                    }
                }
            }

            System.out.println("#" + tc + " " + answer);
        }

    }

    static int check(int r, int c, int dir) {
        int score = 0;

        int nowR = r;
        int nowC = c;
        boolean moved = false;

        while (true) {

            // 제자리(예외 처리)
            if (nowR == r && nowC == c && moved)
                break;

            // 블랙홀(예외 처리)
            if (map[nowR][nowC] == -1)
                break;

            nowR = nowR + dx[dir];
            nowC = nowC + dy[dir];
            moved = true;

            // 벽에 부딪힘(이 케이스부터 처리하는 것이 중요)
            if (!inRange(nowR, nowC)) {
                nowR = nowR - dx[dir];
                nowC = nowC - dy[dir];

                dir = (dir + 2) % 4;

                score++;
            }

            // 블록에 부딪힘
            if (map[nowR][nowC] >= 1 && map[nowR][nowC] <= 5) {
                int block = map[nowR][nowC];

                dir = dirChange[block][dir];
                score++;
                continue;
            }

            // 웜홀
            if (map[nowR][nowC] > 5) {
                int block = map[nowR][nowC];
                int r1 = wormhole[block - 6].get(0).r;
                int c1 = wormhole[block - 6].get(0).c;

                int r2 = wormhole[block - 6].get(1).r;
                int c2 = wormhole[block - 6].get(1).c;

                if (nowR == r1 && nowC == c1) {
                    nowR = r2;
                    nowC = c2;
                } else {
                    nowR = r1;
                    nowC = c1;
                }
            }

        }

        return score;
    }
	static boolean inRange(int x, int y) {
		return 0 <= x && x < N && 0 <= y && y < N;
	}

}

class Pos {
    int r, c;

    Pos(int r, int c) {
        this.r = r;
        this.c = c;
    }

}
```