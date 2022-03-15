# [오전수업] DB 10차
> 8차에서 실시한 시험의 해설 및 9차 수업 복습 실시
> 9차 수업 이어서 실시

### 연결연산자(||)
- 연결연산자로 연결된 요소들을 하나로 연결한 결과를 연산하여 출력하는 문법
- oracle에서는 ||이 연결연산자로 동작하지만, mysql에서는 OR의 연산 형태로 동작

```

mysql에서는 CONCAT 함수로 이 기능을 활용
mysql> SELECT last_name, job_id, CONCAT(last_name, job_id) AS "Employees" FROM HR_EMPLOYEES;

+-------------+------------+---------------------+
| last_name   | job_id     | Employees           |
+-------------+------------+---------------------+
| King        | AD_PRES    | KingAD_PRES         |
| Kochhar     | AD_VP      | KochharAD_VP        |
| De Haan     | AD_VP      | De HaanAD_VP        |
| Hunold      | IT_PROG    | HunoldIT_PROG       |
… 

컬럼의 내용을 조합하여 출력 시 고정적으로 출력해야할 문자열 요소가 있는 경우 리터럴 문자를 활용
mysql> SELECT last_name, job_id, CONCAT(last_name, ' ', job_id) AS "Employees" FROM HR_EMPLOYEES;

+-------------+------------+----------------------+
| last_name   | job_id     | Employees            |
+-------------+------------+----------------------+
| King        | AD_PRES    | King AD_PRES         |
| Kochhar     | AD_VP      | Kochhar AD_VP        |
| De Haan     | AD_VP      | De Haan AD_VP        |
| Hunold      | IT_PROG    | Hunold IT_PROG       |
…

```

### DISTINCT
- 행의 데이터의 중복값을 제거하여 출력
- DISTINCT 키워드의 위치는 SELECT절의 첫번째 자리
- 여러 컬럼이 SELECT절에 오는 경우 개별 컬럼 별로 중복제거가 아닌 컬럼의 값들을 하나의 세트로 보고 모든 컬럼의 값이 일치한 경우 중복값으로 제거하여 출력

```


mysql> SELECT department_id
    -> FROM HR_EMPLOYEES;

+---------------+
| department_id |
+---------------+
|          NULL |
|            10 |
|            20 |
|            20 |
|            30 |
|            30 |
… 





mysql> SELECT DISTINCT department_id FROM HR_EMPLOYEES;

+---------------+
| department_id |
+---------------+
|          NULL |
|            10 |
|            20 |
|            30 |
|            40 |
|            50 |
|            60 |
|            70 |
|            80 |
|            90 |
|           100 |
|           110 |
+---------------+






아래의 두개의 컬럼에 대한 중복값 제거는 department_id, job_id의 값이 모두 일치한 경우에만 중복값으로 제거   
mysql> SELECT DISTINCT department_id, job_id FROM HR_EMPLOYEES;

+---------------+------------+
| department_id | job_id     |
+---------------+------------+
|            90 | AD_PRES    |
|            90 | AD_VP      |
|            60 | IT_PROG    |
|           100 | FI_MGR     |
|           100 | FI_ACCOUNT |
|            30 | PU_MAN     |
|            30 | PU_CLERK   |
|            50 | ST_MAN     |
|            50 | ST_CLERK   |
|            80 | SA_MAN     |
|            80 | SA_REP     |
|          NULL | SA_REP     |
|            50 | SH_CLERK   |
|            10 | AD_ASST    |
|            20 | MK_MAN     |
|            20 | MK_REP     |
|            40 | HR_REP     |
|            70 | PR_REP     |
|           110 | AC_MGR     |
|           110 | AC_ACCOUNT |
+---------------+------------+

```

# [오후수업] JSP 27차

## 모듈화
- 어떤 기능을 담당하는 작은 단위를 모듈(Module)이라고 함
- 하나의 프로그램에서 특정 기능에 따라 작은 단위의 클래스 또는 메서드 형태로 잘게 분리하는 것을 모듈화라고 함

> StudyJSP Project의 src/main/java 폴더에 JdbcUtil.java 파일을 생성함
> JdbcUtill.java 파일은 계속해서 업데이트 해 나갈 예정.

```jsp
---------------------------------------JdbcUtil.java---------------------------------------

package jsp10_javabeans;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;

// JDBC 활용 시 데이터베이스 접근 관련 작업을 수행하는 용도의 클래스 정의
// => DB 접속, DB 자원 반환, 커밋 또는 롤백 작업 등을 수행
public class JdbcUtil {
	// 1. DB 접속을 수행하는 getConnection() 메서드 정의
	// => 파라미터 : 없음, 리턴타입 : java.sql.Connection
	// => DB 접속을 수행하여 접속 성공 시 접속 정보를 객체로 관리하는 Connection 타입 객체를 외부로 리턴
	// => 외부에서 인스턴스 생성없이도 메서드 호출이 가능하도록 static 메서드로 선언
	//	  (클래스명.메서드명() 형태로 호출 가능 => ex. JdbcUtil.getConnetction())
	public static Connection getConnection() {
		String driver = "com.mysql.jdbc.Driver";
		String url = "jdbc:mysql://localhost:3306/study_jsp3";
		String user = "root";
		String password = "1234";
		
		// java.sql.Connection 타입 객체를 저장할 변수 선언
		Connection con = null;
		
		try { // 예외(= 오류)가 발생할 것으로 예상되는 코드들을 모아놓는 곳(= 예외가 발생하는지를 탐지하는 곳)
			// 1단계. JDBC 드라이버 클래스 로드
			Class.forName(driver); 
			// 드라이버 클래스 위치가 잘못되었을 경우 ClassNotFoundException 예외 발생할 수 있음
			// => 따라서, try ~ catch 블록을 사용하여 catch 블록에서 ClassNotFoundException 발생에 대한 대비책을 세워야함 
			// => try 블록 내에서 현재 예외 위치 아래쪽의 코드 실행이 중지되고 catch (ClassNotFoundException e) {} 부분으로 흐름이 이동함
			
			// 2단계. DB 접속
			con = DriverManager.getConnection(url, user, password);
			// DB 접속이 실패했을 경우 SQLException 예외 발생할 수 있음
			// => 따라서, try ~ catch 블록을 사용하여 catch 블록에서 SQLException 발생에 대한 대비책을 세워야함
			// => DB 접속 정보(URL, 계정명, 패스워드) 중 하나라도 잘못되면 문제가 발생하므로 처리 필요
			// => 현재 예외가 발생하는 위치 아래쪽의 코드 실행이 중지되고 catch (SQLException e) {} 부분으로 흐름이 이동함
		} catch (ClassNotFoundException e) {
			// 드라이버 클래스 위치가 잘못되었을 경우 자동으로 실행되는 코드 위치
			e.printStackTrace();
			System.out.println("드라이버 로드 실패!");
		} catch (SQLException e) {
			// DB 접속이 불가능할 경우(정보가 틀렸을 경우) 자동으로 실행되는 코드 위치
			e.printStackTrace();
			System.out.println("DB 접속 실패!");
		}
		
		// Connection 객체 리턴
		return con;
		
	}
	
	// 2. DB 자원을 반환하는 close() 메서드 정의
	// => 반환해야하는 대상 객체 : Connection, PreparedStatement, ResultSet
	// => 메서드 이름은 동일하고, 파라미터를 다르게 하는 메서드 오버로딩을 활용하여 메서드 정의
	// 1) java.sql.Connection 객체를 전달받아 반환
	public static void close(Connection con) {
		if(con != null) {
			try {
				con.close();
			} catch (SQLException e) {
				e.printStackTrace();
			}
		}
	}
	
	
	// 2) java.sql.PreparedStatement 객체를 전달받아 반환
	public static void close(PreparedStatement pstmt) {
		if(pstmt != null) {
			try {
				pstmt.close();
			} catch (SQLException e) {
				e.printStackTrace();
			}
		}
		
		System.out.println("");
	}

	// 3) java.sql.ResultSet 객체를 전달받아 반환
	public void close(ResultSet rs) {
		if(rs != null) {
			try {
				rs.close();
			} catch (SQLException e) {
				e.printStackTrace();
			}
		}
	}
	
	
	
	
	

}


---------------------------------------Jdbc_select3.jsp---------------------------------------

<%@page import="jsp9_jdbc.JdbcUtil"%>
<%@page import="java.sql.ResultSet"%>
<%@page import="java.sql.PreparedStatement"%>
<%@page import="java.sql.DriverManager"%>
<%@page import="java.sql.Connection"%>
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<!-- 조회된 테이블 정보를 table 태그를 사용하여 표시하기 위해 테이블 생성 -->
	<!-- while 문을 사용하여 ResultSet 객체를 반복하기 전 제목열 까지는 직접 생성하고, 데이터만 반복 -->
	<table border="1">
		<!-- 제목을 표시하기 위한 행(tr) 생성 -->
		<tr>
			<th width="100">이름</th>
			<th width="100">ID</th>
			<th width="100">비밀번호</th>
<!-- 			<th width="150">주민번호</th> -->
			<th width="150">E-Mail</th>
<!-- 			<th width="100">직업</th> -->
<!-- 			<th width="100">성별</th> -->
<!-- 			<th width="150">취미</th> -->
<!-- 			<th width="150">가입동기</th> -->
			<th></th>
		</tr>
	<%
	// 1, 2단계 작업을 대신 수행해주는 JdbcUtil 클래스의 getConnection() 메서드를 호출하여
	// 드라이버 로드, DB 접속을 수행한 후 Connection 타입 객체를 리턴받아 변수에 저장하기
	// => jsp9_jdbc 패키지의 JdbcUtil 클래스 인스턴스 생성한 후 getConnection() 메서드 호출
	JdbcUtil jdbcUtil = new JdbcUtil();
	Connection con = jdbcUtil.getConnection();
	
	String sql = "SELECT * FROM test4";
	PreparedStatement pstmt = con.prepareStatement(sql);
	ResultSet rs = pstmt.executeQuery();
	
	while(rs.next()) {
	%>
		<tr>
			<td><%=rs.getString("name") %></td>
			<td><%=rs.getString("id") %></td>
			<td><%=rs.getString("passwd") %></td>
<%-- 			<td><%=rs.getString("jumin") %></td> --%>
			<td><%=rs.getString("email") %></td>
<%-- 			<td><%=rs.getString("job") %></td> --%>
<%-- 			<td><%=rs.getString("gender") %></td> --%>
<%-- 			<td><%=rs.getString("hobby") %></td> --%>
<%-- 			<td><%=rs.getString("reason") %></td> --%>
			<td>
				<!-- 상세정보 버튼 클릭 시 jdbc_select3_detail.jsp 페이지로 이동(URL 에 id 파라미터 전달) -->
				<input type="button" value="상세정보" onclick="location.href='jdbc_select3_detail.jsp?id=<%=rs.getString("id") %>'">
				<!-- 삭제 버튼 클릭 시 jdbc_delete_pro2 페이지로 이동(URL 에 id 파라미터 전달) -->
				<input type="button" value="삭제" onclick="location.href='jdbc_delete_pro2.jsp?id=<%=rs.getString("id") %>'">
			</td>
		</tr>
	<%	
	}
	%>
	</table>
</body>
</html>


```

## DAO & DTO 패턴 
- 데이터베이스 작업을 수행하는 어플리케이션에서 대부분 사용하는 디자인 패턴

### DAO(Data Access Object, 데이터 접근 객체)
- 데이터베이스 작업에 필요한 코드만 따로 모아놓은(모듈화 한) 클래스(객체)
- 데이터베이스 연결 및 해제 작업과 각 SQL 구문을 실행하는 코드를 각각의 메서드를 통해 기술해 놓고 외부에서 호출해서 사용할 수 있도록 제공함
  - 외부에서 DAO 객체를 통해 메서드를 호출하여 각 데이터베이스 관련 작업 수행
- XXXDAO 형식의 클래스명을 사용하여 정의
  - 주로, XXX 은 해당 데이터베이스 작업을 수행하는 테이블명을 지정
    - ex) member 테이블 작업을 수행하는 DAO 클래스명 = MemberDAO  

### DTO(Data Transfer Object, 데이터 전송 객체)
- 데이터베이스 작업에 필요한 데이터를 보관하는 클래스(객체)
- DAO 객체가 데이터베이스 작업을 수행하기 위해 사용할 데이터를 전달하거나 DAO 객체로부터 데이터를 리턴받을 때 변수를 사용하여 각각의 데이터를 관리해도 되지만 DTO 객체를 통해 여러 데이터(변수)를 하나의 객체로 관리하고, 전송할 수 있도록 하기 위함
- 데이터베이스 테이블 내의 컬럼에 대응하는 멤버변수와 각 멤버변수에 대한 Getter/Setter 메서드로 구성됨
  - 필요에 따라 생성자도 정의할 수 있으며, 기본생성자는 무조건 포함해야함
- XXXDTO 형식의 클래스명을 사용하여 정의
  - 주로, XXX은 데이터베이스 작업을 수행하는 테이블명을 지정
    - ex) member 테이블 작업을 수행하는 DTO 클래스명 = MemberDTO
  - 단, JSP 어플리케이션 작성 시에는 DTO 클래스를 XXXBean 클래스로 작성함
    - JSP 진영의 JavaBeans 기술에서 사용하는 DTO 클래스의 관례적인 파일명
    - ex) MemberDTO => MemberBean => MemberVo 등
    - 프로젝트 수행하는 사람들의 특성에 따라 DTO 또는 Bean 또는 VO 로 명명   

```jsp

------------------------------------------------testBean_insert_Form.jsp------------------------------------------------

<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<h1>testBean_insert_form.jsp</h1>
	<form action="testBean_insert_pro.jsp" method="post">
		<table border="1">
			<tr>
				<td>학번</td>
				<td><input type="text" name="no"></td>
			</tr>
			<tr>
				<td>이름</td>
				<td><input type="text" name="name"></td>
			</tr>
			<tr>
				<td>나이</td>
				<td><input type="text" name="age"></td>
			</tr>
			<tr>
				<td>성별</td>
				<td>
					<input type="radio" name="gender" value="남">남
					<input type="radio" name="gender" value="여">여
				</td>
			</tr>
			<tr>
				<td colspan="2"><input type="submit" value="전송"></td>
			</tr>
		</table>
	</form>
</body>
</html>

------------------------------------------------testBean_insert_pro.jsp------------------------------------------------

<%@page import="jsp10_javabeans.Test3DTO"%>
<%@page import="jsp10_javabeans.Test3DAO"%>
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
	// testBean_insert_form.jsp 페이지로부터 전달받은 폼 파라미터를 가져와서 변수에 저장 후
	// test4DAO 클래스 인스턴스를 생성하여 메서드를 통해 INSERT 작업을 수행
	// 번호, 이름, 나이, 성별 파라미터 가져오기
	// => 주의! 한글 데이터가 포함되므로 UTF-8 설정 필수!
	request.setCharacterEncoding("UTF-8");
	
	int no = Integer.parseInt(request.getParameter("no")); // String -> int 변환 필수
	String name = request.getParameter("name");
	int age = Integer.parseInt(request.getParameter("age")); // String -> int 변환 필수
	String gender = request.getParameter("gender");
	
	// 작업에 사용될 데이터를 관리하는 Test3DTO 객체 생성 후 폼파라미터 데이터를 저장
	Test3DTO dto = new Test3DTO();
	// Setter 호출하여 데이터 저장
	dto.setNo(no);
	dto.setName(name);
	dto.setAge(age);
	dto.setGender(gender);
	
	// 데이터 작업을 수행하기 위한 Test3DAO 객체를 생성한 후
	// insert() 메서드를 호출하여 작업에 필요한 데이터를 전달 후 int 타입 리턴값 리턴받아 저장(insertCount)
	Test3DAO dao = new Test3DAO();
	int insertCount = dao.insert(dto);
	%>
	<%=insertCount %>
</body>
</html>

------------------------------------------------Test3DAO.java------------------------------------------------

package jsp10_javabeans;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.util.ArrayList;

public class Test3DAO {
	
	// ----------------------- 기초 설명을 위한 주석 포함 -------------------------
	// 회원 추가 작업을 수행하는 insert() 메서드 정의
	// => 파라미터 : 회원 정보가 저장된 Test3DTO 객체(dto), 리턴타입 : int
//	public int insert(Test3DTO dto) {
//		int insertCount = 0;
//		
//		// finally 블록에서 close() 메서드를 호출하여 자원을 반환하기 위해서는
//		// try 블록 바깥(위)쪽에서 Connection, PreparedStatement 등의 변수 선언 필요
//		Connection con = null;
//		PreparedStatement pstmt = null;
//		
//		try {
//			// ----------------------------------------------------------------
//			// 모듈화 없이 JDBC 연결(1단계 & 2단계)을 수행할 경우
////			String driver = "com.mysql.jdbc.Driver";
////			String url = "jdbc:mysql://localhost:3306/study_jsp3";
////			String user = "root";
////			String password = "1234";
//			
//			// 1단계. 드라이버 클래스 로드
////			Class.forName(driver);
//			
//			// 2단계. DB 연결
////			Connection con = DriverManager.getConnection(url, user, password);
//			// ----------------------------------------------------------------
//			// 모듈화를 통해 JDBC 연결(1단계 & 2단계)을 JdbcUtil 클래스 내에서 수행하고
//			// 연결 성공 시 리턴되는 Connection 객체를 리턴받아 DB 작업을 수행할 경우
//			// 1. JdbcUtil 클래스 인스턴스 생성
////			JdbcUtil jdbcUtil = new JdbcUtil();
//			// 2. JdbcUtil 인스턴스의 getConnection() 메서드를 호출하여 
//			//    DB 접속을 수행하고 성공 시 Connection 객체 리턴받기
////			Connection con = jdbcUtil.getConnection();
//			
//			// JdbcUtil 클래스 인스턴스 생성없이 클래스명만으로 접근하여 메서드 호출할 경우
//			// => JdbcUtil 클래스의 메서드들을 static 메서드로 선언해야함
////			Connection con = JdbcUtil.getConnection();
//			con = JdbcUtil.getConnection();
//			// ----------------------------------------------------------------
//			// 3단계. SQL 구문 작성 및 전달
//			String sql = "INSERT INTO test3 VALUES(?,?,?,?)";
////			PreparedStatement pstmt = con.prepareStatement(sql);
//			pstmt = con.prepareStatement(sql);
//			
//			// 만능문자(?) 파라미터 데이터 교체
//			// => 현재 메서드(insert()) 호출 시 전달받은 Test3DTO 객체로부터 데이터 꺼내기
//			pstmt.setInt(1, dto.getNo());
//			pstmt.setString(2, dto.getName());
//			pstmt.setInt(3, dto.getAge());
//			pstmt.setString(4, dto.getGender());
//			
//			// 4단계. SQL 구문 실행 및 결과 처리
//			insertCount = pstmt.executeUpdate();
//			
//		} catch (SQLException e) {
//			// 예외 발생 시(SQLException) 실행될 코드들의 위치(예외 미발생 시 실행되지 않음)
//			e.printStackTrace(); // 예외 발생 정보를 추적하여 모든 내용을 콘솔에 출력해줌
////			System.out.println(e.getMessage()); // 예외 발생 메세지를 리턴받아 콘솔에 직접 출력
//			System.out.println("SQL 구문 오류 발생! - insert()");
//		} finally {
//			// 예외 발생 여부와 관계없이 실행될 코드들의 위치
//			// => 주로, 데이터베이스 작업 시 데이터베이스 관련 자원들을 반환하는 코드를 기술
//			// => Connection, PreparedStatement, ResultSet 등 DB 자원을 관리하는 객체의
//			//    close() 메서드를 호출하여 자원을 반환
//			//    단, 자원 반환 순서는 객체 생성 순서의 역순으로 반환해야함
////			pstmt.close(); // 반환할 자원이 없을 경우 SQLException 발생할 수 있음
////			con.close(); 
//			
//			// 반환할 자원이 존재할 경우(= null 이 아닐 경우)에만 자원반환 수행
////			if(pstmt != null) {
////				try {
////					pstmt.close();
////				} catch (SQLException e) {
////					e.printStackTrace();
////				}
////			}
//			
////			if(con != null) {
////				try {
////					con.close();
////				} catch (SQLException e) {
////					e.printStackTrace();
////				}
////			}
//			
//			// JdbcUtil 클래스 내에 오버로딩 된 close() 메서드(static 메서드)를 호출하여
//			// Connection 및 PreparedStatement 객체 반환(= 역순으로 반환)
//			JdbcUtil.close(pstmt);
//			JdbcUtil.close(con);
//			
//		}
//		
//		return insertCount;
//	}
	// --------------------------------------------------------------------------------
	// 회원 추가 작업을 수행하는 insert() 메서드 정의
	// => 파라미터 : 회원 정보가 저장된 Test3DTO 객체(dto), 리턴타입 : int
	public int insert(Test3DTO dto) {
		int insertCount = 0;
		
		Connection con = null;
		PreparedStatement pstmt = null;
		
		try {
			// 1단계 & 2단계
			// JdbcUtil 객체의 getConnection() 메서드를 호출하여 DB 연결 객체 가져오기
			con = JdbcUtil.getConnection();
			
			// 3단계. SQL 구문 작성 및 전달
			String sql = "INSERT INTO test3 VALUES (?,?,?,?)";
			pstmt = con.prepareStatement(sql);
			pstmt.setInt(1, dto.getNo());
			pstmt.setString(2, dto.getName());
			pstmt.setInt(3, dto.getAge());
			pstmt.setString(4, dto.getGender());
			
			// 4단계. SQL 구문 실행 및 결과 처리
			insertCount = pstmt.executeUpdate();
			
		} catch (SQLException e) {
			e.printStackTrace();
			System.out.println("SQL 구문 오류 발생! - insert()");
		} finally {
			// DB 자원 반환 - JdbcUtil 클래스의 static 메서드 close() 호출(오버로딩) - 역순으로
			JdbcUtil.close(pstmt);
			JdbcUtil.close(con);
		}
		
		return insertCount;
		// => testBean_insert_pro.jsp 페이지의 int insertCount = dao.insert(dto); 코드로 돌아감
	}
	
	// 회원 정보 조회하여 리턴하는 select() 메서드 정의
	public ArrayList select() {
//		System.out.println("Test3DAO - select()");
		
		Connection con = null;
		PreparedStatement pstmt = null;
		ResultSet rs = null;
		
		// 전체 레코드(전체 회원 목록 정보)를 저장하는데 사용될 java.util.ArrayList 타입 변수 선언
		ArrayList dtoList = null;
		
		try {
			// 1단계 & 2단계 - JdbcUtil 클래스의 getConnection() 메서드 호출
			con = JdbcUtil.getConnection();
			
			// 3단계. SQL 구문 작성 및 전달
			// => test3 테이블의 모든 레코드 조회
			String sql = "SELECT * FROM test3";
			pstmt = con.prepareStatement(sql);
			
			// 4단계. SQL 구문 실행 및 결과 처리
			rs = pstmt.executeQuery();

			// 전체 레코드를 저장하기 위해 ArrayList 객체 생성
			// 주의! 반복문을 통해 Test3DTO 객체를 차례대로 저장해야하므로
			// 반복문보다 윗쪽(반복문의 바깥)에서 생성해야함
			dtoList = new ArrayList();
			
			// while 문을 사용하여 ResultSet 객체의 다음 레코드가 존재할 동안 반복
			while(rs.next()) { // 커서를 다음 레코드로 이동시켰을 때 존재할 경우 true 리턴됨
				
				// ResultSet 객체에 저장되어 있는 테이블 정보를 가공하여 외부로 리턴할 객체 생성
				// 1) 1개 레코드 정보를 저장하기 위한 DTO(Test3DTO) 객체 생성
				Test3DTO dto = new Test3DTO();
				
				// 2) Test3DTO 객체에 ResultSet 객체로부터 꺼낸 데이터 저장
				//    => ResultSet 객체의 getXXX() 메서드를 호출하여 데이터를 꺼내고(XXX : 타입명)
				//       Test3DTO 객체의 setXXX() 메서드를 호출하여 데이터 저장(XXX : 변수명)
				// 2-1) ResultSet 객체의 getInt() 메서드를 호출하여 번호를 꺼내고
				//      Test3DTO 객체의 setNo() 메서드를 호출하여 꺼낸 번호를 저장하기
				dto.setNo(rs.getInt("no"));
				// 2-2) 이름 : rs.getString(), dto.setName()
				dto.setName(rs.getString("name"));
				// 2-3) 나이 : rs.getInt(), dto.setAge()
				dto.setAge(rs.getInt("age"));
				// 2-4) 성별 : rs.getString(), dto.setGender()
				dto.setGender(rs.getString("gender"));
				
				// 3) 복수개의 Test3DTO 객체를 하나로 묶기 위해 ArrayList 객체 생성 후
				//    ArrayList 객체에 Test3DTO 객체를 차례대로 추가
				//    => ArrayList 객체의 add() 메서드를 호출하여 파라미터로 DTO 객체 전달
//				Object o = dto; // Test3DTO -> Object 타입으로 업캐스팅 발생
				dtoList.add(dto); // Test3DTO -> Object 타입으로 업캐스팅 발생
				// 이 과정을 반복하면 test3 테이블의 모든 레코드가 
				// Test3DTO 객체에 하나씩 저장되고, Test3DTO 객체를 차례대로 ArrayList 에 저장하게됨
			}
			
		} catch (SQLException e) {
			e.printStackTrace();
			System.out.println("SQL 구문 오류 발생! - select()");
		} finally {
			// DB 자원 반환(Connection, PreparedStatement, ResultSet => 역순)
			JdbcUtil.close(rs);
			JdbcUtil.close(pstmt);
			JdbcUtil.close(con);
		}
		
		// 전체 레코드가 저장된 ArrayList 객체 리턴
		return dtoList;
		
	}
	
}



------------------------------------------------Test3DTO.java------------------------------------------------

package jsp10_javabeans;

// test3 테이블에 사용될 데이터를 관리하는 Test3DTO 클래스 정의
public class Test3DTO {
	// test3 테이블의 각 컬럼에 대응하는 멤버변수 선언
	// => 외부에서 접근하지 못하도록 접근제한자를 private 으로 선언
	private int no;
	private String name;
	private int age;
	private String gender;
	
	// 생성자를 별도로 정의하지 않으면 컴파일러에 의해 기본생성자가 자동 정의됨
	// => 단, 파라미터 생성자를 정의할 경우 관례적으로 기본생성자도 정의해야함
	
	
	// private 으로 선언된 멤버변수를 대신 접근할 Getter / Setter 정의
	// => 누구나 접근 가능하도록 접근제한자를 public 으로 선언
	
	public int getNo() {
		return no;
	}
	public void setNo(int no) {
		this.no = no;
	}
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public int getAge() {
		return age;
	}
	public void setAge(int age) {
		this.age = age;
	}
	public String getGender() {
		return gender;
	}
	public void setGender(String gender) {
		this.gender = gender;
	}
	

	
	
}


```

```





