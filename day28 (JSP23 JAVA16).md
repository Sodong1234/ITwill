# [오전수업] JSP 23차

> mysql 작업을 위하여 초기세팅 실시

> - study_jsp 데이터베이스 생성 -> test 테이블 생성 -> idx컬럼(INT타입) 생성 -> idx 컬럼에 임의의 숫자 2개 추가(INSERT INTO test VALUES (1); INSERT INTO test VALUES (2);)
> - https://downloads.mysql.com/archives/c-j/ -> Platform Independent로 변경 -> zip 파일 다운로드 및 압축풀기 (파일에 대한 내용은 아래 기술)
> - 압축 풀 때 나온 jar 파일 복사 -> 이클립스에서 StudyJSP 폴더 -> src -> main -> webapp -> WEB-INF -> lib에 붙여넣기 -> jar 마우스 오른쪽 클릭 -> Build Path 클릭 -> Add to build Path 클릭



### 라이브러리(Library)
- 함수(또는 메서드)들의 집합
- 사용해야하는 함수(또는 메서드)들을 미리 만들어서 모아놓은 곳

### API(Application Programming Interface)
- 라이브러리에 접근하기 위한 규칙들을 정의한 것
- 라이브러리에서 제공하는 함수(또는 메서드)들에 접근하기 위해 API에 정의된 규칙에 따라 입력을 통해 결과를 전달받아 사용할 수 있도록 해주는 것
- 라이브러리 = API (= 클래스들의 모음집)

## JDBC(Java DataBase Connectivity) 
- 자바에서 데이터베이스에 접근하기 위한 드라이버 클래스 등을 묶어서 제공하는 표준화 된 API(라이브러리)
- MySQL, Oracle 등 각 데이터베이스 제조사의 개발자가 프로그래밍 언어를 통해 자사의 데이터베이스에 쉽게 접근 할 수 있도록 해당 베이스 접근에 필요한 기능들을 드라이버 클래스 형태로 제공해줌
- 따라서, 프로그램 개발자는 데이터베이스 내부 구조를 알지 못해도 JDBC를 활용하여 드라이버 클래스를 사용하면 프로그래밍언어-DB 연결을 수행 가능
  - 데이터베이스 접근 방법을 표준 형태로 제공해주므로 데이터베이스가 변경되더라도 드라이버 교체 및 간단한 설정 변경을 통해 기존 코드를 거의 수정하지 않고도 데이터베이스 교체가 가능 

```java
[ MySQL 을 위한 JDBC 드라이버 준비 방법 ]
1. MySQL 연동을 위한 드라이버가 포함된 API(라이브러리) 다운로드
  1) https://www.mysql.com 접속 후 상단 Downloads 클릭
  2) 아래쪽 MySQL community (GPL) Downloads 링크 클릭
    (또는 https://www.mysql.com/downloads/ 접속)
  3) Connector/J 링크 클릭
  4) Archive 메뉴 클릭
  5) Operation System 항목에서 Platform Independent 클릭
  6) ZIP 파일 다운로드 후 압축 풀기
  
2. 압축 푼 폴더 내의 mysql-connector-java-x.x.xx.jar 파일 사용법
  1) 특정 프로젝트에서만 사용할 경우
    - 이클립스 내의 프로젝트명 -> src -> main -> webapp -> WEB-INF -> lib 폴더에 복사
    - 해당 파일 우클릭 - Build Path - Add to Build Path 선택
    - 프로젝트 내의 Referenced Libraries 항목에 mysql-connector-java-x.x.xx.jar 파일 추가 확인
  2) 모든 프로젝트에서 해당 jar 파일을 사용할 경우
    - 이클립스에서 설정된 JRE 폴더 열기 (Window 메뉴 - Preferences - Java - Installed JREs 메뉴에서 확인 및 설정)
    - 해당 JRE 폴더 - lib - ext 폴더 내에 mysql-connector-java-x.x.xx.jar 파일 복사
    - 이클립스 재시작 후 프로젝트 내의 JRE System Libraries 항목에 mysql-connector-java-x.x.xx.jar 파일 추가 확인
```
```java
[ JDBC 를 활용한 MySQL 연동(사용) 방법 - 4단계
1단계. JDBC 드라이버 로드
  - java.lang 패키지의 Class 클래스의 static 메서드 forName() 를 호출하여
    드라이버 클래스가 위치한 패키지명과 클래스명을 문자열로 전달
  - com.mysql.jdbc 패키지 내의 Driver.class 파일을 지정해야함
    => "com.mysql.jdbc.Driver" 형식으로 지정(주의! .class 생략)
    
    < 문법 >
    Class.forName("드라이버클래스")
    ex) Class.forName("com.mysql.jdbc.Driver");
    => 주의! 드라이버 클래스 위치 또는 파일명이 틀렸을 경우(= 파일이 없을 경우) 오류 발생
    java.lang.ClassNotFoundException: com.mysql.jdbc.Drive
    => com.mysql.jdbc.Drive 클래스를 찾지 못해서 발생한 오류라는 의미 

2단계. DB 연결(접속)
  - java.sql 패키지의 DriverManager 클래스의 static 메서드인 getConnection() 호출
  - 파라미터로 DB 접속 URL, 계정명, 패스워드를 전달(문자열)
  
  < URL 형식 >
  "jdbc:mysql://접속할주소:포트번호/DB명"
  ex) "jdbc:mysql://localhost:3306/study_jsp3"
     
---------------------------------------------------------------------------- 
포트번호 :
- 특정 프로토콜을 사용하여 외부와 통신을 할 때 사용하는 번호(= 출입구)
- 반드시 하나의 서비스 당 하나의 포트만 사용 가능하며 현재 사용중인 포트를 다른 서비스가 중복 사용 불가능(= 충돌 발생)
- 0 ~ 65535 번까지의 포트가 제공됨

[ Well-known 포트 ]
HTTP(웹) : 80
HTTPS(웹보안) : 443
FTP(파일전송) : 21, 20
TELNET(원격접속) : 23
SSH(원격접속-보안) : 22
----------------------------------------------------------------------------     
    
    < 문법 >
    DriverManager.getConnection("URL", "계정명", "패스워드");
    => 주의! 반드시 DriverManager 클래스는 자동 완성을 통해 java.sql 패키지가 import 될 수 있도록 해야함
    (<%@page import="java.sql.DriverManager"%>)
    
    
    
    
3단계. SQL 구문 작성 및 전달
  - 현재 1단계, 2단계 과정을 통해 DB에 접속된 상태에서 접속 정보를 담고 있는 Connection 객체를 통해 데이터베이스 관련 작업 수행 가능
   (반드시 2단계 과정에서 Connection 타입 변수에 객체를 리턴받아 저장되어 있어야함)
  - Connection 객체에 prepareStatement() 메서드를 호출하여 실행할 SQL 구문 전달

    < 문법 >
    PreparedStatement pstmt = con.prepareStatement("SQL구문");
    
    
    
4단계. SQL 구문 실행 및 결과 처리
  - 3단계에서 리턴받은 PreparedStatement 객체의 executeXXX() 메서드를 호출하여 전달받은 SQL 구문 실행
    1) ExecuteUpdate() - DB에 조작을 가하는 쿼리문을 실행하는 용도의 메서드
                         => 주로, DML 중 INSERT, UPDATE, DELETE와 DDL 중 CREATE, ALTER, DROP 등 실행하는 용도
                         => 작업 실행 후 결과값이 int 타입으로 리턴되는데 이는, 영향을 받은 레코드 수가 리턴됨(DML 한정, 나머지는 0)
    2) ExecuteQuery() - DB에 조작을 가하지 않는 쿼리문(SELECT)을 실행하는 용도의 메서드
                        => 작업 실행 후 조회된 결과 테이블을 java.sql.ResultSet 타입 객체로 리턴함
```

```jsp

-----------------------------jdbc_connect_test1.jsp-----------------------------
<%@page import="java.sql.DriverManager"%>
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
	/*
	1단계. JDBC 드라이버 로드
  - java.lang 패키지의 Class 클래스의 static 메서드 forName() 를 호출하여
    드라이버 클래스가 위치한 패키지명과 클래스명을 문자열로 전달
  - com.mysql.jdbc 패키지 내의 Driver.class 파일을 지정해야함
    => "com.mysql.jdbc.Driver" 형식으로 지정(주의! .class 생략)
    
    < 문법 >
    Class.forName("드라이버클래스")
    ex) Class.forName("com.mysql.jdbc.Driver");
    => 주의! 드라이버 클래스 위치 또는 파일명이 틀렸을 경우(= 파일이 없을 경우) 오류 발생
    java.lang.ClassNotFoundException: com.mysql.jdbc.Drive
    => com.mysql.jdbc.Drive 클래스를 찾지 못해서 발생한 오류라는 의미
	*/
	
	Class.forName("com.mysql.jdbc.Driver");
	%>
	<h1>드라이버 로드 성공!</h1>
	<hr>
	
	<%
	/*
	2단계. DB 연결(접속)
	- java.sql 패키지의 DriverManager 클래스의 static 메서드인 getConnection() 호출
	- 파라미터로 DB 접속 URL, 계정명, 패스워드를 전달(문자열)
	 < URL 형식 >
	  "jdbc:mysql://접속할주소:포트번호/DB명"
	  ex) "jdbc:mysql://localhost:3306/study_jsp3"

	 < 문법 >
    DriverManager.getConnection("URL", "계정명", "패스워드");
    => 주의! 반드시 DriverManager 클래스는 자동 완성을 통해 java.sql 패키지가 import 될 수 있도록 해야함
	*/
	DriverManager.getConnection("jdbc:mysql://localhost:3306/study_jsp", "root", "1234");
	
	%>
	<h1>DB 연결 성공!</h1>
	
	<!-- 2단계까지가 데이터베이스 제품별로 달라지는 부분 -->
	<!-- 3단계부터 실제 데이터베이스에서 SQL 구문(쿼리)을 사용한 작업을 수행하는 부분(= 공통) -->
</body>
</html>

-----------------------------jdbc_connect_test2.jsp-----------------------------

<%@page import="java.sql.Connection"%>
<%@page import="java.sql.DriverManager"%>
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
	// DB 연결에 필요한 정보 문자열 4가지를 변수에 저장
	String driver = "com.mysql.jdbc.Driver"; // 드라이버 클래스 위치 및 클래스명
	String url = "jdbc:mysql://localhost:3306/study_jsp"; // 접속 URL 및 DB명
	String user = "root"; // DB 접속 계정명
	String password = "1234"; // DB 접속 계정 패스워드
	
	// 1단계. JDBC 드라이버 로드
	Class.forName(driver);
	out.println("<h1>드라이버 로드 성공!</h1>");
	
	
	
	// 2단계. DB 연결
// 	DriverManager.getConnection(url, user, password);
	
	// getConnection() 메서드를 통해 DB 연결 성공 시 
	// DB 접속 정보를 관리하는 java.sql.Connection 타입 객체가 리턴됨
	// => Connection 타입 변수를 선언하여 리턴되는 객체 저장
	Connection con = DriverManager.getConnection(url, user, password);
	
// 	out.println("<h1>DB 연결 성공!</h1>");
	
	%>
	<h1>DB 연결 성공! (객체 내용 : <%=con %>)</h1>
	
	
</body>
</html>

-----------------------------jdbc_connect_test3.jsp-----------------------------
* study_jsp3 데이터베이스, test 테이블을 새로 생성



<%@page import="java.sql.PreparedStatement"%>
<%@page import="java.sql.Connection"%>
<%@page import="java.sql.DriverManager"%>
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
	// DB 연결에 필요한 정보 문자열 4가지를 변수에 저장
	String driver = "com.mysql.jdbc.Driver"; // 드라이버 클래스 위치 및 클래스명
	String url = "jdbc:mysql://localhost:3306/study_jsp3"; // 접속 URL 및 DB명
	String user = "root"; // DB 접속 계정명
	String password = "1234"; // DB 접속 계정 패스워드
	
	// 1단계. JDBC 드라이버 로드
	Class.forName(driver);
	out.println("<h1>드라이버 로드 성공!</h1>");
	
	
	
	// 2단계. DB 연결
	Connection con = DriverManager.getConnection(url, user, password);
	out.println("<h1>DB 연결 성공!</h1>");
	
	// 3단계. SQL 구문 작성 및 전달
	// 1) SQL 구문 작성
// 	String sql = "INSERT INTO test VALUES (3);";

	// 일반적으로는 외부로부터 데이터를 입력(전달)받아 변수에 저장하여 데이터베이스에 전달
	int idx = 2; // 외부로부터 추가할 데이터를 입력받았다고 가정
	// 데이터가 위치할 부분은 문자열 결합을 통해 변수를 결합하여 SQL 구문을 작성해야한다!
	String sql = "INSERT INTO test VALUES (" + idx + ")";
	
	// 2) Connection 객체(con)의 prepareStatement() 메서드를 호출하여 SQL 구문 전달
	//	  => 파라미터 : SQL 구문(String 타입)	리턴타입 : java.sql.preparedStatement 객체
	PreparedStatement pstmt = con.prepareStatement(sql);
	
	/*
	 	4단계. SQL 구문 실행 및 결과 처리
	 	 - 3단계에서 리턴받은 PreparedStatement 객체의 executeXXX() 메서드를 호출하여 전달받은 SQL 구문 실행
	    	1) ExecuteUpdate() - DB에 조작을 가하는 쿼리문을 실행하는 용도의 메서드
	                         	=> 주로, DML 중 INSERT, UPDATE, DELETE와 DDL 중 CREATE, ALTER, DROP 등 실행하는 용도
	                         	=> 작업 실행 후 결과값이 int 타입으로 리턴되는데 이는, 영향을 받은 레코드 수가 리턴됨(DML 한정, 나머지는 0)
	    	2) ExecuteQuery() - DB에 조작을 가하지 않는 쿼리문(SELECT)을 실행하는 용도의 메서드
	                        	=> 작업 실행 후 조회된 결과 테이블을 java.sql.ResultSet 타입 객체로 리턴함
	*/
	
	// INSERT 구문 실행을 위해 preparedStatement 객체의 executeUpdate() 메서드를 호출
	// => 이 때, SQL 구문을 다시 전달하지 않도록 주의(executeUpdate(sql) 메서드는 다른 객체에 있음)
	// => 또한, 메서드 실행 후 리턴되는 데이터를 int 타입 변수에 저장하면
	//	  DML 실행 후 영향을 받은 레코드 수를 알 수 있다! (결과 판별 가능)
	int insertCount = pstmt.executeUpdate();
	%>
	<hr>
	<h1>INSERT 작업 완료 - <%=insertCount %> 개 레코드 추가됨</h1> 
	
	
	
</body>
</html>
```

```jsp
-----------------------------jdbc_connect_test3_form.jsp-----------------------------

<%@page import="java.sql.PreparedStatement"%>
<%@page import="java.sql.Connection"%>
<%@page import="java.sql.DriverManager"%>
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<h1>번호 입력 시스템</h1>
	<form action="jdbc_connect_test3_pro.jsp">
		번호 : <input type="text" name="idx">
		<input type="submit" value="저장">	
	</form>
</body>
</html>
-----------------------------jdbc_connect_test3_pro.jsp-----------------------------
<%@page import="java.sql.PreparedStatement"%>
<%@page import="java.sql.Connection"%>
<%@page import="java.sql.DriverManager"%>
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
	// jdbc_connect_test3_form.jsp 페이지에서 입력받은 번호(idx 파라미터) 를 가져와서 변수에 저장
	String idx = request.getParameter("idx");
	
	
	%>
	<%
	// DB 연결에 필요한 정보 문자열 4가지를 변수에 저장
	String driver = "com.mysql.jdbc.Driver"; // 드라이버 클래스 위치 및 클래스명
	String url = "jdbc:mysql://localhost:3306/study_jsp3"; // 접속 URL 및 DB명
	String user = "root"; // DB 접속 계정명
	String password = "1234"; // DB 접속 계정 패스워드
	
	// 1단계. JDBC 드라이버 로드
	Class.forName(driver);
	out.println("<h1>드라이버 로드 성공!</h1>");
	
	
	
	// 2단계. DB 연결
	Connection con = DriverManager.getConnection(url, user, password);
	out.println("<h1>DB 연결 성공!</h1>");
	
	// 3단계. SQL 구문 작성 및 전달
	// 1) SQL 구문 작성
// 	String sql = "INSERT INTO test VALUES (3);";

	// 일반적으로는 외부로부터 데이터를 입력(전달)받아 변수에 저장하여 데이터베이스에 전달
// 	int idx = 2; // 외부로부터 추가할 데이터를 입력받았다고 가정
	// 데이터가 위치할 부분은 문자열 결합을 통해 변수를 결합하여 SQL 구문을 작성해야한다!
	String sql = "INSERT INTO test VALUES (" + idx + ")";
	
	// 2) Connection 객체(con)의 prepareStatement() 메서드를 호출하여 SQL 구문 전달
	//	  => 파라미터 : SQL 구문(String 타입)	리턴타입 : java.sql.preparedStatement 객체
	PreparedStatement pstmt = con.prepareStatement(sql);
	
	/*
	 	4단계. SQL 구문 실행 및 결과 처리
	 	 - 3단계에서 리턴받은 PreparedStatement 객체의 executeXXX() 메서드를 호출하여 전달받은 SQL 구문 실행
	    	1) ExecuteUpdate() - DB에 조작을 가하는 쿼리문을 실행하는 용도의 메서드
	                         	=> 주로, DML 중 INSERT, UPDATE, DELETE와 DDL 중 CREATE, ALTER, DROP 등 실행하는 용도
	                         	=> 작업 실행 후 결과값이 int 타입으로 리턴되는데 이는, 영향을 받은 레코드 수가 리턴됨(DML 한정, 나머지는 0)
	    	2) ExecuteQuery() - DB에 조작을 가하지 않는 쿼리문(SELECT)을 실행하는 용도의 메서드
	                        	=> 작업 실행 후 조회된 결과 테이블을 java.sql.ResultSet 타입 객체로 리턴함
	*/
	
	// INSERT 구문 실행을 위해 preparedStatement 객체의 executeUpdate() 메서드를 호출
	// => 이 때, SQL 구문을 다시 전달하지 않도록 주의(executeUpdate(sql) 메서드는 다른 객체에 있음)
	// => 또한, 메서드 실행 후 리턴되는 데이터를 int 타입 변수에 저장하면
	//	  DML 실행 후 영향을 받은 레코드 수를 알 수 있다! (결과 판별 가능)
	int insertCount = pstmt.executeUpdate();
	%>
	<hr>
	<h1>INSERT 작업 완료 - <%=insertCount %> 개 레코드 추가됨</h1> 
	
	
	
</body>
</html>
```

# [오후수업] JAVA 16
