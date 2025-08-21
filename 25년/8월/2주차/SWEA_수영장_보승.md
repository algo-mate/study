## 수영장(SWEA 1952)

## 풀이

완전탐색으로 해결하는 문제이다.

4^12라서 3억 미만이라 가능하다. DP나 그리디 풀이로 풀다가 포기하고 완전탐색으로 해결했다.

각 달마다 일일권, 한달권, 3달권을 선택하는 모든 경우의 수를 탐색한다.

## 해답

```java
/*
    1952. 수영장
    메모리 공간: 25,344 kb
    실행시간: 94ms
    아이디어: 1. 완전탐색
            2. 4^12라서 3억 미만이라 가능
            3. DP? 그리디? 풀이로 풀다가 포기..
*/
public class Solution_1952_강보승 {
    static int T, minCost;
    static int[] costs, plans;
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        T = Integer.parseInt(br.readLine());
        for(int t = 1; t <= T; t++){
            StringTokenizer st = new StringTokenizer(br.readLine());
            costs = new int[4];
            for(int i = 0; i < 4; i++){
                costs[i] = Integer.parseInt(st.nextToken());
            }
            plans = new int[12 + 1];
            st = new StringTokenizer(br.readLine());
            for(int i = 1; i <= 12; i++){
                plans[i] = Integer.parseInt(st.nextToken());
            }
            minCost = Integer.MAX_VALUE;
            choose(1, 0);
            System.out.println("#" + t + " " + minCost);
        }
    }
    static void choose(int depth, int sum){
        if(depth >= 13){
            minCost = Math.min(Math.min(minCost, sum), costs[3]);
            return;
        }
        if(plans[depth] >= 0){
            choose(depth + 1, sum + costs[0] * plans[depth]);
            choose(depth + 1, sum + costs[1]);
            // depth를 N번 건너뜀
            choose(depth + 3, sum + costs[2]);
        }
    }
}
```