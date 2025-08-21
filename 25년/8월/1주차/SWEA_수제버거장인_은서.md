## 수제 버거 장인(SWEA 3421)

## 풀이

비트 마스크를 이용하여 모든 재료의 조합을 탐색하는 문제이다.

같은 버거에 들어가면 안 되는 재료의 관계를 List로 구현한다.

주어진 N개 재료의 모든 비트 마스크를 순회하면서 함께 포함되어서는 안 되는 조합을 제외하고 카운트한다.

## 해답

```java
/* 3421. 수제 버거 장인
 * 메모리 : 36572 kb
 * 시간 : 568 ms
 * 아이디어 :
 *   1. 같은 버거에 들어가면 안 되는 재료의 관계 => set으로 구현(같은 쌍이 주어질 수 있으므로) 
 *   => (변경) 배열로 받으면, 객체의 참조값을 비교하기 때문에 set의 기본 특성인 중복 제거가 안됨. 재정의 해주어야 함. 
 *   => 현재 주어진 변수의 범위로는 List로 충분하기 때문에 재정의 X, 범위가 커져 중복 제거가 반드시 필요하면 재정의 
 *   
 *   2. 주어진 N개 재료의 모든 비트 마스크를 순회한다.
 *   
 *   3. 함께 포함되서는 안 되는 조합을 제외하고 count 한다.
 */
public class Solution_3421 {

	public static void main(String[] args) throws IOException {
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
		StringBuilder sb = new StringBuilder();
		
		int T = Integer.parseInt(br.readLine());
		
		for(int t = 1; t <= T; t++) {
			// N, M 입력 받기 (N: 재료의 수, M: 불가능 조합 개수)
			StringTokenizer st = new StringTokenizer(br.readLine());
			
			int N = Integer.parseInt(st.nextToken());
			int M = Integer.parseInt(st.nextToken());
			
			List<int[]> unlikedComb = new ArrayList<>();
	
			for(int i = 0; i < M; i++) {
				st = new StringTokenizer(br.readLine());
				
				int element1 = Integer.parseInt(st.nextToken());
				int element2 = Integer.parseInt(st.nextToken());
				unlikedComb.add(new int[] {element1-1, element2-1});
				
			}
			
			//만들 수 있는 조합의 수 카운트 초기화
			int HamburgerCount = 0;

			//재료 수의 부분 집합 수 (2의 N승)
			for(int i = 0; i < (1<<N); i++) {
				//만들 수 있는 조합인지? 판단하는 boolean
				boolean isOkayMake = true;
				
				//주어진 M번의 불가능한 재료 조합을 하나씩 탐색 (현재의 부분 집합이 두 재료(원소)를 모두 포함하고 있는지)
				for(int j = 0; j < M; j++) {
					//불가능 조합의 재료 1, 2를 꺼내서 임시 array 에 저장
					int[] temparr = unlikedComb.get(j);
					
					//동시 포함 여부를 확인하는 핵심 로직: (i & (1 << temparr[0])) != 0 => 현재의 부분 집합이 재료 1을 포함하고 있나?  
					if(((i & (1 << temparr[0])) != 0) && ((i & (1 << temparr[1])) != 0)){
						//포함한다면 break, 카운트 X
						isOkayMake = false;
						break;
					}
					
				}
			
				if(isOkayMake) ++HamburgerCount;
				
			}
			
			sb.append("#").append(t).append(" ").append(HamburgerCount).append("\n");
		}

		System.out.println(sb.toString());
	}
}
```