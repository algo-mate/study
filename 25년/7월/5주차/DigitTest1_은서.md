## 도형찍기 실습(DigitTest1)

## 풀이

숫자로 삼각형 모양을 출력하는 실습 문제이다.

각 행마다 공백의 개수와 숫자의 개수가 변하는 패턴을 파악하여 구현한다.

공백은 행이 바뀔 때마다 i번 반복 출력하고, 숫자는 5-i번 반복 출력한다.

## 해답

```java
public class DigitTest1 {

	public static void main(String[] args) {
		//숫자 1부터 시작
		int num = 1;
		
		//가장 바깥 반복문 : 5개의 행 출력
		for(int i=0; i<5; i++) {
			//공백은 행이 바뀔 때마다 i번 반복 출력 (0->1->2->3->4)
			for(int k=0; k<i; k++) {
				System.out.printf("   ");
			}
			
			//숫자는 행이 바뀔 때마다 5-i 번 반복 출력(5->4->3->2->1)
			for(int j=0; j<5-i; j++) {
				System.out.printf("%-3d", num);
				num++;
			}
			
			System.out.println();
		}
	
	}
}
```