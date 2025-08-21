## 규영이와 인영이의 카드게임(SWEA 6808)

## 풀이

규영이와 인영이의 카드를 초기화한 후 DFS를 통해 인영이 카드의 순열을 구하는 문제이다.

depth가 9일 때까지 DFS를 진행하여 카드 9장 조합이 완성되면 규영이 카드와 승부한다.

승부 결과는 static 전역 변수로 선언하여, 각 케이스마다의 승패 수를 출력한다.

## 해답

```java
//SWEA 6808. 규영이 인영이 카드 게임
//난이도: D3
//메모리 : 30140kb
//실행시간 : 3956ms 
//아이디어: 규영이, 인영이 카드 초기화  후 dfs를 통해 인영이 카드 순열을 구한다.(depth == 9 일 떄까지)
//		카드 9장 조합이 완성 되면 규영이 카드와 승부한다.
//		승부 결과는 static 전역 변수로 선언하여, 각 케이스마다의 승패 수를 출력한다.

public class Swea_6808 {
	private static List<Integer> gyuCard;
	private static List<Integer> inCard;
	private static boolean[] visited;
	private static List<Integer> cardCase;
	private static int winCnt, loseCnt;
	public static void main(String[] args) throws IOException {
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		StringBuilder sb = new StringBuilder();
		
		int T = Integer.parseInt(br.readLine());

		for(int test_case=1; test_case<=T; test_case++){
			StringTokenizer st = new StringTokenizer(br.readLine());
			
			gyuCard = new ArrayList<>();
			inCard = new ArrayList<>();
			cardCase = new ArrayList<>();
			visited = new boolean[9];
			winCnt = 0;
			loseCnt = 0;
			
			for(int i=0; i<9; i++) 
				gyuCard.add(Integer.parseInt(st.nextToken()));
			
			for(int i=1; i<=18; i++) 
				if(!gyuCard.contains(i)) inCard.add(i);
				
			dfs(0);
			
			sb.append("#").append(test_case).append(" ").append(winCnt).append(" ").append(loseCnt).append('\n');

		}
		System.out.println(sb);

	}

	private static void dfs(int depth) {
		
		if(depth == 9) {
			int gyuWin = 0, inWin = 0;
			for(int i=0; i<9; i++) {
				
				if(gyuCard.get(i) > cardCase.get(i)) 
					gyuWin += gyuCard.get(i) + cardCase.get(i) ;				
				else if(gyuCard.get(i) < cardCase.get(i))
					inWin += gyuCard.get(i) + cardCase.get(i) ;
			}
			
			if (gyuWin > inWin) winCnt++;
			else if (gyuWin < inWin) loseCnt++;
			
			return;
		}
		
		for(int i = 0; i < 9; i++) {
			if(!visited[i]) {
				visited[i] = true;
				cardCase.add(inCard.get(i));
				dfs(depth + 1);
				cardCase.remove(cardCase.size() - 1);
				visited[i] = false;
			}
		}
	}
}
```