## 문제(골드5) 

[배열돌리기1](https://jungol.co.kr/problem/1338?cursor=NiwwLDEw)

## 해설

정교한 배열 인덱싱이 중요한 문제이다.
layer의 수는 Math(N,M)/2 



## 정답
```java
import java.io.*;
import java.util.*;

public class Main{
    public static void main(String[] args)throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());
        int N = Integer.parseInt(st.nextToken());
        int M = Integer.parseInt(st.nextToken());
        int R = Integer.parseInt(st.nextToken());
        
        int [][] arr = new int[N][M];
        for(int i = 0 ; i < N; i++){
            StringTokenizer st1 = new StringTokenizer(br.readLine());
            for(int j = 0; j < M; j++){
                arr[i][j] = Integer.parseInt(st1.nextToken());
            }
        }
        
        int layer = Math.min(N,M)/2;
        for(int i = 0; i < R; i++){
            for(int j = 0; j < layer; j++){
                int save = arr[j][j];
                for(int k = j + 1; k < M - j; k++){
                    arr[j][k-1] = arr[j][k];
                }
                for(int k = j + 1; k < N - j; k++){
                    arr[k-1][M-1-j] = arr[k][M-1-j];
                }
                for(int k = M-2-j; k >= j; k-- ){
                    arr[N-1-j][k+1] = arr[N-1-j][k];
                }
                for(int k = N-2-j; k >= j ; k--){
                    arr[k+1][j] = arr[k][j];
                }
                arr[j+1][j] = save;
            }
            
        }
        
        for(int[] row : arr){
            for(int e : row){
                System.out.print(e+" ");
            }
            System.out.println();
        }
        
    }
    
    
}
```