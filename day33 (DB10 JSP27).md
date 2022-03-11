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

```jsp
---------------------------------------JdbcUtil.java---------------------------------------

package jsp9_jdbc;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;

// JDBC 활용 시 데이터베이스 접근 관련 작업을 수행하는 용도의 클래스 정의
// => DB 접속, DB 자원 반환, 커밋 또는 롤백 작업 등을 수행
public class JdbcUtil {
	// 1. DB 접속을 수행하는 getConnection() 메서드 정의
	// => 파라미터 : 없음, 리턴타입 : java.sql.Connection
	// => DB 접속을 수행하여 접속 성공 시 접속 정보를 객체로 관리하는 Connection 타입 객체를 외부로 리턴
	public Connection getConnection() {
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

public class Test3DAO {

	// 회원 추가 작업을 수행하는 insert() 메서드 정의
	// => 파라미터 : 회원 정보가 저장된 Test3DTO 객체(dto), 리턴타입 : int
	public int insert(Test3DTO dto) {
		int insertCount = 0;
		
		// 임시. Test3DTO 객체에 저장된 데이터 출력해보기
		System.out.println("번호 : " + dto.getNo());
		System.out.println("이름 : " + dto.getName());
		System.out.println("나이 : " + dto.getAge());
		System.out.println("성별 : " + dto.getGender());

		return insertCount;
	}
	
}


------------------------------------------------Test3DTO.java------------------------------------------------

package jsp10_javabeans;

// test3 테이블에 사용될 데이터를 관리하는 Test3DTO 클래스 정의
public class Test3DTO {
	// test3 테이블의 각 컬럼에 대응하는 멤버변수 선언
	// => 외부에서 접근하지 모하도록 접근제한자를 private 으로 선언
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







