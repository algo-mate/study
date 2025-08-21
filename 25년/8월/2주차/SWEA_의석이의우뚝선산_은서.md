## 의석이의 우뚝 선 산(SWEA 4796)

## 풀이

각 봉우리별로 왼쪽 오르막길 길이, 오른쪽 내리막길 길이를 저장하는 문제이다.

이후 값이 존재할 때, 각 봉우리별로 저장한 (오르막길 X 내리막길)을 계산하여 구간의 수를 구한다.

왜 곱하는가? 꼭대기를 기준으로, 오르막길/내리막길 배열에서 선택할 수 있는 구간 = 각 배열 길이이기 때문이다.

## 해답

```java
/* 날짜: 2025.08.13
 * 문제 : Solution_4796. 의석이의 우뚝 선 산
 * 난이도 : D4
 * 메모리 : 101,548 kb
 * 시간 : 623 ms
 * 아이디어 : 각 봉우리 별로 왼쪽 오르막길 길이, 오른쪽 내리막길 길이를 저장한다. 
 * 		이후 값이 존재할 때에, 각 봉우리 별로 저장한 (오르막길 X 내리막길)을 계산하여 구간의 수를 구한다.
 * 		왜 곱하나? -> 꼭대기를 기준으로, 오르/내리막길 배열에서 선택할 수 있는 구간 = 각 배열 길이 
 * 				(ex. 1 2 5 4 3 -> 꼭대기 5 기준, 오르막 배열 =  [1, 2] -> 선택할 수 있는 오르막 구간 = [2], [1, 2])
 *											내리막 배열 = [4, 3] -> 선택할 수 있는 오르막 구간 = [4], [4, 3])
 */

public class Solution_4796 {
	public static void main(String[] args) throws IOException {
        Scanner sc = new Scanner(System.in);
        
        int T = sc.nextInt();
        
        for(int t=1; t<=T; t++) {
        	int N = sc.nextInt();
    		int[] mountain = new int[N];
    		
    		for(int i=0; i<N; i++) {
    			mountain[i] = sc.nextInt();
    		}
   
    		//현재 내 위치의 봉우리 기준, 내 왼쪽에 있는 오르막길 길이는?
    		int[] left = new int[N];
    		left[0] = 0; //맨 처음 봉우리의 왼쪽에는 아무도 없어 => 오르막길 없음
    		for(int i=1; i<N; i++) {
    			if(mountain[i-1] < mountain[i]) { //오른쪽에 위치한 봉우리가 더 높을 때만 left에 넣기(오르막길)
    				left[i] = left[i-1] + 1;
    			}else {
    				left[i] = 0; //더 낮을 때 = 0
    			}
    		}
    		
    		//현재 내 위치의 봉우리 기준, 내 오른쪽에 있는 내리막길 길이는?
    		int[] right = new int[N];
    		right[N-1] = 0; //맨 마지막 봉우리의 오른쪽에는 아무도 없어 => 내리막길 없음
    		for (int i = N-2; i >= 0; i--) {
    			if(mountain[i] > mountain[i+1]) { //오른쪽에 위치한 봉우리가 더 낮을 때만 right에 넣기(내리막길)
    				right[i] = right[i+1] + 1;
    			}else {
    				right[i] = 0; //더 낮을 때 = 0
    			}
    		}
    		
    		long sumCnt = 0L;
    		
    		for (int i = 1; i < N-1; i++) {
    			if (left[i]>0 && right[i]>0) {
    				sumCnt += (long) left[i]* (long) right[i];
    			}
    		}
    		
    		System.out.println("#" + t+ " " + sumCnt);
        }
	}
}
```