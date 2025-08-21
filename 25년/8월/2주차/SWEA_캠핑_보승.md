## 의석이의 우뚝 선 산(SWEA 4796)

## 풀이

투포인터 알고리즘을 사용하는 문제이다.

먼저 우뚝 선 산이 될 수 있는 후보를 찾고, 우뚝 선 산의 인덱스 주변을 기준으로 시작점과 끝점을 탐색한다.

시작점 * 끝점을 통해 구간 개수를 도출한다.

## 해답

```java
/* 
 * 4796. 의석이의 우뚝 선 산
 * 실행시간: 860 ms
 * 메모리: 103,592kb
 * 아이디어: 투포인터
 *       1. 먼저  우뚝 선 산이 될 수 있는 후보 찾기
 *       2. 우뚝 선 산의 인덱스 주변을 기준으로 시작 점과 끝점 탐색
 *       3. 시작점 * 끝점을 통해 구간 개수 도출!
 */
public class Solution_4796_강보승 {
	static int n;
	static int[] heights;
	public static void main(String[] args) throws IOException {
		Scanner sc = new Scanner(System.in);
		int T = sc.nextInt();
		for (int t = 1; t <= T; t++) {
			n = Integer.parseInt(sc.next());
			heights = new int[n + 1];
			for(int i = 1; i <= n; i++) {
				heights[i] = Integer.parseInt(sc.next());
			}
			int count = 0;
			int m = 0;
			
			for(int i = 1 + 1;  i <= n - 1; i++) {
				// 1. 먼저  우뚝 선 산이 될 수 있는 후보 찾기
				if(heights[i - 1] < heights[i] && heights[i] > heights[i + 1]) {
					m = i;
					int left = m - 1;
					int right = m + 1;
					int leftCount = 0;
					int rightCount = 0;

					 // 2. 우뚝 선 산의 인덱스 주변을 기준으로 시작 점과 끝점 탐색(투포인터)
					while(left >= 2) {
						if(heights[left - 1] < heights[left]) {
							left -= 1;
							leftCount++;
						}else 
							break;
					}
					while(right <= n - 1) {
						if(heights[right] > heights[right + 1]) {
							right += 1;
							rightCount++;
						}else
							break;
					}
					// 3. 시작점 * 끝점을 통해 구간 개수 도출!
					count += (leftCount + 1) * (rightCount + 1);
				}
			}
			System.out.println("#" + t + " " + count);
		}
	}
}
```