## 도형찍기2([Jungol](http://www.jungol.co.kr))

## 풀이

다이아몬드 모양으로 숫자를 출력하는 문제입니다.
위쪽 삼각형과 아래쪽 삼각형으로 나누어 각각 출력하며, 공백과 숫자의 개수를 계산하여 다이아몬드 형태를 만듭니다.

## 해답

```java
/*
도형찍기2
메모리: -
시간: -
아이디어: 위쪽과 아래쪽 삼각형으로 나누어 다이아몬드 모양 출력
*/
package algo_homework;

public class DigitTest2_장현준 {
	
	private static int num = 1;
	private static int printMax = 5;
	private static int printMid = (printMax + 1) / 2;
	
	public static void main(String[] args) {
		for (int print = 0; print < printMid; print++) {
			for (int space = 0; space < print; space++) {
				System.out.print("   ");
			}
			
			for (int printNum = 0; printNum < printMax - print * 2; printNum++) {
				System.out.printf("%3d", num);
				num++;
			}	
			System.out.println();
		}
		
		for (int print = printMid - 2; print >= 0; print--) {
			for (int space = print; space > 0; space--) {
				System.out.print("   ");
			}
			
			for (int printNum = 0; printNum < printMax - print * 2; printNum++) {
				System.out.printf("%3d", num);
				num++;
			}	
			System.out.println();
		}
	}

}
```