## 트리순회(실버 1)

## 풀이

트리 클래스를 만들어서 접근하는게 정석이지만, 간단하게 배열로 만들어서 접근했다.

시간복잡도나 공간복잡도 측면에서도 더 효율적인 풀이.

## 정답

```java

public class Main_1991_강보승 {
    static int[][] arr;
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int n = Integer.parseInt(br.readLine());
        arr = new int[26 + 1][2];
        for(int i = 0; i < n; i++){
            StringTokenizer st = new StringTokenizer(br.readLine());
            char a = st.nextToken().charAt(0);
            char b = st.nextToken().charAt(0);
            char c = st.nextToken().charAt(0);
            int index = a - 'A';
            if(a != '.'){
                //int형 인덱스로 변경
                arr[index][0] = b - 'A';
            }else{
                // 리프 노드 처리
                arr[index][0] = -1;
            }
            if(b != '.'){
                //int형 인덱스로 변경
                arr[index][1] = c - 'A';
            }else{
                // 리프 노드 처리
                arr[index][1] = -1;
            }
        }
        preOrder(0);
        System.out.println();
        midOrder(0);
        System.out.println();
        postOrder(0);
    }

    static void preOrder(int num) {
        if(num == -1){
            return;
        }
        System.out.print((char)(num + 'A'));
        preOrder(arr[num][0]);
        preOrder(arr[num][1]);
    }
    static void midOrder(int num) {
        if(num == -1){
            return;
        }
        midOrder(arr[num][0]);
        System.out.print((char)(num + 'A'));
        midOrder(arr[num][1]);
    }
    static void postOrder(int num) {
        if(num == -1){
            return;
        }
        postOrder(arr[num][0]);
        postOrder(arr[num][1]);
        System.out.print((char)(num + 'A'));
    }
}

```