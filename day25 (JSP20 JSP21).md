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
