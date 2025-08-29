## 활주로건설([SWEA](https://swexpertacademy.com/main/code/problem/problemDetail.do?contestProbId=AWIeRZV6kBUDFAVH) 4014)

## 풀이

시뮬레이션 구현 문제로, 2차원 격자에서 활주로를 건설할 수 있는 행과 열의 개수를 구합니다.

활주로 건설 조건:
- 높이 차이가 1을 초과하면 불가능
- 높이 차이가 1인 경우, 경사로(길이 X)를 설치해야 함
- 경사로는 같은 높이의 연속된 칸에 설치되며, 중복 설치 불가

핵심 아이디어:
1. 2차원 배열을 1차원 배열로 변환하여 처리 (행별, 열별)
2. 각 1차원 배열에서 활주로 건설 가능성 검사
3. 높이 차이에 따른 경사로 설치 가능 여부 판단
4. used 배열로 경사로 중복 설치 방지

## 해답

```java
package com.example.algorithm.test;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

/*
    4014. 활주로 건설
    메모리: 27,136 kb
    실행시간: 93ms
    아이디어: 구현
            1. 2차원 배열 -> 1차원
            2. break -> return (for문 대신 메서드로 분리)
 */

public class Solution_4014_강보승 {
    static int T, n, x, answer;
    static boolean[] used;
    static int[][] graph;
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        T = Integer.parseInt(br.readLine());
        for (int t = 1; t <= T; t++) {
            StringTokenizer st = new StringTokenizer(br.readLine());
            n = Integer.parseInt(st.nextToken());
            x = Integer.parseInt(st.nextToken());
            graph = new int[n][n];
            for (int i = 0; i < n; i++) {
                st = new StringTokenizer(br.readLine());
                for (int j = 0; j < n; j++) {
                    graph[i][j] = Integer.parseInt(st.nextToken());
                }
            }
            answer = 0;
            // 2차원 배열 -> 1차원 배열 변환
            for(int i = 0; i < n; i++){
                if(canMakeAirStrip(graph[i])){
                    answer++;
                }
            }
            for(int i = 0; i < n; i++){
                int[] tmp = new int[n];
                // 2차원 배열 -> 1차원 배열 변환
                for(int j = 0; j < n; j++){
                    tmp[j] = graph[j][i];
                }
                if(canMakeAirStrip(tmp)){
                    answer++;
                }
            }
            System.out.println("#" + t + " " + answer);
        }
    }
    static boolean canMakeAirStrip(int[] arr){
        used = new boolean[n];
        for(int i = 0; i < n - 1; i++){
            int diff = arr[i] - arr[i + 1];
            // return이 true이라고 가정하고, 예외케이스를 false로 반환하거나 continue 사용해서 처리
            if(diff > 1 || diff < -1) return false;
            if(diff == 0) continue;
            if(diff == 1){
                for(int j = i + 1; j <= i + x; j++){
                    if(n <= j || arr[i + 1] != arr[j] || used[j]) return false;
                    used[j] = true;
                }
                // i, i+1, i+2이고 x=2이면 i+2까지 확인되었고, i+3이 다를 수 있으므로 i+2부터 시작
                i += x - 1;
            }else{//diff == -1
                for(int j = i; j > i - x; j--){
                    if(j < 0 || arr[i] != arr[j] || used[j]) return false;
                    used[j] = true;
                }
            }
        }
        return true;
    }
}
```