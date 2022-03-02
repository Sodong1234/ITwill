# [오전수업] 직업훈련 3차

---

# [오후수업] JSP 22차

## URL 정보에 파라미터값 전달
- URL 정보에 파라미터값을 포함하여 전달하는 기본문법 
  - 이동할 페이지 URL 뒤에 ? 기호를 붙이고 파라미터명=값 을 한 세트로 지정하며, 복수개의 파라미터가 있을 경우 각 파라미터간에 & 기호를 통해 구분해야함
  - 이동할페이지URL?파라미터명1=파라미터값1&파라미터명2=파라미터값2
  
```jsp
------------------------------------------------requestForm5.jsp------------------------------------------------
<body>
	<!-- 
	URL 정보에 파라미터값을 포함하여 전달하는 기본문법 
	- 이동할 페이지 URL 뒤에 ? 기호를 붙이고 파라미터명=값 을 한 세트로 지정하며,
	  복수개의 파라미터가 있을 경우 각 파라미터간에 & 기호를 통해 구분해야함
	- 이동할페이지URL?파라미터명1=파라미터값1&파라미터명2=파라미터값2 
	-->
	<input type="button" value="이동" onclick="location.href='requestPro5.jsp?id=admin&passwd=1234'">
</body>

------------------------------------------------requestPro5.jsp------------------------------------------------
<body>
	<h1>requestPro5.jsp</h1>
	<!-- URL 을 통해 파라미터를 전달하거나 form 태그의 method="get" 지정 시 GET 방식 요청 -->
	<%
	// URL 을 통해 전달받은 id, passwd 파라미터값 가져와서 변수에 저장 후 출력
	String id = request.getParameter("id");
	String passwd = request.getParameter("passwd");
	%>
	<h3>아이디 : <%=id %></h3>
	<h3>패스워드 : <%=passwd %></h3>
</body>
```

## 액션 태그(Action Tag)
- JSP 페이지에서 자바 코드 등을 사용하지 않고도 동일한 작업을 수행하도록 만든 태그(XML)
- <jsp:액션태그명 속성명="값 /> 형태로 사용하며, 태그 내에 또 다른 태그를 포함해야 하는 경우 끝 태그 </jsp:액션태그명> 을 사용하여 포함해야함
- 주로 사용하는 액션 태그명 : forward, include, useBean, setProperty 등

### forward 액션 태그
- pageContext 객체의 forward() 메서드 기능을 수행하는 태그
  - 즉, 페이지 이동 처리를 수행하는 액션 태그
- 현재 페이지가 갖고 있는 request 객체를 그대로 유지하여 페이지 이동만 수행
	- 주소표시줄의 URL 이 유지되며, request 객체의 데이터가 유지됨 = Dispatcher 방식 포워딩
- 포워딩 시 전달할 데이터는 주소(URL) 뒤에 파라미터 형식으로 붙여서 전달하거나, jsp:param 태그를 사용하여 데이터를 포함시켜 전달 가능(= input type="hidden" 과 동일)
	- 단, jsp:param 태그로 전달하는 데이터는 URL 뒤에 표시되지 않음

```jsp
 < 기본 문법 >
	  <jsp:forward page="포워딩 할 페이지" /> 또는
	  <jsp:forward page="포워딩 할 페이지">
	  		<jsp:param name="파라미터명1" value="데이터1" />
	  		<jsp:param name="파라미터명2" value="데이터2" />
	  </jsp:forward>	
```    

```jsp
------------------------------------------------forwardForm.jsp------------------------------------------------
<body>
	<h1>forwardForm.jsp - 액션태그(forward)</h1>
	<form action="forwardPro.jsp" method="post">
		<input type="hidden" name="jumin" value="901010-1234567">
		<input type="hidden" name="forwardPage" value="forwardPro2">
		
		<h3>아이디 : <input type="text" name="id"></h3>
		<h3>패스워드 : <input type="password" name="passwd"></h3>
		
		<input type="submit" value="전송">
	</form>
</body>

------------------------------------------------forwardPro.jsp------------------------------------------------
<body>

	<%
	// 폼 파라미터로 전달받은 4개의 파라미터값 가져와서 변수에 저장 후 출력
	String id = request.getParameter("id");
	String passwd = request.getParameter("passwd");
	String jumin = request.getParameter("jumin");
	String forwardPage = request.getParameter("forwardPage");
	%>

	<h1>forwardPro.jsp - 액션태그</h1>
	<h3>아이디 : <%=id %></h3>
	<h3>패스워드 : <%=passwd %></h3>
	<h3>주민번호 : <%=jumin %></h3>
	<h3>포워드 페이지 : <%=forwardPage %></h3>
	
	 <!-- pageContext 객체의 forward() 메서드를 통한 포워딩 수행 -->
	 <!-- => forwardPro2.jsp 페이지 사용 -->
<%--   <%pageContext.forward("forwardPro2.jsp"); %> --%>
   
   
	 <!-- forward 액션 태그를 사용하여 forwardPro2.jsp 페이지로 포워딩 수행 -->
	 <!-- 별도의 끝 태그 없이 시작 태그에서 바로 끝 표시 -->
<%-- 	 <jsp:forward page="forwardPro2.jsp" /> --%>
	 <!--
	 주의! 맨 앞에 / 를 붙이면 상대경로에서 최상위 루트(= 컨텍스트루트 = 프로젝트명)가 되므로
	 폴더 구조에 유의하여 페이지를 지정해야한다!
	 ex) http://localhost:8080/StudyJSP/jsp6_action_tag/forwardPro2.jsp 페이지로 이동할 경우
	 	1) page="forwardPro2.jsp"	=>	현재 위치와 같은 경로상의 forwardPro2.jsp 페이지 지정
		2) page="/jsp6_action_tag/forwardPro2.jsp"	=>	컨텍스트루트상의 jsp6_action_tag 폴더 지정
		-->
<%--	<jsp:forward page="/jsp6_action_tag/forwardPro2.jsp" />	--%>

	<!-- 포워딩 페이지를 동적으로 변화시키기 위해 변수 조합도 가능 -->
	<jsp:forward page="<%=forwardPage %>" />
	
	 <!-- 별도의 끝 태그를 사용하고, 사이에 jsp:param 태그를 통해 데이터 저장할 경우 -->
<%--	 <jsp:forward page="forwardPro2.jsp"> --%>
<%--	 	<jsp:param name="paramValue" value="param"/> --%>
<%--	 	<jsp:param name="paramValue2" value="<%=id %>"/> --%>
<%--	 </jsp:forward> --%>
	 
</body>

------------------------------------------------forwardPro2.jsp------------------------------------------------
<body>

	<%
	// 폼 파라미터로 전달받은 4개의 파라미터값 가져와서 변수에 저장 후 출력
	String id = request.getParameter("id");
	String passwd = request.getParameter("passwd");
	String jumin = request.getParameter("jumin");
	String forwardPage = request.getParameter("forwardPage");
	%>

	<h1>forwardPro2.jsp - 포워딩 페이지</h1>
	<h3>아이디 : <%=id %></h3>
	<h3>패스워드 : <%=passwd %></h3>
	<h3>주민번호 : <%=jumin %></h3>
	<h3>포워드 페이지 : <%=forwardPage %></h3>
	
	<!-- jsp:forward 액션태그의 jsp:param 태그를 통해 전달받은 파라미터 가져와서 출력 -->
	<h3>jsp:param 값1 : <%=request.getParameter("paramValue") %></h3>
	<h3>jsp:param 값2 : <%=request.getParameter("paramValue2") %></h3>

</body>
```

### include 액션태그
- 현재 페이지에 특정 페이지를 포함(include) 시키는 용도의 액션 태그
- 포함시키는 페이지로 제어권이 넘어간 후 해당 패키지의 작업이 끝나면 다시 원래의 페이지로 제어권이 돌아옴
- 이 때, forward 액션태그와 마찬가지로 include 되는 페이지에 파라미터를 전달하려면 jsp:param 액션태그를 사용하여 전달 가능

```jsp
< 기본 문법 >
	<jsp:include page="요청할 페이지" /> 또는
	<jsp:include page="요청할 페에지">
		<jsp:param ... />
	</jsp:include>	
```
```jsp
------------------------------------------------includeTest1.jsp------------------------------------------------

<body>
	<%--
	
	<%request.setCharacterEncoding("UTF-8"); %>
	
	<h1>includeTest1.jsp - 액션태그(include)</h1>
	<!-- includeTest2.jsp 페이지로 제어권이 이동하여, 해당 페이지 처리 후 결과값을 현재 위치에 포함시킴 -->
	<jsp:include page="includeTest2.jsp">
		<jsp:param name="param" value="홍길동"/>
	</jsp:include>
	
</body>

------------------------------------------------includeTest1.jsp------------------------------------------------

<body>
<%-- 	<%request.setCharacterEncoding("UTF-8"); %> --%>
	<h1>includeTest2 - 액션태그(include)</h1>
	<h3>includeTest1.jsp 로부터 전달받은 파라미터값 : <%=request.getParameter("param") %></h3>
</body>
```

탬플릿 만들기
```jsp
------------------------------------------------include_top.jsp------------------------------------------------
<body>
	<h3>include_top.jsp</h3>
	<h4>메뉴1 메뉴2 메뉴3 메뉴4 메뉴5</h4>
	<h6><%=request.getParameter("id") %>님 환영합니다.</h6>
</body>

------------------------------------------------include_bottom.jsp------------------------------------------------

<h3>include_bottom.jsp</h3>
	<h4>오시는길</h4>
	<h5>부산광역시 부산진구 동천로109 삼한골든게이트 7층</h5>
  
------------------------------------------------include_left.jsp------------------------------------------------

  <h5>include_left.jsp</h5>
<h3>메뉴1</h3>
<h3>메뉴2</h3>
<h3>메뉴3</h3>

------------------------------------------------include_test3.jsp------------------------------------------------

<body>
	<h1>includeTest3.jsp - 메인페이지</h1>
	<table border="1">
		<tr>
			<td colspan="2" width="1200" height="150">
			<%--
			상단에 include_top.jsp 페이지 포함시키기
			단, 포함할 페이지에 id 파라미터로 "admin" 값 전달하기 
			include_top.jsp 페이지에서 id 파라미터 가져와서 출력하기
			--%>
			<jsp:include page="include_top.jsp">
				<jsp:param name="id" value="admin"/>
			</jsp:include>
			</td>
		</tr>
		<tr>			
			<td width="200" height="400">
			<%-- 테이블 좌측에 include_left.jsp 페이지 포함시키기 --%>
			<jsp:include page="include_left.jsp" />
			</td>
			<td width="1000" height="400">
				<h3>includeTest3.jsp 본문</h3>
			</td>
		</tr>
		<tr>		
			<td colspan="2" width="1200" height="150">
			<%-- 테이블 하단에 include_bottom.jsp 페이지 포함시키기 --%>	
			<jsp:include page="include_bottom.jsp" />
			</td>
		</tr>
	</table>
</body>
```
![캡처](https://user-images.githubusercontent.com/95197594/156314517-a77d3aec-aae9-4386-a7bd-5500c0dcb876.PNG)


