# [오전수업] DB 23차

> 22차 수업 복습 실시

### INLINE VIEW
- 가상의 테이블을 구현할 수 있는 VIEW의 일종인 INLINE VIEW는 서브쿼리를 통해서 구현가능하다.
- 서브쿼리의 출력 결과가 가상의 테이블로 사용된다. 이렇게 생성된 VIEW의 경우 뷰의 이름을 활용하기 어렵기 때문에 필수적으로 table alias를 활용한다.
- 또한 서브쿼리의 출력 컬럼이 view의 컬럼으로 사용되는데 컬럼명으로 특수기호들은 사용이 안되므로 함수나 표현식의 연산 내용이 있는 경우 이를 column alias로 이름을 대체해야 한다.
- column alias를 사용한 경우 view의 컬럼명은 column alias로 적용된다.

```

SELECT a.last_name, a.salary, a.department_id, b.salavg
FROM employees a
JOIN (
    SELECT department_id, trunc(AVG(salary)) salavg
    FROM employees
    GROUP BY department_id
) b ON a.department_id = b.department_id
       AND a.salary > b.salavg;

LAST_NAME|SALARY|DEPARTMENT_ID|SALAVG|
---------+------+-------------+------+
King     | 24000|           90| 19333|
Hunold   |  9000|           60|  5760|
Ernst    |  6000|           60|  5760|
Greenberg| 12008|          100|  8601|
Faviet   |  9000|          100|  8601|
Raphaely | 11000|           30|  4150|
Weiss    |  8000|           50|  3475|
Fripp    |  8200|           50|  3475|
Kaufling |  7900|           50|  3475|
Vollman  |  6500|           50|  3475|
Mourgos  |  5800|           50|  3475|
Ladwig   |  3600|           50|  3475|
Rajs     |  3500|           50|  3475|
Russell  | 14000|           80|  8955|
Partners | 13500|           80|  8955|
…


-----------------------------------------------------------------------------------------------------------------------------------------------



TOP n 쿼리
rownum은 의사컬럼으로 테이블에 숨겨진 컬럼이다. rownum은 테이블의 기본 입력 순서값을 저장하고 있고, 데이터를 입력한 순으로 값이 매겨진다.
INLINE VIEW로 테이블을 원하는 순서로 정렬해서 출력하게 되면 이 inline view에서는 rownum의 값이 정렬된 순서로 적용된다.(테이블 원본은 바뀌지 않음)

SELECT employee_id, last_name, salary
FROM (
    SELECT *
    FROM employees
    ORDER BY salary DESC
)
WHERE rownum <= 10;


-----------------------------------------------------------------------------------------------------------------------------------------------



VIEW로 만들기
view는 구문 형태로만 만들어지는 오브젝트로 생성하더라도 자원을 거의 소모하지 않는다.
베이스 테이블의 데이터를 가져와서 출력하는 형태이므로 데이터의 중복 문제 없이 다양하게 원하는 형태로 구조를 가공하여 사용할 수 있는 장점이 있는 오브젝트 → 10단원에서 다룰 예정!!
CREATE VIEW salary_top_10 AS
    SELECT employee_id, last_name, salary
    FROM (
        SELECT *
        FROM employees
        ORDER BY salary DESC
    )
    WHERE ROWNUM <= 10;


SELECT *
FROM salary_top_10;


```


연습문제
```
p.28
SELECT employee_id, last_name, salary
FROM employees
WHERE salary >= (
    SELECT AVG(salary)
    FROM employees
)
ORDER BY salary ASC;

EMPLOYEE_ID|LAST_NAME |SALARY|
-----------+----------+------+
        203|Mavris    |  6500|
        123|Vollman   |  6500|
        165|Lee       |  6800|
        113|Popp      |  6900|
        155|Tuvault   |  7000|
        161|Sewall    |  7000|
        178|Grant     |  7000|
        164|Marvins   |  7200|
        172|Bates     |  7300|
        171|Smith     |  7400|
        154|Cambrault |  7500|
```


---

# [오후수업] JSP 53차
- qna_board_view.jsp & qna_board_delete.jsp 파일 수정
- qna_board_modify.jsp 파일 생성

```jsp
---------------------------------------------qna_board_view.jsp---------------------------------------------

<%@page import="vo.BoardDTO"%>
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%
// BoardDTO article = (BoardDTO)request.getAttribute("article"); // request 객체에 저장된 article 객체 저장
// String pageNum = request.getParameter("page"); // URL 에 전달받은 page 파라미터 저장
%>
    
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>MVC 게시판</title>
<style type="text/css">
	#articleForm {
		width: 500px;
		height: 550px;
		border: 1px solid red;
		margin: auto;
	}
	
	h2 {
		text-align: center;
	}
	
	table {
		border: 1px solid black;
		border-collapse: collapse; 
	 	width: 500px;
	}
	
	th {
		text-align: center;
	}
	
	td {
		width: 150px;
		text-align: center;
	}
	
	#basicInfoArea {
		height: 70px;
		text-align: center;
	}
	
	#articleContentArea {
		background: orange;
		margin-top: 20px;
		height: 350px;
		text-align: center;
		overflow: auto;
		white-space: pre-line;
	}
	
	#commandList {
		margin: auto;
		width: 500px;
		text-align: center;
	}
</style>
</head>
<body>
	<!-- 게시판 상세내용 보기 -->
	<section id="articleForm">
		<h2>글 상세내용 보기</h2>
		<section id="basicInfoArea">
			<table border="1">
			<tr><th width="70">제 목</th><td colspan="3" >${article.getBoard_subject() }</td></tr>
			<tr>
				<th width="70">작성자</th><td>${article.getBoard_name() }</td>
				<th width="70">작성일</th><td>${article.getBoard_date() }</td>
			</tr>
			<tr>
				<th width="70">첨부파일</th>
				<td>
					<!-- 
					파일 다운로드 링크 사용법(중복 파일명 처리로 인해 실제 파일과 업로드 한 파일명이 다를 수 있음)
					<a href="다운로드 할 실제 파일명(위치포함)" download> 화면에 보여줄 파일명 
					-->
					<a href="./upload/${article.getBoard_real_file() }" download="${article.getBoard_file() }">
						${article.getBoard_file() }
					</a>
				</td>
				<th  width="70">조회수</th>
				<td>${article.getBoard_readcount()}</td>
			</tr>
			</table>
		</section>
		<section id="articleContentArea">
			${article.getBoard_content() }
		</section>
	</section>
	<section id="commandList">
		<input type="button" value="답변" onclick="location.href='BoardReplyForm.bo?board_num=${param.board_num}&page=${param.page}'">
		<input type="button" value="수정" onclick="location.href='BoardModifyForm.bo?board_num=${param.board_num}&page=${param.page}'">
		<input type="button" value="삭제" onclick="location.href='BoardDeleteForm.bo?board_num=${param.board_num}&page=${param.page}'">
		<input type="button" value="목록" onclick="location.href='BoardList.bo?page=${param.page}'">
	</section>
</body>
</html>


---------------------------------------------qna_board_delete.jsp---------------------------------------------

<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>MVC 게시판</title>
<style>
	#passForm {
		width: 300px;
		margin: auto;
		border: 1px solid orange;
		text-align: center;
	}
	
	h2 {
		text-align: center;
	}
	
	table {
		width: 300px;
		margin: auto;
		text-align: center;
	}
	
</style>
</head>
<body>
	<!-- 게시판 글 삭제 -->
	<h2>게시판 글 삭제</h2>
	<section id="passForm">
		<form action="BoardDeletePro.bo" name="deleteForm" method="post">
			<!-- input type="hidden" 속성을 통해 board_num, page 파라미터를 다시 전달해야함 -->
			<input type="hidden" name="board_num" value="${param.board_num }">
			<input type="hidden" name="page" value="${param.page }">
			<table>
				<tr>
					<td><label>글 비밀번호 : </label></td>
					<td><input type="password" name="board_pass" required="required"></td>
				</tr>
				<tr>
					<td colspan="2">
						<input type="submit" value="삭제">&nbsp;&nbsp;
						<input type="button" value="돌아가기" onclick="javascript:history.back()">
					</td>
				</tr>
			</table>
		</form>
	</section>
</body>
</html>


---------------------------------------------qna_board_modify.jsp---------------------------------------------


<%@page import="vo.BoardDTO"%>
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%
// request 객체에 저장된 BoardDTO 객체("article") 가져오기
BoardDTO article = (BoardDTO)request.getAttribute("article");
%>    
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>MVC 게시판</title>
<style type="text/css">
	#modifyForm {
		width: 500px;
		height: 610px;
		border: 1p solid red;
		margin: auto;
	}
	
	h1 {
		text-align: center;
	}
	
	table {
		margin: auto;
		width: 450px;
	}
	
	.td_left {
		width: 150px;
		background: orange;
	}
	
	.td_right {
		width: 300px;
		background: skyblue;
	}
	
	#commandCell {
		text-align: center;
	}
</style>
</head>
<body>
	
	<!-- 게시판 글 수정 -->
	<section id="modifyForm">
		<h1>게시판 글 수정</h1>
		<!-- 수정 대상에서 파일은 제외시킬 경우 enctype 속성 제거(일반 form) -->
		<form action="BoardModifyPro.bo" name="boardForm" method="post">
			<!-- input type="hidden" 사용하여 글번호(board_num)와 페이지번호(page) 전달 -->
			<input type="hidden" name="board_num" value="<%=article.getBoard_num()%>">
			<input type="hidden" name="page" value="<%=request.getParameter("page")%>">
			<table>
				<tr>
					<td class="td_left"><label for="board_name">글쓴이</label></td>
					<td class="td_right">
						<input type="text" name="board_name" value="<%=article.getBoard_name() %>" required="required" />
					</td>
				</tr>
				<tr>
					<td class="td_left"><label for="board_pass">비밀번호</label></td>
					<td class="td_right"><input type="password" name="board_pass" required="required" /></td>
				</tr>
				<tr>
					<td class="td_left"><label for="board_subject">제목</label></td>
					<td class="td_right"><input type="text" name="board_subject" value="<%=article.getBoard_subject() %>" required="required" /></td>
				</tr>
				<tr>
					<td class="td_left"><label for="board_content">내용</label></td>
					<td class="td_right">
						<textarea id="board_content" name="board_content" cols="40" rows="15" required="required"><%=article.getBoard_content() %></textarea>
					</td>
				</tr>
				<tr>
					<td class="td_left"><label for="board_file">파일</label></td>
					<!-- 파일 수정 기능은 제외(파일명만 표시) -->
					<td class="td_right"><%=article.getBoard_file() %> (수정불가)</td>
				</tr>	
			</table>
			<section id="commandCell">
				<input type="submit" value="등록">&nbsp;&nbsp;
				<input type="reset" value="다시쓰기">&nbsp;&nbsp;
				<input type="button" value="취소" onclick="history.back()">
			</section>
		</form>
	</section>
	
</body>
</html>


```

- action 폴더의 BoardDeleteProAction.java 파일 수정
- action 폴더에 BoardModifyProAction.java, BoardModifyProAction.java 파일 생성

```java
---------------------------------------------BoardDeleteProAction.java---------------------------------------------

package action;

import java.io.PrintWriter;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import svc.BoardDeleteProService;
import vo.ActionForward;

public class BoardDeleteProAction implements Action {

	@Override
	public ActionForward execute(HttpServletRequest request, HttpServletResponse response) throws Exception {
		System.out.println("BoardDeleteProAction");
		
		ActionForward forward = null;
		
		// 전달받은 파라미터 가져오기
		int board_num = Integer.parseInt(request.getParameter("board_num"));
		String pageNum = request.getParameter("page");
		
		String board_pass = request.getParameter("board_pass");
		
		// BoardDeleteProService 클래스의 isArticleWriter() 메서드를 호출하여 패스워드 판별 요청
		// => 파라미터 : 글번호(board_num), 패스워드(board_pass)
		//	  리턴타입 : boolean(isArticleWriter)
		BoardDeleteProService service = new BoardDeleteProService();
		boolean isArticleWriter = service.isArticleWriter(board_num, board_pass);
				
		// 패스워드가 일치하지 않을 경우 자바스크립트를 사용하여
		// "삭제 권한이 없습니다!" 출력 후 이전페이지로 돌아가기
		if(!isArticleWriter) {
			response.setContentType("text/html; charset=UTF-8");
			PrintWriter out = response.getWriter();
			out.println("<script>");
			out.println("alert('삭제 권한이 없습니다!')");
			out.println("history.back()");
			out.println("</script>");
		} else { // 패스워드가 일치할 경우(= 삭제 권한 있음)
			// 글번호(board_num)를 사용하여 삭제 작업 수행 
			// => service 클래스의 removeArticle() 메서드 호출
			//	  리턴타입 : boolean(isDeleteSuccess)
			boolean isDeleteSuccess = service.removeArticle(board_num);
			
			// 삭제 작업 후 리턴받은  결과 판별
			// 삭제 실패 시(isDeleteSuccess 가 false) 자바스크립트를 통해
			// "삭제 실패!" 출력하고 이전페이지로 돌아가기
			if(!isDeleteSuccess) {
				response.setContentType("text/html; charset=UTF-8");
				PrintWriter out = response.getWriter();
				out.println("<script>");
				out.println("alert('삭제 실패!')");
				out.println("history.back()");
				out.println("</script>");
			} else {
				// 삭제 성공 시(isDeleteSuccess 가 true) BoardList.bo 페이지 포워딩 설정
				// => 페이지 번호를 URL 에 포함시켜 포워딩
				forward = new ActionForward();
				forward.setPath("BoardList.bo?page=" + pageNum);
				forward.setRedirect(true);
			}
		}
		
		return forward;
	}

}

---------------------------------------------BoardModifyFormAction.java---------------------------------------------

package action;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import svc.BoardDetailService;
import vo.ActionForward;
import vo.BoardDTO;

public class BoardModifyFormAction implements Action {

	@Override
	public ActionForward execute(HttpServletRequest request, HttpServletResponse response) throws Exception {
		System.out.println("BoardModifyFormAction");
		
		ActionForward forward = null;
		
		// request 객체를 통해 전달받은 파라미터(board_num) 가져오기
		int board_num = Integer.parseInt(request.getParameter("board_num"));
		
		// 글 수정에 필요한 데이터를 조회하기 위해
		// 이미 만들어진 BoardDetailService 클래스의 getArticle() 메서드를 호출하여
		// 게시물 상세 정보를 리턴받아 qna_board_modify.jsp 페이지로 포워딩
		// => 단, 조회수 증가 작업은 수행하지 않음
		BoardDetailService service = new BoardDetailService();
		BoardDTO article = service.getArticle(board_num);
		
		// request 객체에 BoardDTO 객체 저장
		request.setAttribute("article", article);
		
		// board/qna_board_modify.jsp 페이지로 포워딩 설정 => Dispatcher 방식
		forward = new ActionForward();
		forward.setPath("./board/qna_board_modify.jsp");
		forward.setRedirect(false);
		
		return forward;
	}

}


---------------------------------------------BoardModifyProAction.java---------------------------------------------

package action;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import svc.BoardModifyProService;
import vo.ActionForward;

public class BoardModifyProAction implements Action {

	@Override
	public ActionForward execute(HttpServletRequest request, HttpServletResponse response) throws Exception {
		System.out.println("BoardModifyProAction");
		
		ActionForward forward = null;
		
		int board_num = Integer.parseInt(request.getParameter("board_num"));
		String board_name = request.getParameter("board_name");
		String board_pass = request.getParameter("board_pass");
		String board_subject = request.getParameter("board_subject");
		String board_content = request.getParameter("board_content");
				
		// 게시물 수정 권한 판별을 위해 전달받은 파라미터 중 패스워드 비교
		// => BoardModifyProService 의 isArticleWriter() 메서드 호출
		BoardModifyProService service = new BoardModifyProService();
		boolean isArticleWriter = service.isArticleWriter(board_num, board_pass);
		
		// 수정 가능 여부 판별
		
		// 수정 가능할 경우 BoardModifyProService - modifyArticle() 메서드 호출하여 수정 요청
		// => 파라미터 : 글번호, BoardDTO 객체(article)
		// => 리턴타입 : boolean(isUpdataSuccess)
		
		// BoardModifyProService 에서 BoardDAO - updateArticle() 메서드 호출하여 수정
		// => 파라미터 : 글번호, BoardDTO 객체(article)
		// => 리턴타입 : int(updataCount)
		
		// 수정 완료 결과 판별 후 실패 시 자바스크립트로 "수정 실패!" 출력 후 이전페이지,
		// 성공 시 BoardDetail.bo 로 포워딩 요청
		
		return forward;
	}

}
```

- svc 폴더의 BoardDeleteProService.java 파일 수정
- svc 폴더에 BoardModifyProService.java 파일 생성

```java

---------------------------------------------BoardDeleteProService.java---------------------------------------------

package svc;

import static db.jdbcUtil.*;

import java.sql.Connection;

import dao.BoardDAO;

public class BoardDeleteProService {
	
	// 게시물 삭제를 위한 패스워드 판별 작업을 요청하는 isArticleWriter() 메서드 정의
	public boolean isArticleWriter(int board_num, String board_pass) {
		
		boolean isArticleWriter = false;
		
		Connection con = getConnection();
		
		BoardDAO boardDAO = BoardDAO.getInstance();
		
		boardDAO.setConnection(con);
		// BoardDeleteProService 에서 BoardDAO 의 isArticleWriter() 메서드를 호출하여 패스워드 판별
				// => 파라미터 : 글번호(board_num), 패스워드(board_pass)
				// 	  리턴타입 : boolean(isArticleWriter)
		isArticleWriter = boardDAO.isArticleWriter(board_num, board_pass);
		
		close(con);
		
		return isArticleWriter;
	}
	
	// 글 삭제 작업 요청을 수행하는 removeArticle() 메서드 정의
	public boolean removeArticle(int board_num) {
		boolean isDeleteSuccess = false;
		
		Connection con = getConnection();
		
		BoardDAO boardDAO = BoardDAO.getInstance();
		
		boardDAO.setConnection(con);
		// BoardDAO 의 deleteArticle() 메서드 호출하여 글 삭제 작업 수행
		// => 파라미터 : 글번호(board_num)	리턴타입 : int(deleteCount)
		int deleteCount = boardDAO.deleteArticle(board_num);
		
		// deleteCount 가 0보다 크면 commit, 아니면 rollback 작업 수행
		if(deleteCount > 0) {
			commit(con);
			// isDeleteSuccess 를 true 로 변경
			isDeleteSuccess = true;
		} else {
			rollback(con);
		}
		
		close(con);
		
		return isDeleteSuccess;
	}
}

---------------------------------------------BoardModifyProService.java---------------------------------------------

package svc;

import static db.jdbcUtil.close;
import static db.jdbcUtil.getConnection;

import java.sql.Connection;

import dao.BoardDAO;

public class BoardModifyProService {
		// 게시물 수정을 위한 패스워드 판별 작업을 요청하는 isArticleWriter() 메서드 정의
	public boolean isArticleWriter(int board_num, String board_pass) {
		
		boolean isArticleWriter = false;
		
		Connection con = getConnection();
		
		BoardDAO boardDAO = BoardDAO.getInstance();
		
		boardDAO.setConnection(con);
		
		// BoardDeleteProService 에서 BoardDAO 의 isArticleWriter() 메서드를 호출하여 패스워드 판별
				// => 파라미터 : 글번호(board_num), 패스워드(board_pass)
				// 	  리턴타입 : boolean(isArticleWriter)
		isArticleWriter = boardDAO.isArticleWriter(board_num, board_pass);
		
		close(con);
		
		return isArticleWriter;
	}
}
```

- dao 폴더의 BoardDAO.java 파일 수정

```java
package dao;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.util.ArrayList;


import vo.BoardDTO;

import static db.jdbcUtil.*;

// 실제 비즈니스 로직(DB 작업)을 처리할 BoardDAO 클래스 정의
// => 인스턴스를 여러개 생성할 필요가 없으므로 싱글톤 디자인 패턴을 통해 
//    하나의 인스턴스를 생성하여 모두가 공유하도록 할 수 있음
public class BoardDAO {
	// --------------- 싱글톤 디자인 패턴을 활용한 BoardDAO 인스턴스 생성 작업 --------------
	// 1. 외부에서 인스턴스 생성이 불가능하도록 생성자 정의 시 private 접근제한자 적용
	// 2. 자신의 클래스 내부에서 직접 인스턴스 생성하여 변수에 저장
	// => 외부에서 변수에 접근이 불가능하도록 private 접근제한자 적용
	// => 클래스 로딩 시점에 인스턴스가 생성되도록 static 멤버변수로 선언
	// 3. 생성된 인스턴스를 외부로 리턴하기 위한 Getter 메서드 정의
	// => 외부에서 인스턴스 생성없이도 호출 가능하도록 static 메서드로 정의
	// => 이 때, 2번에서 선언된 변수도 static 변수로 선언되어야 함
	// (static 메서드 내에서 인스턴스 멤버에 접근 불가능하며, static 멤버만 접근 가능하므로)
	private static BoardDAO instance = new BoardDAO();

	private BoardDAO() {
	}

	public static BoardDAO getInstance() {
		return instance;
	};

	// ---------------------------------------------------------------------------------------
	// 외부(Service 클래스)로부터 Connection 객체를 전달받아 관리하기 위해
	// Connection 타입 멤버 변수와 Setter 메서드 정의
	Connection con;

	public void setConnection(Connection con) {
		this.con = con;
	}

	// ---------------------------------------------------------------------------------------
	// 글쓰기 작업 수행 insertArticle() 메서드 정의
	// => 파라미터 : BoardDTO 객체(article) 리턴타입 : int(insertCount)
	public int insertArticle(BoardDTO article) {
		System.out.println("BoardDAO - insertArticle()");

		// INSERT 작업 결과를 리턴받아 저장할 변수 선언
		int insertCount = 0;

		PreparedStatement pstmt = null;
		ResultSet rs = null;

		int num = 1; // 새 글 번호를 저장할 변수

		try {
			// 현재 새 글의 번호로 사용될 번호를 조회
			// => 기존 글번호(board_num) 중에서 가장 큰 값 조회한 후 + 1 값을 새 글 번호로 설정
			String sql = "SELECT MAX(board_num) FROM board";
			pstmt = con.prepareStatement(sql);
			rs = pstmt.executeQuery();

			if (rs.next()) {
				// 조회된 레코드 중 Auto_increment 컬럼 값을 num 에 저장
				num = rs.getInt(1) + 1;
			}

			close(pstmt);

			// 전달받은 데이터(BoardDTO 객체)를 사용하여 board 테이블 INSERT 작업 수행
			sql = "INSERT INTO board VALUES (?,?,?,?,?,?,?,?,?,?,?,now())";
			pstmt = con.prepareStatement(sql);
			pstmt.setInt(1, num); // 새 글 번호
			pstmt.setString(2, article.getBoard_name());
			pstmt.setString(3, article.getBoard_pass());
			pstmt.setString(4, article.getBoard_subject());
			pstmt.setString(5, article.getBoard_content());
			pstmt.setString(6, article.getBoard_file());
			pstmt.setString(7, article.getBoard_real_file());
			// 답글에 사용될 참조글 번호(board_re_ref)는 새 글이므로 새 글 번호와 동일하게 지정
			pstmt.setInt(8, num); // board_re_ref
			pstmt.setInt(9, 0); // board_re_lev
			pstmt.setInt(10, 0); // board_re_seq
			pstmt.setInt(11, 0); // board_readcount

			insertCount = pstmt.executeUpdate();

		} catch (SQLException e) {
			System.out.println("SQL 구문 오류 발생! - insertArticle()");
			e.printStackTrace();
		} finally {
			// DB 자원 반환(주의! Connection 객체 반환 금지!)
			close(pstmt);
			close(rs);
		}

		// INSERT 작업 결과 리턴
		return insertCount;
	}

	// 총 게시물 수를 조회하는 selectListCount() 메서드 정의
	// 파라미터 : 없음 리턴타입 : int(listCount)
	public int selectListCount() {
		System.out.println("BoardDAO - selectListCount()");

		int listCount = 0;

		PreparedStatement pstmt = null;
		ResultSet rs = null;

		try {
			String sql = "SELECT COUNT(*) FROM board";
			pstmt = con.prepareStatement(sql);
			rs = pstmt.executeQuery();

			if (rs.next()) {
				listCount = rs.getInt(1);
			}
		} catch (SQLException e) {
			System.out.println("SQL 구문 오류 발생! - selectListCount()");
			e.printStackTrace();
		} finally {
			// DB 자원 반환(주의! Connection 객체 반환 금지!)
			close(pstmt);
			close(rs);
		}

		return listCount;
	}

	// 게시물 목록을 조회하는 selectArticleList() 메서드 정의
	public ArrayList<BoardDTO> selectArticleList(int pageNum, int listLimit) {
		ArrayList<BoardDTO> articleList = null;

		PreparedStatement pstmt = null;
		ResultSet rs = null;

		// 조회 시작 게시물 번호(행 번호) 계산
		int startRow = (pageNum - 1) * listLimit;

		try {
			// 게시물 목록 조회
			// => 참조글번호(board_re_ref) 기준 내림차순, 순서번호(board_re_seq) 기준 오름차순 정렬
			// => 조회 시작 게시물 번호(startRow) 부터 목록의 게시물 수(listLimit) 만큼 조회
			String sql = "SELECT * FROM board " + "ORDER BY board_re_ref DESC, board_re_seq ASC " + "LIMIT ?,?";
			pstmt = con.prepareStatement(sql);
			pstmt.setInt(1, startRow);
			pstmt.setInt(2, listLimit);

			rs = pstmt.executeQuery();

			// 전체 게시물을 저장할 ArrayList<BoardDTO> 타입 객체 생성
			articleList = new ArrayList<BoardDTO>();

			// while 문을 사용하여 조회 결과에 대한 반복 작업 수행
			while (rs.next()) {
				// 1개 게시물 정보를 저장할 BoardDTO 객체 생성 후 조회 결과 저장
				BoardDTO article = new BoardDTO();
				article.setBoard_num(rs.getInt("board_num"));
				article.setBoard_name(rs.getString("board_name"));
				article.setBoard_pass(rs.getString("board_pass"));
				article.setBoard_subject(rs.getString("board_subject"));
				article.setBoard_content(rs.getString("board_content"));
				article.setBoard_file(rs.getString("board_file"));
				article.setBoard_real_file(rs.getString("board_real_file"));
				article.setBoard_re_ref(rs.getInt("board_re_ref"));
				article.setBoard_re_lev(rs.getInt("board_re_lev"));
				article.setBoard_re_seq(rs.getInt("board_re_seq"));
				article.setBoard_readcount(rs.getInt("board_readcount"));
				article.setBoard_date(rs.getDate("board_date"));

				// 1개 게시물 정보를 다시 전체 게시물 정보 저장 객체(articleList)에 추가
				articleList.add(article);
			}

		} catch (SQLException e) {
			System.out.println("SQL 구문 오류 발생! - selectArticleList()");
			e.printStackTrace();
		} finally {
			// DB 자원 반환(주의! Connection 객체 반환 금지!)
			close(pstmt);
			close(rs);
		}

		return articleList;
	}

	// 게시물 상세 정보를 조회하는 selectArticle() 메서드 정의
	public BoardDTO selectArticle(int board_num) {
		// 글번호(board_num)에 해당하는 게시물 조회하여 BoardDTO 객체(article)에 저장 후 리턴
		BoardDTO article = null;

		PreparedStatement pstmt = null;
		ResultSet rs = null;

		try {
			String sql = "SELECT * FROM board WHERE board_num=?";
			pstmt = con.prepareStatement(sql);
			pstmt.setInt(1, board_num);
			rs = pstmt.executeQuery();

			if (rs.next()) {
				article = new BoardDTO();
				article.setBoard_num(rs.getInt("board_num"));
				article.setBoard_name(rs.getString("board_name"));
				article.setBoard_pass(rs.getString("board_pass"));
				article.setBoard_subject(rs.getString("board_subject"));
				article.setBoard_content(rs.getString("board_content"));
				article.setBoard_file(rs.getString("board_file"));
				article.setBoard_real_file(rs.getString("board_real_file"));
				article.setBoard_re_ref(rs.getInt("board_re_ref"));
				article.setBoard_re_lev(rs.getInt("board_re_lev"));
				article.setBoard_re_seq(rs.getInt("board_re_seq"));
				article.setBoard_readcount(rs.getInt("board_readcount"));
				article.setBoard_date(rs.getDate("board_date"));

//				System.out.println(article);
			}
		} catch (SQLException e) {
			System.out.println("SQL 구문 오류 발생! - selectArticle()");
			e.printStackTrace();
		} finally {
			close(pstmt);
			close(rs);
		}

		return article;
	}

	// 조회수 증가 작업을 수행하는 updateReadcount() 메서드 정의
	public void updateReadcount(int board_num) {
		PreparedStatement pstmt = null;

		try {
			String sql = "UPDATE board SET board_readcount=board_readcount+1 WHERE board_num=?";
			pstmt = con.prepareStatement(sql);
			pstmt.setInt(1, board_num);
			pstmt.executeUpdate();
		} catch (SQLException e) {
			System.out.println("SQL 구문 오류 발생! - updateReadcount()");
			e.printStackTrace();
		} finally {
			close(pstmt);
		}
	}

	// 패스워드가 일치하는 게시물 조회를 통해 본인 여부 판별하는 isArticleWriter() 메서드 정의
	public boolean isArticleWriter(int board_num, String board_pass) {
		boolean isArticleWrtier = false;

		PreparedStatement pstmt = null;
		ResultSet rs = null;

		try {
			String sql = "SELECT * FROM board WHERE board_num=? AND board_pass=?";
			pstmt = con.prepareStatement(sql);
			pstmt.setInt(1, board_num);
			pstmt.setString(2, board_pass);

			rs = pstmt.executeQuery();

			// 조회결과(rs.next()) 가 있을 경우 isArticleWriter 를 true 로 변경
			if (rs.next()) {
				isArticleWrtier = true;
			}

		} catch (SQLException e) {
			System.out.println("SQL 구문 오류 발생! - isArticleWriter()");
			e.printStackTrace();
		} finally {
			close(pstmt);
			close(rs);
		}

		return isArticleWrtier;
	}

	// 글 삭제 작업을 수행하는 deleteArticle() 메서드 정의
	public int deleteArticle(int board_num) {
		int deleteCount = 0;
		
		PreparedStatement pstmt = null;
		
		try {
			String sql = "DELETE FROM board WHERE board_num=?";
			pstmt = con.prepareStatement(sql);
			pstmt.setInt(1, board_num);
			deleteCount = pstmt.executeUpdate();
		} catch (SQLException e) {
			System.out.println("SQL 구문 오류 발생! - deleteArticle()");
			e.printStackTrace();
		} finally {
			close(pstmt);
		}
		
		return deleteCount;
	}
}
```

- controller 폴더의 BoardFrontController.java 파일 수정

```java
package controller;

import java.io.IOException;

import javax.servlet.RequestDispatcher;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import action.Action;
import action.BoardDeleteProAction;
import action.BoardDetailAction;
import action.BoardListAction;
import action.BoardModifyFormAction;
import action.BoardModifyProAction;
import action.BoardWriteProAction;
import vo.ActionForward;

@WebServlet("*.bo")
public class BoardFrontController extends HttpServlet {

	protected void doProcess(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {
		System.out.println("BoardFrontController");

		// POST 방식에 대한 한글 처리
		request.setCharacterEncoding("UTF-8");

		// 서블릿 주소 추출
		String command = request.getServletPath();
		System.out.println("command : " + command);

		// Action 클래스의 공통 타입(슈퍼클래스)인 Action 인터페이스 타입 변수 선언
		Action action = null;
		// 포워딩 정보를 관리하는 ActionForward 타입 변수 선언
		ActionForward forward = null;

		// 서블릿 주소 판별
		// 1. 글쓰기 폼에 대한 요청 판별("/BoardWriteForm.bo")
		if (command.equals("/BoardWriteForm.bo")) {
			// board 디렉토리의 qna_board_write.jsp 페이지로 포워딩
			// => 포워딩 대상이 뷰페이지(*.jsp)일 경우 Dispatcher 방식 포워딩
			// (ActionForward 객체 생성 및 URL과 포워딩 방식(Dispatcher)을 저장
			forward = new ActionForward();
			forward.setPath("./board/qna_board_write.jsp");
			forward.setRedirect(false); // Dispatcher 방식(생략 가능)
			// => Dispatcher 방식이므로 jsp 페이지 주소가 노출되지 않고
			// 이전에 요청된 서블릿 주소(BoardWriteForm.bo)가 그대로 유지됨
		} else if (command.equals("/BoardWritePro.bo")) {
			// 비즈니스 로직 처리를 위한 Action 클래스에 접근
			// => 글쓰기 작업 요청을 위해 BoardWriteProAction 인스턴스 생성 후 execute() 메서드 호출
			// => 생성된 인스턴스를 부모 타입인 Action 타입으로 업캐스팅하여 다루기
			action = new BoardWriteProAction();

			try {
				forward = action.execute(request, response);
			} catch (Exception e) {
				e.printStackTrace();
			}

		} else if (command.equals("/BoardList.bo")) {
			// 비즈니스 로직 처리를 위해 BoardListAction 클래스의 execute() 메서드 호출
			action = new BoardListAction();

			try {
				forward = action.execute(request, response);
			} catch (Exception e) {
				e.printStackTrace();
			}

		} else if (command.equals("/BoardDetail.bo")) {
			// 비즈니스 로직 처리를 위해 BoardDetailAction 클래스의 execute() 메서드 호출
			action = new BoardDetailAction();
			try {
				forward = action.execute(request, response);
			} catch (Exception e) {
				e.printStackTrace();
			}

		} else if (command.equals("/BoardDeleteForm.bo")) {
			forward = new ActionForward();
			forward.setPath("./board/qna_board_delete.jsp");
			forward.setRedirect(false); // Dispatcher 방식(생략 가능)

		} else if (command.equals("/BoardDeletePro.bo")) {
			// 비즈니스 로직 처리를 위해 BoardDetailAction 클래스의 execute() 메서드 호출
			action = new BoardDeleteProAction();
			try {
				forward = action.execute(request, response);
			} catch (Exception e) {
				e.printStackTrace();
			}

		} else if (command.equals("/BoardModifyForm.bo")) {
			// 비즈니스 로직 처리를 위해 BoardDetailAction 클래스의 execute() 메서드 호출
			action = new BoardModifyFormAction();
			try {
				forward = action.execute(request, response);
			} catch (Exception e) {
				e.printStackTrace();
			}

		} else if (command.equals("/BoardModifyPro.bo")) {
			// 비즈니스 로직 처리를 위해 BoardDetailAction 클래스의 execute() 메서드 호출
			action = new BoardModifyProAction();
			try {
				forward = action.execute(request, response);
			} catch (Exception e) {
				e.printStackTrace();
			}
		}
			// ----------------------------------------------------
			// ActionForward 객체에 저장된 포워딩 정보에 따른 포워딩 작업 수행
			if (forward != null) { // ActionForward 객체가 비어있지 않을 경우
				// Redirect 방식 vs Dispatcher 방식 판별하여 각 방식으로 포워딩
				if (forward.isRedirect()) { // Redirect 방식
					response.sendRedirect(forward.getPath());
				} else { // Dispatcher 방식
					RequestDispatcher dispatcher = request.getRequestDispatcher(forward.getPath());
					dispatcher.forward(request, response);
				}
			} else {
				// ActionForward 객체가 비어있을 경우 메세지 출력(임시)
				System.out.println("ActionForward 객체가 null 입니다!");
			}
		}

	protected void doGet(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {
		doProcess(request, response);
	}

	protected void doPost(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {
		doProcess(request, response);
	}

}

```
