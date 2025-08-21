## 배열 탐색 실습(Array_10)

## 풀이

배열의 다양한 탐색 방법을 구현하는 실습 문제이다.

행우선 탐색, 열우선 탐색, 지그재그 탐색, 달팽이 탐색 총 4가지 방법으로 2차원 배열을 탐색한다.

달팽이 탐색에서는 방향 배열과 방문 체크를 활용하여 시계방향으로 탐색한다.

## 해답

```java
public class Array_10 {
	
	private static int R = 4, C= 5;
	private static char[][] map ;
	private static StringBuilder sb = new StringBuilder();
	
	//map 초기화
	private static void setup() {
		map = new char[R][C];
		
		char alpha = 'A';
		for(int i=0; i<R; i++) {
			for(int j=0; j<C; j++) {
				map[i][j] = alpha;
				alpha++;
			}
		}
	}
	
	public static void main(String[] args) {
		setup();
		rowFirst();
		colFirst();
		zigzag();
		snail();
	}
	//행우선 탐색
	private static void rowFirst() {
		for(int i=0; i<R; i++) {
			for(int j=0; j<C; j++) {
				sb.append(map[i][j]);
			}
			
		}
		System.out.println(sb.toString().equals("ABCDEFGHIJKLMNOPQRST"));
	}
	//열우선 탐색
	private static void colFirst() {
		for(int j=0; j<C; j++) {
			for(int i=0; i<R; i++) {
				sb.append(map[i][j]);
			}
			
		}
		System.out.println(sb.toString().equals("ABCDEFGHIJKLMNOPQRST"));
	}
	//지그재그 탐색
	private static void zigzag() {
		for(int i=0; i<R; i++) {
			if(i%2 == 0) {
				for(int j=0; j<C; j++) {
					sb.append(map[i][j]);
				}
			}else {
				for(int j=C-1; j>=0; j--) {
					sb.append(map[i][j]);
				}
			}
			
		}
		System.out.println(sb.toString().equals("ABCDEFGHIJKLMNOPQRST"));
	}
	//달팽이 탐색 
	private static void snail() {
        //우 -> 하 -> 좌 -> 상
		int[] dx = {0, 1, 0, -1};
		int[] dy = {1, 0, -1, 0};
		int x = 0, y = 0;

        // 방문 표시 map
		boolean[][] visited = new boolean[R][C];

		int d=0;
		
		for(int i = 0; i<R*C; i++) {
			sb.append(map[x][y]);
			visited[x][y] = true;
			
			int nx = x+dx[d];
			int ny = y+dy[d];
			
            //방문 여부와 인덱스 범위 체크
			if (nx < 0 || nx >= R || ny < 0 || ny >= C || visited[nx][ny]) {
				//방문했거나 인덱스를 벗어나면 방향 바꾸기
                d = (d+1)%4;
				x = x+dx[d];
				y = y+dy[d];
				continue;
			}
			
			x = nx;
			y = ny;
		}
		
		System.out.println(sb.toString().equals("ABCDEFGHIJKLMNOPQRST"));
		
	}

}
```