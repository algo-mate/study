## 무선 충전([SWEA](https://swexpertacademy.com) 5644)

## 풀이

구현, 시뮬레이션 문제입니다.
제일 까다로운 부분은 서로 겹칠 때인데 n은 8이라서 그냥 완전탐색으로 해결하는 게 제일 간단합니다.

## 해답

```java
/*
5644. 무선 충전
메모리: 27,904KB
시간: 100ms
아이디어: 1. 구현, 시뮬레이션
        2. 제일 까다로운 부분은 서로 겹칠 때 -> n은 8이라서 그냥 완전탐색으로 해결하는게 제일 간단
*/
package com.example.algorithm.test;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.ArrayList;
import java.util.List;
import java.util.StringTokenizer;

public class Solution_5644_강보승 {
    static int T, M, A;
    static int[] dx = {0, -1, 0, 1, 0};
    static int[] dy = {0, 0, 1, 0, -1};
    static int[][] chargeSumArr;
    static int[] moveA, moveB;
    static AP[] apArr;

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        T = Integer.parseInt(br.readLine());
        for (int t = 1; t <= T; t++) {
            StringTokenizer st = new StringTokenizer(br.readLine());
            M = Integer.parseInt(st.nextToken());
            A = Integer.parseInt(st.nextToken());
            moveA = new int[M];
            moveB = new int[M];
            chargeSumArr = new int[2][M + 1];
            st = new StringTokenizer(br.readLine());
            for (int j = 0; j < M; j++) {
                moveA[j] = Integer.parseInt(st.nextToken());
            }
            st = new StringTokenizer(br.readLine());
            for (int j = 0; j < M; j++) {
                moveB[j] = Integer.parseInt(st.nextToken());
            }
            apArr = new AP[A];
            for (int i = 0; i < A; i++) {
                st = new StringTokenizer(br.readLine());
                int y = Integer.parseInt(st.nextToken());
                int x = Integer.parseInt(st.nextToken());
                int range = Integer.parseInt(st.nextToken());
                int charge = Integer.parseInt(st.nextToken());
                apArr[i] = new AP(x - 1, y - 1, range, charge);
            }
            simulate();
            int answer = 0;
            for (int j = 0; j <= M; j++) {
                answer += chargeSumArr[0][j] + chargeSumArr[1][j];
            }
            System.out.println("#" + t + " " + answer);
        }
    }

    static void simulate() {
        int aX = 0;
        int aY = 0;
        int bX = 9;
        int bY = 9;
        inBC(aX, aY, bX, bY, 0);
        for (int i = 0; i < M; i++) {
            int dirA = moveA[i];
            int dirB = moveB[i];
            aX += dx[dirA];
            aY += dy[dirA];
            bX += dx[dirB];
            bY += dy[dirB];
            inBC(aX, aY, bX, bY, i + 1);
        }
    }

    static void inBC(int aX, int aY, int bX, int bY, int time) {
        List<Integer> listA = new ArrayList<>();
        List<Integer> listB = new ArrayList<>();

        for (int i = 0; i < A; i++) {
            if (getDistance(aX, aY, apArr[i].x, apArr[i].y, apArr[i].range)) {
                listA.add(i);
            }
            if (getDistance(bX, bY, apArr[i].x, apArr[i].y, apArr[i].range)) {
                listB.add(i);
            }
        }

        int maxChargeA = 0;
        int maxChargeB = 0;

        if (listA.isEmpty() && listB.isEmpty()) {
        } else if (listA.isEmpty()) {
            for (int b_idx : listB) {
                maxChargeB = Math.max(maxChargeB, apArr[b_idx].charge);
            }
        } else if (listB.isEmpty()) {
            for (int a_idx : listA) {
                maxChargeA = Math.max(maxChargeA, apArr[a_idx].charge);
            }
        } else {
            int maxTotalCharge = 0;

            for (int a_idx : listA) {
                for (int b_idx : listB) {
                    int currentChargeA = apArr[a_idx].charge;
                    int currentChargeB = apArr[b_idx].charge;

                    if (a_idx == b_idx) {
                        currentChargeA /= 2;
                        currentChargeB /= 2;
                    }

                    if (currentChargeA + currentChargeB > maxTotalCharge) {
                        maxTotalCharge = currentChargeA + currentChargeB;
                        maxChargeA = currentChargeA;
                        maxChargeB = currentChargeB;
                    }
                }
            }
        }

        chargeSumArr[0][time] = maxChargeA;
        chargeSumArr[1][time] = maxChargeB;
    }

    static boolean getDistance(int x, int y, int apX, int apY, int range) {
        return (Math.abs(x - apX) + Math.abs(y - apY) <= range);
    }
}

class AP {
    int x;
    int y;
    int range;
    int charge;

    AP(int x, int y, int range, int charge) {
        this.x = x;
        this.y = y;
        this.range = range;
        this.charge = charge;
    }
}
```