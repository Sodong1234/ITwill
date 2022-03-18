# [오전, 오후수업] JSP 29, 30차
> 28차에 실시한 수업의 추가본을 여기서 커밋함

### select 구문
```jsp
-----------------------------------------testBean_select.jsp-----------------------------------------
<%@page import="jsp10_javabeans.Test3DTO"%>
<%@page import="java.util.ArrayList"%>
<%@page import="jsp10_javabeans.Test3DAO"%>
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%
// Test3DAO 클래스의 인스턴스 생성 후 select() 메서드를 호출하여 학생 목록 정보 조회 요청
// => 파라미터 : 없음	리턴타입 : ?(임시로 없다고 가정)
Test3DAO dao = new Test3DAO();
ArrayList dtoList = dao.select();
%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<script type="text/javascript">
function confirmDelete(no) {
	if(confirm("삭제하시겠습니까?")) {
		// 삭제를 위해 testBean_delete.jsp 페이지로 이동(파라미터로 번호 전달)
		location.href="testBean_delete.jsp?no=" + no;
	}
}

</script>
</head>
<body>
	<h1>학생 목록 정보</h1>
	<table border="1">
		<tr>
			<th width="100">학번</th>
			<th width="100">이름</th>
<!-- 			<th width="100">나이</th> -->
<!-- 			<th width="100">성별</th> -->
			<th></th>
		</tr>
		<%
		// 배열과 마찬가지로 ArrayList 객체의 크기(size()) 크기보다 작을동안 반복
		for(int i = 0; i < dtoList.size(); i++) {
			// dtoList(ArrayList) 객체에는 Test3DTO 객체가 여러개 저장되어 있음
			// => ArrayList 객체의 get() 메서드를 호출하여 저장된 객체를 꺼낼 수 있음
			//	  이 때, 파라미터로 객체의 인덱스 지정
			// => get() 메서드를 호출하여 리턴되는 객체를 Test3DTO 타입 변수에 저장해야함
			//	  단, ArrayList 객체에 저장 시 adD() 메서드는 Object 타입 파라미터를 사용하므로
			// 	  Test3DTO -> Object 타입으로 업캐스팅 되어있으며
			//	  이를 다시 Test3DTO 타입으로 저장하려면 다운캐스팅이 필수!
// 			Test3DTO dto = dtoList.get(i); // 오류 발생! Object -> Test3DTO 타입으로 그냥 저장 불가
			Test3DTO dto = (Test3DTO)dtoList.get(i); // Object -> Test3DTO 타입으로 다운캐스팅
			
			// 테이블에 데이터 표시를 위해 td 태그 내에 Test3DTO 객체의 getXXX() 메서드를 호출하여
			// 저장되어 있는 컬럼 데이터 출력
			// => 이름에 하이퍼링크 설정(testBean_select_detail.jsp 페이지로 포워딩)
			//	  파라미터로 학번(no)을 전달
			
			
			%>
			<tr>
				<td><%=dto.getNo() %></td>
				<td><a href="testBean_select_detail.jsp?no=<%=dto.getNo() %>"><%=dto.getName() %></td>
<%-- 				<td><%=dto.getAge() %></td> --%>
<%-- 				<td><%=dto.getGender() %></td> --%>
				<td>
					<!-- 삭제 버튼 클릭 시 testBean_delete.jsp 페이지로 이동(파라미터 : 번호) -->
<%-- 					<input type="button" value="삭제" onclick="location.href='testBean_delete.jsp?no=<%=dto.getNo()%>'"> --%>
					<!-- 삭제 버튼 클릭 시 자바스크립트를 통해 확인 후 삭제하는 경우 -->
					<!-- confirmDelete() 함수 호출 시 파라미터로 삭제할 번호 전달 -->					
					<input type="button" value="삭제" onclick="confirmDelete(<%=dto.getNo()%>)">
				</td>
			</tr>
			<%
		}
		%>
		
	</table>
</body>
</html>

-----------------------------------------testBean_select_detail.jsp-----------------------------------------
<%@page import="jsp10_javabeans.Test3DTO"%>
<%@page import="jsp10_javabeans.Test3DAO"%>
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%
// testBean_select.jsp 페이지로부터 전달받은 URL 파라미터(no) 가져오기
int no = Integer.parseInt(request.getParameter("no"));

// Test3DAO 클래스 인스턴스 생성 후 select_detail() 메서드 호출하여 1명의 회원 정보 가져오기
// => 파라미터 : 번호(no) 	리턴타입 : Test3DTO(dto)

Test3DAO dao = new Test3DAO();
Test3DTO dto = dao.selectDetail(no);
%>    
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<h1>회원 상세정보</h1>
	<table border="1">
			<tr>
				<td>학번</td>
<%-- 				<td><%=dto.getNo() %></td> --%>
					<td><%=no %></td>
			</tr>
			<tr>
				<td>이름</td>
				<td><%=dto.getName() %></td>
			</tr>
			<tr>
				<td>나이</td>
				<td><%=dto.getAge() %></td>
			</tr>
			<tr>
				<td>성별</td>
				<td><%=dto.getGender() %></td>
			</tr>
			<tr>
				<td colspan="2">
					<!-- 수정 버튼 클릭 시 "testBean_update_form.jsp" 페이지로 이동 -->
					<!-- URL 파라미터로 번호(no) 전달 -->
					<input type="button" value="수정" onclick="location.href='testBean_update_form.jsp?no=<%=no%>'">
				</td>
			</tr>
		</table>
</body>
</html>

-----------------------------------------testBean_update_form.jsp-----------------------------------------

<%@page import="jsp10_javabeans.Test3DTO"%>
<%@page import="jsp10_javabeans.Test3DAO"%>
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%
// Test3DAO 객체의 selectDetail() 메서드를 호출하여 회원 1명의 정보 조회 = select 와 동일
// testBean_select.jsp 페이지로부터 전달받은 URL 파라미터(no) 가져오기
int no = Integer.parseInt(request.getParameter("no"));

// Test3DAO 클래스 인스턴스 생성 후 select_detail() 메서드 호출하여 1명의 회원 정보 가져오기
// => 파라미터 : 번호(no) 	리턴타입 : Test3DTO(dto)

Test3DAO dao = new Test3DAO();
Test3DTO dto = dao.selectDetail(no);
%>    
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<h1>회원 정보 수정</h1>
	<form action="testBean_update_pro.jsp" method="post">
	<!-- 번호는 데이터 전달 시 함께 전달해야하므로 input type="hidden" 으로 포함시켜 전달 -->
<%-- 		<input type="hidden" name="no" value="<%=no %>"> --%>
		<table border="1">
			<tr>
				<td>학번</td>
<%-- 				<td><%=no%></td> --%>
				<!-- 또는 편집은 불가능하게 하고, 폼에 포함시키려면 readonly 속성 설정 -->
				<td><input type="text" name="no" value="<%=no%>"></td>
			</tr>
			<!-- 이름, 나이, 성별 편집을 위해 기존 데이터를 폼에 표시 -->
			<tr>
				<td>이름</td>
				<td><input type="text" name="name" value="<%=dto.getName()%>"></td>
			</tr>
			<tr>
				<td>나이</td>
				<td><input type="text" name="age" value="<%=dto.getAge()%>"></td>
			</tr>
			<tr>
				<td>성별</td>
				<td>
					<!-- 성별 데이터에 맞게 선택하기 위해서 checked 속성을 if문으로 실행 여부 결정 -->
					<input type="radio" name="gender" value="남" 
						<%if(dto.getGender().equals("남")) {%>checked="checked"<%}%>>남
					<input type="radio" name="gender" value="여" 
						<%if(dto.getGender().equals("여")) {%>checked="checked"<%}%>>여
				</td>
			</tr>
			<tr>
				<td colspan="2">
					<input type="submit" value="수정">
				</td>
			</tr>
		</table>
	</form>
</body>
</html>

-----------------------------------------testBean_update_pro.jsp-----------------------------------------

<%@page import="jsp10_javabeans.Test3DAO"%>
<%@page import="jsp10_javabeans.Test3DTO"%>
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%
// 폼파라미터로 전달되는 이름, 나이, 성별 가져와서 출력
// => 폼 파라미터에 대한 한글 처리 필수
request.setCharacterEncoding("UTF-8");
int no = Integer.parseInt(request.getParameter("no"));
String name = request.getParameter("name");
int age = Integer.parseInt(request.getParameter("age"));
String gender = request.getParameter("name");

// 수정에 사용될 데이터를 직접 메서드에 전달해도 되지만 하나의 객체로 관리하기 위해 Test3DTO 객체
Test3DTO dto = new Test3DTO();
dto.setNo(no);
dto.setName(name);
dto.setAge(age);
dto.setGender(gender);



// Test3DAO 객체 생성 후 update() 메서드를 호출하여 회원 정보 수정
// => 파라미터 : Test3DTO 객체(dto)			리턴타입 : int(updateCount)
Test3DAO dao = new Test3DAO();
int updateCount = dao.update(dto);

// 리턴받은 결과값 판별
// => updateCount 가 0보다 크면 "testBean_select_detail.jsp" 페이지로 이동(URL에 번호를 포함)
// => 아니면, 자바스크립트를 사용하여 "수정 실패!" 출력 후 이전페이지로 돌아가기
if(updateCount > 0) {
	response.sendRedirect("testBean_select_detail.jsp?no=" + no);
} else {
	%>
	<script>
	alert("정보 수정 실패!");
	history.back();
	</script>
	<%
}
%>    
<%=no %>




-----------------------------------------testBean_delete.jsp-----------------------------------------

<%@page import="jsp10_javabeans.Test3DAO"%>
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%
// 파라미터로 전달받은 번호 가져오기
int no = Integer.parseInt(request.getParameter("no"));

// Test3DAO 인스턴스 생성 후 delete() 메서드 호출하여 회원 정보 삭제 작업 요청
// => 파라미터 : 번호(no)		 리턴타입 : int(deleteCount)
Test3DAO dao = new Test3DAO();
int deleteCount = dao.delete(no);



// 리턴받은 결과값 판별
// => deleteCount 가 0보다 크면 "testBean_select.jsp" 페이지로 이동
// => 아니면, 자바스크립트를 사용하여 "삭제 실패!" 출력 후 이전페이지로 돌아가기
if(deleteCount > 0) {
	response.sendRedirect("testbean_select.jsp");
} else {
	%>
	<script>
	alert("삭제 실패!");
	history.back();
	</script>
	<%
}
%>


```

```java
-----------------------------------------TEST3DTO.java-----------------------------------------


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




-----------------------------------------TEST3DAO.java-----------------------------------------
package jsp10_javabeans;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.util.ArrayList;

import com.mysql.cj.xdevapi.Result;

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
			// => 예외 
			JdbcUtil.close(rs);
			JdbcUtil.close(pstmt);
			JdbcUtil.close(con);
		}
		
		// 전체 레코드가 저장된 ArrayList 객체 리턴
		return dtoList;
		
	}
	
	
	public Test3DTO selectDetail(int no) {
		Test3DTO dto = null;
		
		Connection con = null;
		PreparedStatement pstmt = null;
		ResultSet rs = null;
		
		try {
			// 1단계 & 2단계
			con = JdbcUtil.getConnection();
			
			// 3단계. SQL 구문 작성
			// => test3 테이블의 번호(no)에 해당하는 레코드 조회
			String sql = "SELECT * FROM test3 WHERE no=?";
			pstmt = con.prepareStatement(sql);
			pstmt.setInt(1, no);
			
			// 4단계. SQL 구문 실행 및 결과 처리
			rs = pstmt.executeQuery();
			// 반복 작업이 아니므로 while문 대신 if문 사용 가능
			
			if(rs.next()) {
				// 조회 결과(번호, 이름, 나이, 성별)를 Test3DTO 객체에 저장
				dto = new Test3DTO();
				dto.setNo(rs.getInt("no")); // 파라미터로 전달받았으므로 생략 가능
				dto.setName(rs.getString("name"));
				dto.setAge(rs.getInt("age"));
				dto.setGender(rs.getString("gender"));
			} 
			
			
		} catch (SQLException e) {
			e.printStackTrace();
			System.out.println("SQL 구문 오류 발생! - selectDetail()");
		} finally {
			// 자원 반환
			JdbcUtil.close(rs);
			JdbcUtil.close(pstmt);
			JdbcUtil.close(con);
		}
		// 1개 레코드가 저장된 Test3DTO 객체 리턴
		
		return dto;
	}
	
	public int update(Test3DTO dto) {
		int updateCount = 0;
		
		// 번호에 해당하는 레코드의 이름, 나이, 성별을 수정(UPDATE)
		Connection con = null;
		PreparedStatement pstmt = null;
		
		try {
			con = JdbcUtil.getConnection();
			
			String sql = "UPDATE test3 SET name=?, age=?, gender=? WHERE no=?";
			pstmt = con.prepareStatement(sql);
			pstmt.setString(1,dto.getName());
			pstmt.setInt(2,dto.getAge());
			pstmt.setString(3,dto.getGender());
			pstmt.setInt(4, dto.getNo());
			
			updateCount = pstmt.executeUpdate();
		} catch (SQLException e) {
			e.printStackTrace();
			System.out.println("SQL 구문 오류 발생! - update()");
		} finally {
			JdbcUtil.close(con);
			JdbcUtil.close(pstmt);
		}

		return updateCount;
		
		
	}
	
	public int delete(int no) {
		int deleteCount = 0;
		
		Connection con = null;
		PreparedStatement pstmt = null;
		
		try {
			con = JdbcUtil.getConnection();
			
			String sql = "DELETE FROM test3 WHERE no=?";
			pstmt = con.prepareStatement(sql);
			pstmt.setInt(1, no);
			
			deleteCount = pstmt.executeUpdate();
		} catch (SQLException e) {
			
			e.printStackTrace();
			System.out.println("SQL 구문 오류 발생! - delete()");

		} finally {
			JdbcUtil.close(con);
			JdbcUtil.close(pstmt);
		}
		
		return deleteCount;
	}
}

```



### 간단한 게시판 만들어보기
> 계속해서 추가해나갈 예정

```jsp
------------------------------------------main.jsp------------------------------------------

<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%
// 세션 객체에 저장된 아이디("sId") 가져와서 변수 id 에 저장하기
String id = (String)session.getAttribute("sId"); // Object -> String 변환 필요
%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<h1>메인페이지</h1>
	<h3><a href="./board/write_form.jsp">글쓰기</a></h3>
	<h3><a href="./board/list.jsp">글목록</a></h3>
	<!-- 로그인이 되지 않았을 경우 회원가입, 로그인 링크 표시 -->
	<!-- 로그인이 되었을 경우 로그인된 아이디, 로그아웃 링크 표시 -->
	<%if(id == null) { %>
	<h3><a href="./member/join_form.jsp">회원가입</a></h3>
	<h3><a href="./member/login_form.jsp">로그인</a></h3>
	<%} else { %>
	<h3><%=id %>님 | <a href="./member/logout.jsp">로그아웃</a></h3>
	<%} %>
</body>
</html>

------------------------------------------login_form.jsp------------------------------------------
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<h1>로그인 페이지</h1>
	<form action="login_pro.jsp" method="post">
		<table border="1">
			<tr>
				<td>아이디</td>
				<td><input type="text" name="id" required="required"></td>
			</tr>
			<tr>
				<td>패스워드</td>
				<td><input type="password" name="passwd" required="required"></td>
			</tr>
			<tr>
				<td colspan="2"><input type="submit" value="로그인"></td>
			</tr>
		</table>
	</form>
</body>
</html>


------------------------------------------login_pro.jsp------------------------------------------

<%@page import="jsp11_board.MemberDAO"%>
<%@page import="jsp11_board.MemberDTO"%>
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%
// 아이디, 패스워드 파라미터 가져오기
String id = request.getParameter("id");
String passwd = request.getParameter("passwd");

MemberDAO dao = new MemberDAO();

// MemberDTO 객체에 id, passwd 저장 여부는 선택사항
// 1. MemberDTO 객체 사용시
// MemberDTO member = new MemberDTO();
// member.setId(id);
// member.setPasswd(passwd);

// boolean isLoginSuccess = dao.login(member);

// 2. MemberDTO 객체 미사용 시
boolean isLoginSuccess = dao.login(id, passwd);

// 로그인 작업 수행 후 결과를 boolean 타입(isLoginSuccess) 으로 리턴받아 결과에 따른 판별 작업을 수행
// 만약, 로그인 성공 시 (isLoginSuccess 가 true) 세션에 아이디("sId" 속성명)를 저장 후 메인페이지로 이동
// 로그인 실패 시 (isLoginSuccess 가 false) 자바스크립트를 통해 "로그인 실패" 출력 후 이전페이지로 이동

if(isLoginSuccess) {
	session.setAttribute("sId", id);
	response.sendRedirect("../main.jsp");
} else {
	%>
	<script>
		alert("로그인 실패");
		history.back();
	</script>
	<%
}
%>

------------------------------------------join_form.jsp------------------------------------------

<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<h1>회원 가입</h1>
	<form action="join_pro.jsp" method="post">
	<table border="1">
			<tr>
				<th>아이디</th>
				<td><input type="text" name="id" required="required"></td>
			</tr>
			<tr>
				<th>패스워드</th>
				<td><input type="password" name="passwd" required="required"></td>
			</tr>
			<tr>
				<th>이름</th>
				<td><input type="text" name="name" required="required"></td>
			</tr>
			<tr>
				<th>E-Mail</th>
				<td><input type="text" name="email" required="required"></td>
			</tr>
			<tr>
				<th>전화번호</th>
				<td><input type="text" name="phone" required="required"></td>
			</tr>
			<tr>
				<td colspan="2">
					<input type="submit" value="회원가입">
					<input type="reset" value="초기화">
					<input type="button" value="취소" onclick="history.back()">
				</td>
			</tr>
		</table>
	
	</form>
</body>
</html>


------------------------------------------join_pro.jsp------------------------------------------

<%@page import="jsp11_board.MemberDAO"%>
<%@page import="jsp11_board.MemberDTO"%>
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%
// POST 방식 한글 처리
request.setCharacterEncoding("UTF-8");

// 폼 파라미터 가져오기
String id = request.getParameter("id");
String passwd = request.getParameter("passwd");
String name = request.getParameter("name");
String email = request.getParameter("email");
String phone = request.getParameter("phone");

// 데이터를 MemberDTO 객체에 저장
// MemberDTO member = new MemberDTO();
// member.setId(id);
// member.setPasswd(passwd);
// member.setName(name);
// member.setEmail(email);
// member.setPhone(phone);

// 만약, 파라미터 생성자를 정의했을 경우
MemberDTO member = new MemberDTO(id, passwd, name, email, phone);

// MemberDAO 클래스 객체 생성 후 insert() 메서드 호출
// => 파라미터 : MemberDTO 객체(member), 리턴타입 : int(insertCount)
MemberDAO dao = new MemberDAO();
int insertCount = dao.insert(member);


// insertCount 가 0보다 크면 main.jsp 페이지로 이동하고
// 아니면, 자바스크립트를 통해 "회원 가입 실패!" 출력 후 이전페이지로 돌아가기
if(insertCount > 0) {
	// 절대경로로 지정할 경우
	// request.getContextPath() 메서드(프로젝트명까지의 경로 리턴)를 활용하여 이동 경로 설정
// 	response.sendRedirect(request.getContextPath()); // http://localhost:8080/StudyJSP/
	response.sendRedirect(request.getContextPath() + "/jsp11_board/main.jsp");

	// 상대경로로 지정할 경우
// 	response.sendRedirect("../main.jsp");
} else {
	%>
	<script>
	alert("회원 가입 실패!");
	history.back();
	</script>
	<%
}

%>


------------------------------------------write_form.jsp------------------------------------------

<%@page import="jsp11_board.BoardDTO"%>
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<h1>글쓰기</h1>
	<!-- form 태그 액션을 writePro.jsp 로 지정하고, 메서드는 POST 방식으로 지정 -->
	<!-- table 태그를 통해 작성자(name), 비밀번호(pass), 제목(subject), 내용(content) 입력 받기 -->
	<!-- submit(글쓰기), reset(초기화), button(취소 => 클릭 시 이전페이지로 돌아가기) -->
	
	<form action="writePro.jsp" method="POST">
		<table border="1">
			<tr>
				<th>작성자</th>
				<td><input type="text" name="name"></td>
			</tr>
			<tr>
				<th>비밀번호</th>
				<td><input type="password" name="pass"></td>
			</tr>
			<tr>
				<th>제목</th>
				<td><input type="text" name="subject"></td>
			</tr>
			<tr>
				<th>내용</th>
				<td><textarea name="content" rows="10" cols="20"></textarea>
			</tr>	
			<tr>
				<td colspan="2">
				<input type="submit" value="글쓰기" onclick="write_pro.jsp">
				<input type="reset" value="초기화">
				<input type="button" value="취소" onclick="history.back()">
				</td>
			</tr>			
		</table>
		
	</form>
</body>
</html>

------------------------------------------writePro.jsp------------------------------------------
<%@page import="jsp11_board.BoardDAO"%>
<%@page import="jsp11_board.BoardDTO"%>
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%
// write_pro.jsp 페이지에서 보여줄(표시할) 웹페이지가 없다면 <HTML> 태그 불필요(삭제 가능)
// POST 방식 한글 처리
request.setCharacterEncoding("UTF-8");

// write_form.jsp 페이지로부터 전달받은 폼파라미터(작성자, 비밀번호, 제목, 내용)을 가져와서
// 변수에 저장 후 출력

String name = request.getParameter("name");
String pass = request.getParameter("pass");
String subject = request.getParameter("subject");
String content = request.getParameter("content");

// 폼파라미터 데이터를 하나의 객체로 다루기 위해 BoardDTO 객체 생성 후 저장

// BoardDTO board = new BoardDTO();
// board.setName(name);
// board.setPass(pass);
// board.setSubject(subject);
// board.setContent(content);
// => 만약 파라미터 생성자가 정의되어 있을 경우 생성자를 통해 데이터 전달
BoardDTO board = new BoardDTO(name, pass, subject, content);

// BoardDAO 객체 생성 후 insert() 메서드를 호출하여 글쓰기 작업 요청
// => 파라미터 : BoardDTO 객체(board)	리턴타입 : int(insertCount)
BoardDAO dao = new BoardDAO();
int insertCount = dao.insert(board);

// 글쓰기 작업 요청 결과에 따른 판별
// => 글쓰기 성공 시 "list.jsp" 페이지로 포워딩
// => 글쓰기 실패 시 자바스크립트를 사용하여 "글쓰기 실패!" 출력 후 이전 페이지로 돌아가기
if(insertCount > 0) {
	response.sendRedirect("list.jsp");
} else {
	%>
	<script>
		alert("글쓰기 실패!")
		history.back();
	</script>
	<%
}


%> 

------------------------------------------list.java------------------------------------------
<%@page import="jsp11_board.BoardDTO"%>
<%@page import="java.util.List"%>
<%@page import="jsp11_board.BoardDAO"%>
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%
// BoardDAO 객체의 selectList() 메서드를 호출하여 전체 레코드 조회 요청
// => 파라미터 : 없음		리턴타입 : ArrayList(boardList)

BoardDAO dao = new BoardDAO();
// List boardList = dao.selectList();

// 컬렉션 객체들(List 등)은 선언 및 객체 생성 시 제네릭타입으로 저장되는 데이터의 타입 지정 가능함
// => 컬렉션클래스명 뒤에 <> 기호 사이에 저장될 객체의 타입을 지정 가능
//	  ex) List 객체에 저장될 데이터가 BoardDTO 타입일 경우 List<BoardDTO> 형태로 명시
List<BoardDTO> boardList = dao.selectList();

%>    
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<h1>글목록</h1>
	<table border="1">
		<tr>
			<th width="150">작성자</th>
			<th width="300">제목</th>
			<th width="500">내용</th>
		</tr>
		<%
		// List 객체 크기만큼 for문을 사용하여 반복
		for(int i = 0; i < boardList.size(); i++) {
			// ArrayList(boardList) 객체에서 1개 레코드를 저장한 BoardDTO 객체 꺼내기
			// => ArrayList 객체의 get() 메서드를 호출하여 인덱스 지정
// 			BoardDTO board = (BoardDTO)boardList.get(i); // 다운캐스팅 필수!
		// ---------------------------------------------------------------------
		// List 객체에 제네릭 타입으로 BoardDTO 타입을 선언한 경우
		// Object 타입으로 업캐스팅 된 객체가 아닌 BoardDTO 객체 그대로 저장되어 있으므로
		// 꺼낸 후 다운캐스팅이 불필요하다!
		BoardDTO board = boardList.get(i);
		%>
		<tr>
			<td><%=board.getName() %></td>
			<td><%=board.getSubject() %></td>
			<td><%=board.getContent() %></td>
		</tr>
		<%
		}
		%>
	</table>
</body>
</html>
------------------------------------------logout.java------------------------------------------

<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%
// 세션 객체 초기화(invalidate()) 를 수행 후 메인페이지로 이동
session.invalidate();
response.sendRedirect("../main.jsp");
%>
```



```java

------------------------------------------MemberDAO.java------------------------------------------

package jsp11_board;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;

public class MemberDAO {

		// 회원 가입을 수행하는 insert() 메서드 정의
//		=> 파라미터 : MemberDTO 객체(member), 리턴타입 : int(insertCount)
		public int insert(MemberDTO member) {
			int insertCount = 0;
			
			Connection con = null;
			PreparedStatement pstmt = null;
			
			try {
				con = JdbcUtil.getConnection();
				
				// memberDTO 객체에 저장된 아이디, 패스워드 이름, 이메일, 전화번호를 추가하고
				// 가입일(date)의 경우 데이터베이스에서 제공되는 now() 함수 사용하여 자동 생성
				String sql = "INSERT INTO member VALUES (?,?,?,?,?,now())";
				pstmt = con.prepareStatement(sql);
				pstmt.setString(1, member.getId());
				pstmt.setString(2, member.getPasswd());
				pstmt.setString(3, member.getName());
				pstmt.setString(4, member.getEmail());
				pstmt.setString(5, member.getPhone());

				
				insertCount = pstmt.executeUpdate();
				
			} catch (SQLException e) {
				e.printStackTrace();
				System.out.println("SQL 구문 오류 발생! - insert()");

			} finally {
				JdbcUtil.close(con);
				JdbcUtil.close(pstmt);
			}
			return insertCount;
		}
		
		// 로그인 작업 수행 후 결과를 boolean 타입으로 리턴하는 login() 메서드 정의
		public boolean login(String id, String passwd) {
			boolean isLoginSuccess = false;
			
			Connection con = null;
			PreparedStatement pstmt = null;
			ResultSet rs = null;
			
			try {
				con = JdbcUtil.getConnection();
				
				String sql = "SELECT id FROM member WHERE id=? AND passwd=?";
				pstmt = con.prepareStatement(sql);
				pstmt.setString(1, id);
				pstmt.setString(2, passwd);

				rs = pstmt.executeQuery();
				// 조회 결과가 존재할 경우(= rs.next() 가 true일 경우)
				// isLoginSuccess 변수값을 true 로 변경
				if(rs.next()) {
					isLoginSuccess = true;
				}
				
			} catch (SQLException e) {
				e.printStackTrace();
				System.out.println("SQL 구문 오류 - login()");
			} finally {
				JdbcUtil.close(con);
				JdbcUtil.close(pstmt);
				JdbcUtil.close(rs);

			}
			return isLoginSuccess;
		}
	}





------------------------------------------MemberDTO.java------------------------------------------

package jsp11_board;

import java.sql.Date;

/*
 * member 테이블 정의
 * 1. 아이디 - id 컬럼(문자 16자) 주키(Primary key)로 지정
 * 2. 패스워드 - passwd 컬럼(문자 16자) null값 금지(NOT NULL 제약조건)
 * 3. 이름 - name 컬럼(문자 10자) null값 금지(NOT NULL 제약조건)
 * 4. 이메일 - email 컬럼(문자 50자) 중복금지(UNIQUE 제약조건),  null값 금지(NOT NULL 제약조건)
 * 5. 전화번호 - phone 컬럼(문자20자) 중복금지(UNIQUE 제약조건),  null값 금지(NOT NULL 제약조건)
 * 6. 가입일 - date 컬럼(날짜 - DATE)
 * 
 * CREATE TABLE member ( 
 * 		id VARCHAR(16) PRIMARY KEY, 
 * 		passwd VARCHAR(16) NOT NULL, 
 * 		name VARCHAR(10) NOT NULL, 
 * 		email VARCHAR(50) UNIQUE NOT NULL, 
 * 		phone VARCHAR(20) UNIQUE NOT NULL,
 * 		date DATE
 */

public class MemberDTO {
	// member 테이블 컬럼에 대응하는 멤버변수 선언
	private String id;
	private String passwd;
	private String name;
	private String email;
	private String phone;
	private Date date;
	
	// 기본생성자
	public MemberDTO() {}
	
	// 파라미터 생성자
	public MemberDTO(String id, String passwd, String name, String email, String phone) {
		this.id = id;
		this.passwd = passwd;
		this.name = name;
		this.email = email;
		this.phone = phone;
	}


	// Getter/Setter 정의
	public String getId() {
		return id;
	}
	public void setId(String id) {
		this.id = id;
	}
	public String getPasswd() {
		return passwd;
	}
	public void setPasswd(String passwd) {
		this.passwd = passwd;
	}
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public String getEmail() {
		return email;
	}
	public void setEmail(String email) {
		this.email = email;
	}
	public String getPhone() {
		return phone;
	}
	public void setPhone(String phone) {
		this.phone = phone;
	}
	public Date getDate() {
		return date;
	}
	public void setDate(Date date) {
		this.date = date;
	}



	

	
	
	public static void main(String[] args) {
		

	}

}


------------------------------------------BoardDAO.java------------------------------------------
package jsp11_board;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.util.ArrayList;
import java.util.List;

public class BoardDAO {

	public int insert(BoardDTO board) {
		int insertCount = 0;

		Connection con = null;
		PreparedStatement pstmt = null;

		try {
			con = JdbcUtil.getConnection();

			String sql = "INSERT INTO board VALUES (?,?,?,?)";
			pstmt = con.prepareStatement(sql);
			pstmt.setString(1, board.getName());
			pstmt.setString(2, board.getPass());
			pstmt.setString(3, board.getSubject());
			pstmt.setString(4, board.getContent());

			insertCount = pstmt.executeUpdate();

		} catch (SQLException e) {
			e.printStackTrace();
			System.out.println("SQL 구문 오류 발생! - insert()");

		} finally {
			JdbcUtil.close(con);
			JdbcUtil.close(pstmt);
		}
		return insertCount;
	}
	
	
	
	// 제네릭타입 BoardDTO 지정
	public List<BoardDTO> selectList() {
		// List 타입 변수 선언 시 제네릭타입으로 BoardDTO 타입을 지정할 경우
		// 해당 객체의 메서드에 사용되는 Object 타입은 모두 BoardDTO 타입으로 변경됨
		List<BoardDTO> boardList = null;
		
		Connection con = null;
		PreparedStatement pstmt = null;
		ResultSet rs = null;
		
		try {
			con = JdbcUtil.getConnection();

			String sql = "SELECT * FROM board";
			pstmt = con.prepareStatement(sql);

			rs = pstmt.executeQuery();
			
			// 객체 생성 시 제네릭 타입 지정 가능
			boardList = new ArrayList<BoardDTO>();
			while(rs.next()) {
//				String name = rs.getString("name");
//				String pass = rs.getString("pass");
//				String subject = rs.getString("subject");
//				String content = rs.getString("content");
//				System.out.println(name + ", " + pass + ", " + subject + ", " + content);
			
			// 1개 레코드에 해당하는 데이터를 BoardDTO 객체에 저장
				BoardDTO board = new BoardDTO();
				board.setName(rs.getString("name"));
//				board.setPass(rs.getString("pass")); // 패스워드는 불필요하므로 제외
				board.setSubject(rs.getString("subject"));
				board.setContent(rs.getString("content"));

				// 모든 레코드를 저장하는 ArrayList 객체에 1개 레코드를 저장한 BoardDTO 객체 추가
				// 만약, 제네릭타입으로 BoardDTO 타입을 선언했을 경우
				// add() 메서드의 파라미터 타입이 Object 가 아닌 BoardDTO 타입으로 사용됨
				// => 즉, Object 타입이 아닌 자기 자신이므로 업캐스팅이 일어나지 않는다!
				boardList.add(board);
				// 제네릭 타입에 맞지 않는 타입은 사용 불가능
//				boardList.add("홍길동");
			}
		} catch (SQLException e) {
			e.printStackTrace();
			System.out.println("SQL 구문 오류 발생! - selectList()");

		} finally {
			JdbcUtil.close(rs);
			JdbcUtil.close(pstmt);
			JdbcUtil.close(con);
			
		}
		return boardList;
	}
}

------------------------------------------BoardDTO.java------------------------------------------
package jsp11_board;

/*
CREATE TABLE board (
 	name VARCHAR(16),
 	pass VARCHAR(16),
 	subject VARCHAR(50),
 	content VARCHAR(2000)
); 
 */
public class BoardDTO {
	private String name;
	private String pass;
	private String subject;
	private String content;
	
	public BoardDTO() {}
	
	public BoardDTO(String name, String pass, String subject, String content) {
		super();
		this.name = name;
		this.pass = pass;
		this.subject = subject;
		this.content = content;
	}
	
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public String getPass() {
		return pass;
	}
	public void setPass(String pass) {
		this.pass = pass;
	}
	public String getSubject() {
		return subject;
	}
	public void setSubject(String subject) {
		this.subject = subject;
	}
	public String getContent() {
		return content;
	}
	public void setContent(String content) {
		this.content = content;
	}
	
	
	
}
```
