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
-----------------------------------------------------------------------------------      
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
6. 거스름돈 계산
```java
N이 32850일 경우,
50000원 : 0개
10000원 : 3개
5000원 : 0개
1000원 : 2개
500원 : 1개
100원 : 3개
50원 : 1개
10원 : 0개
배열, 메서드 사용
-----------------------------------------------------------------------------------   

public class Test1 {

	public static void main(String[] args) {

		
		int N = 32850;
		int[] money = { 50000, 10000, 5000, 1000, 500, 100, 50, 10 };
		
		
		
		
		// while문 (나의 해답)
//		int i = 0;
//		while(i < money.length) {
//			
//			System.out.println(money[i] + "원 : " + (N / money[i]) + "개");
//			N %= money[i];
//			i++;
//		}

		
		
		
		
		// for문
//		for(int i = 1; i < money.length; i++) {
////			int cnt = N / money[i]; // 메서드로 변경
//			int cnt = change(money[i], N);
//			N %= money[i];
//				System.out.println(money[i] + "원 : " + cnt + "개");
//		}

		
		
		
		
		// 거스름돈 갯수를 저장할 배열 cnt[]를 리턴받는 메서드 version
		
		int [] cnt = change(money, N);
		for(int i = 0; i < cnt.length; i++) {
			System.out.println(money[i] + "원 : " + cnt[i] + "개");
		}
		
		
		
		

	}
	// 거스름돈만 계산해서 리턴하는 메서드
	public static int change(int money, int N) {
		return N / money;
	}	
		// money 배열과 기준이 되는 금액(N)을 전달받아 cnt[] 배열을 리턴하는 메서드 작성
	public static int[] change(int[] money, int N) {
			int cnt[] = new int[money.length];
			
			for(int i = 0; i < money.length; i++) {
				cnt[i] = N / money[i];
				N %= money[i];
			}
			return cnt;
		}
	}


```
7. n개의 숫자가 입력되면 n개의 숫자를 왼쪽으로 하나씩 돌려서 출력하시오
```java
입력예시)
5
1 2 3 4 5
출력 예시)
1 2 3 4 5 
2 3 4 5 1
3 4 5 1 2
4 5 1 2 3
5 1 2 3 4 
-----------------------------------------------------------------------------------   

public class Test2 {

	public static void main(String[] args) {


		int[] arr = { 1, 2, 3, 4, 5 };

		// 기본적인 중첩 for문 정답
		for (int i = 0; i < arr.length; i++) {

			// 배열에 요소를 우측으로 출력
			for (int j = 0; j < arr.length; j++) {
				System.out.print(arr[j] + " ");
			}

			System.out.println();

			// 배열의 요소 이동하는 부분 -> for문으로
//			int temp = arr[0];
//			arr[0] = arr[1];
//			arr[1] = arr[2];
//			arr[2] = arr[3];
//			arr[3] = arr[4];
//			arr[4] = temp;

			// 배열 한 바퀴 돌림 (rotation)
			int temp = arr[0];
			for (int j = 0; j < arr.length - 1; j++) {
				arr[j] = arr[j + 1];
			}
			arr[arr.length - 1] = temp;

		}
		System.out.println("==============================================");
		// method로 변경
		for(int i = 0; i < arr.length; i++) {
			printArray(arr);
			rotation(arr);
		}
	}

	// 배열에 요소를 우측으로 출력
	public static void printArray(int[] arr) {
		for (int i = 0; i < arr.length; i++) {
			System.out.print(arr[i] + " ");
		}

		System.out.println();
	}

	// 배열 한 바퀴 돌림 (rotation)
	public static void rotation(int[] arr) {
		int temp = arr[0];
		for (int i = 0; i < arr.length - 1; i++) {
			arr[i] = arr[i + 1];
		}
		arr[arr.length - 1] = temp;
	}
}


