## 장훈이의 높은 선반(SWEA 1486)

## 풀이

완전탐색(부분집합) 문제이다.

전체 부분 집합을 완전 탐색한 후 조건을 만족하는 경우에만 최솟값을 갱신한다.

## 해답

```java
/*
    1486. 장훈이의 높은 선반
    메모리: 25,216 kb
    실행시간: 83 ms
    아이디어: 완전탐색(부분집합)
*/

public class Solution_1486_강보승 {
    static int n, b, T, answer;
    static int[] arr;
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        T = Integer.parseInt(br.readLine());
        for(int t = 0; t < T; t++){
            StringTokenizer st = new StringTokenizer(br.readLine());
            n = Integer.parseInt(st.nextToken());
            b = Integer.parseInt(st.nextToken());
            arr = new int[n];
            st = new StringTokenizer(br.readLine());
            for(int i = 0; i < n; i++){
                arr[i] = Integer.parseInt(st.nextToken());
            }
            answer = Integer.MAX_VALUE;
            choose(0, 0);
            System.out.println("#" + (t + 1) + " " + answer);
        }
    }
    // 전체 부분 집합을 완전 탐색 후 조건 만족시 갱신
    static void choose(int depth, int sum){
        if(depth == n){
            if(sum >= b)
                answer = Math.min(answer, sum - b);
            return;
        }
        choose(depth + 1, sum);
        choose(depth + 1, sum + arr[depth]);
    }
}
```