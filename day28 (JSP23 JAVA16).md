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

# [오후수업] JAVA 16차


## 접근 제한자의 접근 범위
1. public	: 모든 클래스에서 접근 가능 (제한 없음)
2. protected 	: 같은 패키지 또는 다른 패키지이면서 상속관계에 있는 서브클래스만 접근 가능
3. default	: 같은 패키지에서만 접근 가능 (다른 패키지에서 접근 불가!)
4. private	: 자신의 클래스 내에서만 접근 가능 (다른 클래스에서 접근 불가!)


```java

-----------------------------ParentClass.java-----------------------------
package access_modifier;

public class ParentClass {
	public int publicVar;		// 모든 클래스에서 접근 가능(제한없음)
	protected int protectedVar; // 같은 패키지 또는 다른 패키지이면서 상속 관계에 있는 서브클래스만 접근 가능
	int defaultVar;				// 같은 패키지에서만 접근 가능 (다른패키지 접근 X)
	private int privateVar;		// 자신의 클래스 내에서만 접근 가능 (달느 클래스에서 접근 X)
	
	public void useMember() {
		this.publicVar = 10;	// (O)
		this.protectedVar = 20; // (O)
		this.defaultVar = 30;	// (O)
		this.privateVar = 40;	// (O)
	}
}

-----------------------------SamePackageSomeClass.java-----------------------------
package access_modifier;

public class SamePackageSomeClass {

	// 패키지가 동일한 다른 클래스에서 접근 범위(서로 다른 java파일 내)
	public void useMember() {
		ParentClass p = new ParentClass();
		p.publicVar = 10;		// (O) - 누구나 접근 가능
		p.protectedVar = 20;	// (O) - 같은 패키지이므로 접근 가능
		p.defaultVar = 30;		// (O) - 같은 패키지이므로 접근 가능
//		p.privateVar = 40;		// (X) - 다른 클래스에서 접근 불가
	}
}

-----------------------------OtherPackageSomeClass.java-----------------------------
package access_modifier2;

import access_modifier.ParentClass;

public class OtherPackageSomeClass {
	// 패키지가 다르고 상속 관계가 아닌 클래스에서 접근 범위
	public void useMember() {
		ParentClass p = new ParentClass();
		p.publicVar = 10;		// (O) - 누구나 접근 가능
//		p.protectedVar = 20;	// (X) - 다른 패키지이고 상속관계가 아니므로 접근 불가
//		p.defaultVar = 30;		// (X) - 다른 패키지이므로 접근 불가
//		p.privateVar = 40;		// (X) - 다른 클래스에서 접근 불가
		
	}
}

-----------------------------OtherPackageChildClass.java-----------------------------
package access_modifier2;

import access_modifier.ParentClass;

public class OtherPackageChildClass extends ParentClass {

	// 패키지가 다르고 상속 관계에 있는 서브클래스에서 접근 범위
	public void useMember() {
		
		// 주의! 상 관계에서 슈퍼클래스의 멤버에 접근 할 때
		// 슈퍼클래스의 인스턴스를 생성하여 접근할 경우 상속 관계로써의 접근이 아니게 됨
		ParentClass p = new ParentClass();
		p.publicVar = 10;		// (O) - 누구나 접근 가능
//		p.protectedVar = 20;	// (X) - 다른 패키지이고 상속관계가 아니므로 접근 불가
//		p.defaultVar = 30;		// (X) - 다른 패키지이므로 접근 불가
//		p.privateVar = 40;		// (X) - 다른 클래스에서 접근 불가
		
		
		publicVar = 10;		// (O) - 누구나 접근 가능
		protectedVar = 20;	// (O) - 다른 패키지이지만 상속관계이므로 접근 가능
//		defaultVar = 30;	// (X) - 다른 패키지이므로 접근 불가
//		privateVar = 40;	// (X) - 다른 클래스에서 접근 불가
	}
}
```

## 메서드 오버라이딩(Method Overriding)
- 슈퍼클래스로부터 상속받은 메서드를 서브클래스에서 새롭게 재정의 하는 것
- 반드시 상속 관계에서 상속받은 메서드에 대해서만 적용 가능
- 서브클래스에서 오버라이딩 수행 후에는 슈퍼클래스의 메서드는 은닉됨

```
< 메서드 오버라이딩 규칙 >
1. 슈퍼클래스의 메서드와 시그니처(리턴타입, 메서드명, 매개변수)가 동일해야함
2. 접근제한자는 범위가 좁아질 수 없다
   ( => 부모가 public 이면 자식도 public만 선택 가능함)
```

```java

public class Ex1 {

	public static void main(String[] args) {
		
		Child c = new Child();
		c.parentPrn(); // 서브클래스에서 오버라이딩된 parentPrn() 메서드가 호출됨
		// => 이 때, 오버라이딩 된 메서드로 인해 슈퍼클래스의 메서드는 은닉되기 때문에
		//	  Child 인스턴스를 통해서는 Parent 클래스의 parentPrn() 접근이 불가능
		Parent p = new Parent();
		p.parentPrn();
		c.childPrn();
		System.out.println("===================");
		
		Dog dog = new Dog();
		Cat cat = new Cat();
		
		dog.cry();
		cat.cry();
		
		
	}

}

class Animal {
	String name;
	int age;
	
	public void cry() {
		System.out.println("동물 울음소리!");
	}
}

class Dog extends Animal {
	// Animal 클래스의 cry() 메서드 오버라이딩
	public void cry() {
		System.out.println("멍멍!");
	}
}

class Cat extends Animal {
	// Animal 클래스의 cry() 메서드 오버라이딩
	public void cry() {
		System.out.println("야옹!");
	}
}

// ================================================================
class Parent {
	public void parentPrn() {
		System.out.println("슈퍼클래스의 parentPrn()");
	}
}


class Child extends Parent {
	// Parent 클래스의 parentPrn() 메서드를 상속받아 오버라이딩을 수행
	// => 슈퍼클래스인 Parent클래스의 parentPrn() 메서드의 시그니처를 동일하게 정의하고
	//	  접근제한자는 public 이므로 다른 접근제한자로 변경 불가(좁아질 수 없음)
//	public void parentPrn() {
//		System.out.println("서브클래스에서 오버라이딩 된 parentPrn()");
//	}
	
	// 자동 오버라이딩 단축키 : Alt + Shift + S -> V
	@Override
	public void parentPrn() {
		// @Override 표시는 어노테이션(Annotation) 기능으로 자바 컴파일러를 위한 주석이며
		// 이 메서드가 오버라이딩 되어 있다는 표시이므로, 반드시 오버라이딩만 가능하며
		// 오버라이딩 규칙을 위반할 경우 오류가 발생
		System.out.println("서브클래스에서 오버라이딩 된 parentPrn()");
	}
	
	public void childPrn() {
		System.out.println("서브클래스의 childPrn()");
	}
	
}
```


연습문제
```java

public class Test1 {

	public static void main(String[] args) {
		ItwillBank ib = new ItwillBank();
		ib.deposit(50000);
		System.out.println("출금된 금액 : " + ib.withdraw(50000));
		System.out.println("----------------------------------");
		// ItwillBank 클래스에서 오버라이딩 된 withdraw() 메서드가 호출되므로
		// 잔고가 부족하더라도 무조건 출금이 수행됨
		System.out.println("출금된 금액 : " + ib.withdraw(50000));
		System.out.println("----------------------------------");
		
		Car car = new Car();
		car.SpeedUp(10);
		car.SpeedDown(10);
		car.addFuel();
		
		Taxi taxi = new Taxi();
		taxi.SpeedUp(100);
		taxi.SpeedDown(50);
		taxi.addFuel();
		
		ElectricCar ec = new ElectricCar();
		ec.addFuel();
		
		DiselCar dc = new DiselCar();
		dc.addFuel();
		
		
		
	}
}

/*
 * 자동차(Car) 클래스 정의
 * - 멤버변수
 * 	1) 현재속력(speed, 정수형)
 * 	2) 최대속력(maxSpeed, 정수형)
 * 
 * - 메서드
 * 	1) 속력증가 : speedUp()
 * 	- 파라미터로 증가할 속력(speed) 전달, 리턴값 없음
 * 	- "자동차의 속력 증가" 출력 
 * 	2) 속력 감소 : speedDown()
 * 	- 파라미터로 감속할 속력(speed) 전달, 리턴값 없음
 * 	- "자동차의 속력 감소" 출력
 * 	3) 연료 공급 : addFuel()
 * 	- 파라미터 없음, 리턴값 없음
 * 	- "자동차 연료 공급" 출력
 * 
 * 
 * Taxi 클래스 정의 - Car 클래스를 상속받아 정의
 * 	- SpeedUp() 메서드 오버라이딩 : "Taxi의 속력 증가!" 출력
 * 	- speedDown() 메서드 오버라이딩 : "Taxi의 속력 감소!" 출력
 *  
 * Truck 클래스 정의 - Car 클래스를 상속받아 정의
 * 	- SpeedUp() 메서드 오버라이딩 : "Truck의 속력 증가!" 출력
 * 	- speedDown() 메서드 오버라이딩 : "Truck의 속력 감소!" 출력
 * 
 * ElectricCar 클래스 정의 - Car 클래스를 상속받아 정의
 * 	- addFuel() 메서드 오버라이딩 : "전기차 충전소에서 충전!" 출력
 * 
 * DiselCar 클래스 정의 - Car 클래스를 상속받아 정의
 * 	- addFuel() 메서드 오버라이딩 : "주유소에서 디젤 연료 공급!" 출력
 * 
 */


class Car {
	int speed;
	int maxSpeed;
	
	public void SpeedUp(int speed) {
		this.speed += speed;
		System.out.println("자동차의 속력 증가! (현재속도 : " + speed + ")");
	}
	
	public void SpeedDown(int speed) {
		this.speed -= speed;
		System.out.println("자동차의 속력 감소! (현재속도 : " + speed + ")");
	}
	
	public void addFuel() {
		System.out.println("자동차 연료 공급!");
	}
	
}


class Taxi extends Car {
	@Override
	public void SpeedUp(int speed) {
		this.speed += speed;
		System.out.println("Taxi의 속력 증가! (현재속도 : " + speed + ")");
	}
	@Override
	public void SpeedDown(int speed) {
		this.speed -= speed;
		System.out.println("Taxi의 속력 감소! (현재속도 : " + speed + ")");
	}
}

class Truck extends Car {
	@Override
	public void SpeedUp(int speed) {
		this.speed += speed;
		System.out.println("Truck의 속력 증가! (현재속도 : " + speed + ")");
	}
	
	@Override
	public void SpeedDown(int speed) {
		this.speed -= speed;
		System.out.println("Truck의 속력 감소! (현재속도 : " + speed + ")");
	}	
}

class ElectricCar extends Car {
	@Override
	public void addFuel() {
		System.out.println("전기차 충전소에서 충전!");
	}
}

class DiselCar extends Car {
	@Override
	public void addFuel() {
		System.out.println("주유소에서 디젤 연료 공급!");
	}
}





// =================================================================
class Account {
	String accointNo;
	String ownerName;
	int balance;
	
	public void showAccountInfo() {
		System.out.println("계좌번호 : " + accointNo);
		System.out.println("예금주명 : " + ownerName);
		System.out.println("현재잔고 : " + balance);
	}
	
	// 입금기능
	public void deposit(int amount) {
		balance += amount;
		System.out.println("입금금액 : " + amount);
		System.out.println("현재잔고 : " + balance);
	}
	
	// 출금기능
	public int withdraw(int amount) {
		if(amount > balance) { // 출금 불가능한 경우
			System.out.println("출금할 금액 : " + amount);
			System.out.println("잔액이 부족하여 출금 불가! (현재잔고 : " + balance + "원");
			return 0;
		} else { // 출금 가능
			balance -= amount;
			System.out.println("출금할 금액 : " + amount + "원");
			System.out.println("출금 후 현재 잔고 : " + balance + "원");
			return amount;
		}
	}
}



/*
 * ItwillBank 클래스 정의 - Account 클래스 상속
 * - 출금 기능(withdraw()) 메서드 오버라이딩 수행
 * 	=> 잔고가 부족하더라도 무조건 출금 하도록 구현
 * 		은행 잔고에 관계없이 무조건 출금 수행(마이너스 통장)
 * 
 * 
 */

class ItwillBank extends Account {
	
	@Override
	public int withdraw(int amount) {
		balance -= amount;
		System.out.println("출금할 금액 : " + amount + "원");
		System.out.println("출금 후 현재 잔고 : " + balance + "원");
		return amount;
	}
}
```

## 멤버변수에 대한 오버라이딩 & 레퍼런스 super 
- 멤버변수에 대한 오버라이딩
	- Parent 클래스로부터 상속 받은 멤버변수와 동일한 이름의 변수를 서브클래스에서 선언하면 메서드 오버라이딩과 마찬가지로 멤버변수에 대한 은닉이 발생하여 슈퍼클래스의 멤버변수는 보이지 않고, 서브클래스의 멤버변수에만 접근 가능

- 레퍼런스 super
	- 레퍼런스 this와 마찬가지로 인스턴스의 주소를 저장하는 참조변수
	- 레퍼런스 this는 자신의 인스턴스 주소를 저장하는 반면, 레퍼런스 super는 부모의 인스턴스 주소를 저장함
	- 메서드(또는 변수) 오버라이딩으로 인해 슈퍼클래스의 멤버가 은닉되었을 때 서브클래스에서 슈퍼클래스의 은닉된 멤버에 접근하기 위해 사용
	- super.super 형식처럼 super 키워드를 중첩해서 사용할 수 없음

```
< 기본 사용 문법 >
서브클래스의 메서드 내에서 
super.부모의멤버변수 또는 super.부모의메서드()

< 변수 사용 시 선언 방법에 따른 접근 순서 >
1. 변수명만 지정했을 경우
현재 선언된 메서드 내에서 먼저 탐색 -> 없을 경우 자신의 멤버변수에서 탐색 -> 없을 경우 부모의 멤버변수에서 탐색
 
2. this.변수명을 지정했을 경우
자신의 멤버변수에서 탐색 -> 부모의 멤버변수에서 탐색

3. super.변수명을 지정했을 경우
부모의 멤버변수에서 탐색
```
```java

public class Ex2 {

	public static void main(String[] args) {
		
		Child2 c = new Child2();
		System.out.println(c.name);
		System.out.println("우리집 TV : " + c.tv);
		c.watchTv();
		c.watchMyParentTv();
		System.out.println("=================================");
//		c.scope();
		SonOfChild2 s = new SonOfChild2();
		s.scope();
		
		System.out.println("=================================");
		
		SpiderMan sm = new SpiderMan();
		System.out.println("현재 스파이더맨 모드인가? " + sm.isSpider);
		sm.jump();
		
		sm.isSpider = true;
		sm.jump();
	}

}

class parent2 {
	String tv = "부모님이 구입한 TV";
	
	public void watchTv() {
		System.out.println("부모님 댁에서 " + tv + " 를 보자!");
	}
	
	String name = "Parent의 멤버변수 name";
}

class Child2 extends parent2 {
	String tv = "내가 구입한 TV";
	
	@Override
	public void watchTv() {
		System.out.println("우리집에서 " + tv + " 를 보자!");
		System.out.println("super.tv = " + super.tv);
	}
	
	public void watchMyParentTv() {
		super.watchTv();
	}
	
	// 로컬변수와 this.멤버변수와 super.멤버변수 범위의 차이
	String name = "Child의 멤버변수 name";
	
	public void scope() {
		String name = "Child의 클래스 메서드 내의 로컬변수 name";
		
		System.out.println("name = " + name);
		System.out.println("this.name = " + this.name);
		System.out.println("super.name = " + super.name);
	}
	
	public String getParentName() {
		return super.name;
	}
}

class SonOfChild2 extends Child2 {
	String name;
	
	public void scope() {
		System.out.println("this.name = " + this.name);
		System.out.println("super.name = " + super.name);
//		System.out.println("super.super.name = " + super.super.name);
		System.out.println("parent의 name은 굳이 가져오려면? = " + super.getParentName());
	}
}

// ===========================================================
class Person {
	String name;
	int age;
	
	public void jump() {
		System.out.println("일반인의 점프!");
	}
	
	public void eat() {
		System.out.println("먹기!");
	}
}

class SpiderMan extends Person {
	boolean isSpider;

	@Override
	public void jump() {
		// 만약, isSpider가 true이면 "스파이더맨의 강력한 점프!" 를 출력하고
		// 아니면, 슈퍼클래스인 Person 클래스의 jump() 메서드를 호출
		if(isSpider) {
			System.out.println("스파이더맨의 강력한 점프!");
		} else {
			// 자신의 오버라이딩 된 jump() 메서드가 아닌 슈퍼클래스의 jump() 메서드를 호출
			super.jump();
		}
	}
	
}
```
