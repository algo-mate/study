## 구간 합 구하기 4(BOJ 11659)

## 풀이

누적합을 이용하여 구간 합을 효율적으로 구하는 문제이다.

index가 추가될 때마다 값을 합산하여 sum배열에 저장한다.

start와 end가 주어졌을 때 sum[end] - sum[start-1]로 구간 합을 도출한다.

## 해답

```java
/*
Main_11659. 구간합 4
메모리: 54760kb
시간: 560ms
아이디어: index가 추가될 때마다 값 합산 -> sum배열에 저장,
        start와 end 가 주어졌을 때 sum[end] - sum[start-1] 로 구간 합 도출
*/
public class Main {
	static int[] sumArr;
	public static void main(String[] args) throws IOException {
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
		
		StringTokenizer st = new StringTokenizer(br.readLine());
		StringBuilder sb = new StringBuilder();
		
		int N = Integer.parseInt(st.nextToken());
		int M = Integer.parseInt(st.nextToken());
		sumArr = new int[N+1];  // 구간 합 담을 배열
		
		//배열 입력 받기
		st = new StringTokenizer(br.readLine());
		int sum = 0;
		for(int i = 1; i < N+1; i++) {
			sum += Integer.parseInt(st.nextToken());
			sumArr[i] = sum;
		}
		
		//구간 입력 받기 
		for(int i = 0; i<M; i++) {
			st = new StringTokenizer(br.readLine()); //두 개의 수 주어짐
			int startIndex = Integer.parseInt(st.nextToken());
			int endIndex = Integer.parseInt(st.nextToken());
			
			sb.append(sectionSum(startIndex, endIndex)).append("\n");
		}
		
		bw.write(sb.toString());

		br.close();
		bw.close();
		
	}
	
	public static int sectionSum(int startIndex, int endIndex) {
		return sumArr[endIndex] - sumArr[startIndex-1];
	}
}
```