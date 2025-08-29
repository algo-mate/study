## 서로소 집합([SWEA](https://swexpertacademy.com/main/code/problem/problemDetail.do?contestProbId=AWUYFsQqrqYDFAQD) 3289)

## 풀이

서로소 집합 문제를 Union-Find 자료구조로 해결했습니다.
- Union-Find 배열을 생성하여 각 원소의 부모를 관리
- union 연산으로 두 집합을 합치고, find 연산으로 루트를 찾음
- 경로 압축 기법을 사용하여 효율성 향상
- HashSet을 사용해 서로 다른 집합의 개수를 계산

## 해답

```java
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.util.*;
/*
    3289. 서로소 집합
    메모리: 108,864 kb
    실행시간: 383 ms
    아이디어: 서로소 집합
            1. union find로 해결
 */
public class Solution_3289_강보승 {
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