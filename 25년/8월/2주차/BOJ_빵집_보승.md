## 빵집(BOJ 3109)

## 풀이

그리디 + DFS 알고리즘으로 해결하는 문제이다.

언제 최댓값을 가지는지 경로를 따져보면 위에서 시작하면 위에서부터 탐색하면서 채워주면 최댓값을 가지게 된다.

Flag로 탐색을 더 이상 못하게 방지하는 것이 핵심이다.

## 해답

```java
/*
    3109. 빵집
    메모리: 39948kb
    시간: 384ms
    아이디어: 1. 그리디 + DFS
            2. 언제 최댓값을 가지는지 경로를 따져보면 위에서 시작하면 위에서부터 탐색하면서 채워주면 최댓값 가지게 됨
            3. Flag로 탐색 더 이상 못하게 방지하기
*/
public class Main_3109_강보승 {
    static boolean startCount;
    static int n, m, count;
    // 위에서부터 탐색하므로 오른쪽 위, 오른쪽, 오른쪽 아래 순서대로 탐색하도록 설정
    static int[] dx = new int[] {-1, 0, 1};
    static int[] dy = new int[] {1, 1, 1};
    static boolean[][] visited;
    static char[][] arr;
    public static void main(String[] args) throws Exception {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());
        n = Integer.parseInt(st.nextToken());
        m = Integer.parseInt(st.nextToken());
        arr = new char[n][m];
        visited = new boolean[n][m];
        for(int i = 0; i < n; i++) {
            String strLine = br.readLine();
            for(int j = 0; j < m; j++) {
                arr[i][j] = strLine.charAt(j);
            }
        }
        count = 0;
        for(int i = 0; i < n; i++) {
            startCount = true;
            visited[i][0] = true;
            dfs(i, 0);
        }
        System.out.println(count);
    }
        static void dfs(int x, int y) {
        // 경로 찾으면 더 이상 탐색 못하게 막기
        if(y == m - 1 && startCount) {
            count++;
            startCount = false;
            return;
        }
        for(int i = 0; i < 3; i++){
            int nx = x + dx[i];
            int ny = y + dy[i];
            if(inRange(nx, ny) && !visited[nx][ny] && arr[nx][ny] == '.'){
                if(startCount){
                    visited[nx][ny] = true;
                    dfs(nx, ny);
                }
            }
        }
    }
    static boolean inRange(int x, int y){
        return 0 <= x && x < n && 0 <= y && y < m;
    }
}
```