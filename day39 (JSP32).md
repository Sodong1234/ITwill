# [오전수업] JSP 32차

## EL(Expression Language)
- JSP 스크립트 태그(표현식 <%= %>) 대신 사용되는 데이터 처리 표현 방식(= 언어)
- ${데이터} 형식으로 데이터를 표현
  - ex) test 라는 변수값을 EL 로 표현할 경우 ${test}
- JSP 스크립트 태그는 다른 태그와 혼동을 가져올 수 있으나, EL 은 이러한 단점을 보완
- 변수값만 사용 가능한 것이 아니라 제공되는 내장객체 및 연산자도 활용 가능
  - ex) 이전 페이지로부터 전달받은 파라미터값에 접근 : ${param.파라미터명} 형식으로 접근
- 내장객체 : param(단일 파라미터), paramValues(복수 파라미터 = 배열), pageScope, requestScope, sessionScope, applicationScope 등의 영역 관련 객체 등
- EL 문법은 기본적으로 지정된 데이터로 파싱(변환)되는데 자동으로 파싱되지 않도록(= 데이터로 취급되지 않고, 문자 자체로 취급되도록) 하려면 $ 기호 안에 \ 기호를 붙여서 표현할 경우 단순 텍스트로 취급됨
	- ex) ${name} 의 경우 name 변수를 표현하지만, \${name} 은 ${name} 이라는 텍스트로 사용됨


```jsp

------------------------------------------------------- test1.jsp-------------------------------------------------------
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%
// JSP 내장 객체인 session 객체를 활용하여 "testValue" 라는 속성에 "Session value" 값(문자열) 저장
// => 요청 처리 페이지(test1_result.jsp)에서 EL 의 내장 객체로 접근할 데이터 저장
session.setAttribute("testValue", "Session value");
%>    
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<h1>JSTL - test1.jsp</h1>
	<form action="test1_result.jsp">
		이름 <input type="text" name="name">
		<input type="submit" value="전송">
	</form>
</body>
</html>

------------------------------------------------------- test1_result.jsp-------------------------------------------------------

<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<%--
	EL(Expression Language)
	- JSP 스크립트 태그(표현식 <%= %>) 대신 사용되는 데이터 처리 표현 방식(= 언어)
	- ${데이터} 형식으로 데이터를 표현
	  ex) test 라는 변수값을 EL 로 표현할 경우 ${test}
	- JSP 스크립트 태그는 다른 태그와 혼동을 가져올 수 있으나, EL 은 이러한 단점을 보완
	- 변수값만 사용 가능한 것이 아니라 제공되는 내장객체 및 연산자도 활용 가능
	  ex) 이전 페이지로부터 전달받은 파라미터값에 접근 : ${param.파라미터명} 형식으로 접근
	- 내장객체 : param(단일 파라미터), paramValues(복수 파라미터 = 배열),
			   pageScope, requestScope, sessionScope, applicationScope 등의 영역 관련 객체 등
	- EL 문법은 기본적으로 지정된 데이터로 파싱(변환)되는데
	  자동으로 파싱되지 않도록(= 데이터로 취급되지 않고, 문자 자체로 취급되도록) 하려면
	  $ 기호 안에 \ 기호를 붙여서 표현할 경우 단순 텍스트로 취급됨
	  ex) ${name} 의 경우 name 변수를 표현하지만, \${name} 은 ${name} 이라는 텍스트로 사용됨
	 --%>
	<h3>이름(request 객체 활용) : <%=request.getParameter("name")%></h3>
	<!-- request.getParameter() 대신 param.파라미터명 으로 지정 가능 -->
	<h3>이름(EL 활용) : ${param.name }</h3>
	<hr>
	<h3>세션 객체의 testValue 속성값(request 객체 활용) : <%=session.getAttribute("testValue") %></h3>
	<!-- session.getAttribute() 대신 sessionScope.속성명 으로 지정 가능 -->
	<h3>세션 객체의 testValue 속성값(EL 활용) : ${sessionScope.testValue }</h3>
	<hr>
	<!-- EL 문법을 텍스트로 취급해야할 경우 -->
	<h3>${param.name } : ${param.name }</h3>
	<h3>${sessionScope.testValue } : ${sessionScope.testValue }</h3>
	<hr>
	<!-- EL 에서의 연산자 사용 -->
	<h3>\${10 + 20 } = ${10 + 20 }</h3>
	<h3>\${20 / 2 } = ${20 / 2 }</h3>
<%-- 	<h3>\${20 div 2 } = ${20 div 2 }</h3> <!-- / 연산자 대신 div 키워드 사용 가능하나 잘 사용 X --> --%>
	<h3>\${10 >= 20 } = ${10 >= 20 }</h3>
	<h3>\${10 ge 20 } = ${10 ge 20 }</h3> <!-- 10 >= 20 과 동일 -->
	<h3>\${20 == 20 } = ${20 == 20 }</h3>
	<h3>\${20 eq 20 } = ${20 eq 20 }</h3> <!-- 10 == 20 과 동일 -->
</body>
</html>

```

## JSTL(JSP Standard Tag Library)
- JSP 에서 사용 가능한 커스텀 태그 라이브러리
- JSTL 을 사용하려면 라이브러리 다운로드 및 Build path 추가 필요
- 또한, JSP 파일 내에서 라이브러리를 사용하기 위한 선언문 작성 필요
  - => 라이브러리 등록 방법 : <%@ taglib %> 디렉티브 활용
  	- prefix 와 uri 속성을 설정해야함
  	- prefix : 태그에서 사용할 문구(접두어)이며, 주로 c 사용(core 약자)
  	- url : 태그 라이브러리가 존재하는 웹 상의 위치(오타 발생 시 오류)

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%--
JSTL(JSP Standard Tag Library)
- JSP 에서 사용 가능한 커스텀 태그 라이브러리
- JSTL 을 사용하려면 라이브러리 다운로드 및 Build path 추가 필요
  또한, JSP 파일 내에서 라이브러리를 사용하기 위한 선언문 작성 필요
  => 라이브러리 등록 방법 : <%@ taglib %> 디렉티브 활용
  	- prefix 와 uri 속성을 설정해야함
  	  prefix : 태그에서 사용할 문구(접두어)이며, 주로 c 사용(core 약자)
  	  url : 태그 라이브러리가 존재하는 웹 상의 위치(오타 발생 시 오류)
 --%>    
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%
String strMsg = "Hello, World!";
// 자바 문법으로 생성한 일반 변수를 같은 페이지에서 JSTL(EL)을 통해 접근해야할 경우
// 내장 객체인 pageContext 객체에 저장한 뒤 사용 가능
pageContext.setAttribute("strMsg", strMsg);
%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<h1>JSTL - test2.jsp</h1>
	<!-- JSTL 태그는 반드시 시작&끝 태그로 이루어져야하며 태그 사이에 옵션이 없을 경우 끝 태그 대신 
	<시작태그 /> 로 축약 가능 -->
<%-- 	<c:set></c:set> --%>
	<c:set var="msg" value="Hello, World!" />
	<!-- EL 을 사용하여 c:set 태그로 지정한 변수값에 접근 -->
	c:set 태그로 생성한 변수 msg : ${msg }<br>
	<!-- c:out 태그를 사용하여 출력문 역할 수행 가능 -->
<%-- 	c:out 태그로 변수 msg 출력 : <c:out value="msg" /><!-- 주의! name 이 문자열로 취급됨 --><br> --%>
	c:out 태그로 변수 msg 출력 : <c:out value="${msg }" /><!-- msg 가 변수로 취급됨 --><br>
	<!-- 
	자바 문법을 통해 생성된 변수(객체 포함)에 접근하려면
	접근 전 pageContext.setAttribute() 메서드를 통해 page 객체에 저장해 놓아야 함
	=> 그렇지 않을 경우 해당 변수 접근 코드는 무시됨(출력되지 않음) -->
	자바 문법으로 생성한 변수 strMsg : ${strMsg }<br>
	c:out 태그로 자바 변수 strMsg 출력 : <c:out value="${msg }"/><br>
	c:out 태그로 자바 변수 strMsg 출력 : <c:out value="<%=strMsg %>"/><br> <!-- 기존 JSP 변수 접근 -->
	<hr>
	<!-- c:set 태그로 변수 num 선언 및 0으로 초기화(정수 등의 데이터도 ""로 지정 -->
	<c:set var="num" value="10" />
	<!-- EL 을 통해 	연산 과정에서 자동으로 해당 타입 데이터로 처리됨 -->
<%-- 	<h3>num + 10 = ${num + 10 }</h3> --%>

	<!-- if문에 해당하는 커스텀 태그 c:if (c:if text="조건식") -->
	<!-- 변수 num 값이 0보다 큰지 판별 -->
	<c:if test="${num > 0}"> <!-- 주의! 조건식도 EL 문법 사용 -->
	<!--if문과 마찬가지로 조건식 판별 결과가 true 일 경우에만 태그 사이의 문장 실행 -->
		<h3>num이 0보다 크다!</h3>
	</c:if>
	<!-- c:if 태그는 단일 if문과 동일하므로 아닐 때(false)의 조건 설정이 불가 -->
	
	<hr>
	<!-- 
	if ~ else if ~ else 문에 해당하는 커스텀 태그 c:choose, c:when, c:otherwise
	1) c:choose 태그 : 조건 판별을 시작하겠다는 선언(조건 설정하지 않음)
	2) c:when 태그 : 조건식을 기술(복수개 태그 사용하여 복수개의 조건 설정 가능, if와 else if)
	3) c:otherwise 태그 : 조건식이 일치하지 않을 경우(= else 문과 동일)
	-->
	<!--  변수 name 선언 및 임의의 이름을 저장 -->
	<c:set var="name" value="이순신" />
	<!-- 변수 name 에 저장된 이름이 "홍길동" or "이순신" or 나머지 인지 판별 -->
	<!-- 주의! 태그 사이에 HTML 주석 사용 시 오류 발생하므로 JSP 주석 사용해야함 -->
	<c:choose><%-- 조건 판별을 시작하겠다는 선언 --%>
		<%-- 실제 조건식은 c:when 태그로 지정 --%>
		<c:when test="${name eq '홍길동' }">
			<h3>홍길동 입니다!</h3>
		</c:when>
		<c:when test="${name eq '이순신' }">
			<h3>이순신 입니다!</h3>
		</c:when>
		<%-- ----------------------------------------------------------- --%>
		<%-- 만약,name 변수값이 입력되지 않았을 경우 --%>
		<c:when test="${empty name }"><%-- name == '' 과 동일 --%>
			<h3>이름 입력 필수!</h3>
		</c:when>
		<%-- ----------------------------------------------------------- --%>
		<%--else 문에 해당하는 otherwise 로 조건이 일치하지 않을 경우 실행할 문장 지정 --%>
		<c:otherwise>
			<h3>그 외 나머지 입니다!</h3>
		</c:otherwise>
	</c:choose>
</body>
</html>
```

### 반복문
```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<h1>JSTL - test3.jsp</h1>
	<!-- JSTL 에서의 반복문(c:forEach, c:forTokens) -->
	<%--
	c:forEach 문은 일반 for문과 향상된 for문 모두 적용 가능
	1) 일반 for문처럼 사용 시
		c:forEach var="변수명" begin="시작값" end="종료값" step="중감값">
		
		</c:forEach>
	 --%>
	<c:forEach var="value" begin="1" end="10" step="2">
		<%-- var 속성으로 지정한 변수(value)에 begin 부터 end 까지 step 씩 증가하는 값이 저장됨 --%>
		<h3>val 변수값 : ${value }</h3>
	</c:forEach>
	<hr>
	<%-- ------------------------------------------------------------------------- --%>
	<%
	String[] names = {"홍길동", "이순신", "강감찬"};
	
	// 자바의 향상된 for문을 사용하여 names 배열의 각 요소를 name 변수에 저장 후 출력 반복
	// for(데이터 객체로부터 꺼내서 저장할 변수 선언 : 데이터가 저장된 객체) {}
	for(String name : names) {
		%>
		<h3><%=name %></h3>
		<%
		// 또는 out.println("<h3>" + name + "<h3>"); 활용 가능
	}
	%>
	<hr>
	<%
	// 위에서 선언한 배열을 JSTL 을 통해 접근하기 위해 pageContext 객체에 저장
	pageContext.setAttribute("names", names);
	%>
	<%-- c:forEach 태그를 통해 names 배열에 접근(c:forEach var="변수명" items=${객체명}) --%>
	<c:forEach var="name" items="${names }">
		<h3>${name }</h3>
	</c:forEach>
</body>
</html>
```

> 이후 개인 프로젝트에 대한 기초설명 실시


# [오후수업] JAVA

## java.lang.Object 클래스
- 모든 클래스의 최상위 클래스
	- 별도로 상속을 받지 않는 클래스는 묵시적으로 Object 클래스를 상속받음


### toString() 메서드
- 어떤 객체의 정보를 문자열로 변환하여 리턴하는 메서드
- Object 클래스의 toString() 메서드는 객체의 클래스명과 주소값을 결합한 문자열 리턴
	- => getclass() 와 hashCode() 메서드 결과를 변형하여 문자열로 리턴해줌
- 일반적으로 객체의 정보란 객체가 가진 고유의 데이터(멤버변수값)를 의미하므로 클래스 정의 시 Object 클래스의 toString() 메서드 오버라이딩을 통해 객체의 멤버변수 값들을 문자열로 결합하여 리턴하도록 해야한다!
- 출력문 내에서 참조변수명.toString() 을 호출하여 리턴값을 바로 출력하거나 리턴값을 String 타입 변수에 저장 할 수 있다!
- 또한, 출력문 내에서 toString() 메서드 생략이 가능하므로 참조변수명을 출력문 내에 전달 시 자동으로 toString() 메서드가 호출됨
	- ex) System.out.println(str); 또는 System.out.println(str.toString());
- 일반적으로 자바에서 제공되는 대부분의 API에는 toString() 메서드가 오버라이딩 되어 있으므로 객체 내의 멤버변수 값을 쉽게 확인 가능함

```java
package Object;

public class Ex3 {

	public static void main(String[] args) {
		
		Person p1 = new Person("홍길동", 20);
		System.out.println(p1.toString()); // Object.Person@15db9742
		// toString() 메서드 호출하면 "클래스명@해시코드값" 형태로 문자열 리턴됨
		
		System.out.println("클래스명 : " + p1.getClass().getName());
		System.out.println("해시코드 : " + p1.hashCode());
		
		System.out.println("객체 p의 정보 : " + p1);
		
		System.out.println("=======================================");
		
		ToStringPerson p2 = new ToStringPerson("홍길동", 20);
		System.out.println("객체 p2의 정보 : " + p2.toString());
		System.out.println("객체 p2의 정보 : " + p2); // toString() 메서드가 생략되어 있음.
		
		// toString() 메서드가 적용된 대표적인 예 : String 클래스
		String str = "홍길동";
		System.out.println(str);
		System.out.println(str.toString());

		

	}

}



class ToStringPerson {
	String name;
	int age;
	public ToStringPerson(String name, int age) {
		this.name = name;
		this.age = age;
	}
	
	
//	@Override
//	public String toString() {
//		return name + ", " + age;
//	}
	
	// toStrong() 메서드 자동 오버라이딩 단축키 : Alt + Shift + S -> S
	@Override
	public String toString() {
		return "ToStringPerson [name=" + name + ", age=" + age + "]";
	}
	
	
}
```


- 연습문제
```java
package Object;

import java.util.Objects;

public class Test3 {

	public static void main(String[] args) {
		/*
		 * Account 클래스 정의 멤버변수 : 계좌번호(accountNo, String타입), 예금주명(ownerName, String타입), 현재잔고(balance, int타입) 
		 * 생성자 : 멤버변수 모두 전달받아 초기화하는 생성자 
		 * equals() 메서드 오버라이딩 : 모든 멤버변수 값이 같을 경우 true 리턴 
		 * toString() 메서드 오버라이딩 : 모든 멤버변수 정보를 String 타입으로 리턴
		 */
		
		Account acc1 = new Account("111-1111-111", "홍길동", 100);
		Account acc2 = new Account("111-1111-111", "홍길동", 100);
		
		if(acc1.equals(acc2)) {
			System.out.println("두 계좌는 동일!");
		} else {
			System.out.println("두 계좌는 다름!");
		}
		
		System.out.println(acc1);
		System.out.println(acc2);


	}

}

class Account {
	String accountNo;
	String ownerName;
	int balance;

	// Alt + Shift + S -> O
	public Account(String accountNo, String ownerName, int balance) {
		this.accountNo = accountNo;
		this.ownerName = ownerName;
		this.balance = balance;
	}

	// Alt + Shift + S -> H
	@Override
	public boolean equals(Object obj) {
		if (this == obj)
			return true;
		if (obj == null)
			return false;
		if (getClass() != obj.getClass())
			return false;
		Account other = (Account) obj;
		return Objects.equals(accountNo, other.accountNo) && balance == other.balance
				&& Objects.equals(ownerName, other.ownerName);
	}

	// Alt + Shift + S -> S
	@Override
	public String toString() {
		return "Account [accountNo=" + accountNo + ", ownerName=" + ownerName + ", balance=" + balance + "]";
	}
}
```


## java.lang.Math 클래스
- 수학적인 다양한 기능을 상수와 static 메서드로 제공
- 모든 멤버가 static 으로 선언되어 있으므로 클래스명만으로 접근 가능
	- ex) Math.PI, Math.random()

### Math 클래스
```java
package lang;

public class Ex1 {

	public static void main(String[] args) {
		
		// Math 클래스의 상수
		System.out.println("PI 값 : " + Math.PI);
		
		int num = -10;
		// Math 클래스의 static 메서드
		System.out.println("num의 절대값 : " + Math.abs(num));
		System.out.println("num과 20 중 큰 값 : " + Math.max(num, 20));
		System.out.println("num과 20 중 작은 값 : " + Math.min(num, 20));
		System.out.println("4의 제곱근 : " + Math.sqrt(4));
		
		// ------------------------------------------------------------------
		
		double dNum = 3.141592;
		System.out.println("실수 dNum의 소수점 첫째자리 반올림값 : " + Math.round(dNum));
		// 항상 소수점 첫째자리에서 반올림이 일어나므로 X번째 자리 반올림을 위해서는
		// 반올림할 숫자의 반올림 자리 수를 첫번째 자리에 위치하도록 변형해야한다.
		// ex) 실수 3.141592의 소수점 4번째 자리 숫자 (5)를 반올림하기 위해서는
		// 	   해당 숫자를 소수점 첫 번째 자리로 이동시키기 위한 값을 직접 곱하거나
		//	   해당 숫자에 10^(x-1) 값을 곱해야함
		//	   ex) 3.141592 * 1000 또는 3.141592 * (10^(4-1))
		//	   => 또한, 원래 자리로 숫자를 되돌리기 위해 다시 곱한 값만큼 나눗셈 수행
		//	   ex) 3.141592 * 1000 / 1000
		System.out.println("실수 dNum의 소수점 넷째자리 반올림값 : " + Math.round(dNum * 1000) / 1000.0);

		// ----------------------------------------------------------------------------------------
		/*
		 * Math.random()
		 * - 난수(임의의 수) 발생을 위한 메서드
		 * - Math.random() : 0.0 <= x < 1.0 범위의 double 타입 난수 발생
		 * 
		 * < 난수 발생 기본 공식 >
		 * 1. (정수화)(Math.random() * 상한값) : 0 ~ 상한값-1 (0 <= x < 상한값)
		 * 2. (정수화)(Math.random() * 상한값) + 1 : 1 ~ 상한값 (1 <= x <= 상한값)
		 * 3. 복합 공식 (확률적으로 난수 중복을 최소화하기 위한 공식)
		 * 	  (정수화)(Math.random() * (상한값 - 하한값 + 1) + 하한값)
		 */
		
		for(int i = 1; i <= 10; i++) {
//			System.out.println(Math.random());
			
			// 정수 1자리 범위의 난수 발생시키기 위해서는
			// Math.randon() 결과를 원하는 자릿수만큼 정수로 이동시키고
			// 남은 자리 숫자들을 제거
			System.out.println((int)(Math.random() * 10)); // 0 <= x < 10
			
			// 0을 제외하고 1부터 상한값까지의 난수 발생을 위해서는 
			// 난수 발생 결과에 +1을 수행
			System.out.println((int)(Math.random() * 10) + 1); // 1 <= x < 11 (1 ~ 10)
			
			// 0을 제외하고 x부터 상한값까지의 난수 발생을 위해서는
			// 난수 발생 결과에 +x를 수행
			System.out.println((int)(Math.random() * 10) + 5); // 5 <= x < 15 (5 ~ 14)

		}
		
		// 복합 공식 적용 시
		int upperNum = 20;	// 상한값
		int lowerNum = 1;	// 하한값
		
		for(int i = 1; i <= 10; i++) {
			// (정수화)(Math.random() * (상한값 - 하한값 + 1) + 하한값 )
			System.out.println((int)(Math.random() * (upperNum - lowerNum + 1) + lowerNum));
			
		}
	}

}
```

### Arrays 클래스
- 배열 관련 다양한 기능을 제공하는 클래스
- static 메서드가 제공되므로 클래스명만으로 호출 가능

```java
package lang;

import java.util.Arrays;

public class Ex2 {

	public static void main(String[] args) {
	
		int[] myLotto = {40, 45, 10, 33, 1, 42};
		
		// 반복문을 통하여 배열의 모든 요소 출력
		for(int i = 0; i < myLotto.length; i++) {
			System.out.print(myLotto[i] + " ");
		}
		System.out.println();
		
		// Arrays 클래스를 활용하여 배열의 모든 요소 출력
		// => Arrays.toString() 메서드를 사용하면 배열 내의 모든 요소를 문자열로 리턴해줌
		System.out.println(Arrays.toString(myLotto));

		System.out.println("----------------------------------------");
		
		// 배열 요소를 정렬(sort)
		// 정수는 오름차순(0 -> 9) 으로 정렬
		Arrays.sort(myLotto);
		System.out.println("정렬 후 : " + Arrays.toString(myLotto));
		
		System.out.println("----------------------------------------");
		// 문자열은 알파벳 오름차순(A -> Z) 으로 정렬
		String[] subject = {"Java", "Android", "Oracle", "JSP", "HTML5"};
		Arrays.sort(subject);
		System.out.println("정렬 후 : " + Arrays.toString(subject));
		
	}

}
```
