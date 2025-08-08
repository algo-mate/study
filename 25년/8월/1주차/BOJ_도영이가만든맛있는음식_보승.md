## 도영이가 만든 맛있는 음식(실버 2)

## 풀이

부분 집합을 완전탐색하면 간단하게 해결할 수 있는 문제이다.

보통 depth, 합 정보가 파라미터로 사용된다는 점을 기억하면 된다.

## 해답

```java
/*
2961. 도영이가 만든 맛있는 음식
    메모리: 14288kb
    시간: 116ms
    아이디어: 모든 경우의 수 따져주는 부분집합 탐색
*/
public class Main_2961_강보승 {
    static int answer = Integer.MAX_VALUE;
    static int n;
    static int[][] arr;
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        n = Integer.parseInt(br.readLine());
        arr = new int[n][2];
        for(int i = 0; i < n; i++){
            StringTokenizer st = new StringTokenizer(br.readLine());
            arr[i][0] = Integer.parseInt(st.nextToken());
            arr[i][1] = Integer.parseInt(st.nextToken());
        }
        choose(0, 1, 0);
        System.out.println(answer);
    }
    static void choose(int depth, int sourTaste, int bitterTaste){
        if(depth == n){
            // 재료 선택 안된 경우는 제외
            if(bitterTaste != 0){
                answer = Math.min(Math.abs(sourTaste - bitterTaste), answer);
            }
            return;
        }
        // 선택 안하고 depth + 1 증가
        choose(depth + 1, sourTaste, bitterTaste);

        // 선택 후 depth + 1 증가
        choose(depth + 1, sourTaste * arr[depth][0], bitterTaste + arr[depth][1]);
    }
}

```