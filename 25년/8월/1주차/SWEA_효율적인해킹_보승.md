## 효율적인 해킹(실버 1)

## 해설

완전탐색 + dfs 또는 bfs로 간단하게 해결할 수 있는 문제

제한시간도 5초로 넉넉하기 때문에 완전탐색이 가능한 문제이다.

## 해답

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.ArrayList;
import java.util.List;
import java.util.StringTokenizer;
/*
    1325 효율적인 해킹
    1. 메모리: 146496kb, 시간: 10140ms
    2. 아이디어: 완전탐색 + dfs
 */
public class Main_1325_강보승 {
    static int[] arr;
    static boolean[] visited;
    static List<Integer>[] graph;
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());
        int n = Integer.parseInt(st.nextToken());
        int m = Integer.parseInt(st.nextToken());
        graph = new ArrayList[n + 1];
        for(int i = 0; i <= n; i++){
            graph[i] = new ArrayList<>();
        }
        for(int i = 0; i < m; i++){
            st = new StringTokenizer(br.readLine());
            int nodeA = Integer.parseInt(st.nextToken());
            int nodeB = Integer.parseInt(st.nextToken());
            // 단방향 그래프
            graph[nodeB].add(nodeA);
        }
        // 전체 노드 탐색
        arr = new int[n + 1];
        for(int i = 1; i <= n; i++){
            visited = new boolean[n + 1];
            visited[i] = true;
            dfs(i, i);
        }
        int maxNum = 0;
        for(int i = 1; i <= n; i++){
            maxNum = Math.max(maxNum, arr[i]);
        }
        for(int i = 1; i <= n; i++){
            if(arr[i] == maxNum){
                System.out.print(i + " ");
            }
        }
    }
    static void dfs(int num, int root) {
        for (int nextNode : graph[num]) {
            if (!visited[nextNode]) {
                visited[nextNode] = true;
                // 새로 방문할 때마다 root 노드에 1씩 더하기
                arr[root] += 1;
                dfs(nextNode, root);
            }
        }
    }
}

```