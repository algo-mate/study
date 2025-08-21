## 농작물 수확하기(SWEA 2805)

## 풀이

예전에 풀었던 문제라서 그때와 다르게 대칭성을 이용해서 원처럼 거리가 일정 간격 이하일 경우 세는 방식으로 접근했다.

맨해튼 거리를 이용하여 중심에서 일정 거리 이하에 있는 농작물들의 합을 구한다.

## 해답

```java
/*
    2805. 농작물 수확하기
    메모리: 27,648 kb
    실행시간: 92ms
    아이디어: 예전에 풀었던 문제라서 그떄랑 다르게 대칭성을 이용해서 원처럼 거리가 일정 간격 이하일 경우 세는 방식으로 접근
*/

public class Solution_2805_강보승 {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int T = Integer.parseInt(br.readLine());
        for (int t = 1; t <= T; t++) {
            int n = Integer.parseInt(br.readLine());
            int[][] arr = new int[n][n];
            for (int i = 0; i < n; i++) {
                StringTokenizer st = new StringTokenizer(br.readLine());
                String numLine = st.nextToken();
                for (int j = 0; j < n; j++) {
                    arr[i][j] = numLine.charAt(j) - '0';
                }
            }
            int answer = 0;
            for (int i = 0; i < n; i++) {
                for (int j = 0; j < n; j++) {
                    if (Math.abs((n / 2) - i) + Math.abs((n / 2) - j) <= (n / 2)) {
                        answer += arr[i][j];
                    }
                }
            }
            System.out.println("#" + t + " " + answer);
        }
    }
}
```