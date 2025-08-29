## 특이한자석([SWEA](https://swexpertacademy.com/main/code/problem/problemDetail.do?contestProbId=AWIeV9sKkcoDFAVH) 4013)

## 풀이

Deque(LinkedList)를 활용한 자석 회전 시뮬레이션 문제입니다.

4개의 자석이 인접한 자석의 극성에 따라 연쇄적으로 회전하는 규칙을 구현합니다.

핵심 아이디어:
1. 각 자석을 LinkedList로 표현하여 회전을 효율적으로 처리
2. 회전 전파 로직: 인접한 자석의 접촉면 극성이 다르면 반대 방향 회전
3. 모든 회전 방향을 미리 결정한 후 동시에 실행
4. 시계방향(1): removeLast() → addFirst(), 반시계방향(-1): removeFirst() → addLast()

회전 전파는 양방향으로 진행되며, 극성이 같은 면을 만나면 전파가 중단됩니다.

## 해답

```java
package com.example.algorithm.test;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.LinkedList;
import java.util.StringTokenizer;
/*
    4013. 특이한 자석
    메모리: 26,752 kb
    실행시간: 93ms
    아이디어: Deque(LinkedList), 시뮬레이션
 */
public class Solution_4013_강보승 {
    static int K;
    static LinkedList<Integer>[] magnets;

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int T = Integer.parseInt(br.readLine());
        for (int t = 1; t <= T; t++) {
            K = Integer.parseInt(br.readLine());

            magnets = new LinkedList[5];
            for (int i = 1; i <= 4; i++) {
                magnets[i] = new LinkedList<>();
                StringTokenizer st = new StringTokenizer(br.readLine());
                for (int j = 0; j < 8; j++) {
                    magnets[i].add(Integer.parseInt(st.nextToken()));
                }
            }

            for (int i = 0; i < K; i++) {
                StringTokenizer st = new StringTokenizer(br.readLine());
                int magnetNum = Integer.parseInt(st.nextToken());
                int direction = Integer.parseInt(st.nextToken());
                processRotation(magnetNum, direction);
            }

            int score = calculateFinalScore();
            System.out.println("#" + t + " " + score);
        }
    }

    static void processRotation(int startMagnet, int startDir) {
        int[] rotationDirections = new int[5];
        rotationDirections[startMagnet] = startDir;
        // 왼쪽 전파
        for (int i = startMagnet; i > 1; i--) {
            if (!magnets[i].get(6).equals(magnets[i - 1].get(2))) {
                rotationDirections[i - 1] = -rotationDirections[i];
            } else {
                break;
            }
        }
        // 오른쪽 전파
        for (int i = startMagnet; i < 4; i++) {
            if (!magnets[i].get(2).equals(magnets[i + 1].get(6))) {
                rotationDirections[i + 1] = -rotationDirections[i];
            } else {
                break;
            }
        }
        // 방향에 맞게 회전
        for (int i = 1; i <= 4; i++) {
            if (rotationDirections[i] == 1) {
                magnets[i].addFirst(magnets[i].removeLast());
            } else if (rotationDirections[i] == -1) {
                magnets[i].addLast(magnets[i].removeFirst());
            }
        }
    }
    static int calculateFinalScore() {
        int totalScore = 0;
        for (int i = 1; i <= 4; i++) {
            if (magnets[i].get(0) == 1) {
                totalScore += (1 << (i - 1));
            }
        }
        return totalScore;
    }
}
```