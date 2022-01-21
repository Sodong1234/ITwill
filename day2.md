# [오전수업]
## 1. 하드웨어 & 소프트웨어 & 펌웨어
- 하드웨어(Hardware; H/W) : 물리적인 기계 자체
- 소프트웨어(Software; S/W) : 하드웨어 상에서 돌아가는 프로그램
	- 운영체제(Operation System; OS) - 하드웨어와 소프트웨어의 다리 역할
ex) Windows, Linux, Unix, Solaris 등
	- 응용프로그램(Application Program) - 구체적인 작업을 할 수 있도록 해주는 프로그램
ex) Excel, PowerPoint, 한글, 계산기, 포토샵 등
- 펌웨어(Firmware) : 하드웨어 + 소프트웨어

## 2. 프로그래밍
1) 시스템 프로그래밍
  ex) OS 제작, 수정
2) 응용 프로그래밍(Application)
  ex) 엑셀 제작, 원가관리 프로그램 등
3) 웹 프로그래밍(Site, Homepage, Intranet)
  ex) 홈페이지 제작
### 2-1. 프로그래밍 언어
1) 기계어 : 컴퓨터만 이해하고 있는 0,1 (전류가 흐른다&흐르지 않는다)
2) 어셈블리어 : 0,1 대신 -> ADD, SUB, MOVE 등 니모닉 기호 사용. 
- 단점 : 프로그램구조나 자료구조 표현 어려움
	- 기계어와 어셈블리어를 저급언어(Low-level Language) 라고 부름. 우리가 학원에서 배울 일 없음.
3) 고급언어 : Pascal, Basic, C/C++, C#, #Java#. 
- 장점 : 사람이 이해하고 표현하기 쉬움, 복잡한 알고리즘, 자료구조 처리 가능.
        
        
    
    
## 3. 기억장치
### 1)종류
- 주기억장치
ex) RAM
- 보조기억장치
ex) HDD, CD-R/W,, FDD, SSD(Solid State Drive)
### 전제 - ###모든 프로그램은 주기억장치에서만 실행된다!!!###
   

---

### 진법의 종류

- 10진법 (0-9)  2진법 (0,1)  8진법 (0-7)  16진법 (0-15), 16진법은 0-9,A-F까지 셈. 
- 표기는 숫자(진법) ex-10(2) / 30(10) / 1F(16)

	  10진법-> 2&8&16진법, 2&8&16진법 -> 10진법 변환법 :  https://m.blog.naver.com/icbanq/221727893563

### 크기의 단위

  bit(비트) -> byte(바이트) -> KB(킬로바이트) -> MB(메가바이트) -> GB(기가바이트) -> TB(테라바이트) -> PB(페타) -> EB(엑사) -> ZB(엑사) -> YB(요타)
  
    bit : 2진수 1승 (0,1)
    byte : 2진수 8승, 8bit
    KB : 2진수 10승 & 10의 3자리, 1024byte 
    MB : 10의 6승, 1024kb & 약 100000000byte
    GB : 10의 9승, 1024MB
    TB PB EB...
    
*속도의 단위*
  1s -> 1ms
  
  ---
  
IDE(Integrated Development Environment, 통합 개발 환경)
코드 작성, 저장, 컴파일(번역), 실행, 디버깅 등을 별도로 작업하지 않고 하나의 프로그램에서 모든 기능을 제공하는 소프트웨어

-대표적인 IDE : Eclipse, Visual Studio, STS, Intelli J, Android Studio
  
  
  ---
  
  
# [오후수업]

 ## 자바의 특징
 - 모든 운영체제에서 실행가능 (JVM: Java Virtual Machine)
 - 객체 지향 프로그래밍 (OOP: Object-Oriented Programming)
 - 메모리 자동 정리 (GC: Garbage Collector)
 - 다양한 API와 풍부한 무료 라이브러리
 
 
 JAVA는 항상 
 
 ```java
public class Ex1 {

	public static void main(String[] args) {
	

	}
  
}

```

로 시작.


## 주석 사용법
    Ctrl + Shift + C & Ctrl + / 
    블록지정 가능

1. 라인주석 = 단일주석 = 한줄주석 -> // 기호 사용


```java        

public class Ex1 {

	public static void main(String[] args) {
	
//		System.out.println(1);

		
	}

}

```

2. 범위주석 = 블록주석 -> /* */ 기호 사용. 주석 기호 사이에 오는 모든 문장들을 주석 처리. 여러 줄에 걸쳐서 모든 범위를 지정 할 수도 있음.
단축키 : Ctrl + Shift + /
해제 단축키 : Ctrl + Shift + \
	보통 범위주석은 단축키로 사용 잘 안 함 (줄이 변경되는 경우가 많음)

```java

public class Ex1 {

	public static void main(String[] args) {
	
		/*
		System.out.println(1);
		System.out.println(2);
		System.out.println(3);
		System.out.println(4);
		System.out.println(5); 
		System.out.println(6);
		System.out.println(7);
		System.out.println(8);
		System.out.println(9);
		System.out.println(10);
		*/
	}

}

```


### 작성한 한 줄을 블록지정하여 지우는 것보다 Ctrl+D 로 지우는 것이 편함. 



## 문자와 문자열

```java
public class Ex2 {

	public static void main(String[] args) {

		
		System.out.println("아이" + "티윌");
		System.out.println(1+1);
		System.out.println("1"+"1");
		
	}

}
```
 큰따옴표 안의 글자는 '문자' 로 인식. "1"+"1"은 2가 아니라 11.
 
 
```java
 
## 이스케이프
public class Ex2 {

	public static void main(String[] args) {

		System.out.println("선생님이 말했다 \"아! 자바 재밌다!\"");
		
	}

}
```

이스케이프(escape) 
- 문법적인 역할에서 도망(escape) 한다는 의미
- \ 다음은 무조건 문자로 인식. 큰따옴표를 그대로 보여주고 싶은 경우는 \ 사용.

## 여러 줄 출력

```java
public class Ex2 {

	public static void main(String[] args) {

		System.out.println("선생님이 말했다 \n\"아! 자바 재밌다!\"");
		
	}

}
```

- 줄바꿈 효과를 내고 싶다면 \n 명령어 사용.


## 탭 만큼 띄우기
```java

public class Ex2 {

	public static void main(String[] args) {

		System.out.println("영어\t국어\t수학");		
		System.out.println("90\t80\t70");		     
			
			
			영어	국어	수학
			90	 80      70
			
	}

}
```

- \t는 묶어서 tab만큼 간격을 띄우는 기능
- 문자가 정렬 되는 것을 주의 깊게 확인



## 요약
• System.out.print는 ()안의 내용을 출력해 준다!

• Println은 출력후 줄바꿈! print는 단순 출력!

• 숫자이더라도 “ ” 감싸면 문자로 인식한다!

• “ ” 를 문자 그대로 출력하려면 \ 기호를 사용 해야한다!

• \n, \t등 \와 함께하는 이스케이프 문자들!

---
# 변수
## 변수(Variable)
- 자바에서 사용되는 데이터를 저장하는 메모리 공간
- 한번에 하나의 데이터만 저장 가능 => 언제든 다른 데이터로 대체될 수 있다(변할 수 있다)
- 변수를 사용하기 위해서 변수 선언이 먼저 진행이되어야함
- 변수에 데이터를 저장하는 것을 변수 초기화라고 함
 
 
```java

public class Ex3 {

	public static void main(String[] args) {
 
		
		int a;
		a=1;
		System.out.println(a+1);
		a=2;
		System.out.println(a+1);
		
			}

}

```

## 변수의 선언 및 초기화

```java

public class Ex3 {

	public static void main(String[] args) {
 
		
		// 선언과 초기화를 동시에
		int a = 10;
		int b = 20;
		
		// 동시에 여러개 변수를 선언
		int a1, b1;
		a1 = 10;
		b1 = 20;
		
		// 동시에 여러개 변수를 선언과 동시에 초기화
		int a2 = 10, b2 = 20;
		
		System.out.println(a2);
		System.out.println(b2);
		System.out.println(a2 + b2);
		
		
			}

}


```
          
## 문자 할당

- 문자열을 할당하기 위해서는 String을 사용
- “ ” 로 감싼 데이터를 저장


```java

public class Ex3 {

	public static void main(String[] args) {
	
//		String name;
//		name = "양윤석";
//		
//		System.out.println(name);
		
		
		
		String name = "양윤석";
		System.out.println(name);
		
				

	}

}

```

---

## 변수 응용 문제
 돈이 10000원 있고 사과 한 개에 1000원 이라고 할 때
- 최대 구매 가능 개수
- 사과 3개의 가격
- 사과 5개를 사고 남는 금액
- 사과 5개를 사고 쌓이는 적립금 (구매금액의 1% 라고 가정)

		System.out.println 을 사용해서 출력 해보기

```java

public class Ex4 {

	public static void main(String[] args) {
	
		int money = 10000;
		int apple = 1000;
		
		System.out.println(money/apple);
		System.out.println(apple*3);
		System.out.println(money-(apple*5));
		System.out.println(apple*5*0.01);
		
		
		
	}

}

```

- 내 돈과 사과 금액을 바꿔서 출력

```java
public class Ex4 {

	public static void main(String[] args) {
	
		int temp = apple;
		apple = money;
		money = temp;
		System.out.println("money: " + money);
		System.out.println("apple: " + apple);
		
		
	}

}
```

---

## 상수(Constant)
- 변수의 반대 개념 항상 고정된 데이터(변하지 않는 데이터)
- 실제 사용하는 데이터(상수)를 리터럴(Literal)이라고도 한다.

  ex) 정수형 리터럴 1, 실수형 리터럴 3.14 등
  
```java

public class Ex5 {

	public static void main(String[] args) {
		System.out.println(1);		// 정수형(기본형 = int) 리터럴
		System.out.println(3.14);	// 실수형(기본형 = double) 리터럴
		
		System.out.println('A');	// 문자형 리터럴(작은따옴표로 둘러싼 1개의 문자)
		System.out.println('AB');	// 오류! 문자형은 1개의 문자만 입력할 수 있음.
		
		System.out.println(true);	// 논리형(boolean형) 리터럴(true 또는 false)
		System.out.println(TRUE); 	// 오류! 예약어 true는 무조건 소문자로 써야함!
						// 대문자 TRUE는 예약어 아님
		
		System.out.println(100L); 	// 정수형(long형) 리터럴(접미사 L(구분하기 위함) 붙는다)
		System.out.println(1.5f); 	// 실수형(float형) 리터럴(접미사 f 또는 F 붙는다)
		System.out.println("Hello, World"); // 문자열형 리터럴(큰 따옴표로 둘러싼 문자들)
		System.out.println("");		// 문자가 없어도 인식함
		
		final int a;
		a = 100;
		System.out.println(a);		// int 앞에 final을 붙일 경우 변경 할 수 없는 고정상수를 만듬.
	}

}
```
