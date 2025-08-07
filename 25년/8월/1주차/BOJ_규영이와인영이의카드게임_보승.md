## 규영이와 인영이의 카드게임(실버 1)

## 풀이

순열을 dfs로 구현해서 해결한 문제이다. 

재귀함수는 각 단계마다 필요한 depth, 규영이의 합, 인영이의 합 사용해서 계산 
로직을 단순하게 만들었다.

리스트 대신 배열을 사용했으면 아마 더 빨랐을지도..

## 해답

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.ArrayList;
import java.util.List;
import java.util.StringTokenizer;

/*
6808. 규영이와 인영이의 카드게임
메모리: 28,964 kb
실행시간: 2,507 ms
아이디어: 순열, dfs 완전탐색
 */
public class Solution_6808_강보승 {
    static int n, count1, count2;
    static boolean[] visited;
    static List<Integer> numList, choosedGyuYoung, choosedInYoung;
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int T = Integer.parseInt(br.readLine());
        for (int t = 1; t <= T; t++) {
            StringTokenizer st = new StringTokenizer(br.readLine());
            choosedGyuYoung = new ArrayList<>();
            choosedInYoung = new ArrayList<>();
            visited = new boolean[9];
            for(int i = 0; i < 9; i++){
                choosedGyuYoung.add(Integer.parseInt(st.nextToken()));
            }
            // 규영이가 선택 안한거 인영이가 선택
            for(int i = 1; i <= 18; i++){
                if(!choosedGyuYoung.contains(i)){
                    choosedInYoung.add(i);
                }
            }
            count1 = count2 = 0;
            choose(0, 0, 0);
            System.out.println("#" + t + " " + count1 + " " + count2);
        }
    }
    static void choose(int depth, int gyuYoungSum, int inYoungSum){
        if(depth == 9){
            if(gyuYoungSum > inYoungSum)
                count1++;
            else if(gyuYoungSum < inYoungSum)
                count2++;
            return;
        }
        // i가 0~9까지 인영이는 순열 조합으로 선택, 이때 재귀적으로 합까지 표현해서 동시에 해결
        for(int i = 0; i < 9; i++){
            if(!visited[i]){
                visited[i] = true;
                if(choosedGyuYoung.get(depth) > choosedInYoung.get(i)){
                    choose(depth + 1, gyuYoungSum + choosedInYoung.get(i) + choosedGyuYoung.get(depth), inYoungSum);
                }else if(choosedGyuYoung.get(depth) < choosedInYoung.get(i)){
                    choose(depth + 1, gyuYoungSum, inYoungSum + choosedInYoung.get(i) + choosedGyuYoung.get(depth));
                }
                visited[i] = false;
            }
        }
    }
}

```