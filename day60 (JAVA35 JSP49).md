# [오전수업] JAVA 35차

### StringBuilder & StringBuffer
- StringBuffer  : 문자열이 안전하게 변경되도록 보장(O)
- StringBuilder : 문자열이 안전하게 변경되도록 보장(X)
  - (멀티 쓰레드 프로그램 아니라면 StringBuilder 속도가 좀 더 빠름)

```java

public class Ex1 {

	public static void main(String[] args) {
		
		String javaStr = new String("Java");
		// System.identityHashCode 인스턴스가 처음 생성되었을 때 메모리 주소
		System.out.println("javaStr 문자열 주소 : " + System.identityHashCode(javaStr));
		
		StringBuilder buffer = new StringBuilder(javaStr);
		System.out.println(buffer);
		System.out.println("연산 전 buffer 메모리 주소 : " + System.identityHashCode(buffer));
		
		// 문자열 추가
		buffer.append(" and");
		buffer.append(" android");
		buffer.append(" programming is fun!!");
		System.out.println(buffer);
		System.out.println("연산 후 buffer 메모리 주소 : " + System.identityHashCode(buffer));
		
		javaStr = buffer.toString();
		System.out.println(javaStr);
		System.out.println("새로 만들어진 문자열 주소 : " + System.identityHashCode(javaStr));

		System.out.println("---------------------------------------------------");
		
		String str1 = "abc";
		String str2 = str1 + "de";
		String str3 = "abc";
		
		System.out.println(System.identityHashCode(str1));
		System.out.println(System.identityHashCode(str2));
		System.out.println(System.identityHashCode(str3));
		
		if(str1 == str3) {
			System.out.println("주소가 같다!");
		} else {
			System.out.println("주소가 다르다!");
		}
		
		System.out.println("-----------------------------------");
		
		String str = "abc";
		
		System.out.println("변경 전 주소 : " + System.identityHashCode(str));
		
		str += "de";
		
		System.out.println("변경 후 주소 : " + System.identityHashCode(str));
		
		int a = 10;
		System.out.println(System.identityHashCode(a));
		a = 20;
		System.out.println(System.identityHashCode(a));

	}

}
```

### BigInteger, BigDecimal 클래스
- java.math 패키지
- 수치데이터를 처리하는 부가적인 클래스
- 아주 큰 정수 또는 실수를 저장하거나, 실수의 문제점을 해결하는 용도로 사용

1. BigInteger 클래스
- 정수의 최대 크기(long 타입이라고 가정)는 약 922경. 이보다 크거나 작은 정수는 취급이 불가능한 문제점이 발생하여 이를 해결할 용도로 별도로 추가된 클래스(정수 기본형 데이터타입을 확장한 클래스)
- 객체 생성 시 파라미터로 전달할 정수는 "문자열" 형태로 전달함
- 정수 데이터를 내부적으로 int[] 타입으로 관리함
- toString() 메서드가 오버라이딩 되어있음
- 일반적인 산술연산자를 사용한 연산은 불가능하며 반드시 메서드를 통해 연산을 수행
	- add(), subtarct(), multiply(), divide() 등 산술 연산 메서드 제공
- 기본데이터타입으로 변환을 위해 xxxvalue() 메서드를 사용
	- xxx은 기본 데이터타입 이름
  - 각 기본데이터타입 크기에 따라 오버플로우가 발생하여 올바르지 못한 데이터가 저장될 수 있다!

2. BigDecimal 클래스
- 실수 기본형 데이터타입을 확장한 클래스
- 실수 표현방식으로 인한 정확도의 문제를 해결하기 위해 만들어진 클래스
- 실수 데이터를 내부적으로 char[] 타입으로 관리함
- 주의사항! 객체 생성 시 기본 데이터타입(float 또는 double)을 사용할 경우 값의 정확도가 떨어질 수 있으므로 문자열 형태로 생성하는 것을 추천함!
	- ex) BigDecimal bc = new BigDecimal(1.1)	// 부정확한 실수가 저장됨
  - ex) BigDecimal bc = new BigDecimal("1.1")  // 정확한 실수가 저장됨
- 기본적인 사용법(메서드)은 BigInteger 클래스와 거의 동일함         

```java
import java.math.BigDecimal;
import java.math.BigInteger;

public class Ex2 {

	public static void main(String[] args) {

//		Integer i = 10;		// Wrapper 클래스 타입은 오토박싱/오토언박싱에 의해 자동 변환됨
//		BigInteger bi = 10;	// BigInteger 클래스 타입은 자동변환 지원 안 됨
		// => 반드시 생성자를 통해 "문자열" 형태로 정수를 전달해야한다!
//		int a = 9999999999999999999999999999;
//		long l = 9999999999999999999999999999;
		BigInteger bi = new BigInteger("9999999999999999999999999999");	//	28개
		// long 타입보다 큰 정수도 문자열 형태로 전달이 가능하며 접미사 불필요
		
		// toString() 메서드가 오버라이딩 되어 있으므로 출력이 간편함
		System.out.println("저장된 숫자 : " + bi);
		
		// BigInteger -> 기본데이터타입으로 변환 : xxxValue() 메서드 사용
		int num = bi.intValue();	// BigInteger -> int로 변환(오버플로우 발생 가능성 있음)
		System.out.println(num);
		
		// 두 정수의 연산
		BigInteger bi2 = new BigInteger("55555555555555555555"); // 20개
//		System.out.println(bi + bi2); // 일반 산술 연산자 사용이 불가능!
		
		// 1. 덧셈 - add() 메서드
		System.out.println(bi + " + " + bi2 + " = " + bi.add(bi2));
		
		// 2. 뺄셈 - subtarct() 메서드
		System.out.println(bi + " - " + bi2 + " = " + bi.subtract(bi2));
		
		// 3. 곱셈 - multiply() 메서드
		System.out.println(bi + " * " + bi2 + " = " + bi.multiply(bi2));

		// 4. 나눗셈(몫) - devide() 메서드
		System.out.println(bi + " / " + bi2 + " = " + bi.divide(bi2));

		// 5. 나눗셈(나머지) - mod() 메서드
		System.out.println(bi + " % " + bi2 + " = " + bi.mod(bi2));

		// 5-2. 나눗셈(나머지) - mod() 메서드
		// => 나머지 연산에서 두 번째 피연산자가 음수일 경우 java.lang.ArithmeticException 발생(예외)
		
		System.out.println("==============================================================");
		
		double d1 = 2.0;
		double d2 = 1.1;
		System.out.println(d1);
		System.out.println(d2);
		System.out.println(d1 - d2); // 2.0 - 1.1 = 0.899999999999999로 계산됨(= 정확하지 않음)
		// 기본 데이터타입으로 객체를 생성할 경우 => 값이 부정확할 수 있음
//		BigDecimal bc1 = new BigDecimal(2.0);
//		BigDecimal bc2 = new BigDecimal(1.1);
		
		BigDecimal bc1 = new BigDecimal("2.0");
		BigDecimal bc2 = new BigDecimal("1.1");
		System.out.println(bc1);
		System.out.println(bc2);
		System.out.println(bc1 + " - " + bc2 + " = " + bc1.subtract(bc2));

		// 참고. 기본데이터타입 실수형 변수 데이터를 BigDecimal 객체로 변환하는 경우
		double d3 = 1.1;
//		BigDecimal bc3 = new BigDecimal(d3);	// 기본데이터타입 그대로 전달하면
//		System.out.println(bc3);				// 문제가 발생할 수 있음
		
		// double -> 문자열로 변환하여 BigDecimal 객체 생성하는 방법
//		BigDecimal bc3 = new BigDecimal(d3 + "");				// 널스트링과 결합
//		BigDecimal bc3 = new BigDecimal(Double.toString(d3));	// Double.toString() 사용
		BigDecimal bc3 = new BigDecimal(String.valueOf(d3));	// String.valueOf() 사용

		System.out.println(bc3);
		
		

	}

}
```

### 날짜 및 시간 정보에 대한 형식화(Formatting) 클래스
- 날짜 및 시간 정보를 개발자가 원하는 형식으로 표현하기 위한 클래스
- 형식 지정 문자를 사용하여 표현할 형식 지정
1. SimpleDateFormat 클래스
- Date 타입 객체에 대한 형식화
- 형식 지정문자
```
y : 연도(yy: 두자리, yyyy: 네자리)
M : 월
d : 일
E : 요일(E: 한글자, EEEE: 풀네임)
H : 시(0~23), h : 시(1 ~ 12), m : 분, s : 초, a : 오전/오후 표시
```

2. DateTimeFormatter 클래스
- LocalDate, LocalTime, LocalDateTime 타입 객체에 대한 형식화
- 기본적인 형식 지정문자 사용법이 SimpleDateFormat 클래스와 동일함(패턴 동일)
	- 주의사항! LocalDate 또는 LocalTime 의 경우 존재하지 않는 항목에 대해 패턴이 지정될 경우 예외가 발생할 수 있다!

```java
import java.text.SimpleDateFormat;
import java.time.LocalDateTime;
import java.time.format.DateTimeFormatter;
import java.util.Date;

public class Ex3 {

	public static void main(String[] args) {
		
		// --------- SimpleDateFormat 클래스를 사용한 Date 객체 표현 형식 변경 ----------
		Date now = new Date();	 // 오늘 날짜 및 시간 정보를 기준으로 Date 객체 생성
		System.out.println(now); // Tue Apr 19 10:22:51 KST 2022
		
		// 1. 표현 형식을 지정하기 위해 형식 지정문자를 사용한 문자열 패턴 생성
		String pattern = "yyyy년 MM월 dd일 EEEE a hh:mm:ss";
		
		// 2. SimpleDateFormat 클래스 객체 생성 => 파라미터로 패턴 문자열 전달
		SimpleDateFormat sdf = new SimpleDateFormat(pattern);
		
		// 3. SimpleDateFormat 객체의 format() 메서드를 호출하여 Date 객체 전달
		//	  => 저장된 패턴에 따라 Date 객체의 날짜 및 시간 표현을 변경한 문자열 리턴
		String formattingNow = sdf.format(now);
		System.out.println(formattingNow);
		
		// --------- DateTimeFormatter 클래스를 사용한 java.time 패키지 표현 형식 변경 ----------
		LocalDateTime now2 = LocalDateTime.now();
		System.out.println(now2);
		
		String pattern2 = "yyyy년 MM월 dd일 EEEE a hh:mm:ss"; // 패턴 사용법 동일
		
		// DateTimeFormatter 클래스의 ofPattern() 메서드를 호출하여 패턴 전달
		DateTimeFormatter dtf = DateTimeFormatter.ofPattern(pattern2);
		
		// LocalDateTime 객체의 format() 메서드를 호출하여 DateTimeFormatter 객체 전달
		System.out.println(now2.format(dtf));
		
		
	}

}
```

### 숫자 및 문자 등 기본적인 데이터에 대한 형식화 클래스
1. DecimalFormat 클래스
- 숫자 데이터에 대한 형식화 클래스
	- ex) 실수 1234.5 를 1234.50000 형식으로 표현
	- ex) 정수 1234를 ￦1,234 형식으로 표현
- 숫자 데이터를 패턴에 의해 형식화 된 문자열로 변환 : Formatting
	- format() 메서드 사용
- 문자열을 지정된 패턴에 의해 숫자 데이터로 변환 : Parsing
	- parse() 메서드 사용
	- 리턴타입이 Number 타입이며, 기본적인 형식이 long 또는 double 이므로 주의!
	- int형으로 변환하거나 float형으로 변환하면 예외 발생!
- 기존 패턴에 새로운 패턴으로 교체하여 적용
	- applyPattern() 메서드 사용
- 패턴 기호 문자
```
0 : 0 ~ 9 한자리(값이 없으면 0으로 채움)
# : 0 ~ 9 한자리(값이 없으면 자리 제거)
. : 소수점 기호
, : 1000 단위 자릿수 구분(콤마 기호 뒤의 # 또는 0의 갯수마다 자동으로 표시)
- : 음수 표시
E : 지수
```

2. MessageFormat 클래스
- 문자열과 다른 기본타입 데이터 또는 객체데이터를 연결하는 경우 일반적으로 연결연산자(+)를 사용하지만, 문자열 구조가 복잡해지는 문제 발생
- MessageFormat 클래스를 사용하면 결합될 데이터를 변수처럼 사용하여 패턴을 적용한 뒤 문자열 결합을 간편하게 수행할 수 있다.
	- 마치 printf() 메서드를 통해 문자열 결합으로 출력하는 것과 유사함

```java
import java.text.DecimalFormat;
import java.text.MessageFormat;
import java.text.ParseException;

public class Ex4 {

	public static void main(String[] args) throws ParseException {
		double dNum = 1234.5;
		
//		String pattern = "￦#,###.###"; // 형식을 지정할 패턴 문자열(￦1,234.5)
//		String pattern = "￦0,000.000"; // 형식을 지정할 패턴 문자열(￦1,234.500)
		String pattern = "￦#,###.000"; // 형식을 지정할 패턴 문자열(￦1,234.500)
		
		// 1. 기본 데이터타입 숫자데이터 -> 형식화된 문자열
		// => DecimalFormat 클래스의 format() 메서드 사용
		DecimalFormat df = new DecimalFormat(pattern);
		System.out.println(df.format(dNum));
		
		// 2. 문자열에 대한 형식 지정 -> 기본데이터타입 숫자데이터
		// => DecimalFormat 클래스의 parse() 메서드를 사용
		String strNum = df.format(dNum);
		
		// String -> double 변환 방법을 사용하면 예외 발생!
//		double parseNum = Double.parseDouble(strNum);
		
		// 반드시 DecimalFormat 클래스의 parse() 메서드를 활용해야한다!
		// => 리턴타입이 Number 타입이므로 서브클래스 타입인 Double, Long 등으로 변환 필수
		//    오토언박싱도 활용 가능
		double parseNum = (double)df.parse(strNum);
		System.out.println(parseNum);
		
		System.out.println("=====================================================");
		
		// 데이터를 결합하여 출력할 포멧을 패턴문자열로 지정
		// => 실제 데이터가 결합될 부분을 {순서번호} 형태로 표시
		String messagePattern = "이름 : {0}, 나이 : {1}, 주민번호 : {2}";
		
		// 데이터로 사용할 변수 선언
		String name = "홍길동";
		int age = 20;
		String jumin = "901010-1234567";
		
		// 패턴 문자열과 데이터를 결합하기 위해 MessageFormat 클래스의 format() 메서드 호출
		// => MessageFormat.foramt(패턴문자열, 데이터1, 데이터2, ..., 데이터n);
		// => 이때, 패턴데이터는 각각의 순서번호에 맞게 전달되며
		//    일반 변수가 아닌 데이터가 모두 포함된 배열도 전달 가능함(비정형 인자 사용됨)
		String formatStr = MessageFormat.format(messagePattern, name, age, jumin);
		System.out.println(formatStr);
		
		
		Object[] o = {name, age, jumin};
		String arrFormat = MessageFormat.format(messagePattern, o);
		System.out.println(arrFormat);
		
		System.out.println("=========================================================");
		// -----------------------------------------------------------------------
		
		Person[] pArr = {
				new Person("홍길동", 20, "901010-1234567"),
				new Person("이순신", 40, "701122-1212121"),
				new Person("강감찬", 35, "850101-1312131")
		};
		
		// 반복문을 사용하여 Person[] 배열을 반복하면서
		// 각각의 Person 객체 내의 이름(name), 나이(age), 주민번호(jumin) 데이터를
		// MessageFormat 클래스의 format() 메서드에 전달하여 형식 지정 출력
		for(int i = 0; i < pArr.length; i++) {
			Person p = pArr[i];
			String formatStr2 = MessageFormat.format(messagePattern, p.getName(), p.getAge(), p.getJumin());
			System.out.println(formatStr2);
		}
		
		// 향상된 for문을 사용하여 객체 내의 데이터 전달
		for(Person p : pArr) {
			System.out.println(MessageFormat.format(messagePattern, p.getName(), p.getAge(), p.getJumin()));
		}
		
		// ------------------------------------------------------------
		// 문자열로 결합된 3명의 데이터를 구분자 콤마(,)를 사용하여 1명씩 분할 후
		// 1명의 데이터에서 다시 구분자 콜론(:)을 사용하여 각 데이터를 분리하여
		// 포멧 문자열의 데이터로 사용
//		new Person("홍길동", 20, "901010-1234567"),
//		new Person("이순신", 40, "701122-1212121"),
//		new Person("강감찬", 35, "850101-1312131")
		String originalData = "홍길동:20:901010-1234567,이순신:40:701122-1212121,강감찬:35:850101-1312131";
		
//		String[] strArrData = originalData.split(",");
//		
//		for(int i=0; i<strArrData.length; i++) {
//			String[] splitData = strArrData[i].split(":");
//			String formatStr3 = MessageFormat.format(messagePattern, splitData);
//			System.out.println(formatStr3);
//		}
		
		// 향상된 for문
		for(String s : originalData.split(",")) {
			System.out.println(MessageFormat.format(messagePattern, s.split(":")));
		}
		
	}

}

class Person {
	
	private String name;
	private int age;
	private String jumin;
	
	public Person(String name, int age, String jumin) {
		this.name = name;
		this.age = age;
		this.jumin = jumin;
	}

	public String getName() {
		return name;
	}

	public int getAge() {
		return age;
	}

	public String getJumin() {
		return jumin;
	}
	
}
```


연습문제
```java
import java.text.SimpleDateFormat;
import java.util.Date;

public class Test3 {

	public static void main(String[] args) {
		// Date 객체를 생성하여 오늘 날짜 및 현재 시간을 다음과 같이 변환하여 출력
		// ex) 2022년 04월 19일(화) 오전 10시 33분 40초
		Date now = new Date();
		System.out.println(now);
		String pattern = "yyyy년 MM월 dd일(E) a hh시 mm분 ss초";
		
		SimpleDateFormat sdf = new SimpleDateFormat(pattern);
		String strNow = sdf.format(now);
		System.out.println(strNow);
		
		// ------------------------------------------------------
		String pattern2 = "yyyy년 MM월 dd일(E) a hh시 mm분 ss초";
		System.out.println(new SimpleDateFormat(pattern2).format(new Date()));
		
		// ------------------------------------------------------
		
		
	}

}
```

연습문제2
```
import java.text.DecimalFormat;
import java.text.ParseException;

public class Test4 {

	public static void main(String[] args) throws ParseException {
		int money = 1280000;
		// 정수 128000을 128,000원 혁식으로 변경하여 출력
		String pattern = "#,###원";
		DecimalFormat df = new DecimalFormat(pattern);
		String strMoney = df.format(money);
		System.out.println(strMoney);
		// 문자열 "128,000원"을 정수 128000으로 변경하여 출력
		long parsedMoney = (long)df.parse(strMoney);
		System.out.println(parsedMoney);
		
	}
}
```
---

# [오후수업] JSP 49차


- controller - BoardFrontController.java 클래스

```java
package controller;

import java.io.IOException;

import javax.servlet.RequestDispatcher;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import action.Action;
import vo.ActionForward;

@WebServlet("*.bo")
public class BoardFrontController extends HttpServlet {
	
	protected void doProcess(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		System.out.println("BoardFrontController");
		
		// POST 방식에 대한 한글 처리
		request.setCharacterEncoding("UTF-8");
		
		// 서블릿 주소 추출
		String command = request.getServletPath();
		System.out.println("command : " + command);
		
		// Action 클래스의 공통 타입(슈퍼클래스)인 Action 인터페이스 타입 변수 선언
		Action action = null;
		// 포워딩 정보를 관리하는 ActionForward 타입 변수 선언
		ActionForward forward = null;
		
		// 서블릿 주소 판별
		// 1. 글쓰기 폼에 대한 요청 판별("/BoardWriteForm.bo")
		if(command.equals("/BoardWriteForm.bo")) {
			
		}
		
		// ----------------------------------------------------
		// ActionForward 객체에 저장된 포워딩 정보에 따른 포워딩 작업 수행
		if(forward != null) { // ActionForward 객체가 비어있지 않을 경우
			// Redirect 방식 vs Dispatcher 방식 판별하여 각 방식으로 포워딩
			if(forward.isRedirect()) { // Redirect 방식
				response.sendRedirect(forward.getPath());
			} else { // Dispatcher 방식
				RequestDispatcher dispatcher = request.getRequestDispatcher(forward.getPath());
				dispatcher.forward(request, response);
			}
		}
	}
	
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		doProcess(request, response);
	}

	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		doProcess(request, response);
	}

}
```

- action - BoardWriteProAction.java & Action 클래스

```java
-------------------------------BoardWriteProAction.java-------------------------------
package action;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import vo.ActionForward;

/*
 * XXXAction 클래스가 공통으로 갖는 execute() 메서드를 직접 정의하지 않고
 * Action 인터페이스를 상속받아 추상메서드를 구현하여 실수를 예방 가능
 * => 추상메서드 exeucte() 구현을 강제 => 코드의 통일성과 안정성 향상 
 */
public class BoardWriteProAction implements Action {

	@Override
	public ActionForward execute(HttpServletRequest request, HttpServletResponse reponse) throws Exception {
		// TODO Auto-generated method stub
		return null;
	}

}

-------------------------------Action.java-------------------------------
package action;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import vo.ActionForward;

/*
 * FrontController 로 요청이 전달되면 비즈니스 로직의 처리 시 Action 클래스로 이동해야하는데
 * 이 때, Action 클래스의 메서드를 호출하여 request, response 객체를 전달하고
 * 요청받은 작업을 진행한 후 작업 결과를 공통의 ActionForward 객체 형태로 리턴해야함
 * => 위의 과정들을 각각의 Action 클래스에서 동일한 형태로 수행해야하므로 코드의 통일성 필요
 * => 따라서, 각 Action 클래스에서 수행할 메서드를 하나의 추상메서드로 제공하기 위해
 * 	  Action 인터페이스를 설계하고, 공통 항목인 execute() 메서드를 추상메서드로 정의한 뒤
 * 	  각 Action 클래스에서 Action 인터페이스를 상속받아 execute() 메서드를 구현하면
 * 	  코드의 통일성이 향상되고, 차후에 다형성 활용도 가능해짐
 * 
 */
public interface Action {
	// 각 요청을 받아들일 execute() 메서드를 추상메서드로 정의
	// => 파라미터 : HttpServletRequest 객체, HttpServletResponse 객체
	// => 리턴타입 : ActionForward 타입(포워딩 정보)
	// => 내부에서 발생하는 모든 예외를 외부로 위임(던짐)
	public ActionForward execute(HttpServletRequest request, HttpServletResponse reponse) throws Exception;
}

```


- vo - ActionForward.java 클래스

```java
package vo;

/*
 * FrontController(서블릿 클래스)에서 클라이언트로부터의 요청을 받아
 * Action 클래스 등에서 작업 처리 등을 수행한 후
 * View 페이지 또는 다른 서블릿 주소를 요청할 때(= 포워딩 할 때)
 * 포워딩 할 URL(주소) 와 포워딩 방식(Redircet vs Dispatcher)을 관리하기 위한 클래스 정의
 * 
 */
public class ActionForward {
	private String path; // 포워딩 주소 저장
	private boolean isRedirect; // 포워딩 방식 저장
	// true : Redirect 방식, false : Dispatcher 방식
	
	// Getter/Setter 정의
	public String getPath() {
		return path;
	}
	public void setPath(String path) {
		this.path = path;
	}
	public boolean isRedirect() {
		return isRedirect;
	}
	public void setRedirect(boolean isRedirect) {
		this.isRedirect = isRedirect;
	}
	
	
}
```

qna_board_write.jsp
```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<h1>글쓰기</h1>
</body>
</html>
```


## MVC 게시판 흐름
![MVC게시판_흐름](https://user-images.githubusercontent.com/95197594/163960943-51fb8d59-9083-4f40-8c7b-d5eba37031f6.png)


---

## MVC_Board 구조(흐름)
![MVC_Board 구조(흐름)](https://user-images.githubusercontent.com/95197594/163960956-9fca468c-1985-48ce-96ce-fee109bd1124.jpg)


