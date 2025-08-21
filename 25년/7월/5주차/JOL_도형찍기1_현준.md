## 도형찍기1([Jungol](http://www.jungol.co.kr))

## 풀이

삼각형 모양으로 숫자를 출력하는 문제입니다. 
각 줄마다 공백의 개수와 출력할 숫자의 개수를 계산하여 규칙적으로 출력합니다.

## 해답

```java
/*
도형찍기1
메모리: -
시간: -
아이디어: 각 줄마다 공백 수와 숫자 개수를 계산하여 삼각형 모양 출력
*/
package algo_homework;

public class DigitTest1_장현준 {
	
	private static int num = 1;
	private static int printMax = 5;
	
	public static void main(String[] args) {
		for (int print = 0; print < printMax; print++) {
			for (int space = 0; space < print; space++) {
				System.out.print("   ");
			}
			
			for (int printNum = 0; printNum < printMax - print; printNum++) {
				System.out.printf("%-3d", num++);
			}
			System.out.println();
		}

	}

}
```