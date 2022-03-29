# [오전수업] JAVA 26차

enum 연습

```java
package enum_statement;

import java.util.Scanner;

enum PayGroup {
	CASH("현금", new String[] {"ACCOUNT_TRANSFER", "REMITTANCE", "ON_SITE_PAYMENT", "TOSS"}),
	CARD("카드", new String[] {"PAYCO", "CARD", "KAKAO_PAY", "BAMIN_PAY", "NAVER_PAY"}),
	ETC("기타", new String[] {"POINT", "COUPON"}),
	EMPTY("없음", new String[] {""});
	
	private String title;
	private String[] payTypes;

	// Alt + Shift + S -> O
	private PayGroup(String title, String[] payTypes) {
		this.title = title;
		this.payTypes = payTypes;
		
	}
	
	public String getTitle() {
		return title;
	}
	
	public static PayGroup findByPayType(String str) {
		PayGroup result = EMPTY;
		
		for(PayGroup pg : PayGroup.values()) {	// pg: CASH, CARD, ETC, EMPTY, 
			
			for(String s : pg.payTypes) {
				if(s.equals(str)) {
					result = pg;
				}
				
			}
			
		}
		
		return result;
	}
	
}


public class Ex1 {

	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);
		String data = sc.nextLine();
		
		PayGroup pg = PayGroup.findByPayType(data);
		
		switch (pg) {
		case CASH:	System.out.println(pg.getTitle() + " 결제 로직으로 이동"); break;
		case CARD:	System.out.println(pg.getTitle() + " 결제 로직으로 이동"); break;
		case ETC:	System.out.println(pg.getTitle() + " 결제 로직으로 이동"); break;
		case EMPTY:
			System.out.println(pg.getTitle());
			System.out.println("존재하지 않는 결제 코드입니다.");
			System.out.println("에러 처리 로직으로 이동");
			break;
		}
	}
	
	
}
```

## 중첩 클래스 (Nested Class)
- 클래스 내에서 정의한 클래스
- 독립적인 일반 클래스 형태로 작성할 필요가 없지만, 나름대로 클래스 형태(멤버변수, 메서드, 생성자)를 갖춰야할 필요성이 있을 경우 사용
- 자신의 클래스만 접근 가능하도록 전용 클래스로 정의할 때 사용
  - 주로 GUI 구현 시 이벤트 처리를 수행하는 핸들러 클래스 정의 시 사용
- 외부클래스와 내부클래스로 구분됨
- 클래스를 정의하는 위치에 따라 다음과 같이 구분됨
```
		1) 멤버 내부클래스
			- 멤버변수 및 메서드와 동일한 레벨에 정의한 클래스
			- *.class 파일 생성 시 외부클래스명$내부클래스명.class 형태로 생성됨

		2) 정적 내부클래스
			- 정적 멤버변수 및 메서드와 동일한 방식으로 정의한 클래스
			- 클래스 파일은 멤버 내부클래스와 동일함

		3) 로컬 내부클래스
			- 클래스 내의 메서드 내에서 정의한 클래스
			- 특정 메서드 내에서만 사용할 클래스를 정의할 때
			- 메서드 호출 시점에서 메모리에 로딩되므로 메서드 외부에서 접근 불가능하며
			  메서드 내에서도 클래스 정의 코드 아래쪽에서 접근 가능함
			- *.class 파일 생성 시 외부클래스명$1내부클래스명.class 형태로 생성됨 


ex)

class A {
		int num;	// 인스턴스 멤버변수(인스턴스 생성 후 접근)
		class B {}	// 인스턴스 멤버클래스(= 멤버 내부클래스)
		// 인스턴스 생성 후 참조변수를 통해 접근 가능한 클래스
-------------------------------------------------------
		static int num2;		// 정적 멤버변수(클래스명으로 접근)
		static class C {}		// 정적 멤버클래스(= 정적 내부클래스)
		// 클래스명만으로 접근 가능한 클래스
-------------------------------------------------------
		public void method() {
			int num3;	// 로컬 변수(메서드 내에서만 접근 가능 변수)
			class D {}	// 로컬 내부 클래스	
			// 메서드 내에서만 접근 가능한 클래스
		}
}
```

```java
package NestedClass;

public class Ex1 {

	public static void main(String[] args) {
		// 클래스 내의 인스턴스 멤버(변수 및 메서드)에 접근하려면
		// 반드시 클래스의 인스턴스를 생성한 후 참조변수를 통해 멤버에 접근해야한다!
		Outer outer = new Outer();
		outer.method();	// Outer 인스턴스를 통해 멤버메서드 호출
		
		// 인스턴스 멤버 내부클래스도 인스턴스 멤버와 마찬가지로
		// 인스턴스를 통해 접근해야하는데 인스턴스 멤버 내부클래스의 인스턴스를 생성해야만
		// 해당 클래스 내의 멤버에 접근가능하다!
		// => 내부클래스에 접근하기 위해서는 반드시 외부클래스를 통해 접근해야하는데
		// 	  클래스타입 지정은 외부클래스명.내부클래스명으로 지정하고
		//	  인스턴스 생성은 외부클래스타입참조변수명.new 내부클래스(); 형태로 생성
		Outer.Inner inner = outer.new Inner();
		inner.innerMethod();
		// ------------------------------------------------------------------
		// 정적 내부클래스의 인스턴스 생성
		// => 정적 내부클래스 static 키워드가 사용되므로
		//	  클래스가 메모리에 로딩 될 때 정적 내부클래스도 함께 로딩됨
		//	  따라서, 내부클래스 접근 문법은 외부클래스의 인스턴스 생성 없이 
		//	  외부클래스명만으로 접근 가능
		// => 외부클래스명.내부클래스명 참조변수명 = new 외부클래스명.내부클래스명();
		Outer.StaticInner staticInner = new Outer.StaticInner();
		staticInner.staticInnerMethod();
		
	}

}

class Outer {	// 외부 클래스
	
	int num = 10;	// 인스턴스 멤버변수
	public void method() {	// 멤버 메서드
		// 멤버메서드 내에서는 인스턴스 멤버변수 및 메서드에 대해 자유롭게 접근 가능함
		// => private 접근제한자가 적용되어도 상관없이 접근 가능
		System.out.println(num);
		
		// 외부클래스의 멤버메서드 내에서 내부클래스 접근 가능(메서드와 동일)
		Inner inner = new Inner();	// 바로 접근 가능하므로 일반적인 인스턴스 생성 코드와 동일함
		
		// 로컬 내부클래스에 접근하기 위해서는 선언부보다 아래쪽에서 접근 가능함
//		LocalInner localInner;	// 오류 발생! => 아직 존재하지 않는 클래스
		
		// 로컬 내부클래스 정의 => 특정 메서드에서만 사용 가능한 클래스(= 특정 메서드 전용)
		class LocalInner {
			String name = "LocalInnerClass";
		}
		
		// 클래스 선언부(정의)보다  아래쪽에서 클래스 접근 가능
		// => 일반 클래스처럼 사용 가능
		LocalInner localInner = new LocalInner();
		System.out.println(localInner.name);
		
	}
	
	class Inner {	// 내부클래스 (=> 인스턴스 멤버 내부클래스 = 멤버 내부클래스
		// 멤버내부클래스 내에서 외부클래스의 멤버에 자유롭게 접근 가능함
		// => 멤버 메서드와 접근 권한이 동일함
		public void innerMethod() {
			System.out.println(num); // 외부클래스 멤버변수 접근 가능
			method();	// 내부클래스에서 외부클래스 메서드 호출 가능
		}
	}

	static class StaticInner {	// 정적 내부 클래스
		static String name; // 정적 멤버변수
		
		public void staticInnerMethod() {
			// 정적 내부클래스에서 외부클래스의 인스턴스 멤버의 접근이 불가능하다!
			// => 메모리 로딩 시점이 다르기 때문에(= 정적 메서드와 규칙이 동일)
//			System.out.println(num);	// 오류 발생!
		}
	}
}
```

# Wrapper 클래스
- 자바의 기본 데이터타입 8가지와 1:1로 대응하는 8개의 클래스의 모음(java.lang 패키지)
- 대부분 기본 데이터타입 이름의 첫글자를 대문자로 사용하는 클래스명을 가지며, char - Character, int - Integer 로 이름이 다르다!
  - byte = Byte, short - Short, int - Integer, long - Long, char - Character, float - Float, double - Double, boolean - Boolean
- 기본 데이터타입 변수로 할 수 있는 일이 한정적(데이터 저장 및 사용)이므로 이에 대한 클래스를 정의하여 클래스 내에 상수 및 다양한 메서드를 제공함으로써 기본 데이터타입에 대한 다양한 정보를 저장하거나 다양한 작업을 수행하도록 지원
	- 좀 더 효율적인 프로그래밍이 가능하도록 도와주는 클래스들의 모음
- 기본 데이터타입 데이터 (Stack 영역)를 Wrapper 클래스 객체로 변환하여 데이터 관리 가능
	- Wrapper 클래스의 객체 주소를 Wrapper 클래스 타입 참조변수로 관리 가능(클래스 역할 동일)
- Wrapper 클래스의 상수를 사용하여 해당 클래스에 대한 여러 정보 확인 가능
	- Wrapper클래스명.상수명(ex. Integer.SIZE)
- Wrapper 클래스 내부에는 Object 클래스의 toString() 메서드가 오버라이딩 되어 있으므로 Wrapper 클래스 타입 객체에 저장된 값을 출력하면 출력문에 변수값 그대로 사용

```java
package Wrapper;

public class Ex1 {

	public static void main(String[] args) {
		/*
		 * Wrapper 클래스
		 * - 자바의 기본 데이터타입 8가지와 1:1로 대응하는 8개의 클래스의 모음(java.lang 패키지)
		 * - 대부분 기본 데이터타입 이름의 첫글자를 대문자로 사용하는 클래스명을 가지며,
		 * 	 char - Character, int - Integer 로 이름이 다르다!
		 * 	 => byte = Byte, short - Short, int - Integer, long - Long, char - Character
		 * 		float - Float, double - Double, boolean - Boolean
		 * - 기본 데이터타입 변수로 할 수 있는 일이 한정적(데이터 저장 및 사용)이므로
		 * 	 이에 대한 클래스를 정의하여 클래스 내에 상수 및 다양한 메서드를 제공함으로써
		 * 	 기본 데이터타입에 대한 다양한 정보를 저장하거나 다양한 작업을 수행하도록 지원
		 * 	 => 좀 더 효율적인 프로그래밍이 가능하도록 도와주는 클래스들의 모음
		 * - 기본 데이터타입 데이터 (Stack 영역)를 Wrapper 클래스 객체로 변환하여 데이터 관리 가능
		 * 	 => Wrapper 클래스의 객체 주소를 Wrapper 클래스 타입 참조변수로 관리 가능(클래스 역할 동일)
		 * - Wrapper 클래스의 상수를 사용하여 해당 클래스에 대한 여러 정보 확인 가능
		 * 	 => Wrapper클래스명.상수명(ex. Integer.SIZE)
		 * - Wrapper 클래스 내부에는 Object 클래스의 toString() 메서드가 오버라이딩 되어 있으므로
		 * 	 Wrapper 클래스 타입 객체에 저장된 값을 출력하면 출력문에 변수값 그대로 사용
		 * 
		 */
		
		System.out.println("byte 타입 메모리 크기 : " + Byte.SIZE + "bit");
		System.out.println("byte 타입 메모리 크기 : " + Byte.BYTES + "byte");
		System.out.println("byte 타입 최소값 : " + Byte.MIN_VALUE);
		System.out.println("byte 타입 최대값 : " + Byte.MAX_VALUE);
		
		// short
		System.out.println("============= short =============");
		System.out.println(Short.SIZE + "bit");
		System.out.println(Short.BYTES + "byte");
		System.out.println(Short.MIN_VALUE);
		System.out.println(Short.MAX_VALUE);
		
		// int
		System.out.println("============= int =============");
		System.out.println(Integer.SIZE + "bit");
		System.out.println(Integer.BYTES + "byte");
		System.out.println(Integer.MIN_VALUE);
		System.out.println(Integer.MAX_VALUE);
		
		// long
		System.out.println("============= long =============");
		System.out.println(Long.SIZE + "bit");
		System.out.println(Long.BYTES + "byte");
		System.out.println(Long.MIN_VALUE);
		System.out.println(Long.MAX_VALUE);
		
		// float
		System.out.println("============= float =============");
		System.out.println(Float.SIZE + "bit");
		System.out.println(Float.BYTES + "byte");
		System.out.println(Float.MIN_VALUE);
		System.out.println(Float.MAX_VALUE);
		
		
	}

}
```

## 오토박싱 & 언박싱
- 기본 데이터타입은 Stack 공간에 실제 데이터(리터럴) 형태를 직접 저장 및 관리하지만 참조 데이터타입(객체)은 Heap 공간에 실제 데이터가 저장되며, 참조변수가 해당 공간의 주소값을 저장 및 관리함
  - 따라서, 기본 데이터타입 변수와 참조데이터타입 변수는 서로 호환 불가능했음. 그러나, Java(JDK) 1.5 부터 오토 박싱, 오토 언박싱 기능이 제공되면서 두 데이터타입 간에 자동으로 변호나이 되도록 지원됨

1. 오토 박싱(Auto Boxing)
- 기본 데이터타입(물건) -> Wrapper 클래스 타입 객체(박스)로 자동(Auto) 변환(포장)
  - Heap 공간의 객체 박스(Wrapper 클래스 타입)에 기본 데이터타입 변수(물건)를 전달하여 객체 형태로 포장하는 기능
2. 오토 언박싱(Auto Unboxing)
- Wrapper 클래스 타입 객체(박스)의 포장을 뜯어서 기본 데이터타입(물건) 꺼내기
	- Heap 공간의 객체 박스(Wrapper 클래스타입)에 들어있는 물건(기본 데이터타입 값)을 꺼내서 기본 데이터타입으로 변환
	  - 컴파일러에 의해 자동으로 변환(포장 뜯기) 되어진다! 

```java
package Wrapper;

public class Ex2 {

	public static void main(String[] args) {

Integer n1;
		Integer n2 = new Integer(20);
//		Integer n2 = new Integer("20");
		
		// 주의! 정수데이터가 아닐 경우 타입 변환에 대한 오류가 발생할 수 있음!
//		Integer n2 = new Integer("20.5");
//		System.out.println(n2);
		
		System.out.println(n2);	// n2.toString() 메서드 호출과 동일 (내부적으로 toSting() 오버라이딩)
		System.out.println(n2.toString());
		// Integer 타입 변수 n2의 값을 출력하면 주소값이 아닌 실제 데이터가 출력됨
		// => toString() 메서드가 오버라이딩 되어 있으므로 저장된 실제 값을 리턴 하도록 함
		
		System.out.println("=====================================================================");
		
		int num1 = 10;
		// 오토 박싱(Auto Boxing)
		// Integer 타입 변수 n1에 int 타입 변수 num1의 값(리터럴)을 저장하려고 할 경우
		// 1. 자바 초기에 사용하던 방법(Java 1.5 이전)
		//		=> Wrapper클래스타입변수 = Wrapper클래스명.valueOf(기본형데이터 또는 변수)
		//			=> 기본데이터타입을 Wrapper 클래스 타입 객체로 변환하여 저장
		n1 = Integer.valueOf(num1);
		System.out.println("기본데이터타입 num1 = " + num1);
		System.out.println("참조데이터타입 n1 = " + n1);
		
		// 2. Java(JDK 1.5) 부터 사용 가능한 방법
		//		=> Wrapper클래스타입 변수 = 기본형데이터 또는 변수;
		//			=> 오토 박싱 기능에 의해 기본데이터타입 -> 참조데이터타입으로 변환되어 저장
		n1 = num1;	// 오토박싱
		System.out.println("기본데이터타입 num1 = " + num1);
		System.out.println("참조데이터타입 n1 = " + n1);	// n1.toString() 생략되어 있음
		
		int num2 = 20;
		// 오토 언박싱(Auto Unboxing)
		// 1. 자바 초기에 사용하던 방법(Java 1.5 이전)
		// 		=> 기본데이터타입변수 = Wrapper클래스타입변수.xxValue();
		//			=> 이 때, xxx은 자바의 기본데이터타입 이름(ex. intValue(), floatValue())
		num2 = n2.intValue();	// Integer 타입 -> int 타입으로 변환
		System.out.println("기본 데이터타입 num2 = " + num2);
		System.out.println("참조데이터타입 n2 = " + n2);	// n2.toString() 생략되어 있음
		
		// 2. Java(JDK 1.5) 부터 사용 가능한 방법
		//		=> 기본데이터타입 변수 = Wrapper클래스타입변수;
		//			=> 오토 언박싱 기능에 의해 참조데이터타입 -> 기본데이터타입으로 변환되어 저장
		num2 = n2;	// 오토언박싱
		System.out.println("기본 데이터타입 num2 = " + num2);
		System.out.println("참조데이터타입 n2 = " + n2);	// n2.toString() 생략되어 있음
		
		System.out.println("===========================================================");
		
		Integer n3 = 30;
		Object o = n3;	// 업캐스팅
		Object o2 = 40;	// 오토박싱 -> 업캐스팅
		System.out.println(o2);
		
		System.out.println("===========================================================");

		Integer n4 = new Integer(40);
		int num4 = 40;
		System.out.println(n4 + num4); // Integer -> int 오토언박싱
		// => 즉, Integer + int = int + int 형태로 연산됨
		
		

		
	}

}
```


## 문자열 -> 기본 데이터로 변환
- 주로 웹의 <form> 태그나 GUI 환경에서 데이터를 입력받았을 때 수치 데이터를 입력하더라도 모두 문자열로 취급됨
	- (ex. request.getParameter(str) -> 문자열로 된 정수를 int형 정수로 변호나 필요)
- 따라서, 해당 문자열을 실제 연산을 위해 수치데이터 등의 기본 데이터타입으로 변환하려면 Wrapper 클래스에서 제공하는 메서드를 통해 해당 타입으로 변환해야한다!
	
```
Wrapper클래스명.parseXXX (문자열변수 또는 리터럴);
=> XXX은 자바의 기본데이터타입 이름
=> ex) 정수형으로 변환할 경우 : Integer.parseInt("100");
       실수형으로 변환할 경우 : Double.parseDouble("3.14");
```

```java
  package Wrapper;

public class Ex3 {

	public static void main(String[] args) {
  
		String strNum = "3.14";
		System.out.println(strNum + 10); // 문자열 데이터이므로 결합연산으로 동작함("3.1410")
		System.out.println("================");
		double dNum = Double.parseDouble(strNum);
		System.out.println(dNum);
		System.out.println(dNum + 10); // 실수형 데이터이므로 산술연산으로 동작함(13.14)
		
		System.out.println("================");
		strNum = "20";
		
		// int형 변수 num에 문자열 데이터 "20"을 저장 후 출력
		int num = Integer.parseInt(strNum);
		System.out.println(num);
		System.out.println(num + 10);
		
		
		// 주의! 변환 후의 데이터타입과 일치하지 않는 데이터를 변환하려 할 경우
		// NumberFormatException 이라는 예외(오류)가 발생하여 변환되지 않는다!
		strNum = "3.14";
		int num2 = Integer.parseInt(strNum);
	}
	

}
```

## Wrapper 클래스의 다양한 메서드
- Wrapper 클래스 타입을 활용하면 기본 데이터타입들을 다양하게 처리 가능
- 주로 Wrapper 클래스의 static 메서드를 직접 호출하여 기본 데이터타입 데이터를 처리하도록 작업을 수행하는 형태로 사용	
	
```java
package Wrapper;

public class Ex4 {

	public static void main(String[] args) {

		// 기본 데이터타입 변수 선언 시
		int num1 = 10, num2 = 20;
		
		// Integer 타입 객체 생성 시
//		Integer n1 = new Integer(10);
//		Integer n2 = new Integer(20);
		
		// => 오토박싱을 활용할 경우
		Integer n1 = 10, n2 = 20;
		
		int sum = 10 + n2; // Integer -> int 오토언박싱 후 int + int 연산 수
		System.out.println(sum);
		
		System.out.println("=================================================");
		
		
		// Integer 클래스의 static 메서드 활용
		// 1 . 두 정수에 대한 대소 관계 비교
		System.out.println("num1과 num2 중 큰 값 : " + Integer.max(num1, num2));
		System.out.println("n1과 n2 중 작은 값 : " + Integer.min(n1, n2));	// 오토언박싱
		
		// 2. 어떤 정수에 대한 진법 변환
		// => 해당 진법의 문자열 형태로 리턴하는 메서드 사용 : toXXXSTring()
		System.out.println("10진수 num1에 대한 2진수 변환 : " + Integer.toBinaryString(num1));
		System.out.println("10진수 num1에 대한 8진수 변환 : " + Integer.toOctalString(num1));
		System.out.println("10진수 num1에 대한 16진수 변환 : " + Integer.toHexString(num1));

		System.out.println("=============================================");
		
		// Character 클래스의 메서드 활용
		// => 주로 특정 문자에 대한 판별을 Character.isXXX() 메서드로 수행하거나 
		// 	  특정 문자로 변환을 Character.toXXX() 메서드로 수행
		char ch = 'F';
		if(ch >= 'A' && ch <= 'Z') {
			System.out.println(ch + " : 대문자!");
		} else if(ch >= 'a' && ch <= 'z') {
			System.out.println(ch + " : 소문자!");
		} else if(ch >= '0' && ch <= '9') {
			System.out.println(ch + " : 숫자!");
		}
		
		// Character 클래스의 isXXX() 메서드를 호출하여 판별할 경우
		ch = ' ';
		System.out.println(ch + " 문자는 대문자인가? " + Character.isUpperCase(ch));
		System.out.println(ch + " 문자는 소문자인가? " + Character.isLowerCase(ch));
		System.out.println(ch + " 문자는 숫자(0~9)인가? " + Character.isDigit(ch));
		System.out.println(ch + " 문자는 공백문자인가? " + Character.isSpace(ch));
		System.out.println(ch + " 문자는 공백문자인가? " + Character.isSpaceChar(ch));
		// => deprecated 처리된 메서드 : 사용은 가능하지만
		//	  보안 상의 이유나 다른 메서드 제공 등으로 인해
		//	  현재는 사용하는 것을 추천하지 않는 메서드
		//	  @Deprecated 어노테이션을 사용
		
		ch = '3';
		if(Character.isUpperCase(ch)) {
			System.out.println(ch + " : 대문자!");
		} else if(Character.isLowerCase(ch)) {
			System.out.println(ch + " : 소문자!");
		} else if(Character.isDigit(ch)) {
			System.out.println(ch + " : 숫자!");
		} else if(Character.isWhitespace(ch)) {
			System.out.println(ch + " : 공백 문자!");
		} else {
			System.out.println(ch + " : 기타문자!");
		}


	}

}
```
	
# [오후수업] JSP 37차
	
	
