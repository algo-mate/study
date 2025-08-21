## 스택 2([BOJ](https://www.acmicpc.net) 28278)

## 풀이

스택의 기본 연산들을 구현하는 문제입니다.
입출력이 많기 때문에 시간 초과를 방지하기 위해 BufferedReader와 StringBuilder를 사용했습니다.

## 해답

```java
/*
28278. 스택 2
메모리: 246,332KB
시간: 864ms
아이디어: 입출력이 잦아 시간초과가 날 수 있으니 BufferedReader와 StringBuilder사용
*/
package ssafy_algo;

import java.io.*;
import java.util.Stack;
import java.util.*;

public class Main_28278_스택2_남윤서{
    public static void main(String[] args) throws IOException{
    	BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
    	StringBuilder sb = new StringBuilder();
    	Stack<Integer> stack = new Stack<>();
        
        int n = Integer.parseInt(br.readLine());
        for(int i = 0; i < n; i++){
        	StringTokenizer st = new StringTokenizer(br.readLine());
            int m = Integer.parseInt(st.nextToken());
            switch(m){
                case 1:
                    stack.push(Integer.parseInt(st.nextToken()));
                    break;
                case 2:
                    sb.append(stack.isEmpty()? -1: stack.pop()).append("\n");
                    break;
                case 3:
                    sb.append(stack.size()).append("\n");
                    break;
                case 4:
                    sb.append(stack.isEmpty()? 1 : 0).append("\n");
                    break;
                case 5:
                    sb.append(stack.isEmpty()? -1 : stack.peek()).append("\n");
            }
        }
        
        System.out.println(sb);
        
    }
}
```