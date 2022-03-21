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
