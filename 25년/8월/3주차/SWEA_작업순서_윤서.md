## 작업순서([SWEA](https://swexpertacademy.com/main/code/problem/problemDetail.do?contestProbId=AV2-AAkqAzkDFAUu) 1267)

## 풀이

위상정렬을 이용한 문제이다. 진입차수를 기록하는 배열을 만들고 진입차수가 0이면 순서에 등록한다.

작업을 수행할 때마다 연결된 간선을 제거해서 큐에 추가하여 순서대로 처리한다. BFS를 이용하여 진입차수가 0인 노드부터 처리하면서 위상정렬을 완성한다.

## 해답

```java
/* Swea 1267 작업순서 문제
 * 메모리 : 29,440 kb
 * 시간 :110 ms
 * 아이디어 : 진입차수를 기록하는 배열을 하나 만들고 진입차수가 0이면 순서에 등록한다
 * 작업을 수행할 때 마다 연결된 간선을 제거해서 Q에 추가해 순서대로 처리한다
 */
import java.io.*;
import java.util.*;
public class Solution_1267_남윤서 {
  public static void main(String[] args)throws Exception{
    BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
    for(int t = 1; t <= 10 ; t++){
      StringTokenizer st= new StringTokenizer(br.readLine());
      int V = Integer.parseInt(st.nextToken());
      int E = Integer.parseInt(st.nextToken());

      List<Integer>[] graph = new ArrayList[V+1];
      int[] in = new int [V+1];
      for (int i = 0; i <= V; i++) {
        graph[i] = new ArrayList<>();
      } 

      st = new StringTokenizer(br.readLine());
      for(int i = 0; i < E; i++){
        int from = Integer.parseInt(st.nextToken());
        int to = Integer.parseInt(st.nextToken());
        graph[from].add(to);
        in[to]++; // 진입 갯수 세주기
      }

      Queue<Integer> q = new LinkedList<>();
      for(int i = 1; i <= V; i++){
        if(in[i]== 0) q.add(i); // 진입 차수가 0인 친구들을 q에 넣어줌
      }

      StringBuilder sb = new StringBuilder();
      while(!q.isEmpty()){
        int cur = q.poll();
        sb.append(cur).append(" ");
        for(int e: graph[cur]){
          in[e]-=1; // cur과 이어진 자식노드의 간선을 끊어버리기
          if(in[e] == 0) q.add(e);
        }
      }
      System.out.printf("#%d %s\n",t,sb.toString());
  }
  }
}
```