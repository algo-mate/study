## Contact([SWEA](https://swexpertacademy.com) 1238)

## 풀이

단방향 노드와 BFS를 활용한 문제입니다.
BFS에서 조건문 처리에 주의해야 합니다. 각 케이스를 나눠서 처리해야 정확한 결과를 얻을 수 있습니다.

## 해답

```java
/*
1238. Contact
메모리: 25,856KB
시간: 85ms
아이디어: 1. 단방향 노드, BFS
        2. BFS에서 조건문 처리 조심
*/
import java.io.*;
import java.util.*;

public class Solution_1238_강보승 {
    static final int MAX_NUM = 100;
    static int T, n;
    static int[] arr;
    static boolean[] visited;
    static List<Integer>[] graph;
    static Queue<int[]> queue;
    
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        T = 10;
        for (int t = 1; t <= T; t++) {
            StringTokenizer st = new StringTokenizer(br.readLine());
            n = Integer.parseInt(st.nextToken());
            int startNum = Integer.parseInt(st.nextToken());
            st = new StringTokenizer(br.readLine());
            graph = new ArrayList[MAX_NUM + 1];
            visited = new boolean[MAX_NUM + 1];
            for (int i = 1; i <= MAX_NUM; i++) {
                graph[i] = new ArrayList<>();
            }
            for (int i = 0; i < n/2; i++) {
                int from = Integer.parseInt(st.nextToken());
                int to = Integer.parseInt(st.nextToken());
                if(!graph[from].contains(to)){
                    graph[from].add(to);
                }
            }
            queue = new LinkedList<>();
            int lastNum = bfs(startNum);
            System.out.println("#" + t + " " + lastNum);
        }
    }
    
    static int bfs(int startNum){
        queue.add(new int[] {startNum, 0});
        visited[startNum] = true;
        int biggestNum = startNum;
        int count = 0;
        while(!queue.isEmpty()){
            int[] cur = queue.poll();
            for(int nextNode : graph[cur[0]]){
                if(!visited[nextNode]){
                    visited[nextNode] = true;
                    queue.add(new int[]{nextNode, cur[1] + 1});
                    // 각 케이스 나눠서 처리해야 함!
                    // 한번에 처리하려다가 첫 시도 실패
                    if(count == cur[1] + 1){
                        biggestNum = Math.max(biggestNum, nextNode);
                    }else if(count < cur[1] + 1){
                        biggestNum = nextNode;
                        count = cur[1] + 1;
                    }
                }
            }
        }
        return biggestNum;
    }
}
```