## 문제(골드 5)

[탑 보기] (https://www.acmicpc.net/problem/22866)

## 해설


## 정답
```java

import java.io.BufferedReader;
import java.io.*;
import java.util.*;

public class Main {

public static void main(String[] args) throws IOException {
    // TODO Auto-generated method stub
    
    BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
    
    int N = Integer.parseInt(br.readLine());
    int[] height = new int[N];
    
    StringTokenizer st = new StringTokenizer(br.readLine());
    int i = 0;
    
    while(st.hasMoreTokens()) {
        height[i++] = Integer.parseInt(st.nextToken());
    }
    
    //int배열을 인자로 갖는 Stack 초기화
    Stack<int[]> stk = new Stack<>();
    
    //가장 가까운 인덱스와의 거리 저장
    int[] diff = new int[N];
    //볼 수 있는 건물 개수
    int[] cnt = new int[N];
    int[] ans = new int[N];
    //왼쪽 건물 탐색
    for(int k=0; k<N; k++) {
        while(stk.size()>0) {
            //스택의 top에 담겨있는 인자 temp에 임시 저장
            int[] temp = stk.peek();
            //현재 높이보다 높을 때
            if(temp[1] > height[k]) { 
            	diff[k] = k-temp[0];
            	ans[k] = temp[0];
                break;
            } else {
                stk.pop();
            }
        }

        cnt[k] = stk.size();
        
        stk.push(new int[] {k, height[k]});
    }
    
    
    Stack<int[]> stk2 = new Stack<>();
    
    //가장 가까운 인덱스 저장
    int[] ans2 = new int[N];
    //볼 수 있는 건물 개수
    
    //오른쪽 건물 탐색
    for(int k=N-1; k>=0; k--) {
        while(stk2.size()>0) {
            //스택의 top에 담겨있는 인자 temp에 임시 저장
            int[] temp2 = stk2.peek();
            //max 높이보다 높을 때
            if(temp2[1] > height[k]) { 
            	if(diff[k] == 0 || diff[k] > temp2[0]-k) {
            		diff[k] = temp2[0]-k;
            		ans[k] = temp2[0];
            	}
                break;
            } else {
                stk2.pop();
            }
        }
//        System.out.print(k+" "+diff[k]);
//        System.out.println();
        cnt[k] += stk2.size();
        //stk이 비어있거나 while 빠져나오면 해당 순서의 (인덱스와 높이) stk에 push
        stk2.push(new int[] {k, height[k]});
    }
    
    StringBuilder sb = new StringBuilder();
    
    for(int s=0; s<N; s++) {
    	if (cnt[s] != 0) {
    		System.out.print(cnt[s]+" "+(ans[s]+1));
    	}else {
    		System.out.print(0);
    	}
    	System.out.println();
    	
    	
    }
    

}

}
```