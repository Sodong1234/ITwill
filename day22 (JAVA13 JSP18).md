# [오전수업] JAVA 13차
## 변수 정리
```java
package static_var;




public class Ex1 {

	public static void main(String[] args) {
		/*
		 * 변수 선언 위치 따른 분류
		 * 1. 로컬 변수(Local Variable)
		 * - 메서드 내부에서 선언된 변수(for문, if문 등 포함)
		 * - 주로 메서드의 중괄호 내부에서 선언되는 변수를 말하며
		 * 	 for문의 소괄호() 에서 선언되는 제어변수도 로컬변수에 해당
		 * 	 => 메서드 파라미터 부분인 소괄호() 에서 선언되는 변수를 별도로
		 * 	 파라미터 변수라고도 하지만, 로켤 변수로 통일해도 무관함
		 * - 해당 변수가 선언된 위치로부터 소속된 중괄호가 끝나는 지점까지만 접근 가능
		 * 	 => 라이프 사이클(Life Cycle)이라고도 하며, 
		 * 	 로컬 변수의 라이프 사이크클은 선언 지점부터 중괄호가 끝나는 지점까지.
		 * - 반드시 초기화 후에 사용해야함
		 * 
		 * 2. 멤버변수(Member Variable)
		 * - 클래스 내부, 메서드 외부에서 선언되는 변수
		 * - 클래스 내의 어느 위치에서라도 동일한 변수로 취급됨(메모리 생성 시점은 다름)
		 * - 클래스 내의 생성자나 메서드 등에서 접근 가능
		 * - 별도의 초기화를 하지 않을 경우 기본값으로 자동 초기화
		 * 
		 * 		1) 인스턴스 멤버변수
		 * 			- 멤버변수의 선언부 접근제한자 뒤(리턴타입 앞)에 아무것도 붙지 않은 변수
		 * 			- 인스턴스가 생성될 때마다 각각의 공간이 새로 할당되므로
		 * 			  인스턴스가 다르면 인스턴스 멤버변수(저장공간)가 다르며,
		 * 			  인스턴스 멤버변수에 저장되는 값이 다를 수 있음
		 * 			- 인스턴스가 생성(new) 되는 시점에 메모리에 로딩(생성)되며
		 * 			  인스턴스가 제거되는 시점에 메모리에서 제거됨
		 * 		2) 클래스(정적, static) 멤버변수
		 * 			- 멤버 변수의 선언부 접근제한자 뒤(리턴타입 앞)에
		 * 			  static 키워드를 붙여 선언한 변수
		 * 			- 공유한다, 메모리에 먼저 로딩한다
		 * 
		 */			
	

		VariableEx ve = new VariableEx();
		
		// 외부 클래스에서 특정 인스턴스 내의 인스턴스 변수에 접근하려면 
		// 해당 인스턴스를 생성한 후에 인스턴스의 참조변수를 통해 접근 할 수 있음
		System.out.println(ve.instanceMember);
		
		// 클래스 변수도 동일한 방법으로 접근 가능
		System.out.println(ve.classMember);
		
		// 그러나! 인스턴스 내의 메서드 내에서 선언된 로컬 변수는 
		// 외부에서 접근 할 수 없음 => 해당 메서드 내에서만 접근 가능
		// 외부에서 멤버 메서드를 호출 할 수는 있음
		ve.instanceMethod("파라미터 변수");
		
	}

}


class VariableEx {
	// 멤버변수 선언
	String instanceMember = "인스턴스 멤버 변수";
	String instanceMember2; // 초기화 하지 않을 경우
	static String classMember = "클래스 멤버 변수";
	
	// 멤버 메서드 정의
	public void instanceMethod(String parameterVariable) {
		String localVariable = "로컬 변수";
		
		// 각 변수에 접근
		System.out.println(instanceMember2);
		System.out.println(classMember);
		
		System.out.println(parameterVariable);
		System.out.println(localVariable);
		
		// 로컬 변수는 반드시 초기화 후에 사용해야함
		String localVariable2;
//		System.out.println(localVariable2);
		
		for(int i = 0; i <= 3; i++) { // 제어변수 i = 로컬변수
			System.out.println(i);
		}
		// 로컬 변수로 선언된 제어변수 i는 for문 중괄호가 끝난 후에는 제거되므로 접근 불가
//		System.out.println(i);
	}
	
	public void instanceMethod2() {
		System.out.println(instanceMember);
		System.out.println(classMember);
		
		// 자신의 메서드 내에서 선언되지 않은 로컬변수는 접근 불가
//		System.out.println(parameterVariable); // 오류! 다른 메서드의 로컬(파라미터) 변수
//		System.out.println(localVariable); // 오류! 다른 메서드의 로컯변수
	}
	
}
```

## static
```java
package static_var;

public class Ex2 {

	public static void main(String[] args) {
		/*
		 * < 자바 프로그램 실행 과정 >
		 * 1. 소스 코드 작성 및 컴파일(번역) 후 클래스 실행(Ctrl + F11)
		 * 2. 클래스 로딩 - 클래스(정적) 멤버변수 및 메서드가 메모리에 로딩
		 * 3. main() 메서드 호출(실행)
		 * 4. 인스턴스 생성 - 인스턴스 멤버변수 및 메서드가 메모리에 로딩
		 * 5. 메서드 호출(실행) - 메서드 내의 로컬변수가 메모리에 로딩
		 * 6. 결과 출력
		 * 
		 * < static 키워드 >
		 * - 정적(고정된) 이라는 의미를 갖는 키워드
		 * - 클래스, 메서드, 변수의 지정자로 사용 가능
		 * - 멤버 메서드 또는 변수에 static 키워드를 사용할 경우
		 * 	 인스턴스 생성과 관계없이 클래스가 로딩되는 시점에 함께 메모리에 로딩됨
		 * 	 => 인스턴스 생성 전에 메모리에 로딩됨
		 * 		따라서, 인스턴스 생성 없이도 접근 가능(클래스명만으로 접근 가능)
		 * 
		 * < static 멤버변수 >
		 * - 멤버변수 선언부 데이터타입 앞에 static 키워드를 사용하여 선언함
		 * - 인스턴스 생성 전, 클래스가 메모리에 로딩 될 때 static 변수도 함께 로딩됨
		 * 	 => Heap 공간이 아닌, Method Area에 변수가 생성됨(공유 영역)
		 * - 모든 인스턴스가 하나의 변수(메모리)를 공유함
		 * 	 (= 클래스당 하나의 변수(메모리)만 생성되며, 해당 변수를 모든 인스턴스가 공유)
		 * - 참조변수 없이 클래스 명만으로 해당 멤버에 접근 가능
		 * 	 (클래스명.멤버변수명)	
		 * 
		 * < static 메소드 >
		 * - static 멤버변수와 마찬가지로 클래스 로딩 시 함께 메모리에 로딩되므로
		 * 	 클래스명만으로 호출 가능한 메서드
		 * 	 (클래스명.메서드명())
		 * 
		 * 
		 * 
		 */
		// static멤버 (클래스 멤버) 변수는 클래스명만으로 인스턴스 생성과 상관없이 접근 가능
		System.out.println("StaticMember.a = " + StaticMember.a);
		// static멤버로 선언되지 않은 인스턴스 변수 b는 클래스명만으로 접근 불가!
//		System.out.println("StaticMember.a = " + StaticMember.b); // 오류!

		
		StaticMember s1 = new StaticMember();
		StaticMember s2 = new StaticMember();
		System.out.println("s1.a = " + s1.a + ", s2.a = " + s2.a);
		System.out.println("s1.b = " + s1.b + ", s2.b = " + s2.b);
		
		s1.a = 99;
		
		System.out.println("s1.a = " + s1.a + ", s2.a = " + s2.a);
		System.out.println("s1.b = " + s1.b + ", s2.b = " + s2.b);
		// => s1인스턴스의 멤버변수 a의 값만 변경하더라도
		//	  static 변수이므로 모든 인스턴스가 해당 변수값을 "공유" 하게되어
		//	  s2 인스턴스의 멤버변수 a도 99가 되어 있음
		//	  => s1과 s2는 서로 다른 멤버변수가 아닌 하나의 멤버변수를 공유하고 있음
		
		System.out.println("===========================================");
		// 일반메서드와 static 메서드 호출
		s1.normalMethod(); // 참조변수를 통해서 접근
		
//		StaticMember.normalMethod(); // 오류! 일반 메서드는 클래스명만으로 호출 불가능!
		StaticMember.staticMethod(); // static 메서드는 클래스 명만으로 호출 가능!
	}

}


class StaticMember {
	static int a = 10;
	int b = 20;
	
	public void normalMethod() {
		System.out.println("일반 메서드!");
	}
	
	// 메서드 리턴타입 앞에 static 키워드를 붙이면 static(정적) 메서드가 됨
	// => 인스턴스 생성 없이 클래스명만으로 호출 가능
	public static void staticMethod() {
		System.out.println("static 메서드!");
	}
}
```
```java
package static_var;

import java.lang.reflect.Method;

class StaticMethod {
	private int normalVar = 10;			// 인스턴스 멤버변수(인스턴스 생성 시 로딩)
	private static int staticVar = 20;  // 클래스(static) 멤버변수(클래스 로딩 시 로딩)
	
	// 일반 메서드 정의
	public void normalMethod() {
		System.out.println("일반 메서드!");
		System.out.println("일반 메서드에서 인스턴스 변수 접근 : " + normalVar);
		System.out.println("일반 메서드에서 static 변수 접근 : " + staticVar);
	}
	
	// static 메서드 정의
	// => 클래스가 로딩 될 때 메모리에 함께 로딩되는 메서드
	public static void staticMethod() {
		System.out.println("static 메서드!");
//		System.out.println("static 메서드에서 인스턴스 변수 접근 : " + normalVar);
		// => static 멤버가 메모리에 로딩되는 시점에는 인스턴스 멤버는 로딩되지 않은 상태!
		//	  따라서, static 메서드 내에서는 인스턴스 멤버변수 접근이 불가능!
		System.out.println("static 메서드에서 static 변수 접근 : " + staticVar);
	}
	
	// normalVar 변수에 대한 Setter 정의
	public void setNormalVar(int normalVar) {
		this.normalVar = normalVar;
	}
	
	// staticVar 변수에 대한 Setter 정의
	public static void setStaticVar(int staticVar) {
		// 레퍼런스 this? 자신의 "인스턴스(객체) 주소"를 저장하는 참조변수
//		this.staticVar = staticVar; // 오류! static 메서드 내에서 this 사용 불가!
		// => static 멤버가 메모리에 로딩되는 시점에는 인스턴스가 생성되기 전이므로
		//	  인스턴스 주소를 저장하는 레퍼런스 this도 생성되기 전임
		//	  따라서, static 메서드 내에서는 this 사용이 불가능!
		
		// 레퍼런스 this 대신 클래스명을 사용하여 멤버변수 지정 가능
		StaticMethod.staticVar = staticVar;
		
		// static 메서드 내에서 static 메서드 호출
		staticMethod();
//		normalMethod(); // 일반 메서드 호출 불가! (인스턴스 생성 전이므로 로딩 X)
	}
}


public class Ex3 {

	public static void main(String[] args) {
		StaticMethod sm = new StaticMethod();
		sm.normalMethod();
		sm.staticMethod();
		StaticMethod.staticMethod();
	}

}
```
### static 멤버(클래스 멤버)와 인스턴스 멤버의 메모리 할당 순서
```java
package static_var;

public class Ex4 {
	
	/*
	 * < static 멤버(클래스 멤버)와 인스턴스 멤버의 메모리 할당 순서 >
	 * 0. 클래스가 메모리에 로딩
	 * 1. static 키워드가 선언된 모든 멤버(변수 및 메서드)가 메모리에 로딩됨
	 * 2. static 키워드가 선언된 멤버변수 a, c를 로딩하기 위해서 
	 * 	  우변의 check() 메서드가 실행되어야 하므로 check() 메서드가 호출
	 * 	  => 이 때, check() 메서드도 static 메서드이므로 메모리에 로딩된 상태
	 * 	  => a와 c 중 윗줄의 코드부터 차례대로 실행되므로
	 * 		1) static int a가 로딩되기 때문에 check(1) 이 호출되어 "call : 1" 출력됨
	 * 		2) static int c가 로딩되기 때문에 check(3) 가 호출되어 "call : 3" 출력됨
	 * 3. 모든 static 멤버의 로딩이 끝난 후 main() 메서드가 자동으로 호출 됨(프로그램 시작점)
	 * 	  => "main() 메서드" 출력됨
	 * 4. main() 메서드 내에서 Ex클래스의 인스턴스가 생성됨
	 * 	  => 인스턴스 생성 시 인스턴스 멤버변수 int b가 메모리에 로딩 됨
	 * 	  	 이 때, int b의 값 할당을 위해 check(2) 메서드가 호출되므로
	 * 		 "call : 2" 출력됨
	 * 	 */
	int b = check(2);
	
	static int a = check(1);
	
	public static int check(int i) {
		System.out.println("call : " + i);
		return 0;
	}
	
	public static void main(String[] args) {
		System.out.println("main() 메서드");
		Ex4 ex = new Ex4();
	}
	static int c = check(3);
}
```
## 싱글톤
```java
package static_var;

public class Ex5 {

	public static void main(String[] args) {
		/*
		 * 
		 * 싱글톤(singleton)
		 * - 애플리케이션이 시작 될 때 어떤 클래스가 최초 한 번만 메모리를 할당하고(static)
		 *   그 메모리에 인스턴스를 만들어 사용하는 디자인 패턴
		 * - 일반적으로 java에서는 생성자를 private로 선언해서 생성 불가능하게 하고
		 * 	 getInstance() 메서드 작성해서 객체를 받도록 구현
		 * 	 => 싱글톤 패턴은 단 하나의 인스턴스를 생성해 사용하는 디자인 패턴
		 * 
		 * [ 싱글톤 패턴을 쓰는 이유(장점) ]
		 * 1. 고정된 메모리 영역을 얻으면서 한 번의 new로 인스턴스를 사용하기 때문에 메모리 낭비를 방지 할 수 있음
		 * 2. 싱글톤으로 만들어진 클래스의 인스턴스는 전역 인스턴스이기 때문에 다른 클래스의 인스턴스들이 데이터를 공유하기 쉽다. DBCP(Database Connection Poll) 처럼 공통된 객체를
		 *    여러 인스턴스 변수로 사용해야 하는 상황에서 많이 사용
		 *    (쓰레드풀, 캐시 대화상자, 사용자 설정, 레지스트리 설정, 로그기록 객체 등)
		 * 3. 안드로이드 앱 같은 경우 각 액티비티나 클래스별로 주요 클래스들을 일일이 전달하기가 번거롭기 때문에
		 * 	  싱글톤 클래스를 만들어서 어디서나 접근하도록 설계
		 * 
		 * => 결론. 인스턴스가 절대적으로 한개만 존재하는 것을 보장하고 싶을 때
		 * 
		 * [ 문제점 ]
		 * 1. 싱글톤 인스턴스가 너무 많은 일을 하거나 많은 데이터를 공유시킬 경우
		 * 	  다른 클래스의 인스턴스들 간에 결합도가 높아져 수정이(유지보수) 어려워지고 테스트 하기 어려워짐
		 * 2. 멀티쓰레드 환경에서 동기화 처리를 안 하면 인스턴스가 두 개 생성되거나 하는 경우가 발생할 수 있음.
		 * 
		 */
//		Car c = new Car(); // (X)
		Car car = Car.getInstance();
		Car car2 = Car.getInstance();
		
		System.out.println(car == car2);
		
		Person p = Person.getInstance();
		Person p2 = Person.getInstance();
		
		System.out.println(p == p2);
				
	}

}
// Real Singleton
class Car {
	private static Car car = new Car();
	private Car() {}
	static Car getInstance() {
		return car;
	}
}
// Singleton Pattern
class Person {
	private static Person person;
	private Person() {}
	static Person getInstance() {
		person = new Person();
		return person;
	}

}
```

# [오후수업] JSP 18차
> JSP 17차에 실시한 final test의 추가분을 커밋해놓았음

## JSP


```jsp
<%@page import="java.util.Calendar"%>
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<%
	Calendar c = Calendar.getInstance();
	int hour = c.get(Calendar.HOUR);
	int min = c.get(Calendar.MINUTE);
	int sec = c.get(Calendar.SECOND);
	%>    
	<h1>test01.jsp</h1>
	<h3>현재 시각은 <%=hour %>시 <%=min %>분 <%=sec %>초 입니다.</h3>
	<!-- 동작 순서
	1. 클라이언트가 웹브라우저에서 "http://localhost:8080/StudyJSP/jsp1/test01.jsp" 주소 요청
	2. 요청을 받는 웹서버가 웹어플리케이션 서버로 JSP 부분의 처리(자바코드 해석 및 처리)를 요청함 
	3. 웹어플리케이션 서버에서 처리된 자바코드(JSP)에 대한 응답을 웹서버로 전송 
	4. 웹어플리케이션 서버로부터 웹서버가 응답을 받아 사용자에게 응답할 데이터를 생성하여 응답 전송
	5. 웹서버로부터 응답을 전달받은 클라이언트(웹브라우저)가 HTML 코드 해석하여 화면 표시
	-->
	
	
	// JSP파일은 실행 후 소스보기 시 코드가 보이지 않음
	
	
</body>
</html>
```


## JSP 주석
```jsp
<%@page import="java.sql.Timestamp"%>
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<!-- 여기는 HTML 주석입니다. 이 주석은 웹브라우저 소스보기를 통해 확인이 가능합니다 -->
	<h1>test02.jsp</h1>
	<%--
	JSP 주석입니다. 이 주석은 웹브라우저 소스보기를 통해 확인이 불가능합니다.
	=> JSP 코드가 포함될 경우 HTML 주석으로는 주석 처리가 불가능하므로 반드시 JSP 주석을 사용 
	 --%>
	 <%
	 // 이 부분은 자바 코드가 기술되는 부분으로 웹브라우저에서 코드가 표시되지 않고 
	 // 서버측에서 실행 후 결과값만 전달하는 부분입니다. (자바 주석 사용 가능한 공간)
	 Timestamp now = new Timestamp(System.currentTimeMillis());
	 %>
	 <h3>현재 시각 : <%=now %></h3>
	 <hr>
	 
	 
	 <!-- HTML 태그는 HTML 주석으로 처리하여, 실행 대상에서 제외가 가능 -->
	 <!-- <h1>test02.jsp</h1> -->
	 
	 <!-- JSP 코드 부분은 HTML 주석으로 처리하더라도, 서버에서 실행됨(차후 오류발생 가능성 있음) -->
	 <!-- <h3>현재 시각 : <%=now %></h3> -->
	 
	 <!-- JSP 주석을 사용할 경우 서버에서 해당 주석 부분 자체를 실행하지 않고, 브라우저도 실행하지 않음 -->
<%-- 	 <h3>현재 시각 : <%=now %> --%>
</body>
</html>
```

## JSP 표현식과 스크립틀릿
```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%!
// 이 곳은 JSP 선언문(Declaration)으로 JSP 파일 전체(전역)에서 접근 가능한 
// 멤버변수 및 메서드를 선언하는 곳입니다.
// => 자바 클래스 내의 멤버레벨(클래스 내부, 메서드 외부)에 변수 및 메서드가 위치하는 것과 동일
// 1. 멤버변수 선언
String str1 = "멤버(전역) 변수입니다.";
// 2. 메서드 정의
public void method1() {
	System.out.println("선언문의 method1() 메서드 호출됨!");
}

public String method2() {
	//"method2()" 메서드의 리턴값" 문자열을 외부로 리턴
	return "method2() 메서드의 리턴값";
	
}
%>
<%! String str2 = "두 번째 멤버(전역) 변수입니다."; %>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<h1>test03.jsp</h1>
	<%--
	표현식 <%= %>
	- 선언문 또는 스크립틀릿에서 선언된 변수에 접근하여 값을 출력하거나 
	  메서드 호출 후 리턴되는 값을 전달받아 출력할 수 있음
	  - 자바 코드의 System.out.print() 메서드와 동잃한 코드로 동작함
	  	(단, System.out.print() 메서드는 이클립스의 콘솔에 데이터가 출력되지만
	  	표현식은 웹페이지에 출력하므로 out.print() 메서드와 동일한 역할 수행)
	 --%>
	 <h3>멤버변수 str1 = <%=str1 %></h3>
	 <h3>method2() 메서드 호출 결과 : <%=method2() %></h3>
	 
	 <%--
	 스크립틀릿 <% %>
	 - 자바 문장을 그대로 표현 가능한 블럭
	 - 스크립틀릿 내부는 자바에서 메서드 내부와 동일한 위치로 취급됨
	   => 메서드 내에서 수행 가능한 작업들을 코드로 기술 가능
	   => 자동 생성된 서블릿 클래스 내의 jsp_service() 메서드 내에 해당 코드가 모두 포함됨
	 - 스크립틀릿 내에서 선언되는 변수는 로컬(지역) 변수로 취급됨
	   또한, 메서드는 정의할 수 없다!
	  --%>
	  <%
	  // 이 곳은 스크립틀릿 내부입니다.
	  // 변수 선언도 가능하며, 해당 변수는 로컬 변수로 취급됨
	  String str3 = "로컬(지역) 변수입니다.";
	  
	  // 다른 메서드를 호출하거나, 객체 생성 등의 다양한 작업이 가능함
	  method1();
	  
	  System.out.println("이 문장은 이클립스의 콘솔창에 출력됨");
	  
	  // 만약, 스크립틀릿 내에서 웹페이지(HTML 문서, JSP 문서) 내에 데이터를 표시하고 싶을 경우
	  // out.println() 또는 out.print() 메서드를 사용하여 출력(출력 데이터에 문자열로 태그 사용 가능)
	  // => HTML 태그보다 자바 데이터가 많을 경우 주로 사용
	  // => out.println() 메서드와 out.print() 메서드 모두 줄바꿈은 일어나지 않음
	  out.println("<b>스크립틀릿 내에서 출력한 데이터</b>");
	  out.print("out.print() 로 출력한 데이터");
	  out.print("out.print() 로 출력한 데이터");
	  
	  // 스크립틀릿 내에서는 메서드 정의 불가(= 자바의 메서드 내에서도 메서드 정의 불가능)
// 	  public void method3() {} // 컴파일 에러 발생!
		  
	  
	  %>
	  <%-- 로컬변수는 반드시 변수 선언보다 아래쪽에서만 접근 가능 --%>
	  <h3>로컬변수 str3 = <%=str3 %></h3>
	  
	  <%-- 선언문보다 윗쪽에서 멤버변수에 접근하더라도 접근 가능 --%>
	  <h3>로컬변수 str4(선언문보다 위) = <%=str4 %></h3>
	  
	  <%!String str4 = "멤버변수 str4"; %>
	  <h3>로컬변수 str4(선언문보다 아래) = <%=str4 %></h3>
</body>
</html>
```

## 스크립틀릿과 표현식 연습	
```jsp
<%@page import="java.util.Calendar"%>
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<% // 스크립틀릿(자바의 메서드 내부와 동일)
// java.util 패키지의 Calendar 클래스를 사용하여 현재 시스템(서버)의 시각 정보를 
// 시, 분, 초로 분리하여 가져와서 로컬 변수에 저장하기
Calendar c = Calendar.getInstance();
int hour = c.get(Calendar.HOUR_OF_DAY);
int min = c.get(Calendar.MINUTE);
int sec = c.get(Calendar.SECOND);
%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<h1>test03-2.jsp : 스크립틀릿과 표현식 연습</h1>
	<%-- 스크립틀릿 내에서 선언된 변수는 표현식으로 출력 가능 --%>
	<h3>현재 시각 : <%=hour %>시 <%=min %>분 <%=sec %>초</h3>
	
	<%
	// 위의 현재 시각 출력문장과 동일한 결과를 갖는 스크립틀릿 코드 작성
	out.print("<h3>현재 시각 : " + hour + "시 " + min + "분 " + sec + "초</h3>");
	%>
	
	
	<%--
	스크립틀릿 내에서는 자바 문법 사용이 가능하므로 if 문 등도 사용 가능함
	=> 따라서, HTML 태그를 특정 조건에서만 실행되도록 하기 위해서 
	   if문 블록과 HTML 태그를 조합하여 사용할 수 있음
	=> 단, if문 등과 조합하여 HTML 태그를 사용할 때 두 가지 방법 중 하나를 사용
	   1) out.print() 메서드를 사용
	   	   => HTML 태그보다 자바 코드가 더 많을 경우 주로 사용
	   2) 스크립틀릿 외부에 태그 작성
	       => 자바 코드보다 HTML 태그가 더 많을 경우 주로 사용 
	--%>
	<%
	// 1번 방법. 스크립틀릿 내에서 out.print() 메서드를 사용하여 태그를 문자열로 지정하는 방법
	// if문을 사용하여 현재 시각(hour)이 12 미만이면 "오전입니다" 출력, 아니면 "오후입니다" 출력
	if(hour < 12) { // 12 보다 작을 경우 = 오전
		out.print("<h3>오전입니다.</h3>");
	} else { // 아니면 = 오후
		out.print("<h3>오후입니다.</h3>");
	}
	
	%>
	
	<%-- 2번 방법. 스크립틀릿으로 if문만 표현하고, HTML 태그는 스크립틀릿 외부에서 표현 --%>
	<%if(hour < 12) {%>
		<h3>오전입니다.</h3>
	<%} else{%>
		<h3>오후입니다.</h3>
	<%} %>
</body>
</html>
```
