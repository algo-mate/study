## 숫자 만들기(SWEA 4008)

## 풀이

중복 순열로 특정 개수일 때만 최대, 최소값을 갱신하는 문제이다.

연산자가 주어진 개수 이하일 때만 반복 호출하여 중복순열을 구현한다.

## 해답

```java
/*
 * 4008. 숫자 만들기
 * 메모리: 28492kb
 * 시간: 144 ms
 * 아이디어: 중복 순열로 특정 개수일 때만 최대, 최소 갱신
 */
public class Solution_4008_강보승 {
	static int maxValue = Integer.MIN_VALUE;
	static int minValue = Integer.MAX_VALUE;
	static int n;
	static int[] operators, usedOperators, numArr;
	public static void main(String[] args) throws IOException {
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		int T = Integer.parseInt(br.readLine());
		for(int t = 1; t <= T; t++) {
			n = Integer.parseInt(br.readLine());
			numArr = new int[n];
			operators = new int[4];
			usedOperators = new int[4];
			StringTokenizer st = new StringTokenizer(br.readLine());
			for(int i = 0; i < 4; i++) {
				operators[i] = Integer.parseInt(st.nextToken());
			}
			st = new StringTokenizer(br.readLine());
			for(int i = 0; i < n; i++) {
				numArr[i] = Integer.parseInt(st.nextToken());
			}
			maxValue = Integer.MIN_VALUE;
			minValue = Integer.MAX_VALUE;
			permutations(0, numArr[0]);
			System.out.println("#" + t + " " + (maxValue - minValue));
		}
	}
	// 중복순열
	static void permutations(int depth, int sum) {
		// n - 1일 때만 세기!
		if(depth == n - 1) {
			maxValue = Math.max(maxValue, sum);
			minValue = Math.min(minValue, sum);
		}
		for(int i = 0; i < 4; i++) {
			//연산자가 주어진 개수 이하일 때 반복 호출
			if(usedOperators[i] < operators[i]) {
				usedOperators[i] += 1;
				if(i == 0) permutations(depth + 1, sum + numArr[depth + 1]);
				else if(i == 1) permutations(depth + 1, sum - numArr[depth + 1]);
				else if(i == 2) permutations(depth + 1, sum * numArr[depth + 1]);
				else if(i == 3) permutations(depth + 1, sum / numArr[depth + 1]);				
				usedOperators[i] -= 1;
			}
		}
	}
}
```