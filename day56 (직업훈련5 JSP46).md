# [오전수업] 직업훈련 5차

---

# [오후수업] JSP 46차

** index  파일
```java
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<h1>index.jsp</h1>
	<h3><a href="test.jsp">test.jsp 페이지로 이동</a></h3>
	<h3><a href="test2.jsp">test2.jsp 페이지로 이동</a></h3>
	<h3><a href="test3.jsp">test3.jsp 페이지로 이동</a></h3>
	<h3><a href="servletLifeCycle.jsp">servletLifeCycle.jsp 페이지로 이동</a></h3>
	<h3><a href="test4_redirect_dispatcher_main.jsp">로그인 페이지로 이동</a></h3>
	
</body>
</html>

```

** java 파일

```java
--------------------------------------------------------TestServlet3--------------------------------------------------------


import java.io.IOException;


import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

// web.xml 에서 <servlet> 태그와 <servlet=mapping> 태그로 수행하던 매핑 작업을
// 서블릿 클래스 상단에 @WebServlet 어노테이션으로 대체 가능(= 간편함)
// => @WebServlet("URL패턴")
// 1) 단일 패턴의 경우 "/패턴명" 형태로 작성하고, 해당 패턴과 일치하는 URL 만 감지
//	  (또한, "/디렉토리/패턴명" 형태로 디렉토리 구조로 작성도 가능)
//	  ex) "/myServlet" 의 경우 "프로젝트명/myServlet" 주소만 감지
// 2) 다중 패턴의 경우 "*.패턴명" 형태로 작성하고, "xxxxx.패턴명" 형태로 패턴명 부분만 일치하면 
// 	  복수개의 URL 을 모두 감지하여 매핑 가능함
//	  ex) '*.do" 의 경우 "abc.do" "/member/join.do" 등의 패턴을 모두 감지
@WebServlet("/test3/myServlet3") // 현재 프로젝트명(컨텍스트루트) 뒤에 /myServlet3 URL 을 매핑

public class TestServlet3 extends HttpServlet { // /myServlet3 주소 요청 시 실행될 클래스 정의

	@Override
	protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
		System.out.println("GET 방식 요청에 대한 doGet() 메서드!");

	}

	@Override
	protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
		System.out.println("POST 방식 요청에 대한 doPost() 메서드!");

	}

}


--------------------------------------------------------TestServlet4--------------------------------------------------------


import java.io.IOException;


import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

// @WebServlet 어노테이션에 URL 매핑 시 단일 주소 매핑도 가능하지만
// 복수개의 주소를 하나의 패턴으로 묶어서 매핑도 가능함
// @WebServlet("*.패턴") => 해당 패턴으로 끝나는(xxxxx.패턴) URL은 모두 현재 클래스로 요청 전송
// => 주의! 패턴 앞에 "/" 기호를 사용하지 않는다!
@WebServlet("*.my") // => URL 주소의 마지막에 .do로 끝나는 요청 주소를 모두 매핑(감지) 
public class TestServlet4 extends HttpServlet {

	@Override
	protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
		System.out.println("GET 방식 요청에 대한 doGet() 메서드!");

	}

	@Override
	protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
		System.out.println("POST 방식 요청에 대한 doPost() 메서드!");

	}

}



--------------------------------------------------------TestServlet5--------------------------------------------------------




import java.io.IOException;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * Servlet implementation class TestServlet5
 */
@WebServlet(
		description = "서블릿연습", 
		urlPatterns = { 
				"/TestServlet5", 
				"/myServlet5"
		})
public class TestServlet5 extends HttpServlet {
	private static final long serialVersionUID = 1L;
       
    /**
     * @see HttpServlet#HttpServlet()
     */
    public TestServlet5() {
        super();
        // TODO Auto-generated constructor stub
    }

	/**
	 * @see HttpServlet#doGet(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// TODO Auto-generated method stub
		response.getWriter().append("Served at: ").append(request.getContextPath());
	}

	/**
	 * @see HttpServlet#doPost(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// TODO Auto-generated method stub
		doGet(request, response);
	}

}



--------------------------------------------------------ServletLifeCycle--------------------------------------------------------




import java.io.IOException;

import javax.servlet.ServletConfig;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

@WebServlet("/ServletLifeCycle")
public class ServletLifeCycle extends HttpServlet {
	private static final long serialVersionUID = 1L;
	
    public ServletLifeCycle() {
        super();
    }
    
    @Override
    public void init() throws ServletException {
    	// 서블릿 클래스에 대한 요청(= 서블릿 주소 요청) 시 가장 먼저 자동으로 호출됨
    	// => 서블릿 클래스 인스턴스 생성 후 인스턴스 초기화 목적으로 호출됨
    	// => 서블릿 라이프 사이클 상에서 첫 요청에 최초로 단 한 번만 실행됨(= 서버 중지 전까지)
    	
    	System.out.println("init() 메서드 호출됨!");
    	super.init();
    }
    
	@Override
	protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
		// init() 메서드가 호출된 후 자동으로 호출됨
		// => 서블릿 클래스 인스턴스 내에서 쓰레드가 한 개 생성됨
		// => 요청이 있을 때 마다(= 서블릿이 호출될 때마다) 매번 호출됨(= 매번 쓰레드가 생성됨)
		System.out.println("service() 메서드 호출됨!");
		super.service(req, resp);
	}


	@Override
	public void destroy() {
		// 톰캣 서비스가 중지(= 종료)될 때 자동으로 호출됨(= 서버 stop 시)
		System.out.println("destroy() 메서드 호출됨!");
		super.destroy();
	}




	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		System.out.println("doGet() 메서드 호출됨!");
		// GET 방식 요청일 경우 자동으로 호출되는 메서드
		// => 공통으로 작업을 수행할 doProcess() 메서드 호출
		doProcess(request, response);
	}

	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		System.out.println("doPost() 메서드 호출됨!");
		// POST 방식 요청일 경우 자동으로 호출되는 메서드
		// => 공통으로 작업을 수행할 doProcess() 메서드 호출
		doProcess(request, response);
	}
	
	// GET 방식과 POST 방식 요청을 하나의 메서드로 처리하는 경우
	protected void doProcess(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		System.out.println("GET & POST 방식에 대해 공통 작업을 수행할 doProcess() 메서드 호출됨!");
	}

}



--------------------------------------------------------RedirectServlet--------------------------------------------------------






import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

@WebServlet("/redirectServlet")
public class redirectServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;
       
    public redirectServlet() {
    }

	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		System.out.println("RedirectServlet - doGet()");
		
		// 로그인 페이지에서 입력받은 아이디, 패스워드 가져와서 변수에 저장 후 출력
		// => 이전 페이지의 요청은 모두 request 객체가 관리함
		String id = request.getParameter("id");
		int passwd = Integer.parseInt(request.getParameter("passwd"));
//		System.out.println("아이디 : " + id);
//		System.out.println("패스워드 : " + passwd);
		
		// 자바 클래스에서 웹 페이지에 HTML 문서 내용 출력해야하는 경우
		// response 객체의 setContentType() 메서드를 통해 컨텐츠(문서) 타입 설정 및
		// response 객체의 getWriter() 메서드를 통해 PrintWriter 객체(out) 가져와서 HTML 태그 출력
		response.setContentType("text/html; charset=UTF-8");
		PrintWriter out = response.getWriter();
		out.println("<h3>아이디 : " + id + "</h3>");
		out.println("<h3>패스워드 : " + passwd + "</h3>");

		// -----------------------------------------------------
		// 현재 서블릿 클래스에서 작업 완료 후 redirect_result.jsp 페이지로 포워딩(이동)
		// => Redirect 방식 포워딩 수행
		// => response 객체의 sendRedirect() 메서드를 호출하여 포워딩 할 페이지를 전달
		response.sendRedirect("test4_redirect_result.jsp");
		
		// Redirect 방식 특징1. 포워딩 시 포워딩 주소가 주소표시줄에 표시됨(새 주소로 갱신됨)
		// Redirect 방식 특징2. 이전 요청의 request 객체가 유지되지 않음(= 새 request 객체 생성됨)
		//					  따라서, 이전에 저장되어 있던 파라미터는 존재하지 않음
		
	}

}





--------------------------------------------------------DispatcherServlet--------------------------------------------------------





import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.RequestDispatcher;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

@WebServlet("/dispatcherServlet")
public class dispatcherServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;
       
    public dispatcherServlet() {
    }

	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		System.out.println("RedirectServlet - doGet()");
		
		// 로그인 페이지에서 입력받은 아이디, 패스워드 가져와서 변수에 저장 후 출력
		// => 이전 페이지의 요청은 모두 request 객체가 관리함
		String id = request.getParameter("id");
		int passwd = Integer.parseInt(request.getParameter("passwd"));
		System.out.println("아이디 : " + id);
		System.out.println("패스워드 : " + passwd);
		
		// 자바 클래스에서 웹 페이지에 HTML 문서 내용 출력해야하는 경우
		// response 객체의 setContentType() 메서드를 통해 컨텐츠(문서) 타입 설정 및
		// response 객체의 getWriter() 메서드를 통해 PrintWriter 객체(out) 가져와서 HTML 태그 출력
		response.setContentType("text/html; charset=UTF-8");
		PrintWriter out = response.getWriter();
		out.println("<h3>아이디 : " + id + "</h3>");
		out.println("<h3>패스워드 : " + passwd + "</h3>");
		
		// -----------------------------------------------------
		// 현재 서블릿 클래스에서 작업 완료 후 dispatcher_result.jsp 페이지로 포워딩(이동)
		// => Dispatcher 방식 포워딩 수행
		// 1. request 객체의 getRequestDispatcher() 메서드를 호출하여 포워딩 할 페이지를 전달
		//	  => 리턴타입 : javax.servlet.RequestDispatcher
		RequestDispatcher dispatcher = request.getRequestDispatcher("test4_dispatcher_result.jsp");
		// 2. RequestDispatcer 객체의 forward() 메서드를 호출하여 포워딩 작업 수행
		// => 파라미터 : request 객체, response 객체
		dispatcher.forward(request, response);
		
		// Dispatcher 방식 특징1. 포워딩 시 포워딩 주소가 주소표시줄에 표시되지 않고 
		//						이전의 요청 주소가 그대로 유지됨(주소표시줄 주소가 변경X)
		// Redirect 방식 특징2. 포워딩 시점에서 원래 가지고 있던 request 객체를 함께 전달하므로
		//					  포워딩 후에도 request 객체가 그대로 유지됨
		//					  => 원래 저장되어 있던 파라미터 값도 그대로 유지됨(새 페이지에서 공유)
	}
		
		protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
			doGet(request, response);
	}

}


```

** jsp 파일
```jsp

--------------------------------------------------------test--------------------------------------------------------


<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<h1>test.jsp - 서블릿 기초</h1>
	
	<!-- 하이퍼링크를 사용한 서블릿 요청은 GET 방식으로 요청이 발생함 -->
	<h3><a href="myServlet">myServlet 서블릿 주소 요청(GET)</a></h3>
	
	<!-- form 태그를 사용한 서블릿 요청 시 GET 방식은 method 미지정 또는 method="get" 필요 -->
	<form action="myServlet">
		<input type="submit" value="myServlet 서블릿 주소 요청(GET)">
	</form>
	
	<!-- form 태그를 사용한 서블릿 요청 시 POST 방식은 method="POST" 필수 -->
	<form action="myServlet" method="post">
		<input type="submit" value="myServlet 서블릿 주소 요청(POST)">
	</form>
</body>
</html>


--------------------------------------------------------test2--------------------------------------------------------


<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<h1>test2.jsp - 서블릿 기초</h1>
	
	<!-- form 태그를 사용한 서블릿 요청 시 GET 방식은 method 미지정 또는 method="get" 필요 -->
	<form action="myServlet2">
		이름 : <input type="text" name="name"><br>
		나이 : <input type="text" name="age"><br>
		<input type="submit" value="myServlet2 서블릿 주소 요청(GET)">
	</form>
	
	<!-- form 태그를 사용한 서블릿 요청 시 POST 방식은 method="POST" 필수 -->
	<form action="myServlet2" method="post">
		이름 : <input type="text" name="name"><br>
		나이 : <input type="text" name="age"><br>
		<input type="submit" value="myServlet2 서블릿 주소 요청(POST)">
	</form>
</body>
</html>


--------------------------------------------------------test2_result--------------------------------------------------------



<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<h1>test2.jsp - 서블릿 기초</h1>
	
	<!-- form 태그를 사용한 서블릿 요청 시 GET 방식은 method 미지정 또는 method="get" 필요 -->
	<form action="myServlet2">
		이름 : <input type="text" name="name"><br>
		나이 : <input type="text" name="age"><br>
		<input type="submit" value="myServlet2 서블릿 주소 요청(GET)">
	</form>
	
	<!-- form 태그를 사용한 서블릿 요청 시 POST 방식은 method="POST" 필수 -->
	<form action="myServlet2" method="post">
		이름 : <input type="text" name="name"><br>
		나이 : <input type="text" name="age"><br>
		<input type="submit" value="myServlet2 서블릿 주소 요청(POST)">
	</form>
</body>
</html>



--------------------------------------------------------test3--------------------------------------------------------


<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<h1>test3.jsp - 서블릿 기초</h1>
	
	<!-- form 태그를 사용한 서블릿 요청 시 GET 방식은 method 미지정 또는 method="get" 필요 -->
	<form action="myServlet3">
		<input type="submit" value="myServlet3 서블릿 주소 요청(GET)">
	</form>
	
	<!-- form 태그를 사용한 서블릿 요청 시 POST 방식은 method="POST" 필수 -->
	<form action="myServlet3" method="post">
		<input type="submit" value="myServlet3 서블릿 주소 요청(POST)">
	</form>
</body>
</html>


--------------------------------------------------------test4_redirect_dispatcher_main--------------------------------------------------------


<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<h1>로그인</h1>
	
	<!-- form 태그를 사용한 서블릿 요청 시 GET 방식은 method 미지정 또는 method="get" 필요 -->
	<form action="redirectServlet"> <!-- RedirectServlet 클래스의 doGet() 메서드 호출-->
		아이디 : <input type="text" name="id"><br>
		패스워드 : <input type="password" name="passwd"><br>
		<input type="submit" value="로그인(Redirect)">
	</form>
	
	<!-- form 태그를 사용한 서블릿 요청 시 POST 방식은 method="POST" 필수 -->
	<form action="dispatcherServlet"> <!-- DispatcherServlet 클래스의 doGet() 메서드 호출-->
		아이디 : <input type="text" name="id"><br>
		패스워드 : <input type="password" name="passwd"><br>
		<input type="submit" value="로그인(Dispatcher)">
	</form>
</body>
</html>


--------------------------------------------------------servletLifeCycle--------------------------------------------------------


<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<h1>servletLiftCycle.jsp - 서블릿 생명주기</h1>
	
	<!-- form 태그를 사용한 서블릿 요청 시 GET 방식은 method 미지정 또는 method="get" 필요 -->
	<form action="ServletLifeCycle">
		<input type="submit" value="myServlet3 서블릿 주소 요청(GET)">
	</form>
	
	<!-- form 태그를 사용한 서블릿 요청 시 POST 방식은 method="POST" 필수 -->
	<form action="ServletLifeCycle" method="post">
		<input type="submit" value="myServlet3 서블릿 주소 요청(POST)">
	</form>
</body>
</html>

```


