# [오전수업] JSP 8차
```
		 * < 메서드 정의 방법(형태)에 따른 분류 >
		 * 1. 매개변수가 없고, 리턴값도 없는 메서드
		 * 2. 매개변수는 없고, 리턴값만 있는 메서드
		 * 3. 매개변수만 있고, 리턴값은 없는 메서드
		 * 4. 매개변수도 있고, 리턴값도 있는 메서드
```     

- day8의 메서드 정의 방법에 따른 분류 1,2번에 이어 3,4번 보충수업 실시 (day8 커밋에 저장) 

# [오후수업] 자바 5차

**while, for문 응용문제 연습**
```java
int n = 247427723
n에서 7의 갯수 찾기

*while문으로 푸는 방법

		int n = 247427723;
		int count = 0;
		
		while (n > 0) {
			if (n % 10 == 7) {
				count++;
			}
				n /= 10;
		}
		System.out.println("7의 갯수 : " + count);
		
		System.out.println("=============================");
		
*for 문으로 푸는 방법		

		int count = 0;
		for(int n = 247427723; n > 0; n /= 10) {
			if (n % 10 == 7) {
				count++;
			}
		}
		System.out.println("7의 갯수 : " + count);
		
		
이러한 문제 푸는 것은 while 방법 추천 (가독성이 좋음)
```		
		
		

## 중첩 for문
- for문 내에 또 다른 for문을 기술하여 반복 문장을 여러번 반복하는 문
- 기존 반복문을 바깥쪽 for문 이라고 가정했을 때 해당 반복문 내에서 다시 반복을 수행하는 for문을 안쪽 for문이라고 함. 
	- 안 쪽 for문 반복 횟수 = 안 쪽 for문 반복횟수 * 바깥쪽 for문 반복횟수
> - 이클립스 단축키 팁) Alt + Shift + R : 문자열 한 번에 바꾸기
> - 변수 지정 시 i 다음은 j, k, l, ...

```
< 기본 문법 > 
for(초기식1; 조건식1; 증감식1){

	for(초기식2; 조건식2; 증감식2){
 
	}
 
}
```		 


### 중첩 for문을 활용한 시계 구현

```java
// 중첩 for문을 활용한 시계 구현
		// => 0분 ~ 59분 까지 1씩 증가할 동안 각 분마다 0초 ~ 59초 까지 1씩 증가
		/*
		 * 0분 0초 0분 1초 0분 2초 ... 0분 59초 1분 0초 1분 1초 ... 59분 58초 59분 59초
		 * 
		 */
		for (int min = 0; min <= 59; min++) {

			for (int sec = 0; sec <= 59; sec++) {
				System.out.println(min + "분" + sec + "초");
			}
		}

		// 시(0 ~ 23) 분(0 ~ 59) 초(0 ~ 59)
		for (int hour = 0; hour <= 23; hour++) {

			for (int min = 0; min <= 59; min++) {

				for (int sec = 0; sec <= 59; sec++) {
				System.out.println(hour + "시" + min + "분" + sec + "초");
			}

		}
	}
```	

연습문제
```java
for문을 사용하여 구구단 2 ~ 9단 까지 모두 출력 
< 2단 > 
2 * 1 = 2 
2 * 2 = 4 
... 
2 * 9 = 18

< 3단 > 
3 * 1 = 3 
...

< 9단 > 
... 
9 * 9 = 81




	for (int dan = 2; dan <= 9; dan++) {
			
			System.out.println("< " + dan + "단 >");

			for (int i = 1; i <= 9; i++) {
				System.out.println(dan + " * " + i + " = " + (dan * i));
			}
			
			System.out.println();
		}
```		

### 별찍기

```java
// 별찍기
		// *			i = 1일 때,  j = 1 ~ 1 까지
		// **			i = 2일 때,  j = 1 ~ 2 까지
		// ***			i = 3일 때,  j = 1 ~ 3 까지
		// ****			i = 4일 때,  j = 1 ~ 4 까지
		// *****		i = 5일 때,  j = 1 ~ 5 까지
		
		for(int i = 1; i <= 5; i++) {

			for(int j = 1; j <= i; j++) {
				System.out.print("*");
			}
			System.out.println();
		}
		
		
		
		System.out.println("=====================================");
		
		
*1번방법		
		// *****		i = 1일 때, j = 1 ~ 5 까지
		// ****			i = 2일 때, j = 1 ~ 4 까지
		// ***			i = 3일 때, j = 1 ~ 3 까지
		// **			i = 4일 때, j = 1 ~ 2 까지
		// *			i = 5일 때, j = 1 ~ 1 까지
		
		for(int i = 1; i <= 5; i++) {
			
			for(int j = 1; j <= (6-i); j++) {
				System.out.print("*");
			}
			System.out.println();
		}
		
		
*2번방법		
		// *****		i = 5일 때, j = 1 ~ 5 까지
		// ****			i = 4일 때, j = 1 ~ 4 까지
		// ***			i = 3일 때, j = 1 ~ 3 까지
		// **			i = 2일 때, j = 1 ~ 2 까지
		// *			i = 1일 때, j = 1 ~ 1 까지
		
		for(int i = 5; i >= 1; i--) {
			
			for(int j = 1; j <= i; j++) {
				System.out.print("*");
			}
			System.out.println();
		}
		
		
		
// 정수 n을 입력받아 n층 의 별 계단을 출력
		// n = 5인 경우
		// **			i = 0일 때, 공백 = 0, **
		//  **			i = 1일 때, 공백 = 1, **
		//   **			i = 2일 때, 공백 = 2, **
		//    **		i = 3일 때, 공백 = 3, **
		//     **		i = 4일 때, 공백 = 4, **		
		Scanner sc = new Scanner(System.in);
		int n = sc.nextInt();

		for(int i = 0; i < n; i++) {
			
			for (int j = 0; j < i; j++) {
				System.out.print(" ");
			}
			System.out.println("**");
		}

		
		
```
## break & continue
- 반복문 내에서 반복문을 제어하는데 사용
- 주로 조건식(if문 등)과 결합하여 사용

### break문
- 현재 수행중인 반복문을 종료하고 빠져나가는데 사용
- break문을 만나면 반복문 내의 break문보다 아래쪽 문장 실행을 생략하고 즉시 반복문을 빠져나감.

```java
int i = 1;
		while(true) {
			System.out.println(i);
			if(i == 5) {
				break;
			}
			i++;
		}
```

### continue 문
- 현재 수행중인 반복문의 특정 문장 실행을 생략하는데 사용
- 현재 수행중인 반복문의 continue 문보다 아래쪽 문장 실행을 생략하고 다음 반복을 진행하기 위해 위로 점프
- for문에서 continue문은 증감식으로 이동하며, while문은 조건식으로 이동함.
		 
```java
*for문
		for(i = 1; i <= 10; i++) {
			if(i % 2 == 1) { // 홀수일 경우
				continue;
			}
			System.out.println(i);
		}
		
		
*while문		
		int i = 1;
		while(i <= 10) {
			// 주의! while문 내에서 continue 문을 사용해야할 경우
			// 제어변수를 제어하는 증감식이 continue 문보다 윗쪽에 위치해야한다!
			// => 아래쪽에 위치할 경우 실행되지 못하고 무한루프에 빠질 수 있다!
			i++;
			if(i % 2 == 1) {
				continue;
			}
			System.out.println(i);

```		
- 중첩 반복문 내에서 break, continue 문 사용시 break문, continue문이 소속된 반복문에 효과가 적용됨
	- 따라서, 원하는 반복문 블럭에 break, continue문을 적용하고 싶을 경우 레이블(Label) 기능을 활용하여 원하는 블럭 지정하고 break, continue문 사용시 해당 레이블을 지정!
	
```
레이블명: 
원하는 블럭문 {
	안쪽 블럭문 {
		조건식(){
			break 레이블명;
		}
	}
}
```


ex)
```java
*구구단으로 break, continue문 사용

		OUTTER:
		for(int dan = 2; dan <= 9; dan++) {
			System.out.println("< " + dan + "단 >");
			
			INNER:
			for(int i = 1; i <= 9; i++) {
				
				if(dan == 6) {
//				break;			// 안쪽 for문을 종료하고 System.out.println(); 문장이 실행됨
//				continue;		// 아래쪽 실행문을 생략하고 안쪽 for문의 증감식(i++) 으로 이동
					
//				break INNER;		// 일반 break문과 동일
//				continue INNER; 	// 일반 continue 문과 동일
					
//				break OUTTER;		// 바깥쪽 for문을 종료하고 "프로그램 종료!"
				continue OUTTER; 	// 바깥쪽 for문의 증감식(dan++)으로 이동
				// dan이 6일 때 i가 1일 경우 continue 문을 만나 
				// 안 쪽 for문을 포함하여 바깥쪽 for문의 continue문 아래쪽 문장을 생략 후
				// 다음 바깥 for문 반복을 위해 증감식(dan++)으로 이동
				}
				
				System.out.println(dan + " * " + i + " = " + (dan * i));
			}
			System.out.println();
		}
```



연습문제
```java
> 거스름돈 계산 N이 32850일 경우, 
50000원 : 0개
10000원 : 3개
5000원 : 0개
1000원 : 2개
500원 : 1개
100원 : 3개
50원 : 1개
10원 : 0개
출력하기


---------------------------

public class test4 {

	public static void main(String[] args) {


		//50000원
		int N = 32850;
		int money = 50000;
		System.out.println(money + "원 : " + change(N, money) + "개");
		
		// 10000원
		N %= money;
		money = 10000;
		System.out.println(money + "원 : " + change(N, money) + "개");
		
		// 5000원
		N %= money;
		money = 5000;
		System.out.println(money + "원 : " + change(N, money) + "개");
		
		// 1000원
		N %= money;
		money = 1000;
		System.out.println(money + "원 : " + change(N, money) + "개");
		
		// 500원
		N %= money;
		money = 500;
		System.out.println(money + "원 : " + change(N, money) + "개");
		
		// 100원
		N %= money;
		money = 100;
		System.out.println(money + "원 : " + change(N, money) + "개");
		
		// 50원
		N %= money;
		money = 50;
		System.out.println(money + "원 : " + change(N, money) + "개");
		
		// 10원
		N %= money;
		money = 10;
		System.out.println(money + "원 : " + change(N, money) + "개");
		


	} // main 메서드 끝

	// N과 money를 전달받아 money의 갯수
	// (거스름돈의 갯수)를 리턴하는 메서드 change 를 작성
		public static int change(int N, int money) {
			
		return N/money;
		
	}
		
		
		
} // test4 클래스 끝

```









