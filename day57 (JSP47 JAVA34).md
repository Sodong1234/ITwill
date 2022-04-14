# [오전수업] JSP 47차
** index 파일
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
	<h3><a href="WriteForm">글쓰기</a></h3>
	
</body>
</html>
```

** java 파일
```java
---------------------------------------------------------WriteFormServlet---------------------------------------------------------


package test4_board;

import java.io.IOException;

import javax.servlet.RequestDispatcher;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

@WebServlet("/WriteForm")
public class WriteFormServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;
       
    public WriteFormServlet() {
        super();
    }


	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// test4_board 폴더 내의 write.jsp 페이지로 포워딩 => Dispatcher 방식의 포워딩
		RequestDispatcher dispatcher = request.getRequestDispatcher("test4_board/write.jsp");
		dispatcher.forward(request, response);
	}

	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		doGet(request, response);
	}

}


---------------------------------------------------------WriteFormServlet---------------------------------------------------------


package test4_board;

import java.io.IOException;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

@WebServlet("/WritePro")
public class WriteProServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;
       
    public WriteProServlet() {
        super();
    }

	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// 작성자, 제목 가져와서 출력
		request.setCharacterEncoding("UTF-8");
		String name = request.getParameter("name");
		String subject = request.getParameter("subject");
		System.out.println("작성자 : " + name);
		System.out.println("제목 : " + subject);
		

		// 글목록 표시를 위해 List 서블릿 주소 요청
		// => 이전 request 객체 유지가 불필요하며, 새로운 요청을 발생시켜야 하므로
		//	  Redirect 방식으로 포워딩
		response.sendRedirect("List");
	}

	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		doGet(request, response);
	}

}


---------------------------------------------------------WriteFormServlet---------------------------------------------------------


package test4_board;

import java.io.IOException;

import javax.servlet.RequestDispatcher;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

@WebServlet("/List")
public class ListServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;
       
    public ListServlet() {
        super();
        // TODO Auto-generated constructor stub
    }

	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// 작성자, 제목 가져와서 출력
		// => Redirect 방식으로 포워딩했으므로 주소가 변경되고 이전 request 객체가 유지되지 않음
//		request.setCharacterEncoding("UTF-8");
//		String name = request.getParameter("name");
//		String subject = request.getParameter("subject");
//		// 따라서, 이전 request 객체에 저장된 데이터가 없으므로 null 값이 됨
//		System.out.println("작성자 : " + name);
//		System.out.println("제목 : " + subject);
		
		// list.jsp 페이지로 포워딩
		// => 이전 요청인 List 서블릿 주소 유지를 위해 Dispatcher 방식으로 포워딩
		RequestDispatcher dispatcher = request.getRequestDispatcher("/test4_board/list.jsp");
		dispatcher.forward(request, response);
		
	}

	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		doGet(request, response);
	}

}


```

** jsp 파일

```jsp

---------------------------------------------------------write---------------------------------------------------------

<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<h1>글쓰기 폼</h1>
	
	<form action="WritePro" method="post">
		작성자 : <input type="text" name="name"><br>
		제목 : <input type="text" name="subject"><br>
		<input type="submit" value="글쓰기">
	</form>
</body>
</html>


---------------------------------------------------------list--------------------------------------------------------


<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<h1>글목록</h1>
</body>
</html>


---------------------------------------------------------test4_redirect_result---------------------------------------------------------


<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<h1>test4_redirect_result.jsp</h1>
	<%
	// 이전 요청에서 저장되어 있던 아이디, 패스워드 가져와서 변수에 저장 후 출력
	String id = request.getParameter("id");
	int passwd = Integer.parseInt(request.getParameter("passwd"));
	out.println("<h3>아이디 : " + id + "</h3>");
	out.println("<h3>패스워드 : " + passwd + "</h3>");
	// => Redirect 방식으로 포워딩 했기 때문에 request 객체가 그대로 유지되지 않고
	//	  새로운 request 객체가 생성되므로 
	//	  새로 포워딩 된 현재 페이지에서는 이전의 파라미터 값을 가져올 수 없다!
	%>
</body>
</html>
	
	
---------------------------------------------------------WriteFormServlet---------------------------------------------------------


<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<h1>test4_dispatcher_result.jsp</h1>
	<%
	// 이전 요청에서 저장되어 있던 아이디, 패스워드 가져와서 변수에 저장 후 출력
	String id = request.getParameter("id");
	String passwd = request.getParameter("passwd");
	out.println("<h3>아이디 : " + id + "</h3>");
	out.println("<h3>패스워드 : " + passwd + "</h3>");
	// => Dispatcher 방식으로 포워딩 했기 때문에 이전 request 객체가 그대로 유지되고
	//	   새로 포워딩 된 현재 페이지에서도 이전 파라미터 값을 그대로 가져올 수 있다!
	%>
</body>
</html>

```

---

> studyDBCP 프로젝트 생성

- META-INF에 context.xml 파일 생성
```
<?xml version="1.0" encoding="UTF-8"?>
<Context>
<!-- 
< DBCP 설정을 위한 Context.xml 파일 설정 >
Context 태그 내에 Resource 태그를 사용하여 DBCP 정보 설정
1. name 속성(*) : 공유할 리소스 이름
				 => DB 작업 수행 코드에서 DBCP API 를 통해 불러올 때 지정할 이름
2. auth 속성 : 자원 관리를 수행할 대상(인증 대상) 지정 => 톰캣이 수행하므로 "Container" 속성값 지정
3. type 속성 : 웹 상에서 리소스(Connection 객체) 사용 시 실제 사용될 클래스 지정
			  => java.sql.DataSource 인터페이스 타입 지정
			  => name 속성에 지정된 이름을 통해 DCCP 접근 시 DataSource 타입 객체 리턴됨
4. driverClassName 속성 속성(*) : JDBC 드라이버 클래스 지정(드라이버 API 파일 - jar 파일 필요)
								ex) MySQL 의 경우 "com.mysql.jdbc.Driver"			  
								ex) Oracle 의 경우 "oracle.jdbc.OracleDriver"
5. url 속성(*) : JDBC 접속에 필요한 URL 정보 지정
				ex) MySQL 의 경우 "jdbc:mysql://접속주소:포트번호/DB명"
				ex) Oracle 의 경우 "jdbc:oracle:thin:@접속주소:포트번호:DB명"
6. username 속성(*) - DB 접속 계정명
7. password 속성(*) - DB 접속 패스워드
8. maxActive 속성 : 동시에 생성(활성화)해 놓을 Connection 갯수(일반 PC는 보통 8개 정도) - 생략 가능   
9. maxIdle 속성 : 현재 사용중인 Connection 객체 외에 여유분으로 남겨놓을 Connection 객체 - 생략 가능				  				
-->
	<Resource
		name="jdbc/MySQL" 
		auth="Container"
		type="javax.sql.DataSource"
		driverClassName="com.mysql.jdbc.Driver"
		url="jdbc:mysql://localhost:3360/study_jsp3"
		username="root"
		password="1234"
		maxActive="500"
		maxIdle="100"
	/>
</Context>
```

** java 파일
```java
---------------------------------------------------------WriteFormServlet---------------------------------------------------------




import java.io.IOException;

import javax.servlet.RequestDispatcher;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

@WebServlet("/WriteForm")
public class WriteFormServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;
       
    public WriteFormServlet() {
        super();
    }


	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		doProcess(request, response);
	}

	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		doProcess(request, response);
	}
	
	protected void doProcess(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// test4_board 폴더 내의 write.jsp 페이지로 포워딩 => Dispatcher 방식의 포워딩
		RequestDispatcher dispatcher = request.getRequestDispatcher("write.jsp");
		dispatcher.forward(request, response);
	}

}


---------------------------------------------------------WriteProServlet---------------------------------------------------------




import java.io.IOException;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

@WebServlet("/WritePro")
public class WriteProServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;
       
    public WriteProServlet() {
        super();
    }

	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		doProcess(request, response);
	}

	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		doProcess(request, response);
	}
	
	protected void doProcess(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// 작성자, 제목 가져와서 출력
		request.setCharacterEncoding("UTF-8");
		String name = request.getParameter("name");
		String subject = request.getParameter("subject");
		System.out.println("작성자 : " + name);
		System.out.println("제목 : " + subject);
	}

}


```

** jsp 파일

```jsp

---------------------------------------------------------write---------------------------------------------------------


<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<h1>글쓰기 폼</h1>
	
	<form action="WritePro" method="post">
		작성자 : <input type="text" name="name"><br>
		패스워드 : <input type="password" name="pass"><br>
		제목 : <input type="text" name="subject"><br>
		내용 : <input type="text" name="content"><br>
		<input type="submit" value="글쓰기">
	</form>
</body>
</html>


---------------------------------------------------------WriteProServlet---------------------------------------------------------


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
	<h3><a href="WriteForm">글쓰기</a></h3>
</body>
</html>


```

# [오후수업] JAVA 34차

## StringTokenizer 클래스

```java

```

* 연습문제

```java
package util;

import java.util.StringTokenizer;

public class Test1 {

	public static void main(String[] args) {
		String data = "Address:부산광역시 부산진구 동천로109, Floor:7층, Tel:051-803-0909";
		// 1. 각 항목별 분리(StringTokenizer 클래스 사용)
		// 2. 각 항목에서 실제 데이터를 분리하여 출력
		StringTokenizer st = new StringTokenizer(data, ",");
		
		while(st.hasMoreTokens()) {
//			System.out.println(st.nextToken());
			
			// 1. StringTokenizer 사용 시
//			StringTokenizer st2 = new StringTokenizer(st.nextToken(), ":");
//			st2.nextToken();
//			System.out.println(st2.nextToken());
			
			// 2. String 클래스의 split() 메서드 사용 시
			System.out.println(st.nextToken().split(":")[1]);
		}
		
		System.out.println("------------------------------------");
		
		String result = "";
		for(String str : data.split(",")) {
			result += str.split(":")[1] + " ";
		}

		System.out.println(result);
	}

}

```

## java.util.Date 클래스
- 날짜 관련 기능을 제공하는 클래스(시간 정보 포함)
- 대부분의 메서드가 deprecated 처리 되어 있음
  - 그러나, 다양한 API 들은 여전히 Date타입을 사용하는 경우가 많음
- toString() 메서드가 오버라이딩 되어 있음
  - 날짜 및 시간 정보를 문자열로 리턴해줌(단, 영어권 표현 방식)
- 기본 생성자를 호출하여 인스턴스 생성하면 시스템의 날짜 및 시간 정보를 사용하여 Date 객체를 생성하여 리턴

```java
package util;

import java.util.Date;

public class Ex2 {

	public static void main(String[] args) {
		
		Date d1 = new Date();
		System.out.println(d1);
		// "Thu Apr 14 14:55:26 KST 2022" 출력됨(영어권 날짜 및 시간 표현 방법 사용됨)
		
		// long 타입 파라미터를 전달받은 생성자 호출
		Date d2 = new Date(2000000000L);
		// 기준 날짜 및 시간으로부터 해당 long타입 값(ms)만큼 뒤의 날짜 및 시간으로 설정
		System.out.println(d2);
		
		Date d3 = new Date(2000, 6, 5);
		// => 최근에는 사용하지 않는 방법으로 파악할 필요가 없음
		System.out.println(d3);
		
		// 1900년도가 기준이 되어 현재 연도에서 뺸 값 리턴됨
		System.out.println(d1.getYear() + 1900 + "년");
		
		// 시스템상에서 0~11월 형태로 사용하므로 +1 필수
		System.out.println(d1.getMonth() + 1 + "월");
		
		System.out.println(d1.getDate() + "일");
		
		// 요일 정보를 정수로 리턴(0:일요일, 6:토요일)
		System.out.println(d1.getDay());
		
		switch (d1.getDay()) {
		case 0: System.out.println("일요일"); break;
		case 1: System.out.println("월요일"); break;
		case 2: System.out.println("화요일"); break;
		case 3: System.out.println("수요일"); break;
		case 4: System.out.println("목요일"); break;
		case 5: System.out.println("금요일"); break;
		case 6: System.out.println("토요일"); break;
		}
		
		System.out.println("--------------------------------");
		
		long diffDate = d1.getTime() - d2.getTime();
		System.out.println(diffDate); // -350083766070 음수이므로 d2가 미래 날짜
		// ex) 일 단위로 변경 시 밀리초 -> 초 -> 분-> 시 -> 일 순서로 변경
		//		=> / 1000(밀리초 -> 초) / 60(초 -> 분) / 60(분 -> 시간) / 24(시간 -> 일)
		if(diffDate < 0) {
			System.out.println(Math.abs(diffDate / 1000 / 60 / 60 / 24) + "일 남았습니다.");
 		} else if(diffDate > 0) {
			System.out.println(Math.abs(diffDate / 1000 / 60 / 60 / 24) + "일 남았습니다.");
 		} else {
 			System.out.println("오늘입니다!");
 		}

		
		
	}

}
```

## java.util.Calendar 클래스
- 날짜 및 시간을 관리하는 클래스(Date 클래스와 유사함)
- 주로 날짜 및 시간 정보를 조정하는 용도로 사용
  - (과거에는 Calendar 클래스로 날짜 조작, Date 클래스로 날짜 표현함)
- 추상클래스 이므로 인스턴스 생성이 불가능하며, 미리 시스템으로부터 생성되어 제공되는 인스턴스를 리턴받아 사용
  - (싱글톤 디자인 패턴(Singleton Design Pattern) 활용 [한 것 처럼])
    - getInstance() 메서드 호출하여 인스턴스를 리턴 받음
- toString() 메서드가 오버라이딩 되어 있으나 일반적인 날짜 식별은 어려움
- get() 메서드로 날짜 및 시간 정보에 대한 항목을 지정하여 값을 가져오고, set() 메서드로 날짜 및 시간 정보에 대한 항목을 지정하여 값을 설정한다.
  - 파라미터로 사용될 대상 항목은 Calendar 클래스의 상수를 활용
    - ex) cal.get(Calendar.YEAR) => 연도 정보 가져오기

```java
package util;

import java.util.Calendar;

public class Ex3 {

	public static void main(String[] args) {

//		Calendar cal = new Calendar(); // 추상클래스 이므로 인스턴스 생성 불가
		
		// 진짜 싱글톤인지 아닌지 예
//		Calendar cal = Calendar.getInstance();
//		Calendar cal2 = Calendar.getInstance();
//		System.out.println(cal == cal2);	// true가 나와야 진짜 싱글톤 패턴
		
		Calendar cal = Calendar.getInstance();
		
		// get(int field) 메서드를 사용하여 특정 항목에 대한 값 가져오기
		// => 항목 지정을 위해 get() 메서드 파라미터로 Calendar.XXX 상수를 전달하여
		//	  가져오고자 하는 대상 항목을 지정할 수 있다.
		int year = cal.get(Calendar.YEAR);
		System.out.println(year);
		
//		int month = cal.get(Calendar.MONTH); // Date 클래스와 마찬가지로 0 ~ 11월 사용
		int month = cal.get(Calendar.MONTH) + 1; // 따라서, 결과값 + 1 필요
		System.out.println(month);

		int date = cal.get(Calendar.DATE);
		System.out.println(date);
		
		int date2 = cal.get(Calendar.DAY_OF_MONTH);
		System.out.println(date2);
		
		// 요일(DAY_OF_WEEK)
		int day = cal.get(Calendar.DAY_OF_WEEK);
		System.out.println(day); // 일요일 : 1 / 토요일 : 7
		
		// 오전/오후
		int amPm = cal.get(Calendar.AM_PM);
		String strAmPm = "";
		if(amPm == Calendar.AM) {
			strAmPm = "오전";
		} else {
			strAmPm = "오후";
		}
		
		System.out.println(strAmPm);
		
		
		strAmPm = amPm == Calendar.AM ? "오전" : "오후";
		System.out.println("---------------------------------");
		
//		Calendar.HOUR_OF_DAY
		cal.get(Calendar.MINUTE);
		
	}

}
```

## java.time 패키지
- 날짜 및 시간 정보를 관리하는 클래스들의 모음 패키지(JDK 8부터 제공됨)
  - Date 및 Calendar 클래스에 비해 직관적이므로 사용하기 쉽다.
  
- LocalDate 클래스 - 날짜 관련 기능 제공
- LocalTime 클래스 - 시간 관련 기능 제공
- LocalDateTime 클래스 - 날짜 및 시간 관련 기능 제공
- Calendar 클래스 처럼 객체 생성없이 시스템이 생성한 객체를 리턴받아 사용
  - 생성자가 보이지 않도록 은닉되어 있음(= 싱글톤 디자인 패턴)
- 각 클래스의 now() 메서드를 호출하여 현재 시스템의 날짜 및 시간 정보 가져오고 각 클래스의 of() 메서드를 호출하여 날짜 및 시간 정보 설정
  - static 메서드로 제공되므로 클래스명만으로 접근 가능
- getXXX() 메서드를 호출하여 상세 항목 정보 가져오기 가능
	- (ex. 연도 : getYear(), 시간 : getHour())

```java
package util;

import java.time.LocalDate;
import java.time.LocalDateTime;
import java.time.LocalTime;
import java.time.Month;
import java.time.format.TextStyle;
import java.util.Locale;

public class Ex4 {

	public static void main(String[] args) {
		
//		LocalDate date = new LocalDate();	// 생성자가 은닉 되어있음.
		// 1. 날짜 정보를 관리하는 LocalDate 클래스
		LocalDate date = LocalDate.now();
		System.out.println(date); // 2022-04-14 출력됨
		
		// 2. 시간 정보를 관리하는 LocalTime 클래스
		LocalTime time = LocalTime.now();
		System.out.println(time); // 16:05:57:740 (24시간제, 밀리초단위까지 표현)
		
		System.out.println("------------------------------------------");
		// 각 객체의 값 설정(=변경) XXX.of() 메서드 사용
		// 1. LocalDate 객체의 날짜 설정 => 2000년 1월 1일로 변경
		LocalDate date2 = LocalDate.of(2000, 1, 1);
		System.out.println(date2);
		
		// 2. LocalTime 객체의 시간 설정 => 9시 10분 50초로 변경
		LocalTime time2 = LocalTime.of(9, 10, 50);
		System.out.println(time2);
		
		// 3. LocalDateTime 객체의 날짜 및 시간 설정 => 2002년 7월 15일 20시 30분 58초로 변경
		LocalDateTime dateTime2 = LocalDateTime.of(2002, 7, 15, 20, 30, 58);
		System.out.println(dateTime2); // 2002-07-15T20:30:58
		
		System.out.println("-----------------------------------------");
		
		// getXXX() 메서드를 사용한 상세 정보 가져오기
		int year = date.getYear();
		int month = date.getMonthValue(); // 주의! getMonth() 메서드는 Month 타입 객체 리턴
		int day = date.getDayOfMonth();
		System.out.println(year + "/" + month + "/" + day);
		
		// getMonth() 메서드를 통해 리턴되는 Month 타입 객체 활용법
	Month eMonth = date.getMonth(); // enum 타입 객체 형태로 리턴됨
	System.out.println(eMonth); // APRIL
	// => 대한민국 기준 표현법으로 변환할 경우
	//	  Month 타입 객체의 getDisplayName() 메서드를 호출하여
	//	  표시방식(TextStyle.XXX) 과 국가언어(Locale.XXX) 전달
	System.out.println(eMonth.getDisplayName(TextStyle.SHORT, Locale.KOREAN));
	
	System.out.println("-------------------------------------------------");
	
	LocalDate now = LocalDate.now();
	System.out.println("오늘은 " + now + " 입니다.");
	
	// 연도 연산(2년 뒤 => plusYears() 메서드 사용)
//	System.out.println("2년 뒤는 " + now.plusYears(2) + " 입니다.");
	
	LocalDate afterTwoYear = now.plusYears(2);
	System.out.println("2년 뒤는 " + afterTwoYear + " 입니다.");
	
	// 월 연산(2개월 뒤)
	System.out.println("2개월 뒤는 " + now.plusMonths(2) + " 입니다.");
	System.out.println("2개월 뒤는 " + now.withMonth(6) + " 입니다.");
	
	// 일 연산(20일 뒤)
	System.out.println("20일 뒤는 " + now.plusDays(20) + " 입니다.");
	
	// 빌더 패턴(Builder Pattern)을 활용하여 메서드를 연쇄적으로 호출 가능
	System.out.println(now.plusYears(2).plusMonths(2).plusDays(20));
	
	/*
	 * Builder Pattern 이란?
	 * - 어떤 객체의 메서드 리턴 타입이 자기 자신일 때
	 *   리턴값을 전달받아 다시 다른 메서드를 연쇄적으로 호출하는 형태의 코드 작성 기법
	 * - ex) String 클래스의 메서드 리턴타입이 대부분 String 타입이므로
	 * 		 str.trim().replace().split()
	 * 
	 */
	
	}

}
```
