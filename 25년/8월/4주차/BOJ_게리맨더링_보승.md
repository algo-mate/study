## 게리맨더링([BOJ](https://www.acmicpc.net/problem/17471) 17471)

## 풀이

완전탐색(DFS) + BFS를 활용한 그래프 분할 문제입니다.

선거구를 두 개의 연결된 집합으로 나누어 인구 차이를 최소화하는 문제로, 다음과 같은 단계로 해결합니다:

1. DFS로 가능한 모든 집합 분할 경우의 수 생성 (부분집합)
2. 각 분할에 대해 BFS로 두 집합이 각각 연결되어 있는지 검증
3. 연결된 집합이면 인구 수 차이를 계산하여 최솟값 갱신

핵심 아이디어는 N개 구역을 두 개의 연결된 선거구로 나누는 모든 경우를 완전탐색하면서, 각 경우가 유효한지(모든 구역이 연결되어 있는지) BFS로 확인하는 것입니다.

## 해답

```java
package com.example.algorithm.test;

import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.*;
/*
    17471. 게리맨더링
    메모리: 18424kb
    시간: 152ms
    아이디어: 완전탐색(dfs) + BFS
            1. dfs 탐색으로 집합 선정
            2. bfs 탐색으로 실제 연결된 집합인지 확인
            3. 각 집합 인구 수 계산
 */

public class Main_17471_강보승 {
    static int n, minDiffPeople;
    static int[] peopleCount;
    static boolean[] visited;
    static List<Integer> listA, listB;
    static List<Integer>[] nodes;
    public static void main(String[] args) throws Exception {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        n = Integer.parseInt(br.readLine());
        StringTokenizer st;
        peopleCount = new int[n + 1];
        nodes = new ArrayList[n + 1];
        for(int i = 1; i <= n; i++){
            nodes[i] = new ArrayList<>();
        }
        st = new StringTokenizer(br.readLine());
        for(int i = 1; i <= n; i++){
            peopleCount[i] = Integer.parseInt(st.nextToken());
        }
        for(int x = 1; x <= n; x++){
            st = new StringTokenizer(br.readLine());
            int m = Integer.parseInt(st.nextToken());
            for(int j = 0; j < m; j++){
                int y = Integer.parseInt(st.nextToken());
                nodes[x].add(y);
                nodes[y].add(x);
            }
        }
        minDiffPeople = Integer.MAX_VALUE;
        listA = new ArrayList<>();
        listB = new ArrayList<>();
        dfs(1);
        System.out.println((minDiffPeople == Integer.MAX_VALUE) ? -1 : minDiffPeople);
    }
    static void dfs(int depth){
        if(depth > n){
            if(listA.isEmpty() || listA.size() == n){
                return;
            }
            if(isConnected()){
                calculate();
            }
            return;
        }
        dfs(depth + 1);

        listA.add(depth);
        dfs(depth + 1);
        listA.remove(listA.size() - 1);
    }
    static boolean isConnected(){
        if(!bfs(listA.get(0), listA))
            return false;
        listB = new ArrayList<>();
        for(int i = 1; i <= n; i++){
            if(!listA.contains(i)){
                listB.add(i);
            }
        }
        return bfs(listB.get(0), listB);
    }
    static boolean bfs(int startNum, List<Integer> list){
        Queue<Integer> queue = new LinkedList<>();
        queue.add(startNum);
        visited = new boolean[n + 1];
        visited[startNum] = true;
        while(!queue.isEmpty()){
            int node = queue.poll();
            for(int nextNode : nodes[node]){
                if(!visited[nextNode] && list.contains(nextNode)){
                    queue.add(nextNode);
                    visited[nextNode] = true;
                }
            }
        }
        boolean allConnected = true;
        for(int num : list){
            if(!visited[num]){
                allConnected = false;
                break;
            }
        }
        return allConnected;
    }
    static void calculate(){
        int listAPeopleSum = 0;
        for(int node: listA){
            listAPeopleSum += peopleCount[node];
        }
        int listBPeopleSum = 0;
        for(int node: listB){
            listBPeopleSum += peopleCount[node];
        }
        minDiffPeople = Math.min(Math.abs(listBPeopleSum - listAPeopleSum), minDiffPeople);
    }
}
```