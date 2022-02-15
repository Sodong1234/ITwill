# [오전수업] JAVA 10차
> JAVA 9차에서 실시한 복습문제 이어서 실시

3. 구구단을 단보다 곱하는 수가 작거나 같은 경우까지만 출력하는 프로그램 만들기 (break문 사용)

```java
		for (int dan = 2; dan < 10; dan++) {
			System.out.println("< " + dan + "단 >");
			for (int i = 1; i < 10; i++) {
				if (dan < i) {
					break;
				}
				System.out.println(dan + " * " + i + " = " + (dan * i));

			}
			System.out.println();
		}
```

4. 반복문을 사용하여 다음 모양을 출력하는 프로그램 만들기
```java
      *			// i = 3일 때, 좌우공백 : 3개, 별 : 1개
    * * *		// i = 2일 때, 좌우공백 : 2개, 별 : 3개
  * * * * *		// i = 1일 때, 좌우공백 : 1개, 별 : 5개
* * * * * * *		// i = 0일 때, 좌우공백 : 0개, 별 : 7개



int n = 4; // n을 층 수로 생각
		

n = 4일 때, i = 3, 별의갯수 = 1
n = 4일 때, i = 2, 별의갯수 = 3
n = 4일 때, i = 1, 별의갯수 = 5
n = 4일 때, i = 0, 별의갯수 = 7
2씩 곱하고 계산하였을때,
8 - 6 = 2
8 - 4 = 4
8 - 2 = 6
8 - 0 = 8
별의 갯수가 -1씩 적은 것을 확인 할 수 있음.

		
		
		for(int i = n-1; i >= 0; i--) { // 행
			
			for(int j = 0; j < i; j++) {System.out.print(" ");} // 좌공백
			for(int j = 1; j <= (n*2) - (i*2) - 1; j++) {System.out.print("*");} // 별
			for(int j = 0; j < i; j++) {System.out.print(" ");} // 우공백
			
			System.out.println();
		}
```

5. 반복문과 조건문을 사용하여 다음 모양을 출력하는 프로그램 만들기
```java
  
      *			// i = 0일 때, 좌우공백 : 3개, 별 : 1개
    * * *		// i = 1일 때, 좌우공백 : 2개, 별 : 3개
  * * * * *		// i = 2일 때, 좌우공백 : 1개, 별 : 5개
* * * * * * *		// i = 3일 때, 좌우공백 : 0개, 별 : 7개
  * * * * *		// i = 4일 때, 좌우공백 : 1개, 별 : 5개
    * * *		// i = 5일 때, 좌우공백 : 2개, 별 : 3개
      *			// i = 6일 때, 좌우공백 : 3개, 별 : 1개
      
package ch04;

public class Ex5 {

	public static void main(String[] args) {
      
      
      
		int line = 7; // 홀수만 가능
		int space = line / 2; // 공백
		int star = 1;
		for (int i = 1; i < line; i++) { // 행

			for (int j = 0; j < space; j++) {
				System.out.print(" ");
			} // 좌공백
			for (int j = 0; j < star; j++) {
				System.out.print("*");
			} // 별
			for (int j = 0; j < space; j++) {
				System.out.print(" ");
			} // 우공백
			System.out.println();
			if (i < line / 2) {
				space = space - 1;
				star = star + 2;
			} else {
				space = space + 1;
				star = star - 2;
			}

		}

		System.out.println("------------------------------");
		// method 버전
		line = 7;
		space = line/2;
		star = 1;
		
		for(int i = 0; i < line; i++) {
			
			printSpace(space);
			printStar(star);
			printSpace(space);
			System.out.println();
			
			if(i < line/2) {
				space -= 1;
				star += 2;
			} else {
				space += 1;
				star -= 2;
			}
		}
		
	}
	
	// 공백을 찍을 메서드
	public static void printSpace(int 공백갯수) {
		for(int i=0; i<공백갯수; i++) {
			System.out.print(" ");
		}
	}
	
	// 별찍을 메서드
	public static void printStar(int 별갯수) {
		for(int i=0; i<별갯수; i++) {
			System.out.print("*");
		}
	}

}
```
