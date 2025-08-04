## 문제(브론즈 3)

[문자삼각형1](https://jungol.co.kr/problem/1338?cursor=NiwwLDEw)

## 해설

문자 저장이 핵심 아이디어인 문제이다.

이외에는 인덱스 변수와 문자 인덱스 변수만 잘 분리하면 크게 어렵지 않은 문제였다!

## 정답

```java
import java.util.*;
public class Main {
    static int n;
    public static void main(String[] args){
        Scanner sc = new Scanner(System.in);
        n = sc.nextInt();
        char[][] arr = new char[n][n];
        int charIdx = 0;
        for(int i = 0; i < n; i++){
        	for(int j = 0; j < n; j++) {
        		arr[i][j] = ' ';
        	}
        }
        for(int i = 0; i < n; i++){
            int x = i; // + 1
            int y = n - 1; // - 1
            int idx = 0;
            while(true){
                int nx = x + idx * 1;
                int ny = y + idx * -1;
                if(inRange(nx, ny)){
                    arr[nx][ny] = (char)('A' + (charIdx) % 26);
                    charIdx += 1;
                    idx += 1;
                }else{
                    break;
                }
            }
        }

        for(int i = 0; i < n; i++){
            for(int j = 0; j < n; j++){
                System.out.print(String.valueOf(arr[i][j]) + " ");
            }
            System.out.println();
        }
    }
    static boolean inRange(int x, int y){
        return 0 <= x && x < n && 0 <= y && y < n;
    }
}

```
