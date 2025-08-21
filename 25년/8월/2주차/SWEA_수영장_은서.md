## 수영장(SWEA 1952)

## 풀이

모든 월에 대해 일일/월/3개월 이용권 선택 또는 미선택하는 모든 경우를 탐색하는 문제이다.

연간 이용권을 최대 금액으로 잡고, 이후 금액들과 비교하며 최소 금액을 갱신한다.

가지치기를 통해 현재까지의 합이 최소값보다 크거나 같으면 탐색을 중단한다.

## 해답

```java
/* 날짜: 2025.08.14
 * 문제 : Solution_1952. 수영장
 * 난이도 : -
 * 메모리 : 26,496 kb
 * 시간 : 136 ms
 * 아이디어 : 모든 월에 대해 일일/월/3개월 이용권 선택or미선택 모든 경우
 */
public class Swim {

	static int D, M, tM, Y;
	static int[] plan;
	static List<Integer>[] planArr;
	static int minCost;
	public static void main(String[] args) throws IOException {
		
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		
		int T = Integer.parseInt(br.readLine());
		
		for(int t=1; t<=T; t++) {
			StringTokenizer st = new StringTokenizer(br.readLine());
			
			D = Integer.parseInt(st.nextToken());
			M = Integer.parseInt(st.nextToken());
			tM = Integer.parseInt(st.nextToken());
			Y = Integer.parseInt(st.nextToken());
			
			minCost = Y; //연간 이용권을 최대 금액으로 잡고, 이후 금액들과 비교하며 최소 금액 갱신
			
			plan = new int[13];
			
			st = new StringTokenizer(br.readLine());
			
			int sumDay = 0; //총 이용 날짜 구하기 
			int sumMonth = 0; //총 이용 월수 구하기
			
			//값 입력 받으면서 결과 계산
			for(int i=1; i<=12; i++) { 
				plan[i] = Integer.parseInt(st.nextToken());
				if(plan[i] != 0) { //해당 월에 이용 날짜가 있는 경우 
					sumDay += plan[i];
					sumMonth++;				
				}
			}
			
			//최소값 갱신
			minCost = Math.min(minCost, Math.min(sumDay*D, sumMonth*M));
			
			planArr = new ArrayList[sumMonth];
			//일일과 월간 이용권의 조합은 함수로 따로 구현
			dayMonthComb(0, 0);
			
			System.out.printf("#%d %d", t, minCost);
			System.out.println();
		}
		
	}
	
	
	static void dayMonthComb(int depth, int tempSum) {
		//가지치기
		if(tempSum >= minCost) {
			return;
		}
		
		//모든 월을 탐색했을 때
		if(depth == 12) {
			minCost = tempSum;
			return;
		}
		
		dayMonthComb(depth+1, tempSum+plan[depth+1]*D);
		dayMonthComb(depth+1, (plan[depth+1]!=0)?tempSum+M:tempSum);
		
		//완전 탐색이기 때문에 3개월 구간 내 이용일 체크하지 않아도 됨
		dayMonthComb((depth+3>=12)?12:depth+3, tempSum+tM);
		
	}

}
```