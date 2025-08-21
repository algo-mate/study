## 구간 합 구하기 4([BOJ](https://www.acmicpc.net) 11659)

## 풀이

이중 for문으로는 시간 초과가 발생하기 때문에 누적합을 미리 계산하는 방식으로 해결했습니다.
입력과 함께 누적합을 계산하여 배열에 저장하고, 구간합 질의에서는 단순히 두 누적합의 차이를 구하여 답을 구합니다.

## 해답

```java
/*
11659. 구간 합 구하기 4
메모리: 58,744KB
시간: 1156ms
아이디어: 이중 for문으로는 시간 초과가 발생해서 합을 입력과 함께 미리 계산하는 방식으로 품.
*/
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

public class Main_11659_장현준 {

	public static void main(String[] args) throws IOException {
		BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(System.in));
		StringTokenizer stringTokenizer = new StringTokenizer(bufferedReader.readLine());
		
		int N = Integer.parseInt(stringTokenizer.nextToken());
		int M = Integer.parseInt(stringTokenizer.nextToken());
		
		stringTokenizer = new StringTokenizer(bufferedReader.readLine());
		int sum = 0;
		int[] numArr = new int[N + 1]; // 문제에서 인덱스를 1번부터 사용해서 크기 +1로 사용.
		for (int i = 1; i <= N; i++) {
			sum += Integer.parseInt(stringTokenizer.nextToken());
			numArr[i] = sum;
		}
		
		for (int i = 0; i < M; i++) {
			stringTokenizer = new StringTokenizer(bufferedReader.readLine());
			
			int start = Integer.parseInt(stringTokenizer.nextToken());
			int end = Integer.parseInt(stringTokenizer.nextToken());
			
			System.out.println(numArr[end] - numArr[start - 1]); // 합 계산 출력
		}
	}

}
```