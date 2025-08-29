## 바이러스([BOJ](https://www.acmicpc.net/problem/2606) 2606)

## 풀이

BFS를 사용하여 1번 컴퓨터에서 시작해서 연결된 모든 컴퓨터를 방문합니다. 그래프를 인접리스트로 표현하고, 큐를 이용한 BFS로 1번 컴퓨터와 연결된 모든 컴퓨터를 찾아 개수를 세어 반환합니다.

## 해답

```java
import java.io.*;
import java.util.*;
/*
    2606. 바이러스
    메모리: 14160kb
    시간: 104ms
    아이디어: BFS
 */
public class Main_2606_강보승 {
    static int T, n, m, answer;
    static int[] dx = {-1, 1, 0, 0};
    static int[] dy = {0, 0, -1, 1};
    static int[] visited;
    static List<Integer>[] map;

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
        StringBuilder sb = new StringBuilder() ;
        n = Integer.parseInt(br.readLine());
        m = Integer.parseInt(br.readLine());
        map = new ArrayList[n + 1];
        for (int i = 1; i <= n; i++) {
            map[i] = new ArrayList<>();
        }
        visited = new int[n + 1];
        for (int i = 0; i < m; i++) {
            StringTokenizer st = new StringTokenizer(br.readLine());
            int x = Integer.parseInt(st.nextToken());
            int y = Integer.parseInt(st.nextToken());
            map[x].add(y);
            map[y].add(x);
        }

        System.out.println(bfs(1));
    }

    static int bfs(int x) {
        int count = 0;
        visited = new int[n + 1];
        Queue<int[]> queue = new LinkedList<>();
        queue.add(new int[]{x});
        visited[x] = 1;
        while (!queue.isEmpty()) {
            int[] curr = queue.poll();
            x = curr[0];
            for(int nextNode : map[x]) {
                if(visited[nextNode] == 0) {
                    visited[nextNode] = 1;
                    queue.add(new int[]{nextNode});
                    count++;
                }
            }
        }
        return count;
    }
}
```