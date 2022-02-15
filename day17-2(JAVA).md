> 수업 종료 후 희망자에 한하여 보충수업 실시

# [보충수업] JAVA method part

```java

public class Ex1 {

	public static void main(String[] args) {
		
		A a = new A();
		a.methodA();
		
		methodB();
	}
	
	public static void methodB() {
		System.out.println("methodB다!");
	}

}

class A {
	// 멤버변수
	// 생성자
	// 메서드
	void methodA() {
		System.out.println("methodA다!");
		methodC();
	}
	void methodC() {
		System.out.println("methodC다!");
	}
}
```

- 1 ~ 10까지 출력하는 메서드 printTen 작성 후 호출
```java

public class Ex1 {
	


	public static void main(String[] args) {
		/*
		 * method : 리턴타입 이름(매개변수...)
		 */
		
		
		// 1 ~ 10까지 출력하는 메서드 printTen 작성 후 호출
		printTen();
		
		
	} // main() 메서드 끝
	
	
	
	public static void printTen() {
		for(int i = 1; i < 11; i++) {
			System.out.println(i);
		}
	}
	
	
	
} // Ex1 Class 끝

```


연습문제들
```java
import java.util.ArrayList;
import java.util.Arrays;

public class Ex1 {
	


	public static void main(String[] args) {
		/*
		 * method : 리턴타입 이름(매개변수...)
		 */
		
		
		// 1 ~ 10까지 출력하는 메서드 printTen 작성 후 호출
//		printTen();
		
		
		
		// 정수 n을 전달받아 1 ~ n(전달받은) 까지 출력하는 메서드로 변경
//		printTen(30);
		
		
		
		// 정수 start, end 를 전달받아 start ~ end 까지 출력하는 메서드로 변경
//		printTen(10, 30);
		
		
		
		// 1 ~ 10 까지 배열을 리턴하는 getArray() 메서드를 작성
//		int[] arr = getArray();
//		System.out.println(Arrays.toString(arr));

		
		
		// 정수 n을 전달받아 1 ~ n 까지 배열을 리턴하는 메서드로 변경
//		int[] arr = getArray(10);
//		System.out.println(Arrays.toString(arr));
		
		
		
		// ArrayList로
		ArrayList arr = getArray2(20);
		for(int i = 0; i < arr.size(); i++) {
			System.out.print(arr.get(i) + " ");
		}
		
		
		
	} // main() 메서드 끝
	
	
//	public static void printTen() {
//		for(int i = 1; i <= 10; i++) {
//			System.out.println(i);
//		}
//	}
	
	
	
//	public static void printTen(int n) {
//		for(int i = 1; i <= n; i++) {
//			System.out.println(i);
//		}
//	}
	
	
	
//	public static void printTen(int start, int end) {
//		for(int i = start; i <= end; i++) {
//			System.out.println(i);
//		}
//	}
	
	
	
//	public static int[] getArray() {
//		int[] arr = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
//		return arr;
//	}
	
	
	
//	public static int[] getArray(int n) {
////		int[] arr = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
//		int[] arr = new int[n];
//		for(int i = 0; i < arr.length; i++) {
//			arr[i] = i+1;
//		}
//		return arr;
//	}

	
	
	public static ArrayList getArray2(int n) {
		ArrayList arr = new ArrayList();
		
		for(int i = 1; i <= n; i++) {
			arr.add(i);
		}
		return arr;
	}
	
	
	
	
	
	
} // Ex1 Class 끝

```






