## 문제(실버 2) 

[배열돌리기1](https://www.acmicpc.net/problem/16926) 

## 해설
구현 과정에서 배열의 행과 열에 어떤 변수를 설정해줄지 까다로운 문제이다.

AI 풀이를 참고하니까 i, j 같은 변수 대신 배열의 시작과 끝을 rStart, rEnd와 같이 명확하게 설정하여 관리하는 방식을 통해 해결했다.

이를 통해 2차원 배열을 1차원 배열로 변환하는 과정이 훨씬 단순해졌다.

회전 횟수 R 변환 중에는 문제에서 주어진 변수를 마음대로 바꿨다가 계속 테스트 케이스가 틀리는 문제가 발생하기도 했다.

R 변환 이후에는 큐를 사용해서 배열에 다시 값을 넣어주었다.


## 풀이

```java
import java.io.*;
import java.util.*;

public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());
        int n = Integer.parseInt(st.nextToken());
        int m = Integer.parseInt(st.nextToken());
        int r = Integer.parseInt(st.nextToken());
        int[][] arr = new int[n][m];
        Queue<Integer> list = new LinkedList<>();
        for(int i = 0; i < n; i++) {
            st = new StringTokenizer(br.readLine());
            for(int j = 0; j < m; j++) {
                arr[i][j] = Integer.parseInt(st.nextToken());
            }
        }

        int layer = Math.min(n, m) / 2;

        for(int i = 0; i < layer; i++){
            // 2차원 배열을 1차원 배열로 변환하기 위해 시작과 끝을 변수들로 단순화
            int rStart = i;
            int rEnd = n - 1 - i;
            int cStart = i;
            int cEnd = m - 1 - i;
            for(int j = cStart; j < cEnd; j++){
                list.add(arr[rStart][j]);
            }
            for(int j = rStart; j < rEnd; j++){
                list.add(arr[j][cEnd]);
            }
            for(int j = cEnd; j > cStart; j--){
                list.add(arr[rEnd][j]);
            }
            for(int j = rEnd; j > rStart; j--){
                list.add(arr[j][cStart]);
            }
            // 변수 잘못된 재갱신 실수
            // r = (r % ((rEnd - rStart) * 2 + (cEnd - cStart) * 2));
            //int effectiveR = (r % ((rEnd - rStart) * 2 + (cEnd - cStart) * 2));
            int effectiveR = r % list.size();

            for(int j = 0; j < effectiveR; j++){
                int tmp = list.poll();
                list.add(tmp);
            }

            for(int j = cStart; j < cEnd; j++){
                int tmp = list.poll();
                arr[rStart][j] = tmp;
            }
            for(int j = rStart; j < rEnd; j++){
                int tmp = list.poll();
                arr[j][cEnd] = tmp;
            }
            for(int j = cEnd; j > cStart; j--){
                int tmp = list.poll();
                arr[rEnd][j] = tmp;
            }
            for(int j = rEnd; j > rStart; j--){
                int tmp = list.poll();
                arr[j][cStart] = tmp;
            }
        }
        StringBuilder sb = new StringBuilder();
        for(int i = 0; i < n; i++){
            for(int j = 0; j < m; j++){
                sb.append(arr[i][j]);
                sb.append(" ");
            }
            sb.append("\n");
        }
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
        bw.write(sb.toString());
        bw.flush();
        bw.close();
    }
}
```