## 작업순서([SWEA](https://swexpertacademy.com) 1267)

## 풀이

BFS와 위상정렬을 활용한 문제입니다.
노드를 추가할 때 위상정렬 배열에 +1을 하고, 위상정렬 배열이 0이 될 때마다 새로 큐에 추가하는 방식으로 구현했습니다.

## 해답

```java
/*
1267. 작업순서
메모리: 29,184KB
시간: 109ms
아이디어: BFS + 위상정렬
*/
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.util.ArrayList;
import java.util.LinkedList;
import java.util.List;
import java.util.Queue;
import java.util.StringTokenizer;

public class Solution_1267_강보승 {
    static int n, m;
    static int[] linkedNode;
    static int[] indegree;
    static List<Integer> result;
    static List<Integer>[] nodes;
    static Queue<Integer> queue;
    
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
        int T = 10;
        for (int t = 1; t <= T; t++) {
            StringTokenizer st = new StringTokenizer(br.readLine());
            n = Integer.parseInt(st.nextToken());
            m = Integer.parseInt(st.nextToken());
            nodes = new ArrayList[n + 1];
            indegree = new int[n + 1];
            queue = new LinkedList<>();
            for(int i = 1; i <= n; i++) {
                nodes[i] = new ArrayList<>();
            }
            linkedNode = new int[m * 2];
            st = new StringTokenizer(br.readLine());
            for(int i = 0; i < m * 2; i++) {
                linkedNode[i] = Integer.parseInt(st.nextToken());
            }
            for(int i = 0; i < m; i++) {
                nodes[linkedNode[i * 2]].add(linkedNode[i * 2 + 1]);
                // 노드 추가 시 위상정렬배열 + 1
                indegree[linkedNode[i * 2 + 1]] += 1;
            }
            for(int i = 1; i <= n; i++) {
                if(indegree[i] == 0) {
                    queue.add(i);
                }
            }
            result = new ArrayList<>();
            StringBuffer sb = new StringBuffer();
            sb.append("#").append(t).append(" ");
            while(!queue.isEmpty()) {
                int node = queue.poll();
                result.add(node);
                for(int nextNode : nodes[node]) {
                    indegree[nextNode] -= 1;
                    // 위상정렬배열 0될 때마다 새로 큐에 추가
                    if(indegree[nextNode] == 0) {
                        queue.add(nextNode);
                    }
                }
            }
            for(int node : result) {
                sb.append(node);
                sb.append(" ");
            }
            sb.append("\n");
            bw.write(sb.toString());
        }
        bw.close();
    }
}
```