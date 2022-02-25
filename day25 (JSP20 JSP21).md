# [오전수업] JSP 20차

## response 객체
- 클라이언트의 HTTP 요청(request)에 대한 HTTP 응답(response) 정보를 관리하는 객체
- 기본적으로 request 객체와 response 객체는 묶음으로 동작함(요청에 대한 응답)
- 서버에서 클라이언트에 대한 여러가지 응답 데이터 관리(변경도 가능)
- respnse.XXX() 메서드를 호출하여 값 변경 및 기능 호출 가능

```
1) 클라이언트 응답 데이터의 헤더값 변경
  => response.setHeader("헤더이름", "변경할 헤더값");
  
2) 클라이언트 응답 데이터의 컨텐츠 타입 변경
  => response.setContentType("Text/html; charset=UTF-8");
  
3) 클라이언트 쿠키값 생성 및 저장
  => response.addCookie("쿠키값");
4) 클라이언트 요청에 대한 페이지 이동 처리(= 포워딩) 처리

  => response.sendRedirect("이동할 페이지 URL");
  - 포워딩 발생 시 주소 표시줄의 URL이 이동할 페이지의 URL로 변경됨
     => 이러한 방식을 "Redirect 방식" 의 포워딩이라고 함
  => 자바스크립트에서 location.href="이동할 페이지 URL" 과 동일한 기능 수행
  => 하이퍼링크(a 태그)를 사용하여 이동하는 방식과도 동일함(반드시 링크 클릭 필요)
    ex) <a href="이동할 페이지 URL">클릭</a>
```   

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
	<h1>responseTest1.jsp</h1>

	<%
	// 1. 자바 코드 내에서 response 객체를 사용하여 페이지 이동
	// => 자바스크립트와 마찬가지로 실행되는 즉시 페이지가 이동
// 	response.sendRedirect("responseTest1_result.jsp");
	response.sendRedirect("http://www.naver.com");
	%>
	<!-- 2. 하이퍼링크를 사용하여 페이지 이동(responseTest1_result.jsp) -->
	<a href="responseTest1_result.jsp">responseTest_1.result.jsp 페이지로 이동</a>
	
	<!-- 3. 자바스크립트를 사용하여 페이지 이동 
	 단, 자바스크립트 코드가 로딩되면 즉시 해당 페이지로 이동하므로 응답 페이지만 표시됨 -->
 	<script type="text/javascript">
// location.href = "responseTest1_result.jsp";
	</script> 
	
</body>
</html>
```

### response 객체를 이용한 로그인 처리
```jsp

---------------------------------requestForm4.jsp---------------------------------
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<h1>requestPro4.jsp - 로그인</h1>

<form action="requestPro4.jsp" method="post">
		<table border="1">
			<tr>
				<td>아이디</td>
				<td><input type="text" name="id"></td>
			</tr>
			<tr>
				<td>패스워드</td>
				<td><input type="password" name="passwd"></td>
			</tr>
			<tr>
				<td colspan="2"><input type="submit" value="로그인"></td>
			</tr>
		</table>
	</form>

</body>
</html>

---------------------------------requestPro4.jsp---------------------------------
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<h1>requestPro4.jsp - 로그인 처리</h1>
	<%
	// 폼 파라미터로 전달받은 아이디, 패스워드 가져오기
	String id = request.getParameter("id");
	String passwd = request.getParameter("passwd");
	%>
	<h3>아이디 : <%=id %></h3>
	<h3>패스워드 : <%=passwd %></h3>
	
	<!-- 아이디가 "admin" 이고, 패스워드가 "1234" 일 경우
	=> requestPro4_response_login_success.jsp 페이지로 이동
	아니면, "requestPro4_response_logion_fail.jsp" 페이지로 이동
	-->
	
	<%
	if(id.equals("admin") && passwd.equals("1234")) { 
		// 아이디와 패스워드가 일치할 경우(= 로그인 성공일 경우) 
		response.sendRedirect("requestPro4_response_login_success.jsp");
	} else { 
		// 아이디 또는 패스워드가 일치하지 않을 경우(= 로그인 실패일 경우) 
		response.sendRedirect("requestPro4_response_login_fail.jsp");
	} 
	%>
</body>
</html>
```

## pageContext 객체
- JSP 페이지와 관련된 프로그램에서 다른 내장 객체를 얻어내거나 현재 페이지의 요청/응답 제어권을 다른 페이지로 넘겨주는 용도로 사용
- request, session, application 등의 내장 객체 속성을 제어하는 용도로도 사용

### pageContext 객체의 메서드
1) forward() 메서드
- pageContext 내장객체의 메서드이며, 지정된 페이지로 포워딩을 수행하는 메서드
- 포워딩 시 주소 표시줄의 주소가 새로운(요청된) 주소로 변경되지 않고 기존의 주소가 그대로 유지됨 
  - 이처럼, 새로운 요청이 발생해도 기존의 요청 주소(URL)가 그대로 유지되는(= 변경되지 않는) 이동(포워딩) 방식을 "Dispatcher 방식"의 포워딩이라고 함

2) include() 메서드
- 현재 페이지의 요청과 응답에 관한 제어권을 지정된 URL로 임시로 전달함
- 해당 include 된 페이지의 처리가 끝나면 제어권이 다시 원래 페이지로 돌아옴
- 따라서, include 로 지정된 페이지의 내용을 원래 페이지에 삽입하는 효과

```jsp
---------------------------------pageContextTest1---------------------------------
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%
/*

// response 객체의 sendRedirect() 메서드를 사용하여 pageContextTest2.jsp 페이지로 이동
// response.sendRedirect("pageContextTest2.jsp");
// => 포워딩 시 주소표시줄의 주소가 새로운 요청 주소인 "pageContextText2.jsp" 로 변경됨
//	  ex) 포워딩 결과 : http://localhost:8080/StudyJSP/jsp3/pageContext2.jsp

// pageContext 객체의 forward() 메서드를 사용하여 pageContextTest2.jsp 페이지로 이동
pageContext.forward("pageContextTest2.jsp");
// => 포워딩 시 주소표시줄의 주소가 기존의 주소인 "pageContextTest1.jsp" 로 유지되고
//	  새로운 요청 주소인 "pageContextTest2.jsp" 주소는 보이지 않는다!
//	  단, 실제 웹브라우저에 표시되는 내용은 새로운 요청 주소인 "pageContextTest2.jsp" 페이지가 표시됨
//	  ex) 포워딩 결과 : http://localhost:8080/StudyJSP/jsp3/pageContextTest1.jsp

%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<h1>pageContextTest1.jsp</h1>
	<script type="text/javascript">
	alert("확인");
	</script>
</body>
</html>

---------------------------------pageContextTest2---------------------------------

<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%
// pageContext 객체
%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<h1>pageContextTest2.jsp</h1>
	<h1>include() 메서드 호출 전</h1>
	<hr>
	<%
	// pageContext 객체의 include() 메서드를 호출하여 
	// 현재 위치에 pageContextTest2_include1.jsp를 포함(해당 페이지의 내용을 표시) 
	pageContext.include("pageContextTest2_include1.jsp");
	
	out.print("<hr>");
	
	// pageContext 객체의 include() 메서드를 호출하여 
	// 현재 위치에 pageContextTest2_include2.jsp를 포함(해당 페이지의 내용을 표시)
	pageContext.include("pageContextTest2_include2.jsp");
	%>
	<hr>
	<h1>include() 메서드 호출 후</h1>
</body>
</html>
```

![2](https://user-images.githubusercontent.com/95197594/155644885-bb22f91d-555d-409d-be80-65d1d6f2c92d.PNG)
---

## session 객체

- javax.servlet.http.HttpSession 객체를 사용하여 세션 정보를 관리하는 내장 객체
- 클라이언트와 서버 사이의 연결 정보에 대한 역할을 수행하는 객체
- 주로, 페이지와 관계없이 접속 정보 등을 유지하기 위해 사용
	- ex) 로그인 후 로그인 한 사용자 정보를 기억하여 페이지를 이동해도 로그인이 유지되도록 함
	- ex) 장바구니에 상품을 담았을 때 해당 장바구니 정보가 유지되도록 함
- 클라이언트(웹브라우저)가 서버에 접속 시 기본적으로 해당 클라이언트에 대한 기억장소(= 세션)이 생성되고 자동으로 아이디(= 세션ID) 값이 부여됨
	  
```		
[ 세션 정보 제거 방법 ]
1. 세션은 유지 시간이 존재하며, maxInactiveInterval 변수에 지정된 값만큼 세션 정보가 유지되고 
   유지 시간 동안 아무런 동작이 없을 경우 시간이 만료되어 세션 정보가 사라짐
2. 세션 타이머와 관계없이 invalidate() 메서드를 호출하면 세션이 초기화
3. 웹브라우저를 종료하면 세션이 초기화(동일한 웹브라우저 여러 개 실행 시 모두 종료할 경우)
```
