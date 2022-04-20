# [오전수업 & 오후수업] JSP 50 & 51차

- controller 폴더 BoardFrontController.java 수정

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
			forward = new ActionForward();
			forward.setPath("./board/qna_board_list.jsp");
			forward.setRedirect(false);
		} else if(command.equals("/BoardDeleteForm.bo")) {
			forward = new ActionForward();
			forward.setPath("./board/qna_board_delete.jsp");
			forward.setRedirect(false);
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

- action 폴더의 BoardWriteProAction.java 수정

```java
package action;

import java.io.PrintWriter;

import javax.servlet.ServletContext;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import com.oreilly.servlet.MultipartRequest;
import com.oreilly.servlet.multipart.DefaultFileRenamePolicy;

import svc.BoardWriteProService;
import vo.ActionForward;
import vo.BoardDTO;

/*
 * XXXAction 클래스가 공통으로 갖는 execute() 메서드를 직접 정의하지 않고
 * Action 인터페이스를 상속받아 추상메서드를 구현하여 실수를 예방 가능
 * => 추상메서드 execute() 구현을 강제 => 코드의 통일성과 안정성 향상
 */

public class BoardWriteProAction implements Action {

	@Override
	public ActionForward execute(HttpServletRequest request, HttpServletResponse response) throws Exception {
		System.out.println("BoardWriteProAction");
		
		// 포워딩 정보를 저장하는 ActionForward 타입 변수 선언
		ActionForward forward = null;
		
		// -----------------------------------------------
		// 비즈니스 로직(데이터베이스 처리)을 위한 데이터 준비 작업 수행
		// => 글쓰기 폼에서 작성 후 글쓰기 버튼 클릭 시 현재 객체로 이동
		// => 폼 파라미터를 가져와서 준비 작업 수행(= 게시물 정보 저장)
		// 주의! form 태그에서 enctype="multipart/form-data" 를 명시했을 경우
		// 파라미터들은 request 객체에서 바로 접근할 수 없다!
//		String board_name = request.getParameter("board_name");
//		System.out.println(board_name); // null 값 출력됨
		
		// multipart/form-data 타입의 경우 com.oreilly.servlet.MultipartRequest 객체를 통해 접근 필요
		// => www.servlets.com/cos/ 사이트에서 cos-20.08.zip 파일 다운로드 후 압축 풀고
		//    cos.jar 라이브러리를 프로젝트에 등록
		
		// 파일 업로드 관련 정보 처리를 위해 MultiPartRequest 객체 활용
		// 1. 업로드 할 파일이 저장되는 이클립스 프로젝트 상의 경로를 변수에 저장
		String uploadPath = "upload";
		
		// 2. 업로드 가능한 파일의 크기를 정수 형태로 지정(10MB 제한 설정)
//		int fileSize = 10485760; // 10MB 크기를 직접 지정 시(2진수 기준으로 계산된 크기)
		// 유지보수를 유용하게 하기 위해서 크기 지정 등의 단위 사용 시
		// 기본 단위부터 계산 과정을 차례대로 명시하면 편리함(MB 의 경우 byte -> KB -> MB 로 명시)
		int filesize = 1024 * 1024 * 10; //byte(1) -> KB(1024Byte) -> MB(1024KB) -> 10MB 단위로 변환 
		
		// 3. 현재 프로젝트(서블릿)를 처리하는 객체인 서블릿 컨텍스트 객체 얻어오기
		// -> request 객체의 getServletContext() 메서드를 호출
		ServletContext context = request.getServletContext();
		
		// 4. 업로드 할 파일이 저장되는 실제 경로를 얻어오기
		// => ServletContext 객체의 getRealPath() 메서드를 호출
		String realPath = context.getRealPath(uploadPath); // 가상의 업로드 폴더명을 파라미터로 전달
		System.out.println(realPath);
		// F:\workspace_JSP_model2\.metadata\.plugins\org.eclipse.wst.server.core\tmp0\wtpwebapps\MVC_Board 폴더 내의 upload 폴더
		// => 실제 업로드 될 폴더 위치(워크스페이스 내의 프로젝트 폴더에 있는 upload 폴더는 가상 폴더)
		
		// 5. 작성된 게시물 정보는 폼 파라미터 형태로 전달되어 request 객체에 저장되어 있으므로
		//	  해당 파라미터를 가져와서 BoardDTO 객체에 저장해야함
		//	  단, multipart/form-data 형식이므로 MultipartRequest 객체를 통해 가져와야 하며
		//	  MultipartRequest 객체 생성 시 request 객체를 전달하여 데이터를 관리해야함
		//	  (cos.jar 라이브러리 필수!)
		MultipartRequest multi = new MultipartRequest(
				request, // 1) 실제 요청 정보가 포함된 request 객체
				realPath, // 2) 실제 업로드 폴더 경로 
				filesize, // 3) 업로드 파일 크기(10MB 제한)
				"UTF-8", // 4) 한글 파일명에 대한 인코딩 방식
				new DefaultFileRenamePolicy()); // 5) 중복 파일명에 대한 이름 변경 처리를 담당하는 객체
		// => 객체 생성 시점에 이미 업로드 파일이 폴더에 실제 업로드 됨
		
		// 6. MultipartRequest 객체의 getParameter() 메서드를 호출하여 폼 파라미터 데이터 가져오기
		// => 주의! request.getParameter() 메서드가 아님!
		// => 가져온 데이터는 BoardDTO 객체에 저장
		BoardDTO board = new BoardDTO();
		board.setBoard_name(multi.getParameter("board_name"));
		board.setBoard_pass(multi.getParameter("board_pass"));
		board.setBoard_subject(multi.getParameter("board_subject"));
		board.setBoard_content(multi.getParameter("board_content"));
		
		// 주의! 파일 정보는 getParameter() 메서드로 가져올 수 없으면 별도의 추가 작업 필요
//		board.setBoard_file(multi.getParameter("board_file")); // null
		// 1) 파일명을 관리하는 객체에 접근하여 파일 정보 가져오기(board_file 파라미터명 가져오기)
		String fileElement = multi.getFileNames().nextElement().toString();
		// 2) 1번 작업을 통해 가져온 이름을 사용하여 원본 파일명과 실제 업로드 된 파일명 가져오기 
		String board_file = multi.getOriginalFileName(fileElement);
		String board_real_file = multi.getFilesystemName(fileElement);
//		System.out.println("원본 파일명 : " + board_file + ", 실제 파일명 : " + board_real_file);
		
		board.setBoard_file(board_file);
		board.setBoard_real_file(board_real_file);
		System.out.println(board); // toString() 메서드 생략 가능
		
		BoardWriteProService service = new BoardWriteProService();
		boolean isWriteSuccess = service.registArticle(board);
		
		// Service 클래스로부터 글쓰기 작업 요청 처리 결과를 전달받아 성공/실패 여부 판별
				if(!isWriteSuccess) { // 글쓰기 실패 시(결과값이 false 일 경우)
					// 자바스크립트를 통해 "글쓰기 실패!" 출력하고 이전페이지로 돌아가기
					// => 자바 클래스에서 웹브라우저를 통해 HTML 코드 등을 출력하려면
					//    response 객체를 통해 문서 타입 설정 및 PrintWriter() 객체를 통해 태그 출력하기
					// 1) response 객체의 setContentType() 메서드를 호출하여 문서 타입(ContentType) 지정
					//    => jsp 파일 맨 위에 page 디렉티브 내의 contentType=XXXX 항목과 동일
					response.setContentType("text/html; charset=UTF-8");
					// 2) response 객체의 getWrite() 메서드를 호출하여 출력스트림 PrintWriter 객체 얻어오기
					PrintWriter out = response.getWriter();
					// 3) PrintWriter 객체의 println() 메서드를 호출하여 출력할 태그 작성
					out.println("<script>");
					out.println("alert('글쓰기 실패!')");
					out.println("history.back()");
					out.println("</script>");
				} else { // 글쓰기 성공 시(결과값이 true 일 경우)
					// ActionForward 객체를 통해 "BoardList.bo" 서블릿 주소 요청
					// => 새로운 요청이므로 서블릿 주소 변경을 위해 Redirect 방식으로 포워딩 설정
					forward = new ActionForward();
					forward.setPath("BoardList.bo");
					forward.setRedirect(true);
				}
		
		// ---------------------------------------------------
		// 옵션. 작성자의 IP 주소를 가져오기
		// request 객체의 getRemoteAddr() 메서드를 호출
//		String userIpAddr = request.getRemoteAddr();
//		System.out.println(userIpAddr);
		// ---------------------------------------------------
		
		// 포워딩 정보가 저장된 ActionForward 객체 리턴
		return forward;
	}

}
```

- vo 폴더의 BoardDTO.java 수정

```java
package vo;

import java.sql.Date;

/*
 * 게시판 게시물 하나의 정보를 저장하는 DTO 클래스 정의
 * CREATE DATABASE mvc_board3;
 * 
 * CREATE TABLE board (
 * 		board_num INT PRIMARY KEY AUTO_INCREMENT,
 * 		board_name VARCHAR(20) NOT NULL,
 * 		board_pass VARCHAR(16) NOT NULL,
 * 		board_subject VARCHAR(50) NOT NULL,
 * 		board_content VARCHAR(2000) NOT NULL,
 * 		board_file VARCHAR(50) NOT NULL,
 * 		board_real_file VARCHAR(50) NOT NULL,
 * 		board_re_ref INT NOT NULL,
 * 		board_re_lev INT NOT NULL,
 * 		board_re_seq INT NOT NULL,
 * 		board_reqdcount INT NOT NULL,
 * 		board_date DATE
 * );
 * 
 // => board_num 컬럼은 AUTO_INCREMENT 속성에 의해 번호가 자동으로 증가함
 */

public class BoardDTO {
	// board 테이블 컬럼에 대응하는 멤버변수 선언
	private int board_num;
	private String board_name;
	private String board_pass;
	private String board_subject;
	private String board_content;
	private String board_file;	// 원본 파일명
	private String board_real_file;	// 실제 업로드 된 파일명(중복처리 된 파일명)
	private int board_re_ref;
	private int board_re_lev;
	private int board_re_seq;
	private int board_readcount;
	private Date date; // java.sql.Date
	
	// Getter/Setter 정의
	public int getBoard_num() {
		return board_num;
	}
	public void setBoard_num(int board_num) {
		this.board_num = board_num;
	}
	public String getBoard_name() {
		return board_name;
	}
	public void setBoard_name(String board_name) {
		this.board_name = board_name;
	}
	public String getBoard_pass() {
		return board_pass;
	}
	public void setBoard_pass(String board_pass) {
		this.board_pass = board_pass;
	}
	public String getBoard_subject() {
		return board_subject;
	}
	public void setBoard_subject(String board_subject) {
		this.board_subject = board_subject;
	}
	public String getBoard_content() {
		return board_content;
	}
	public void setBoard_content(String board_content) {
		this.board_content = board_content;
	}
	public String getBoard_file() {
		return board_file;
	}
	public void setBoard_file(String board_file) {
		this.board_file = board_file;
	}
	public String getBoard_real_file() {
		return board_real_file;
	}
	public void setBoard_real_file(String board_real_file) {
		this.board_real_file = board_real_file;
	}
	public int getBoard_re_ref() {
		return board_re_ref;
	}
	public void setBoard_re_ref(int board_re_ref) {
		this.board_re_ref = board_re_ref;
	}
	public int getBoard_re_lev() {
		return board_re_lev;
	}
	public void setBoard_re_lev(int board_re_lev) {
		this.board_re_lev = board_re_lev;
	}
	public int getBoard_re_seq() {
		return board_re_seq;
	}
	public void setBoard_re_seq(int board_re_seq) {
		this.board_re_seq = board_re_seq;
	}
	public int getBoard_readcount() {
		return board_readcount;
	}
	public void setBoard_readcount(int board_readcount) {
		this.board_readcount = board_readcount;
	}
	public Date getDate() {
		return date;
	}
	public void setDate(Date date) {
		this.date = date;
	}
	@Override
	public String toString() {
		return "BoardDTO [board_num=" + board_num + ", board_name=" + board_name + ", board_pass=" + board_pass
				+ ", board_subject=" + board_subject + ", board_content=" + board_content + ", board_file=" + board_file
				+ ", board_real_file=" + board_real_file + ", board_re_ref=" + board_re_ref + ", board_re_lev="
				+ board_re_lev + ", board_re_seq=" + board_re_seq + ", board_readcount=" + board_readcount + ", date="
				+ date + "]";
	}
	
	
	
	

	
}
```

- svc 폴더에 BoardWriteProService.java 생성

```java
package svc;

import java.sql.Connection;

import vo.BoardDTO;

// db.JdbcUtil 클래스의 모든 static 메서드를 import 할 경우 - static import 필요
import static db.jdbcUtil.*;

//Action 클래스로부터 요청(지시)을 받아 DAO 클래스와 상호작용을 통해
//실제 DB 작업 수행을 지시하는 클래스
//=> 주로, Connection 객체 관리, DAO 객체 관리, DAO 객체의 메서드 호출
// 작업 수행 후 결과에 대한 판별을 통해 트랜잭션 관리
public class BoardWriteProService {
	
	// 글쓰기 작업 요청을 위한 registArticle() 메서드 정의
	// => 파라미터 : BoardDTO 객체(board)	리턴타입 : boolean(isWriteSuccess)
	public boolean registArticle(BoardDTO board) {
		System.out.println("BoardWriteProService - registArticle()");
		
		// 1. 글쓰기 작업 요청 처리 결과를 판별하여 저장할 boolean 타입 변수 선언
		boolean isWriteSuccess = false;
		
		// 2. JdbcUtil 클래스로부터 Connection Pool 에 저장된 Connection 객체 가져오기 - 공통
//		Connection con = jdbcUtil.getConnection(); 
		Connection con = getConnection(); // static import 적용되어 있을 경우
		
		// 3. BoardDAO
		
		return isWriteSuccess;
	}
}
```

- dao 폴더에 BoardDAO.java 생성

```java
package dao;

// 실제 비즈니스 로직(DB 작업)을 처리할 BoardDAO 클래스 정의
// => 인스턴스를 여러개 생성할 필요가 없으므로 싱글톤 디자인 패턴을 통해
//	  하나의 인스턴스를 생성하여 모두가 공유하도록 할 수 있음
public class BoardDAO {
	// -------------- 싱글톤 디자인 패턴을 활용한 BoardDAO 인스턴스 생성 작업 -------------
	// 1. 외부에서 인스턴스 생성이 불가능하도록 생성자 정의 시 private 접근제한자 적용
	// 2. 자신의 클래스 내부에서 직접 인스턴스 생성하여 변수에 저장
	//	  => 외부에서 변수에 접근이 불가능하도록 private 접근제한자 적용
	//	  => 클래스 로딩 시점에 인스턴스가 생성되도록 static 멤버변수로 선언
	// 3. 생성된 인스턴스를 외부로 리턴하기 위한 Getter 메서드 정의
	//	  => 외부에서 인스턴스 생성 없이도 호출 가능하도록 static 메서드로 정의
	//	  => 이 때, 2번에서 선언된 변수도 static 변수로 선언되어야 함
	//	     (static 메서드 내에서 인스턴스 멤버에 접근 불가능하며, static 멤버만 접근 가능하므로)
	
//	private BoardDAO() {}
//	
//	BoardDAO instance = new BoardDAO();
//
//	public static BoardDAO getInstance() {
//		return instance;
//	}
	
	// 실제 구현 시 순서
	private static BoardDAO instance = new BoardDAO();
	
	private BoardDAO() {}

	public static BoardDAO getInstance() {
		return instance;
	};
	

}
```
- qna_board_write.jsp 파일 수정

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>MVC 게시판</title>
<style type="text/css">
	#registForm {
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
	
	<!-- 게시판 등록 -->
	<section id="writeForm">
		<h1>게시판 글 등록</h1>
		<!--
		form 데이터 중 파일 정보가 포함될 경우
		<form> 태그 속성에 enctype="multipart/form-data" 명시 필수!
		(생략 시 enctype="application/x-www-form-urlencoded" 속성이 기본값으로 설정됨)
		-->
		<form action="BoardWritePro.bo" name="boardForm" method="post" enctype="multipart/form-data">
			<table>
				<tr>
					<td class="td_left"><label for="board_name">글쓴이</label></td>
					<td class="td_right"><input type="text" name="board_name" required="required" /></td>
				</tr>
				<tr>
					<td class="td_left"><label for="board_pass">비밀번호</label></td>
					<td class="td_right"><input type="password" name="board_pass" required="required" /></td>
				</tr>
				<tr>
					<td class="td_left"><label for="board_subject">제목</label></td>
					<td class="td_right"><input type="text" name="board_subject" required="required" /></td>
				</tr>
				<tr>
					<td class="td_left"><label for="board_content">내용</label></td>
					<td class="td_right">
						<textarea id="board_content" name="board_content" cols="40" rows="15" required="required"></textarea>
					</td>
				</tr>
				<tr>
					<td class="td_left"><label for="board_file">파일 첨부</label></td>
					<!-- 파일 첨부 형식은 input 태그의 type="file" 속성 사용 -->
					<td class="td_right"><input type="file" name="board_file" required="required">
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

- index.jsp 파일 생성

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
</body>
</html>
```

