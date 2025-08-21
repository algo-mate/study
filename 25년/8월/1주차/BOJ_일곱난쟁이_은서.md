## 일곱 난쟁이(BOJ 2309)

## 풀이

크기가 9로 고정되어 있기 때문에, 이중 for문으로 모든 케이스를 확인해도 성능이 충분한 문제이다.

전체 합에서 100을 뺀 값이 제외해야 할 두 난쟁이의 키의 합이므로, 이를 만족하는 두 난쟁이를 찾는다.

시간 복잡도가 낮아서(36번) 이중 for문 단순 완전탐색이 가장 직관적으로 효율적이다.

## 해답

```java
// 2025.08.04
// Main_2309 일곱난쟁이
// 난이도: 브론즈1
// 메모리: 14,220kb
// 시간: 104ms
// 아이디어: 크기가 9로 고정되어 있기 떄문에, 이중 for문으로 모든 케이스를 확인해도 성능 충분. 시간 복잡도 낮음(36번)
//		이럴 경우는 이중 for문 단순 완전탐색이 가장 직관적으로 효율적임(DFS, 백트래킹 등의 알고리즘 활용할 필요 없음)		

public class Main {

	public static void main(String[] args) throws IOException {
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		int[] arr = new int[9];
		int sum = 0;
		
      for (int i = 0; i < 9; i++) {
            arr[i] = Integer.parseInt(br.readLine());
            sum += arr[i];
      }
      
      int diff = sum - 100;
      
      int x = -1, y = -1;
      
      for(int i=0; i<9; i++) {
    	  for(int j=i+1; j<9; j++) {
    		  if(arr[i]+arr[j] == diff) {
    			  x = i;
    			  y = j;
    			  break;
    		  }
    	  }
    	  if(x!=-1) break;
      }
      
      
      List<Integer> resultArray = new ArrayList<>();
      for(int i=0; i<9; i++) {
    	  if(i==x || i==y) continue;
    	  resultArray.add(arr[i]);
      }
      
      //List 정렬 Collections
      Collections.sort(resultArray);
      
      for(int r : resultArray) {
    	  System.out.println(r);
      }
	}
}
```