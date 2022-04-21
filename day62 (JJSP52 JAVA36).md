# [오전수업] JSP 52차

- index.jsp 파일 수정

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<h1>MVC_Board 메인페이지</h1>
	<h3><a href="./BoardWriteForm.bo">글쓰기</a></h3>
	<h3><a href="./BoardList.bo">글목록</a></h3>
</body>
</html>
```

- qna_board_list.jsp 파일 수정 (JSTL 사용) & qna_board_view 파일 생성

```jsp
--------------------------------------------qna_board_list.jsp--------------------------------------------
<%@page import="vo.BoardDTO"%>
<%@page import="vo.PageInfo"%>
<%@page import="java.util.ArrayList"%>
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>MVC 게시판</title>
<style type="text/css">
	#listForm {
		width: 1024px;
		max-height: 610px;
		margin: auto;
	}
	
	h2 {
		text-align: center;
	}
	
	table {
		margin: auto;
		width: 1024px;
	}
	
	#tr_top {
		background: orange;
		text-align: center;
	}
	
	table td {
		text-align: center;
	}
	
	#subject {
		text-align: left;
		padding-left: 20px;
	}
	
	#pageList {
		margin: auto;
		width: 1024px;
		text-align: center;
	}
	
	#emptyArea {
		margin: auto;
		width: 1024px;
		text-align: center;
	}
	
	#buttonArea {
		margin: auto;
		width: 1024px;
		text-align: right;
	}
</style>
</head>
<body>
	<!-- JSTL 의 c:set 태그를 사용하여 PageInfo 객체의 값들을 변수에 저장 -->
	<c:set var="pageNum" value="${pageInfo.getPageNum() }" />
	<c:set var="maxPage" value="${pageInfo.getMaxPage() }" />
	<c:set var="startPage" value="${pageInfo.getStartPage() }" />
	<c:set var="endPage" value="${pageInfo.getEndPage() }" />
	<c:set var="listCount" value="${pageInfo.getListCount() }" />
	

	<!-- 게시판 리스트 -->
	<section id="listForm">
	<h2>게시판 글 목록</h2>
	<table>
		<tr id="tr_top">
			<td width="100px">번호</td>
			<td>제목</td>
			<td width="150px">작성자</td>
			<td width="150px">날짜</td>
			<td width="100px">조회수</td>
		</tr>
		<!-- JSTL의 forEach 태그를 사용하여 articleList 에서 BoardDTO 객체를 꺼내서 내용 출력 -->
		<!-- 단, 게시물 목록이 하나라도 존재할 경우에만 출력 c:if 태그 사용 -->
		<c:if test="${not empty articleList && pageInfo.getListCount() > 0}">
			<c:forEach var='board' items="${articleList}">
				<tr>
					<td>${board.getBoard_num() }</td>
					<td id="subject">
						<a href="BoardDetail.bo?board_num=${board.getBoard_num() }&page=${pageNum }">
							${board.getBoard_subject() }
						</a>
					</td>
					<td>${board.getBoard_name() }</td>
					<td>${board.getBoard_date() }</td>
					<td>${board.getBoard_readcount() }</td>
				</tr>
			</c:forEach>
		</c:if>
	</table>
	</section>
	<section id="buttonArea">
		<input type="button" value="글쓰기" onclick="location.href='BoardWriteForm.bo'" />
	</section>
	<section id="pageList">
		<!-- 
		현재 페이지 번호(pageNum)가 1보다 클 경우에만 Prev 링크 동작
		=> 클릭 시 notice.jsp 로 이동하면서 
		   현재 페이지 번호(pageNum) - 1 값을 page 파라미터로 전달
		-->
		<c:choose>
			<c:when test="${pageNum > 1}">
				<input type="button" value="이전" onclick="location.href='BoardList.bo?page=${pageNum - 1}'">
			</c:when>
			<c:otherwise>
				<input type="button" value="이전">
			</c:otherwise>
		</c:choose>

		<!-- 페이지 번호 목록은 시작 페이지(startPage)부터 끝 페이지(endPage) 까지 표시 -->
		<c:forEach var="i" begin="${startPage }" end="${endPage }">
			<!-- 단, 현재 페이지 번호는 링크 없이 표시 -->
			<c:choose>
				<c:when test="${pageNum eq i}">
					${i }
				</c:when>
				<c:otherwise>
					<a href="BoardList.bo?page=${i }">${i }</a>
				</c:otherwise>
			</c:choose>
		</c:forEach>

		<!-- 현재 페이지 번호(pageNum)가 총 페이지 수보다 작을 때만 [다음] 링크 동작 -->
		<c:choose>
			<c:when test="${pageNum < maxPage}">
				<input type="button" value="다음" onclick="location.href='BoardList.bo?page=${pageNum + 1}'">
			</c:when>
			<c:otherwise>
				<input type="button" value="다음">
			</c:otherwise>
		</c:choose>

	</section>
</body>
</html>


--------------------------------------------qna_board_view.jsp--------------------------------------------

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
					<a href="./upload/${article.getBoard_file() }" download>${article.getBoard_file() }</a>
				</td>
				<th  width="70">조회수</th>
				<td>${article.getBoard_readcount()}</td>
			</tr>
			</table>
		</section>
		<section id="articleContentArea">
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
import action.BoardDetailAction;
import action.BoardListAction;
import action.BoardWriteProAction;
import vo.ActionForward;

@WebServlet("*.bo")
public class BoardFrontController extends HttpServlet {
	
	protected void doProcess(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
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
		if(command.equals("/BoardWriteForm.bo")) {
			// board 디렉토리의 qna_board_write.jsp 페이지로 포워딩
			// => 포워딩 대상이 뷰페이지(*.jsp)일 경우 Dispatcher 방식 포워딩
			//	  (ActionForward 객체 생성 및 URL과 포워딩 방식(Dispatcher)을 저장
			forward = new ActionForward();
			forward.setPath("./board/qna_board_write.jsp");
			forward.setRedirect(false); // Dispatcher 방식(생략 가능)
			// => Dispatcher 방식이므로 jsp 페이지 주소가 노출되지 않고
			//	  이전에 요청된 서블릿 주소(BoardWriteForm.bo)가 그대로 유지됨
		} else if(command.equals("/BoardWritePro.bo")) {
			// 비즈니스 로직 처리를 위한 Action 클래스에 접근
			// => 글쓰기 작업 요청을 위해 BoardWriteProAction 인스턴스 생성 후 execute() 메서드 호출
			// => 생성된 인스턴스를 부모 타입인 Action 타입으로 업캐스팅하여 다루기
			action = new BoardWriteProAction();
			
			try {
				forward = action.execute(request, response);
			} catch (Exception e) {
				e.printStackTrace();
			}
			
		} else if(command.equals("/BoardList.bo")) {
			// 비즈니스 로직 처리를 위해 BoardListAction 클래스의 execute() 메서드 호출
			action = new BoardListAction();
			
			try {
				forward = action.execute(request, response);
			} catch (Exception e) {
				e.printStackTrace();
			}
			
		} else if(command.equals("/BoardDetail.bo")) {
			// 비즈니스 로직 처리를 위해 BoardDetailAction 클래스의 execute() 메서드 호출
			action = new BoardDetailAction();
			try {
				forward = action.execute(request, response);
			} catch (Exception e) {
				e.printStackTrace();
			}
			
		} else if(command.equals("/BoardDeleteForm.bo")) {
			forward = new ActionForward();
			forward.setPath("./board/qna_board_delete.jsp");
			forward.setRedirect(false); // Dispatcher 방식(생략 가능)
		}
		
		// ----------------------------------------------------
		// ActionForward 객체에 저장된 포워딩 정보에 따른 포워딩 작업 수행
		if(forward != null) { // ActionForward 객체가 비어있지 않을 경우
			// Redirect 방식 vs Dispatcher 방식 판별하여 각 방식으로 포워딩
			if(forward.isRedirect()) { // Redirect 방식
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
	
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		doProcess(request, response);
	}

	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		doProcess(request, response);
	}

}

```

- action 폴더에 BoardDetailAction.java 파일 생성

```java
package action;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import svc.BoardDetailService;
import vo.ActionForward;
import vo.BoardDTO;

public class BoardDetailAction implements Action {

	@Override
	public ActionForward execute(HttpServletRequest request, HttpServletResponse response) throws Exception {
		System.out.println("BoardDetailAction");
		
		ActionForward forward = null;
		
		// request 객체를 통해 전달받은 파라미터(board_num) 가져오기
		int board_num = Integer.parseInt(request.getParameter("board_num"));
		
		// BoardDetailService 인스턴스 생성 및 getArticle() 메서드 호출
		// => 파라미터 : 글번호(board_num)    리턴타입 : BoardDTO(article)
		BoardDetailService service = new BoardDetailService();
		BoardDTO article = service.getArticle(board_num);
		
		// 조회수 증가 작업 요청(단, 게시물 조회 성공 시에만 수행)
		if(article != null) {
			// BoardDetailService 객체의 increaseReadcount() 메서드 호출
			// => 파라미터 : 글번호(board_num)
			service.increaseReadcount(board_num);
		}
		
		// request 객체의 setAttribute() 메서드 호출하여 
		// BoardDTO("article") 객체 저장
		request.setAttribute("article", article);
		
		// ActionForward 객체 생성 및 포워딩 정보 설정(Dispatcher 방식)
		// => board 폴더의 qna_board_view.jsp
		forward = new ActionForward();
		forward.setPath("board/qna_board_view.jsp");
		forward.setRedirect(false);
		
		return forward;
	}

}

```

- svc 폴더에 BoardDetailService.java 파일 생성

```java
package svc;

import static db.jdbcUtil.*;

import java.sql.Connection;
import java.util.ArrayList;

import dao.BoardDAO;
import vo.BoardDTO;

public class BoardDetailService {
	
	// 게시물 1개 정보를 조회 요청하는 getArticle() 메서드 정의
	public BoardDTO getArticle(int board_num) {
		System.out.println("BoardDetailService - getArticle()");
		
		// 1. 리턴할 데이터를 저장할 변수 선언
		BoardDTO article = null;
		
		// 2. JdbcUtil 클래스로부터 Connection Pool 에 저장된 Connection 객체 가져오기 - 공통
		Connection con = getConnection(); // static import 적용되어 있을 경우
		
		// 3. BoardDAO 클래스로부터 BoardDAO 인스턴스 가져오기 - 공통
		BoardDAO boardDAO = BoardDAO.getInstance();
		
		// 4. BoardDAO 객체에 Connection 객체 전달하기 - 공통
		boardDAO.setConnection(con);
		
		// 5. BoardDAO 객체의 selectArticle() 메서드를 호출하여 게시물 상세 정보 조회
		// => 파라미터 : board_num    리턴타입 : BoardDTO(article)
		article = boardDAO.selectArticle(board_num);
		
		// 6. JdbcUtil 클래스로부터 가져온 Connection 객체를 반환 - 공통
		close(con);
		
		// 7. 조회 결과 리턴
		return article;
	}

	public void increaseReadcount(int board_num) {
		// 2. JdbcUtil 클래스로부터 Connection Pool 에 저장된 Connection 객체 가져오기 - 공통
		Connection con = getConnection(); // static import 적용되어 있을 경우
		
		// 3. BoardDAO 클래스로부터 BoardDAO 인스턴스 가져오기 - 공통
		BoardDAO boardDAO = BoardDAO.getInstance();
		
		// 4. BoardDAO 객체에 Connection 객체 전달하기 - 공통
		boardDAO.setConnection(con);
		
		// 5. BoardDAO 객체의 updateReadcount() 메서드를 호출하여 게시물 조회수 증가
		// => 파라미터 : board_num
		boardDAO.updateReadcount(board_num);
		
		// 6. commit 작업 수행
		commit(con);
		
		// 7. JdbcUtil 클래스로부터 가져온 Connection 객체를 반환 - 공통
		close(con);
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
	//    => 외부에서 변수에 접근이 불가능하도록 private 접근제한자 적용
	//    => 클래스 로딩 시점에 인스턴스가 생성되도록 static 멤버변수로 선언
	// 3. 생성된 인스턴스를 외부로 리턴하기 위한 Getter 메서드 정의
	//    => 외부에서 인스턴스 생성없이도 호출 가능하도록 static 메서드로 정의
	//    => 이 때, 2번에서 선언된 변수도 static 변수로 선언되어야 함
	//       (static 메서드 내에서 인스턴스 멤버에 접근 불가능하며, static 멤버만 접근 가능하므로)
	private static BoardDAO instance = new BoardDAO();
	
	private BoardDAO() {}

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
	// => 파라미터 : BoardDTO 객체(article)   리턴타입 : int(insertCount)
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
			
			if(rs.next()) {
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
	// 파라미터 : 없음   리턴타입 : int(listCount)
	public int selectListCount() {
		System.out.println("BoardDAO - selectListCount()");
		
		int listCount = 0;
		
		PreparedStatement pstmt = null;
		ResultSet rs = null;
		
		try {
			String sql = "SELECT COUNT(*) FROM board";
			pstmt = con.prepareStatement(sql);
			rs = pstmt.executeQuery();
			
			if(rs.next()) {
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
			String sql = "SELECT * FROM board "
						+ "ORDER BY board_re_ref DESC, board_re_seq ASC "
						+ "LIMIT ?,?";
			pstmt = con.prepareStatement(sql);
			pstmt.setInt(1, startRow);
			pstmt.setInt(2, listLimit);
			
			rs = pstmt.executeQuery();
			
			// 전체 게시물을 저장할 ArrayList<BoardDTO> 타입 객체 생성
			articleList = new ArrayList<BoardDTO>();
			
			// while 문을 사용하여 조회 결과에 대한 반복 작업 수행
			while(rs.next()) {
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
			
			if(rs.next()) {
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

			
		
		
	}

```
---

# [오후수업] JAVA 36차

## 자바 I/O (Input / Output)
- java.io 패키지에 있는 클래스들의 모음
- 자바에서 각종 입출력을 담당
- Node(노드) : 자바에서 입출력을 수행하는 대상
	- (입력 노드 : 키보드, 마우스, 파일, 네트워크, 데이터베이스 등)
	- (출력 노드 : 모니터, 스피커, 파일, 네트워크, 데이터베이스 등)

- Stream(스트림) : 입력 또는 출력 데이터가 한 방향으로 끊임없이 전송되는 것
	- 출발지 노드 -> 도착지 노드
- 입력스트림 : 자바에서 데이터가 입력될 때 처리하는 스트림
- 출력스트림 : 자바에서 데이터가 출력될 때 처리하는 스트림

** 스트림 종류
1. byte 기반(단위) 스트림
- 그림, 사진, 영상 등 바이너리(Binary) 데이터를 입출력
- InputStream, OutputStream을 최상위 클래스로 두고 XXXInputStream, XXXOutputStream 클래스가 하위클래스로 존재함
2. char 기반(단위) 스트림
- 문자 데이터(텍스트)를 입출력
- Reader, Writer 를 최상위 클래스로 두고 XXXReader, XXXWriter 클래스가 하위클래스로 존재함

** 표준 입출력 : 컴퓨터에서 기본적으로 사용하는 입출력
- 표준 입력 장치 : 키보드
- 표준 출력 장치 : 모니터
- 표준 입출력을 담당하는 클래스 : System
1) System.in : 표준 입력을 담당(키보드에서 입력받기 가능)
2) System.out : 표준 출력을 담당(모니터로 출력 가능)
3) System.err : 모니터에 에러 정보 출력(잘 사용하지 않음)

### 키보드로부터 데이터를 입력받아 처리하는 방법
```java
package io;

import java.io.IOException;
import java.io.InputStream;

public class Ex1 {

	public static void main(String[] args) {
		/*
		 * 키보드로부터 데이터를 입력받아 처리하는 방법
		 * 1. InputStream 객체를 사용하여 1Byte 단위로 입력데이터를 처리하는 방법
		 * 		- read() 메서드를 사용하여 1Byte 만큼의 데이터를 가져올 수 있음
		 * 		- 아무리 많은 데이터가 입력되어도 read() 메서드는 한 번에 1Byte만 처리되므로
		 * 		  더 많은 데이터나 더 큰 단위 처리가 불가능
		 * 		  => 영문 또는 숫자 등의 데이터 1글자만 처리 가능
		 * 		  => 한글이나 한자 등 2Byte(char 단위) 문자들은 처리 불가능
		 * 		  => 읽어온 데이터가 int형 이므로 문자로 변환 등의 후속 작업 필요
		 * 		- 가장 저수준의 입력 방법
		 * 
		 */
//		InputStream is = null;
//		
//	try {
//		System.out.println("데이터를 입력하세요!");
//		is = System.in;
//		// => System.in 코드에 의해 키보드로부터 데이터 입력이 가능하며
//		// 입력 스트림 객체를 InputStream 타입 변수에 저장(연결)
//		int n = is.read(); // 입력스트림 데이터 중 1Byte 만큼의 데이터 읽어서 변수에 저장
//		System.out.println("입력 데이터 : " + n);
//		System.out.println("입력 데이터를 문자로 변환 : " + (char)n);
//	} catch (IOException e) {
//		e.printStackTrace();
//	} finally {
//		if(is != null) try { is.close(); } catch (IOException e) {} 
//	}
		
	// ==================================================
	/*
	 * try ~ resource 구문을 사용하여 자원 반환(close())을 자동으로 수행
	 * - 기본적으로 자원을 사용하는 객체(Connection, InputStream 등)는
	 * 	 사용 후 close() 메서드 호출을 통해 사용중인 자원을 반환해아하며
	 * 	 자원이 반환되지 않으면 반복적인 자원 요청으로 인해 자원이 고갈되어
	 * 	 더 이상 다른 사용자의 작업 요청을 수행할 수 없게 된다!
	 * 	 => 예외 발생 여부와 관계업이 finally 블록 내에서 자원반환 코드 기술했음
	 * - try ~ resource 구문은 try문에서 반환할 자원을 갖는 객체를 생성하고
	 * 	 try ~ catch 블록 작업이 끝나면 자동으로 자원을 반환해 주도록 함
	 * 
	 * < 기본 문법 >
	 * try(자원을 반환할 객체 생성 및 변수에 저장) {
	 * 		// 작업 수행
	 * } catch(...) {
	 * 		// 예외 처리 작업 수행
	 * }
	 */
	
	// try문의 소괄호() 내부에 예외처리가 필요한 객체의 생성 코드를 작성
//	try(InputStream is = System.in) {
//		int n = is.read();
//		System.out.println("입력 데이터 : " + n);
//		System.out.println("입력 데이터(문자로 변환) : " + (char)n);
//	} catch(IOException e) {
//		e.printStackTrace();
//	}
		
	// => 별도의 close() 메서드를 호출하지 않아도 자동으로 자원이 반환됨
	
	// ====================================================
	// 반복문을 사용하여 1Byte씩 여러번 반복하여 입력 처리
	System.out.println("데이터를 입력하세요. (취소 시 Ctrl + Z)");
	try (InputStream is = System.in) {
		
		int n = is.read();
		while(n != -1) {
			System.out.println("입력데이터 : " + n + ", 문자로변환 : " + (char)n);
			n = is.read();
		}
		
	} catch(IOException e) {
		e.printStackTrace();
	}

	}
}

```

```java
package io;

import java.io.IOException;
import java.io.InputStream;

public class Ex2 {

	public static void main(String[] args) {
		/*
		 * 키보드로부터 데이터를 입력받아 처리하는 방법
		 * 2. InputStream 객체를 사용하여 1Byte 단위로 입력 데이터를 처리하지 않고
		 * 	  배열을 사용하여 한 번에 여러 Byte를 모아서 처리하는 방법
		 * 	  - read(byte[] b) 메서드를 호출하여 입력데이터를 배열크기만큼 읽어와서 저장
		 * 	  - 아무리 많은 데이터가 입력되어도 배열 크기만큼만 다룰 수 있기 때문에
		 * 	    더 많은 데이터나 더 큰 단위 처리가 불가능
		 * 	  - 가장 저수준의 입력 방법
		 */
//		System.out.println("데이터를 입력하세요.");
//		
//		try(InputStream is = System.in) {
//			// 1Byte씩 묶음으로 처리할 byte[] 배열을 생성
//			byte[] bArr = new byte[10]; // 10Byte 단위로 묶을 경우
//			
//			// read() 메서드 파라미터로 byte[] 배열을 전달할 경우
//			// 입력되는 스트림을 자동으로 배열 크기만큼 읽어서 배열에 저장
//			// => 배열에 저장된 데이터 크기(읽어들인 바이트 수)를 리턴
//			int n = is.read(bArr);
//			System.out.println("입력 데이터 크기 : " + n + "바이트");
//			
//			for(byte b : bArr) {
//				System.out.println("입력 데이터 : " + b + ", 문자로 변환 : " + (char)b);
//			}
//			
//			// String 클래스를 활용하면 byte	[] 배열 데이터를 문자열로 변환 가능
//			String str = new String(bArr);
//			System.out.println("입력데이터(문자열) : " + str);
//			
//			
//		} catch (Exception e) {
//			e.printStackTrace();
//		}

		// ==================================================
		System.out.println("데이터를 입력하세요. (Ctrl + Z)"); // -1 입력됨
		
		try(InputStream is = System.in) {
			byte[] bArr = new byte[10];
			
			int n = is.read(bArr);
			while(n > 0) {
				
				String str = new String(bArr);
				System.out.println("입력데이터(문자열) : " + str);
				
				n = is.read(bArr);
			}
			
		} catch(IOException e) {
			e.printStackTrace();
		}
		
		
	}

}
```
```java
package io;

import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;

public class Ex3 {

	public static void main(String[] args) {
		/*
		 * 키보드로부터 데이터를 입력받아 처리하는 방법
		 * 3. InputStreamReader 객체를 사용하여 char 단위로 입력데이터를 처리하는 방법
		 * 	- InputStream 객체를 파라미터로 갖는 InputStreamReader 객체 생성
		 * 	  => 보조스트림을 사용하는 스트림 체이닝(Stream Chaning) 방식 문법 구성
		 * 	- read() 메서드를 호출하여 입력데이터를 char단위(2Byte) 만큼 읽어와서 저장
		 * 	- 아무리 많은 데이터가 입력되어도 2Byte(char) 만큼만 다룰 수 있기 때문에
		 * 	  더 많은 데이터나 더 큰 단위 처리가 불가능
		 * 	  => 영문 또는 숫자 등의 데이터 1글자만 처리 가능
		 * 	  => 한글이나 한자 등 2Byte(char 단위) 문자도 처리 가능(1글자)
		 * 	  => 읽어온 데이터를 문자로 변환하는 후속작업 필요
		 * - InputStream 보다는 유용하지만, 여전히 낮은 수준의 입력 처리 방식
		 * 
		 * < 기본 문법 >
		 * InputStreamReader reader = new InputStreamReader(InputStream 객체);
		 * 
		 */
		// 1. System.in을 사용하여 입력 스트림 가져오기
//		InputStream is = System.in;
		
		// 2. InputStreamReader 객체 생성 => 파라미터로 InputStream 객체를 전달
//		InputStreamReader reader = new InputStreamReader(is);
		
		// ------------------------- 위의 문법을 한 문장으로 결합 --------------------------------
//		InputStreamReader reader = new InputStreamReader(System.in);

//		System.out.println("데이터를 입력하세요.");
//		try(InputStreamReader reader = new InputStreamReader(System.in)) {
//			// InputStreamReader 객체의 read() 메서드를 호출하여 char 단위 읽어오기
//			int n = reader.read();
//
//			System.out.println("입력된 데이터 : " + n + ", 문자로 변환 : " + (char)n);
//			
//		} catch (IOException e) {
//			e.printStackTrace();
//		}
		
		// =======================================================================
		System.out.println("데이터를 입력하세요. (Ctrl + Z)");
		
		try(InputStreamReader reader = new InputStreamReader(System.in)) {
			
			int n = reader.read();
			
			while(n != -1) {
				System.out.println("입력된데이터 : " + n + ", 문자로 변환 : " + (char)n);
				n = reader.read();
			}
		} catch (IOException e) {
			e.printStackTrace();
		}
		
		
	}

}
```

```java
package io;

import java.io.IOException;
import java.io.InputStreamReader;

public class Ex4 {

	public static void main(String[] args) {
		/*
		 * 키보드로부터 데이터를 입력받아 처리하는 방법
		 * 4. InputStreamReader 객체를 사용하여 char 단위로 읽어온 데이터를
		 * 	  배열을 사용하여 한 번에 여러 문자로 모아서 처리하는 방법
		 */
		System.out.println("데이터를 입력하세요.");
		
		try(InputStreamReader reader = new InputStreamReader(System.in)) {
			char[] chArr = new char[10];
			
			int n = reader.read(chArr);
			
			System.out.println("입력데이터(문자열) : " + new String(chArr));
			
		} catch (IOException e) {
			e.printStackTrace();
		}

	}

}
```

```java
package io;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;

public class Ex5 {

	public static void main(String[] args) {
		/*
		 * 키보드로부터 데이터를 입력받아 처리하는 방법
		 * 5. BufferReader 객체를 사용하여 String 단위로 입력 데이터를 처리하는 방법
		 * 		- inputStream 객체를 파라미터로 갖는 inputStreamReader 객체 생성 후
		 * 		  다시 InputStreamReader 객체를 파라미터로 갖는 BufferedReader 객체 생성
		 * 		  => 보조스트림을 사용하는 스트림 체이닝(Stream Chaning) 방식 문법 구성
		 * 		  => 이처럼 기본 스트림을 꾸며주는 역할을 하는 보조스트림을 적용하여
		 * 			 입출력을 처리하는 방식을 데코레이션 패턴(Decoration Pattern) 이라고 함
		 * 		- read() 메서드가 아닌 readLine() 메서드를 사용하여 String 단위로 처리
		 * 		  => 즉, 데이터를 한 문장(라인 = 엔터키 기준)단위로 읽어들여 처리
		 * 		- 키보드를 통해 입력되는 데이터를 처리하는 최종적인 방법(가장 효율적)
		 */

		// 스트림 체이닝을 통한 데코레이션 패턴 구현
		// 1. 기본 입력스트림 객체(InputStream) 생성 = byte 단위 처리
//		InputStream is = System.in;
		
		// 2. 입력스트림을 연결하는 보조스트림 InputStreamReader 객체 생성 = char 단위 처리
//		InputStreamReader reader = new InputStreamReader(is);
		
		// 3. 향상된 입력 보조스트림 BufferedReader 객체 생성 = String 단위 처리
//		BufferedReader buffer = new BufferedReader(reader);
		
		// ------------------- 위 세 문장을 하나의 문장으로 결합 --------------------
//		BufferedReader buffer = new BufferedReader(new InputStreamReader(System.in));
		
//		System.out.println("데이터를 입력하세요.");
//		
//		try(BufferedReader buffer = new BufferedReader(new InputStreamReader(System.in))) {
//			
//			String str = buffer.readLine();
//			System.out.println("입력데이터 : " + str);
//			
//		} catch (IOException e) {
//			e.printStackTrace();
//		}
//		
		// ======================================================
		// 반복문을 사용하여 Ctrl + Z 입력 시 까지의 모든 문자열을 출력
		// => 주의! Ctrl + 입력데이터가 정수일 때는 -1, 문자열일 때는 null 값 사용
		System.out.println("데이터를 입력하세요. (취소: Ctrl + Z)");
		
		try(BufferedReader buffer = new BufferedReader(new InputStreamReader(System.in))) {
			
			String str = buffer.readLine();
			
			while(str != null) {
				System.out.println("입력데이터 : " + str);
				str = buffer.readLine();
			}
		} catch (IOException e) {
			e.printStackTrace();
		}
		System.out.println("입력 종료!");
		
		
	}

}

```

### 모니터로 데이터를 출력하는 방법

```java
package io;

import java.io.IOException;
import java.io.OutputStream;

public class Ex6 {

	public static void main(String[] args) {
		/*
		 * 모니터로 데이터를 출력하는 방법
		 * 1. 기본 출력스트림인 OutputStream 사용(byte 단위로 처리)
		 * 		- write() 메서드를 호출하여 byte 단위 출력
		 * 		- byte 단위로 처리되므로 문자열 데이터 자체를 처리할 수 없음
		 * 
		 */

		// OutputStream 객체를 연결하기 위해서는 System.out 사용
//		OutputStream os = System.out;
		
//		char ch = 'A';
//		
//		try(OutputStream os = System.out) {
//			os.write(ch);	// 1byte 단위로 출력하므로 한글, 한자 등 출력 불가능
//		} catch (IOException e) {
//			e.printStackTrace();
//		}
		
		// ==============================================================
		// String 타입 데이터(문자열)를 OutputStream 으로 출력
		String str = "Hello, 자바";
		
		try(OutputStream os = System.out) {
			// write(byte[] b) 메서드를 호출하여 출력할 데이터를 배열로 전달
		} catch (IOException e) {
			e.printStackTrace();
		}
	}

}
```

연습문제
```java
package io;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.OutputStream;

public class Test6 {

	public static void main(String[] args) {
		// BufferedReader 를 사용하여 입력받은 문자열을
		// OutputStream 을 사용하여 출력
		
		// try ~ resource 구문 작성
		// try() 문장 소괄호 내에 복수개의 객체를 세미콜론(;) 으로 구분하여 전달 가능
		try(BufferedReader buffer = new BufferedReader(new InputStreamReader(System.in)); 
				OutputStream os = System.out;) {
			
			// 입력 스트림에서 한 줄의 데이터(문자열) 읽어오기
			String str = buffer.readLine();
			
			// 출력 스트림을 통해 입력데이터 출력하기
			os.write(str.getBytes());
			
		} catch (IOException e) {
			e.printStackTrace();
		}
	}

}
```
```java
package io;

import java.io.IOException;
import java.io.OutputStreamWriter;

public class Ex7 {

	public static void main(String[] args) {
		/*
		 * 모니터로 데이터를 출력하는 방법
		 * 2. OutputStreamWriter 사용 (char 단위로 처리)
		 * - write() 메서드를 호출하여 char[] 배열 또는 String 객체를 전달하여
		 * 	 문자 데이터 출력 가능
		 * 	 => String 클래스는 char[] 배열로 관리되므로 Writer 계열에서 처리 가능
		 * - 데코레이션 패턴을 활용하기 위해 BufferedWriter 객체 사용 가능
		 *   => OutputStreamWriter 보다 BufferedWriter 의 처리속도가 빠르다!
		 * 
		 */
		
		try(OutputStreamWriter writer = new OutputStreamWriter(System.out)) {
			String str = "Hello, 자바";
			writer.write(str);
			
		} catch (IOException e) {
			e.printStackTrace();
		}
		

	}

}
```
