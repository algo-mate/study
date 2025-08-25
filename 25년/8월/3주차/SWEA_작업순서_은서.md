## 작업순서([SWEA](https://swexpertacademy.com/main/code/problem/problemDetail.do?contestProbId=AV2-AAkqAzkDFAUu) 1267)

## 풀이

위상정렬(Topological Sort) 문제이다. 각 노드의 진입차수를 관리하고, 진입차수가 0인 노드부터 처리한다.

해당 노드에 연결되어있는 노드들의 진입 차수를 1씩 빼주면서 0이 되면 큐에 넣어준다.

## 해답

```java
package swea;

/* 날짜: 2025.08.21
 * 문제: Solution_1267. 작업순서
 * 난이도: D6
 * 메모리: 28,288 kb
 * 시간: 99ms
 * 아이디어:  위상 정렬 (Topological Sort)
 */
import java.io.*;
import java.util.*;
public class Solution_1267 {

    public static void main(String[] args) throws IOException {
        // TODO Auto-generated method stub
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringBuilder sb = new StringBuilder();
        
        for(int t=1; t<=10; t++) {
        	StringTokenizer st = new StringTokenizer(br.readLine());
            
            int V = Integer.parseInt(st.nextToken()); // 정점의 개수
            int E = Integer.parseInt(st.nextToken()); // 간선의 개수
            
            //간선 배열
            List<Integer>[] nodes = new ArrayList[V+1];
            
            //각 노드의 진입차수 관리 배열
            int[] inCount = new int[V+1]; 
            
            //node 배열 초기화
            for (int i = 1; i <= V; i++) {
    			nodes[i] = new ArrayList<>();
    		}
            
            //간선 입력 받기
            st = new StringTokenizer(br.readLine());
            for (int i = 0; i < E; i++) {
            	int from = Integer.parseInt(st.nextToken());
            	int to = Integer.parseInt(st.nextToken());
    			nodes[from].add(to); //노드 리스트에 저장
    			inCount[to] += 1;	 // to의 진입차수+1
    		}
            
            //진입차수가 0인 노드 번호 넣기
            Queue<Integer> queue = new ArrayDeque<>();
            for (int i = 1; i < V+1; i++) {
    			if(inCount[i] == 0) {
    				queue.offer(i);
    			}
    		}
            
            sb.append("#").append(t).append(" ");
            
            //해당 노드에 연결되어있는 노드들의 친입 차수 -1 해주기
            while(!queue.isEmpty()) {
            	int in = queue.poll(); //진입차수 0인 노드 하나 pop 해주기
            	sb.append(in).append(" ");
            	
            	for(int out : nodes[in]) { // in -> out 하나씩 보면서 out 노드들의 진입 차수 1씩 뺴기
            		inCount[out]--; //어차피 0 체크하니까 굳이 0이 아닐때 조건문 필요없음
            		if (inCount[out] == 0) { //0 됐으면 큐에 넣어주기
            			queue.offer(out);
            		}
            	}
            	
            }
            
            sb.append("\n");
        }
        
        System.out.println(sb.toString());

	}

}
```