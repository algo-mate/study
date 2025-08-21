## 키 순서(SWEA 5643)

## 풀이

모든 노드와 노드 사이의 관계 파악이 필요한 문제로 플로이드 워셜 알고리즘을 사용한다.

순방향 BFS + 역방향 BFS로도 해결할 수 있다.

각 학생에 대해 자신보다 키가 큰 학생들과 작은 학생들의 관계를 모두 파악할 수 있으면 순서를 알 수 있다.

## 해답

```java
/*
    5643. 키 순서
    메모리: 97,532 kb
    실행시간: 1,316 ms
    아이디어: 1. 모든 노드랑 노드 사이의 관계 파악 필요 -> 플로이드 워셜 알고리즘
            2. 순방향 BFS + 역방향 BFS
*/
public class Solution_5643_강보승 {
    static final int MAX_NUM = 500;
    static int T, n, m;
    static int[][] arr;
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        T = Integer.parseInt(br.readLine());
        for (int t = 1; t <= T; t++) {
            n = Integer.parseInt(br.readLine());
            m = Integer.parseInt(br.readLine());
            arr = new int[n + 1][n + 1];
            for(int i = 1; i <= n; i++){
                for(int j = 1; j <= n; j++){
                    arr[i][j] = MAX_NUM;
                }
            }
            for (int i = 0; i < m; i++) {
                StringTokenizer st = new StringTokenizer(br.readLine());
                int a = Integer.parseInt(st.nextToken());
                int b = Integer.parseInt(st.nextToken());
                arr[a][b] = 1;
            }
            for(int k = 1; k <= n; k++){
                for(int i = 1; i <= n; i++){
                    for(int j = 1; j <= n; j++){
                        // (i -> k -> j) 다른 정점을 통해 이동하는 모든 경우의 수 파악
                        if(arr[i][k] != MAX_NUM && arr[k][j] != MAX_NUM){
                            arr[i][j] = 1;
                        }
                    }
                }
            }
            int count = 0;
            for(int i = 1; i <= n; i++){
                int tmpCount = 0;
                for(int j = 1; j <= n; j++){
                    // 만약 (i, j) 또는 (j, i)가 서로 관계를 안다면
                    if(arr[i][j] != MAX_NUM || arr[j][i] != MAX_NUM){
                         tmpCount++;
                    }
                }
                if(tmpCount == n - 1){
                    count++;
                }
            }
            System.out.println("#" + t + " " + count);
        }
    }
}
```