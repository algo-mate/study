## 결혼식 (실버2)

## 해설

dfs, bfs 어느 것으로도 풀어도 가능한 문제지만, dfs로 잘못 풀면 틀릴 가능성이 높음.

노드랑 노드 사이의 거리이기 때문에 bfs로 푸는 게 훨씬 직관적이고 좋은 접근방법


## 정답

```java
public class Main_5567_강보승 {
	static final int MAX_NUM = 500;
	static int n, m;
	static boolean[] visited = new boolean[MAX_NUM + 1];
	static List<Integer>[] graph = new ArrayList[MAX_NUM + 1];
    public static void main(String[] args) throws IOException{
    	BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
    	n = Integer.parseInt(br.readLine());
    	m = Integer.parseInt(br.readLine());
    	for(int i = 1; i <= n; i++) {
    		graph[i] = new ArrayList<>();
    	}
    	for(int i = 0; i < m; i++) {
    		StringTokenizer st = new StringTokenizer(br.readLine());
    	  	int person1 = Integer.parseInt(st.nextToken());
    	  	int person2 = Integer.parseInt(st.nextToken());
    	  	graph[person1].add(person2);
    	  	graph[person2].add(person1);
    	}
    	visited[1] = true;
    	dfs(1, 0);
    	int count = 0;
    	for(int i = 2; i <= n; i++) {
    		if(visited[i]) {
    			count++;
    		}
    	}
    	System.out.println(count);
    }
    static void dfs(int node, int depth) {
    	if(depth == 2) {
    		return;
    	}
    	for(int i = 0; i < graph[node].size(); i++) {
    		int nextNode = graph[node].get(i);
    		// 일반적인 dfs랑 다르게 1->2->3 depth == 2 만족으로 1->3->4 경로 탐색 불가
//    		if(!visited[nextNode]) {
			visited[nextNode] = true;
			dfs(nextNode, depth + 1);
//    		}
    	}
    }
}

```