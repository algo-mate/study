## 보호필름(SWEA 2112)

## 풀이

완전탐색(부분집합)인 줄 알았지만, 가지치기 안하면 타임아웃이 발생하는 문제이다.

DFS로 더 효율적인 풀이가 가능해보이지만, 시간이 없어서 부분집합으로 해결했다.

가지치기를 통해 현재까지 선택한 행의 개수가 이미 찾은 최솟값보다 크거나 같으면 탐색을 중단한다.

## 해답

```java
/*
    2112. 보호필름
    메모리: 109,448 kb
    실행시간: 4,525 ms
    아이디어: 1.완전탐색(부분집합)인 줄 알았지만, 가지치기 안하면 타임아웃 발생
            2. DFS로 더 효율적인 풀이 가능해보이지만, 시간 없어서 일단 제출
*/
public class Solution_2112_강보승 {
    static int r, c, k, n, minValue;
    static int[][] arr, clonedArr;
    static List<Integer> list, listAIdx, listBIdx;
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
        int T = Integer.parseInt(br.readLine());
        for(int t = 1; t <= T; t++){
            StringTokenizer st = new StringTokenizer(br.readLine());
            r = Integer.parseInt(st.nextToken());
            c = Integer.parseInt(st.nextToken());
            k = Integer.parseInt(st.nextToken());
            arr = new int[r][c];
            clonedArr = new int[r][c];
            for(int i = 0; i < r; i++){
                st = new StringTokenizer(br.readLine());
                for(int j = 0; j < c; j++){
                    arr[i][j] = Integer.parseInt(st.nextToken());
                }
            }
            minValue = Integer.MAX_VALUE;
            clonedArr();
            simulate();
            if(minValue == Integer.MAX_VALUE)
                minValue = k;
            StringBuffer sb = new StringBuffer();
            sb.append("#").append(t).append(" ").append(minValue).append("\n");
            bw.write(sb.toString());
            System.out.println("#" + t + " " + minValue);
        }
        bw.close();
    }
    static void simulate(){
        if(performanceTest()){
            minValue = 0;
            return;
        }
        list = new ArrayList<>();
        choose(0);
    }
    // 약품 투입 행 선택
    static void choose(int depth){
        // 가지치기 안하면 50개 케이스 중 마지막 1 케이스 타임아웃 발생
        if(list.size() >= minValue){
            return;
        }
        if(depth == r){
            if(list.size() < k)
                change();
            return;
        }
        choose(depth + 1);

        list.add(depth);
        choose(depth + 1);
        list.remove(list.size() - 1);
    }
    static void change(){
        n = list.size();
        listAIdx = new ArrayList<>();
        chooseIdx(0);
    }
    // A, B 선택
    static void chooseIdx(int depth){
        if(depth == n){
            startTest();
            return;
        }
        chooseIdx(depth + 1);

        listAIdx.add(list.get(depth));
        chooseIdx(depth + 1);
        listAIdx.remove(listAIdx.size() - 1);
    }
    // A, B 선택 후 약품 채우기 및 테스트
    static void startTest(){
        listBIdx = new ArrayList<>();
        for(int num : list){
            if(!listAIdx.contains(num))
                listBIdx.add(num);
        }
        clonedArr();
        for(int indexA : listAIdx){
            for(int i = 0; i < c; i++){
                clonedArr[indexA][i] = 0;
            }
        }
        for(int indexB : listBIdx){
            for(int i = 0; i < c; i++){
                clonedArr[indexB][i] = 1;
            }
        }
        if(performanceTest()){
            minValue = Math.min(minValue, list.size());
        }
    }
    static void clonedArr(){
        for(int i = 0; i < r; i++){
            for(int j = 0; j < c; j++){
                clonedArr[i][j] = arr[i][j];
            }
        }
    }
    static boolean performanceTest(){
        boolean[] testResult = new boolean[c];
        for(int i = 0; i < c; i++){
            int lastValue = clonedArr[0][i];
            int count = 1;
            for(int j = 1; j < r; j++){
                if(lastValue == clonedArr[j][i])
                    count++;
                else
                    count = 1;
                if(count == k){
                    testResult[i] = true;
                    break;
                }
                lastValue = clonedArr[j][i];
            }
        }
        for(int i = 0; i < c; i++){
            if(!testResult[i])
                return false;
        }
        return true;
    }
}
```