## Ladder1(SWEA 1210)

## 풀이

사다리 타기 문제로 목적지에서 출발점을 찾는 역추적 문제이다.

Solution1은 기준 세로 라인을 먼저 저장하고, 왼쪽이나 오른쪽 인덱스 값이 1일 때 한번에 해당 사다리 컬럼으로 이동하는 방식이다.

Solution2는 왼쪽이나 오른쪽 인덱스 값이 1일 때, 해당 row에서 1이 끝날 때까지 계속 탐색하는 방식이다.

더 직관적이고 단순한 방식인 Solution2가 성능 측면에서 효율적이다.

## 해답

```java
// 2025.08.06
// SWEA 1210. Ladder1
// 난이도: D4
// 메모리 36072kb(solution1) / 32768kb (solution2)
// 실행시간: 173ms / 156ms
// 아이디어: solution1 - 기준 세로 라인(열)을 먼저 저장, 왼or오 인덱스 값이 1일 때 한번에 해당 사다리 컬럼으로 이동하는 방식
//		solution2 - 왼or오 인덱스 값이 1일 때, 해당 row에서 1이 끝날 때까지 계속 탐색하는 방식
//		차이: 1번 방식이 더 효율적이라고 생각했으나, 더 직관적이고 단순한 방식인 2번 방식이 성능 측면에서 효율적임
public class Swea_1210 {
	private static final int SIZE = 100;
	private static int N;
	private static int[][] ladder;

	public static void main(String[] args) throws IOException {
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		
		//총 10번의 케이스가 주어짐
		for(int i=1; i<=10; i++) {
			N = Integer.parseInt(br.readLine());
			ladder = new int[SIZE][SIZE];
			
			// 사다리 입력 받기 
			for(int x=0; x<SIZE; x++) {
				StringTokenizer st = new StringTokenizer(br.readLine());
				for(int y=0; y<SIZE; y++) {
					ladder[x][y] = Integer.parseInt(st.nextToken());
				}
			}
			
			System.out.printf("#%d %d", N, solution2(ladder)); 
			System.out.println();
			
		}

	}
	
	private static int solution2(int[][] ladder) {
		int arrivedCol = -1;
		int arrivedRow = SIZE-1;
		
		for(int c=0; c<SIZE; c++) {
			if(ladder[arrivedRow][c] == 2) {
				arrivedCol = c;
				break;
			}
		}
		
		int x = arrivedRow;
		int y = arrivedCol;

		//방법1. 행 인덱스가 0일 때까지(출발지까지 도달할 때까지) 반복문
		while(x > 0){
			//왼쪽 값 체크
			if(0<=y-1 && y-1<SIZE && ladder[x][y-1] == 1) {
				//왼쪽으로 계속 이동
				while(0 <= y-1 && ladder[x][y-1]==1) {
					y--;
				}

			}
			//오른쪽 값 체크
			else if(0<=y+1 && y+1<SIZE && ladder[x][y+1] == 1) {
				//오른쪽으로 계속 이동
				while(y+1 < SIZE && ladder[x][y+1]==1) {
					y++;
				}
			}
			//양쪽이 다 0이면
			//그냥 위로 올라감(행 인덱스가 1 작아짐)
			x--;
		
		}

		return y;
	}
}
```