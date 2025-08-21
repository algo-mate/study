## 햄버거 다이어트(SWEA 5215)

## 풀이

Next Permutation을 활용한 조합 구현 문제이다.

뽑을 재료를 1로 표현하고, Next Permutation 알고리즘을 사용하여 모든 조합을 생성한다.

## 해답

```java
/*
5215. 햄버거 다이어트
메모리: 28,044 kb
실행시간: 148 ms
아이디어: Next Permutation을 활용한 조합 구현
*/
public class Solution_5215_강보승 {
    static int n, maxCal, maxScore;
    static int[][] ingredients;
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int T = Integer.parseInt(br.readLine());
        for (int t = 1; t <= T; t++) {
            StringTokenizer st = new StringTokenizer(br.readLine());
            n = Integer.parseInt(st.nextToken());
            maxCal = Integer.parseInt(st.nextToken());
            ingredients = new int[n][2];
            for (int i = 0; i < n; i++) {
                st = new StringTokenizer(br.readLine());
                ingredients[i][0] = Integer.parseInt(st.nextToken());
                ingredients[i][1] = Integer.parseInt(st.nextToken());
            }
            maxScore = 0;
            for (int i = 1; i <= n; i++) {
                int[] arr = new int[n];
                // 뽑을 재료는 1로 표현
                for (int j = 0; j < i; j++) {
                    arr[n - 1 - j] = 1;
                }
                do {
                    int currentScore = 0;
                    int currentCal = 0;
                    for (int j = 0; j < n; j++) {
                        if (arr[j] == 1) {
                            currentScore += ingredients[j][0];
                            currentCal += ingredients[j][1];
                        }
                    }
                    if (currentCal <= maxCal) {
                        maxScore = Math.max(maxScore, currentScore);
                    }
                } while (np(arr));
            }
            System.out.println("#" + t + " " + maxScore);
        }
    }

    // Next Permutation 알고리즘
    private static boolean np(int[] p) {
        // 1. 뒤에서부터 오름차순이 깨지는 위치 찾기
        int i = p.length - 1;
        while (i > 0 && p[i - 1] >= p[i]) {
            i--;
        }
        if (i == 0) return false;

        // 2. i-1 위치의 값보다 큰 첫번째 값 찾기
        int j = p.length - 1;
        while (p[i - 1] >= p[j]) {
            j--;
        }

        // 3. 교환하기
        swap(p, i - 1, j);

        // 4. i 위치부터 끝까지를 오름차순으로 정렬
        int k = p.length - 1;
        while (i < k) {
            swap(p, i++, k--);
        }
        return true;
    }

    private static void swap(int[] p, int a, int b) {
        int temp = p[a];
        p[a] = p[b];
        p[b] = temp;
    }
}
```