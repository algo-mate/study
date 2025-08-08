## 수제버거 장인

## 풀이

비트마스킹을 활용해서 부분집합을 전체 탐색했다.

근데 재귀를 활용해서 풀어도 상관없다.

## 해답

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.StringTokenizer;
/*
3421. 수제 버거 장인
메모리: 35,240 kb
실행시간: 387 ms
아이디어: 비트마스킹을 활용한 부분집합 전체 탐색
 */
public class Solution_3421_강보승 {
    static int n, m;
    static int[][] arr;
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int T = Integer.parseInt(br.readLine());
        for(int t = 1; t <= T; t++) {
            StringTokenizer st = new StringTokenizer(br.readLine());
            n = Integer.parseInt(st.nextToken());
            m = Integer.parseInt(st.nextToken());
            arr = new int[m][2];
            for(int i = 0; i < m; i++){
                st = new StringTokenizer(br.readLine());
                arr[i][0] = Integer.parseInt(st.nextToken());
                arr[i][1] = Integer.parseInt(st.nextToken());
            }
            int count = 0;
            for(int i = 0; i < (1 << n); i++){
                boolean countFlag = true;
                for(int j = 0; j < m; j++){
                    // 같은 재료 동시 포함 시 세지 않기(0부터 시작하므로 arr[j][0] - 1 주의)
                    if(((i & (1 << arr[j][0] - 1)) != 0) && ((i & (1 << arr[j][1] - 1)) != 0)){
                        countFlag = false;
                        break;
                    }
                }
                if(countFlag)
                    count++;
            }
            System.out.println("#" + t + " " + count);
        }
    }
}

```