# [오전수업] 직업훈련 7차
> 상담 실시

---

# [오후수업] JSP 63차

## cookie
```
HTTP 프로토콜 = 상태가 지속적으로 유지되지 않음

세션(Session) vs 쿠키(Cookie)

1. 세션(Session)
- 서버(웹컨테이너)에 정보가 저장됨
- 웹브라우저를 종료하거나 세션 타이머가 만료되거나 서버가 재시작되면 초기화
- 주로, 로그인 정보 등을 저장하는 용도로 사용함
- 보안에 유리함

2. 쿠키(Cookie)
- 클라이언트에 정보가 파일 형태로 저장됨
- 웹브라우저가 종료되거나 서버가 재시작되어도 초기화되지 않음(서버와 무관)
- 주로, 로그인 정보에 대해 항상 로그인 또는 아이디 저장을 수행하는 옵션이 사용될 때 사용함
  또는, 자주 사용되는 웹사이트 이미지를 매번 서버로부터 다운로드 받지 않고
  클라이언트에 저장하여 로딩 시간을 줄이는 용도로도 사용함
- 보안에 취약함
```

```jsp
----------------------------------------------cookieTest1.jsp----------------------------------------------
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<h1>cookieTest1</h1>
	<h3><a href="cookieTest1_make.jsp">쿠키 생성하기</a></h3>
	<h3><a href="cookieTest1_use.jsp">쿠키 확인하기</a></h3>
	<h3><a href="cookieTest1_delete.jsp">쿠키 삭제하기</a></h3>	
</body>
</html>
----------------------------------------------cookieTest1_make.jsp----------------------------------------------
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%
// 1. 쿠키 생성 - Cookie 객체 생성(실제 쿠키 파일이 생성되는 것은 아니다!)
// => Cookie 객체의 생성자 파라미터로 쿠키 이름과 쿠키에 저장할 값을 지정(String 타입)
Cookie cookie = new Cookie("cookieName", "cookieValue");
// => cookieName 이라는 속성명(이름)으로 cookieValue 라는 값 저장

// 2. 생성된 쿠키의 유효기간 설정
// => Cookie 객체의 setMaxAge() 메서드를 호출하여 유효기간 지정(초 단위)
//	  만약, 생략 시 웹브라우저 동작할 동안만 쿠키가 유지돔(= 웹브라우저 종료 시 쿠키 삭제됨)
cookie.setMaxAge(60); // 60초 = 1분간 쿠키 유지됨

// 3. 쿠키를 클라이언트로 전송(= 클라이언트에 실제 쿠키 파일 생성)
// => response 객체의 addCookie() 메서드를 호출하여 생성된 Cookie 객체를 파라미터로 전달
response.addCookie(cookie);

// -----------------------------------------------------
// 새로운 쿠키 생성하여 추가하기
Cookie cookie2 = new Cookie("id", "admin");
cookie2.setMaxAge(1800);
response.addCookie(cookie2);
%>    
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<h1>쿠키 생성 페이지</h1>
	<%--
	Cookie 객체 정보 확인(임시로 생성한 페이지에서 확인)
	- getName() 메서드 : 쿠키명 확인
	- getMaxAge() 메서드 : 쿠키의 유효기간 확인
	--%>
	<h3>쿠키명 : <%=cookie.getName() %> </h3>
	<h3>쿠키값 : <%=cookie.getValue() %> </h3>	
	<h3>쿠키 유효기간 : <%=cookie.getMaxAge() %>초</h3>
	
	<button onclick="location.href='cookieTest1_use.jsp'">쿠키 확인</button>
</body>
</html>
----------------------------------------------cookieTest1_use.jsp----------------------------------------------
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<h1>클라이언트에 저장된 쿠키 사용하기</h1>
	<%
	// 1. request 객체의 헤더에 "Cookie" 문자열 존재 여부 확인
	String cookieHeader = request.getHeader("Cookie");
	
	// 2. 리턴받은 헤더 정보 문자열이 null 이 아닐 경우 쿠키가 존재한다는 의미
	if(cookieHeader != null) {
		// 3. request 객체로부터 Cookie 객체(들) 가져오기
		// => request 객체의 getCookies() 메서드를 호출하여 Cookie[] 배열 타입으로 리턴받기
		// => 복수개의 쿠키가 존재할 수 있으므로 배열로 관리되며
		//	  for문을 사용하여 배열 크기만큼 반복하면서 쿠키에 접근 가능
		Cookie[] cookies = request.getCookies();
// 		for(int i = 0; i < cookies.length; i++) {}
		for(Cookie cookie : cookies) {
			// 4. Cookie 객체의 getName() 메서드를 호출하여 쿠키 이름 가져오기
			out.println("쿠키명 : " + cookie.getName() + "<br>");
			// 5. Cookie 객체의 getValue() 메서드를 호출하여 쿠키 값 가져오가
			out.println("쿠키값 : " + cookie.getValue() + "<br>");
		}
		
	}
	%>
</body>
</html>
	%>
</body>
</html>
```
```jsp
----------------------------------------------cookieTest2.jsp----------------------------------------------
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%
// 현재 페이지의 언어 설정값을 한국어("kor-kr"로 기본 설정)
String language = "ko-kr";

// 쿠키 중에서 "language" 쿠키가 있을 경우 해당 쿠키의 값을 language 변수에 저장
String cookieHeader = request.getHeader("Cookie");
if(cookieHeader != null) {
	Cookie[] cookies = request.getCookies(); // 전체 쿠키 가져오기
	
	for(Cookie cookie : cookies) {
		// 원하는 쿠키 탐색("language" 탐색)
		if(cookie.getName().equals("language")) {
			// language 변수에 쿠키값 저장
			language = cookie.getValue();
		}
	}
}
%>    
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<!-- language 변수에 저장된 언어 설정값이 "ko-kr" 이면 한국어, "en-us"면 영어로 페이지 표시 -->
	<%if(language.equals("ko-kr")) {%>
		<h1>안녕하세요. 쿠키 예제입니다.</h1>
	<%} else if(language.equals("en-us")) {%>
		<h1>Hello, This is Cookie example.</h1>
	<%} %>
	
	<!-- 각 언어 버튼 클릭 시 cookieTest2_set.jsp 페이지로 포워딩(파라미터로 언어를 전달) -->
	<button onclick="location.href='cookieTest2_set.jsp?lang=ko-kr'">한국어</button>
	<button onclick="location.href='cookieTest2_set.jsp?lang=en-us'">영어</button>
	<!-- 
	cookieTest2_set.jsp 페이지에서 language 라는 쿠키명으로 설정값(언어)을 저장 후
	cookieTest2.jsp 페이지로 포워딩(쿠키 유효기간 : 1일)
	-->
	
</body>
</html>
----------------------------------------------cookieTest2_set.jsp----------------------------------------------
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%
Cookie cookie = new Cookie("language", request.getParameter("lang"));
cookie.setMaxAge(60 * 60 * 24); // 1일(86400초) = 60초 * 60분 * 24시간
response.addCookie(cookie);
response.sendRedirect("cookieTest2.jsp");
%>    
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>

</body>
</html>
```
---

# 이후 Spring 4.4.12.0 버전 설치 및 기본설정 실시
>> - Preferences에서 General - Editors - Text Editor - Spelling에서 스펠링 체크 해제
>> - worlspace - UTF8로 변경, Web browser - external 선택 후  Chrome,
>> - Java - Complier에서 버전 1.8 변경, Installed JREs에서 삭제 후 jre 1.8로 설정
>> - Server - Runtime Enviroments에서 톰캣 8.5 추가, Web - JSP Files 인코딩 UTF8로 변경
> - 실행 후 Help -> Marketplace에서 Eclipse Enterprise Java and Web Developer Tools & Spring Tools 3 Add-On for Spring Tools 설치
>> - Spring Tools 3 Add-On for Spring Tools은 설치 시 에러 발생하면 OK 누르고 아래 사진과 같이 진행
>> ![제목 없음](https://user-images.githubusercontent.com/95197594/167985116-55a798be-7e1e-4109-b763-1f72db997bf5.jpg)
>> - 오류 한 번 더 발생 시 체크 항목 체크하고 Trust selected 클릭 -> 다음 창에서 continue 클릭
> - 설치 후 1시방향 Open Perspective 클릭 -> Spring으로 교체
> - Spring Legacy Project -> 맨 아래 Spring MVC Project 선택 -> Next -> com.itwillbs.test 입력 후 Finish
> - 서버 추가 후 생성한 프로젝트 마우스 우클릭 -> Build Path -> Add Libraries -> Server Runtime -> 서버 선택 -> Finish
> - 생성한 서버 더블클릭 -> 포트번호 8005 입력
> - 프로젝트 마우스 오른쪽 -> Run As -> Run on Server -> 추후에 뜨는 메시지 창 Remind me later 클릭


