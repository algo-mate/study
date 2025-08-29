## 창용마을무리의개수([SWEA](https://swexpertacademy.com/main/code/problem/problemDetail.do?contestProbId=AWvGMxG6AccDFAW4) 7465)

## 풀이

서로소 집합(Union-Find) 알고리즘을 사용하여 해결합니다. 아는 사람들을 같은 집합으로 묶고, 최종적으로 서로 다른 집합의 개수를 세어 무리의 개수를 구합니다. find 함수에서 경로 압축을 사용하여 효율성을 높이고, HashSet을 이용해 루트 노드의 개수를 세어 무리의 개수를 계산합니다.

## 해답

```java
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.util.*;
/*
    7465. 창용 마을 무리의 개수
    메모리: 28,160 kb
    실행시간: 97 ms
    아이디어: 서로소 집합
            1. union find로 해결
 */

public class Solution_7465_강보승 {
    static int T, n, m;
    static int[] uf;

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
        StringBuilder sb = new StringBuilder();
        T = Integer.parseInt(br.readLine());
        for (int t = 1; t <= T; t++) {
            StringTokenizer st = new StringTokenizer(br.readLine());
            n = Integer.parseInt(st.nextToken());
            m = Integer.parseInt(st.nextToken());
            uf = new int[n + 1];
            for (int i = 1; i <= n; i++) {
                uf[i] = i;
            }
            sb.append("#").append(t).append(" ");
            for (int i = 1; i <= m; i++) {
                st = new StringTokenizer(br.readLine());
                int a =  Integer.parseInt(st.nextToken());
                int b = Integer.parseInt(st.nextToken());
                union(a, b);
            }
            Set<Integer> set = new HashSet<>();
            for (int i = 1; i <= n; i++) {
                set.add(find(uf[i]));
            }
            sb.append(set.size()).append("\n");
        }
        bw.write(sb.toString());
        bw.close();
    }
    static void union(int x, int y){
        int X = find(x);
        int Y = find(y);
        if(X < Y){
            uf[Y] = X;
        }else{
            uf[X] = Y;
        }
    }
    static int find(int x){
        if(uf[x] == x){
            return x;
        }
        uf[x] = find(uf[x]);
        return uf[x];
    }
}
```