## 문제(골드 3) 

[탑 보기](https://www.acmicpc.net/problem/22866)

## 해설

직관적으로 완전탐색이 떠오르지만, N이 10만이라서 O(N) 시간복잡도로 해결해야 하는 문제이다.

이를 위해 스택 자료구조를 활용해야한다. 

하지만 '왜 스택 자료구조를 써야 하는가?'라는 질문에 답할 수 있어야 한다.

스택 자료구조는 `탐색해온 과거 경로에서 쌓아온 데이터` 중 조건이 맞는 경우에 `pop()을 통해 가져오는게` 핵심이다.

이 문제의 경우 `i 주변에서부터 계속 최고 높이를 갱신`하면서 `순차 탐색`해야 하기 때문에 최고 높이를 갱신하는 과정에서 스택 자료구조가 효과적으로 활용된다.

이외에도 배열에 인덱스를 적절히 잘 저장하는 것도 효율성을 위한 포인트이다.

## 풀이

```java
import java.io.*;
import java.util.*;

// 변수: 자신과 거리가 가장 가까운 건물 중 '작은 번호 -> 인덱스'
// 1. N개, 양 옆, 현재 높이 L -> L보다 높은 건물 찾기(같은 높이도 안됨)
// 1-1. 단순하게 순차 탐색은 안되고(자신과 거리가 가까운이므로), i 주변에서 시작해야 함
// 1-2. 이진탐색은 안되고, i 주변에서부터 계속 최고 높이를 갱신하면서 순차 탐색해야 함
// 1-3. 스택의 경우 지나온 과거 경로 데이터랑 순차적 비교하는 방식으로 최적화 가능
// 2. N: 10만, L: 10만 -> O(n)만 가능
public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int n = Integer.parseInt(br.readLine());
        int[] arr = new int[n + 1];
        StringTokenizer st = new StringTokenizer(br.readLine());
        for (int i = 1; i <= n; i++) {
            arr[i] = Integer.parseInt(st.nextToken());
        }

        // 보이는 건물 개수, 가장 가까운 건물의 번호
        int[] count = new int[n + 1];
        int[] closest = new int[n + 1];

        Stack<Integer> stack = new Stack<>();

        // 왼쪽 -> 오른쪽 순회 (i의 왼쪽에 보이는 건물들 계산)
        for (int i = 1; i <= n; i++) {
            // 자신보다 작거나 같은 건물은 스택에서 제거
            while (!stack.isEmpty() && arr[stack.peek()] <= arr[i]) {
                stack.pop();
            }
            // 남아있는 건물 개수를 더함
            count[i] += stack.size();
            // 스택에 남은 건물 중 가장 가까운 것은 top
            if (!stack.isEmpty()) {
                closest[i] = stack.peek();
            }
            stack.push(i);
        }

        stack.clear();

        // 오른쪽 -> 왼쪽 순회 (i의 오른쪽에 보이는 건물들 계산)
        for (int i = n; i >= 1; i--) {
            while (!stack.isEmpty() && arr[stack.peek()] <= arr[i]) {
                stack.pop();
            }
            count[i] += stack.size();
            // 가장 가까운 건물 정보 갱신
            if (!stack.isEmpty()) {
                int rightClosest = stack.peek();
                // 기존에 저장된 왼쪽 건물보다 오른쪽 건물이 더 가깝다면 갱신
                if (closest[i] == 0 || (rightClosest - i) < (i - closest[i])) {
                    closest[i] = rightClosest;
                }
            }
            stack.push(i);
        }

        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
        for (int i = 1; i <= n; i++) {
            if (count[i] == 0) {
                bw.write("0\n");
            } else {
                bw.write(count[i] + " " + closest[i] + "\n");
            }
        }
        bw.flush();
        bw.close();
    }
}

```
