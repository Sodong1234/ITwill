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

