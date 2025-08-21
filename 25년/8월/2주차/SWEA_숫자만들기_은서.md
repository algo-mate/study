## 숫자 만들기(SWEA 4008)

## 풀이

각 연산자의 개수를 매개변수로 넣고, 백트래킹으로 구현하는 문제이다.

조건은 개수가 0보다 큰지 확인하고, 연산자 개수를 차감하는 것이다.

기저조건에서 depth가 (숫자 배열 크기 - 1), 즉 연산자를 모두 소진했을 때 최대, 최소값을 구한다.

## 해답

```java
/* 
 * 날짜: 2025.08.12
 * 문제 : 4008. 숫자 만들기
 * 난이도 : -
 * 메모리 : 28,652 kb
 * 시간 : 114 ms
 * 아이디어 : 각 연산자의 개수를 매개변수로 넣고, 백트래킹으로 구현(조건: 개수가 0보다 큰지 확인. 연산자 개수 차감)
 * 	
 */

public class Solution_4008 {
	
	static int[] operatorArr;
	static int operatorSum;
	static int[] numberArr;
	static int N, min, max;
	static int PLUS = 0, MINUS = 1, MULTI = 2, DIV = 3;
	
	
	public static void main(String[] args) throws FileNotFoundException, IOException {
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		StringBuilder sb = new StringBuilder();
		
		int T = Integer.parseInt(br.readLine());
		
        for(int t=1; t<=T; t++){
        	min = Integer.MAX_VALUE;
        	max = Integer.MIN_VALUE;
        	
            N = Integer.parseInt(br.readLine().trim());
            StringTokenizer st = new StringTokenizer(br.readLine().trim());;

            operatorArr = new int[4]; // [ +, -, *, / ] 순서 고정
            
           //각 연산자 개수 입력 받기
            for(int i=0; i<4; i++) {
                operatorArr[i] = Integer.parseInt(st.nextToken());
            }
            //슷지 입력 받기
            numberArr = new int[N];
            st = new StringTokenizer(br.readLine().trim());;
            for(int i=0; i<N; i++) {
                numberArr[i] = Integer.parseInt(st.nextToken());
            }

            perm(0,operatorArr[PLUS], operatorArr[MINUS], operatorArr[MULTI], operatorArr[DIV], numberArr[0]);

            System.out.printf("#%d %d%n", t, max-min);
           
        }
		
	}
	
	public static void perm(int depth, int plus, int minus, int multi, int div, int ans) {
		//기저조건: depth가 (숫자 배열 크기 - 1), 즉 연산자를 모두 소진했을 떄 최대, 최소값 구하기 
		if(depth == N-1) {
			min = Math.min(min, ans);
			max = Math.max(max, ans);
			return;
		}
		
		if (plus > 0) 
			perm(depth+1, plus-1, minus, multi, div, ans+numberArr[depth+1]);
		
		if (minus > 0) 
			perm(depth+1, plus, minus-1, multi, div, ans-numberArr[depth+1]);
		
		if (multi > 0) 
			perm(depth+1, plus, minus, multi-1, div, ans*numberArr[depth+1]);
		
		if (div > 0) 
			perm(depth+1, plus, minus, multi, div-1, ans/numberArr[depth+1]);
	
	}

}
```