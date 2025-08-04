## 문제(실버 2)

[참외밭](https://www.acmicpc.net/problem/2477)

## 해설



## 정답

```java
public class BOJ_2477_Main {

	public static void main(String[] args) throws IOException {
		BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(System.in));

		int K = Integer.parseInt(bufferedReader.readLine());

		int[] directions = new int[6];
        int[] lengths = new int[6];

        for (int i = 0; i < 6; i++) {
            StringTokenizer st = new StringTokenizer(bufferedReader.readLine());
            directions[i] = Integer.parseInt(st.nextToken());
            lengths[i] = Integer.parseInt(st.nextToken());
        }

        int maxHorizIndex = -1;
        int maxVertIndex = -1;
        int maxHorizLength = 0;
        int maxVertLength = 0;


        for (int i = 0; i < 6; i++) {
            int dir = directions[i];
            int len = lengths[i];
            if ((dir == 1 || dir == 2) && len > maxHorizLength) {
                maxHorizLength = len;
                maxHorizIndex = i;
            }
            if ((dir == 3 || dir == 4) && len > maxVertLength) {
                maxVertLength = len;
                maxVertIndex = i;
            }
        }

        int bigRectArea = maxHorizLength * maxVertLength;

        int smallHorizLength = lengths[(maxVertIndex + 3) % 6];
        int smallVertLength  = lengths[(maxHorizIndex + 3) % 6];

        int smallRectArea = smallHorizLength * smallVertLength;

        int melonArea = bigRectArea - smallRectArea;
        int totalMelons = melonArea * K;

        System.out.println(totalMelons);
	}

}

```