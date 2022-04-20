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

- action 폴더의 BoardWriteProAction.java & BoardListAction.java 수정

```java
--------------------------------------BoardWriteProAction.java--------------------------------------
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
		int fileSize = 1024 * 1024 * 10; //byte(1) -> KB(1024Byte) -> MB(1024KB) -> 10MB 단위로 변환 
		
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
				fileSize, // 3) 업로드 파일 크기(10MB 제한)
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
		// --------------------------------------------------------------------------
		// BoardWriteProService 클래스의 인스턴스 생성 후 
		// registArticle() 메서드 호출하여 글쓰기 작업 요청
		// => 파라미터 : BoardDTO 객체     리턴타입 : boolean(isWriteSuccess)
		
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



--------------------------------------BoardListAction.java--------------------------------------



package action;

import java.util.ArrayList;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import svc.BoardListService;
import vo.ActionForward;
import vo.BoardDTO;
import vo.PageInfo;

public class BoardListAction implements Action {

	@Override
	public ActionForward execute(HttpServletRequest request, HttpServletResponse response) throws Exception {
		System.out.println("BoardListAction");
		
		ActionForward forward = null;
		
		// 페이징 처리를 위한 변수 선언
		int pageNum = 1;	// 현재 페이지 번호
		int listLimit = 10;	// 한 페이지 당 표시할 게시물 목록 갯수
		int pageLimit = 10; // 한 페이지 당 표시할 페이지 목록 갯수
		
		// URL 로 전달받은 파라미터 중 "page" 파라미터가 있을 경우 해당 값을 pageNum 값으로 저장
		if(request.getParameter("page") != null) {
			pageNum = Integer.parseInt(request.getParameter("page"));
		}
		
		// BoardListService 클래스 인스턴스 생성 후 getListCount() 메서드 호출하여 총 게시물 수 조회
		// => 파라미터 : 없음 	리턴타입 : int(listCount)
		BoardListService service = new BoardListService();
		int listCount = service.getListCount();
//		System.out.println("총 게시물 수 : " + listCount);
		
		// BoardListService 의 getArticleList() 메서드를 호출하여 게시물 목록 가져오기
		// => 파라미터 : 페이지번호(pageNum), 페이지 당 게시물 수(listLimit)
		// => 리턴타입 : ArrayList<BoardDTO>(articleList)
		ArrayList<BoardDTO> articleList = service.getArticleList(pageNum, listLimit);
		System.out.println(articleList);
		
		// 페이징 처리를 위한 계산 작업
		// 1. 현재 페이지에서 표시할 전체 페이지 수 계산
		// => 총 게시물 수 / 페이지 당 표시할 게시물 수 + 0.9
		// => 주의! 총 게시물 수(int) / 페이지 당 표시할 게시물 수(int) 연산 시 double 타입 연산 필요
		// => 계산된 결과값을 다시 int 타입으로 변환 필요
		// int maxPage = (int)((double)listCount / listLimit + 0.9);

		// java.lang.Math 클래스의 ceil() 메서드를 사용하여 반올림 가능
		int maxPage = (int)Math.ceil((double)listCount / listLimit);

		// 2. 현재 페이지에서 보여줄 시작 페이지 번호(1, 11, 21 등의 시작 번호) 계산
		int startPage = ((int)((double)pageNum / pageLimit + 0.9) - 1) * pageLimit + 1;

		// 3. 현재 페이지에서 보여줄 끝 페이지 번호(10, 20, 30 등의 끝 번호) 계산
		int endPage = startPage + pageLimit - 1;

		// 4. 만약, 끝 페이지(endPage)가 현재 페이지에서 표시할 총 페이지 수(maxPage)보다 클 경우
//		    끝 페이지 번호를 총 페이지 수로 대체
		if(endPage > maxPage) {
			endPage = maxPage;
		}

		// 페이징 처리 정보를 Pageinfo 객체에 저장
		PageInfo pageInfo = new PageInfo(pageNum, maxPage, startPage, endPage, listCount);
		
		// 뷰페이지(jsp)로 객체 전달을 위해 request 객체의 setAttribute() 메서드를 호출하여 객체 저장
		request.setAttribute("pageInfo", pageInfo); // 페이징 처리 정보 객체
		request.setAttribute("articleList", articleList); // 게시물 목록 객체
		
		// ActionForward 객체를 생성하여 포워딩 정보 저장
		// => board 폴더 내의 qna_board_list.jsp 페이지
		// => request 객체를 유지한 채 포워딩 해야하므로 Dispatcher 방식 포워딩
		forward = new ActionForward();
		forward.setPath("./board/qna_board_list.jsp");
		forward.setRedirect(false); // Dispatcher 방식(생략 가능)
		
		
		return forward;
	}

}



```

- vo 폴더의 BoardDTO.java 수정 & PageInfo.java 생성

```java

---------------------------------------------BoardDTO.java---------------------------------------------
package vo;

import java.sql.Date;

/*
 * 게시판 게시물 하나의 정보를 저장하는 DTO 클래스 정의
  CREATE DATABASE mvc_board3;
  
  CREATE TABLE board (
  		board_num INT PRIMARY KEY,
  		board_name VARCHAR(20) NOT NULL,
  		board_pass VARCHAR(16) NOT NULL,
  		board_subject VARCHAR(50) NOT NULL,
  		board_content VARCHAR(2000) NOT NULL,
  		board_file VARCHAR(50) NOT NULL,
  		board_real_file VARCHAR(50) NOT NULL,
  		board_re_ref INT NOT NULL,
  		board_re_lev INT NOT NULL,
  		board_re_seq INT NOT NULL,
  		board_readcount INT NOT NULL,
  		board_date DATE
  );
  
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
	private Date board_date; // java.sql.Date
	
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
	public Date getBoard_date() {
		return board_date;
	}
	public void setBoard_date(Date board_date) {
		this.board_date = board_date;
	}
	@Override
	public String toString() {
		return "BoardDTO [board_num=" + board_num + ", board_name=" + board_name + ", board_pass=" + board_pass
				+ ", board_subject=" + board_subject + ", board_content=" + board_content + ", board_file=" + board_file
				+ ", board_real_file=" + board_real_file + ", board_re_ref=" + board_re_ref + ", board_re_lev="
				+ board_re_lev + ", board_re_seq=" + board_re_seq + ", board_readcount=" + board_readcount
				+ ", board_date=" + board_date + "]";
	}

	

	
}

---------------------------------------------PageInfo.java---------------------------------------------
package vo;

// 페이징 처리 관련 정보를 저장하는 클래스
public class PageInfo {
	private int pageNum;	// 현재 페이지 번호
	private int maxPage;	// 최대 페이지 수
	private int startPage;	// 시작 페이지 번호
	private int endPage;	// 끝 페이지 번호
	private int listCount;	// 총 게시물 수
	
	public PageInfo() {}
	
	public PageInfo(int pageNum, int maxPage, int startPage, int endPage, int listCount) {
		this.pageNum = pageNum;
		this.maxPage = maxPage;
		this.startPage = startPage;
		this.endPage = endPage;
		this.listCount = listCount;
	}

	public int getPageNum() {
		return pageNum;
	}

	public void setPageNum(int pageNum) {
		this.pageNum = pageNum;
	}

	public int getMaxPage() {
		return maxPage;
	}

	public void setMaxPage(int maxPage) {
		this.maxPage = maxPage;
	}

	public int getStartPage() {
		return startPage;
	}

	public void setStartPage(int startPage) {
		this.startPage = startPage;
	}

	public int getEndPage() {
		return endPage;
	}

	public void setEndPage(int endPage) {
		this.endPage = endPage;
	}

	public int getListCount() {
		return listCount;
	}

	public void setListCount(int listCount) {
		this.listCount = listCount;
	}
	
}


```

- svc 폴더에 BoardWriteProService.java & BoardListService.java 생성

```java
-----------------------------------BoardWriteProService.java-----------------------------------
package svc;

import java.sql.Connection;

import dao.BoardDAO;
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
		
		// 3. BoardDAO 클래스로부터 BoardDAO 인스턴스 가져오기 - 공통
		//    (getInstance() 메서드를 호출하여 싱글톤 패턴으로 생성된 인스턴스 리턴받기)
		BoardDAO boardDAO = BoardDAO.getInstance();
		
		// 4. BoardDAO 객체에 Connection 객체 전달하기 - 공통
		//    (setConnection() 메서드를 호출하여 Connection 객체를 파라미터로 지정)
		boardDAO.setConnection(con);
		
		// 5. BoardDAO 객체의 XXX 메서드를 호출하여 요청받은 XXX 작업 수행 및 결과 리턴받기
		// BoardDAO 객체의 insertArticle() 메서드를 호출하여 글쓰기 작업 수행 후 결과값 리턴받기
		// => 파라미터 : BoardDTO 객체(board)	리턴타입 : int(insertCount)
		int insertCount = boardDAO.insertArticle(board);
		
		// 6. 리턴받은 작업 수행 결과를 통해 판별 후 처리 작업 수행(트랜잭션 처리)
		if(insertCount > 0) { // 작업 성공 시
			// 트랜잭션 적용을 위해 JdbcUtil 클래스의 commit() 메서드를 호출하여 commit 작업 수행
			commit(con);
			
			// 작업 처리 결과를 성공으로 표시하기 위해 isWriteSuccess 를 true 로 변경
			isWriteSuccess = true;
		} else { // 작업 실패 시
			// 트랜잭션 취소를 위해 JdbcUtil 클래스의 rollback() 메서드를 호출하여 rollback 작업 수행
			rollback(con);
		}
		
		// 7. JdbcUtil 클래스로부터 가져온 Connection 객체를 반환 - 공통
		//	  => 주의! DAO 클래스에서 Connection 객체 반환 금지!
		close(con);
		
		// 8. 작업 처리 결과 리턴
		return isWriteSuccess;
	}
}

-----------------------------------BoardListService.java-----------------------------------

package svc;

import java.sql.Connection;

import java.util.ArrayList;

import dao.BoardDAO;
import vo.BoardDTO;

import static db.jdbcUtil.*;

public class BoardListService {
	public int getListCount() {
		System.out.println("BoardListService - getListCount()");
		
		// 1. 리턴할 데이터를 저장할 변수 선언
		int listCount = 0;
		
		// 2. JdbcUtil 클래스로부터 Connection Pool 에 저장된 Connection 객체 가져오기 - 공통
		Connection con = getConnection(); // static import 적용되어 있을 경우
		
		// 3. BoardDAO 클래스로부터 BoardDAO 인스턴스 가져오기 - 공통
		BoardDAO boardDAO = BoardDAO.getInstance();
		
		// 4. BoardDAO 객체에 Connection 객체 전달하기 - 공통
		boardDAO.setConnection(con);
		
		// 5. BoardDAO 객체의 selectListCount() 메서드를 호출하여 총 게시물 수 조회
		listCount = boardDAO.selectListCount();
		
		// 6. JdbcUtil 클래스로부터 가져온 Connection 객체를 반환 - 공통
		close(con);
		
		// 7. 조회 결과 리턴
		return listCount;
	}

	public ArrayList<BoardDTO> getArticleList(int pageNum, int listLimit) {
		System.out.println("BoardListService - getArticleList()");
		
		// 1. 리턴할 데이터를 저장할 변수 선언
		ArrayList<BoardDTO> articleList = null;
		
		// 2. JdbcUtil 클래스로부터 Connection Pool 에 저장된 Connection 객체 가져오기 - 공통
		Connection con = getConnection(); // static import 적용되어 있을 경우
		
		// 3. BoardDAO 클래스로부터 BoardDAO 인스턴스 가져오기 - 공통
		BoardDAO boardDAO = BoardDAO.getInstance();
		
		// 4. BoardDAO 객체에 Connection 객체 전달하기 - 공통
		boardDAO.setConnection(con);
		
		// 5. BoardDAO 객체의 selectArticleList() 메서드를 호출하여 게시물 목록 조회
		// => 파라미터 : pageNum, listLimit	리턴타입 : ArrayList<BoardDTO>
		articleList = boardDAO.selectArticleList(pageNum, listLimit);
		
		// 6. JdbcUtil 클래스로부터 가져온 Connection 객체를 반환 - 공통
		close(con);
		
		// 7. 조회 결과 리턴
		return articleList;
	}
}

```

- dao 폴더에 BoardDAO.java 생성

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
	// -----------------------------------------------------------------
	// 외부(Service 클래스)로부터 Connection 객체를 전달받아 관리하기 위해
	// Connection 타입 변수와 Setter 메서드 정의
	Connection con;

	public void setConnection(Connection con) {
		this.con = con;
	}
	// -----------------------------------------------------------------
	// 글쓰기 작업 수행 insertArticle() 메서드 정의
	// => 파라미터 : BoardDTO 객체(board)	 리턴타입 : int(insertCount)
	public int insertArticle(BoardDTO board) {
		System.out.println("BoardDAO - insertArticle()");
		
		// INSERT 작업 결과를 리턴받아 저장할 변수 선언
		int insertCount = 0;
		
		PreparedStatement pstmt = null;
		ResultSet rs = null;
		
		int num = 1; // 새 글 번호를 저장할 변수
		
		
		try {
			// 현재 새 글의 번호로 사용될 번호를 조회
			// => 기존 글번호(board_num) 중에서 가장 큰 값 조회
			
			String sql = "SELECT MAX(board_num) FROM board;";
			pstmt = con.prepareStatement(sql);
			rs = pstmt.executeQuery();
			
			if(rs.next()) {
				// 조회된 레코드 중 Auto_increment 컬럼 값을 num 에 저장
				num = rs.getInt(1) + 1;
			}
			
			close(pstmt);
			
			// 전달받은 데이터(BoardDTO 객체)를 사용하여 board 테이블 INSERT 작업 수행
			// => 단, 글번호는 null, board_re_ref&lev&seq&readcount 는 0, board_date 는 now()
			sql = "INSERT INTO board VALUES (?,?,?,?,?,?,?,?,?,?,?,now())";
			pstmt = con.prepareStatement(sql);
			pstmt.setInt(1, num); // 새 글 번호
			pstmt.setString(2, board.getBoard_name());
			pstmt.setString(3, board.getBoard_pass());
			pstmt.setString(4, board.getBoard_subject());
			pstmt.setString(5, board.getBoard_content());
			pstmt.setString(6, board.getBoard_file());
			pstmt.setString(7, board.getBoard_real_file());
			// 답글에 사용될 참조글 번호(board_re_ref)는 새 글이므로 새 글 번호와 동일하게 지정
			pstmt.setInt(8, num); // board_re_ref(참조글 번호 => 계산된 새 글 번호(num)로 등록)
			pstmt.setInt(9, 0);
			pstmt.setInt(10, 0);
			pstmt.setInt(11, 0);

			insertCount = pstmt.executeUpdate();
		} catch (SQLException e) {
			System.out.println("SQL 구문 오류 발생!");
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
	// 파라미터 : 없음		리턴타입 : int(listCount)
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
			
			// 전체 게시물을 저장할 ArrayList<BoardBean> 타입 객체 생성
			articleList = new ArrayList<BoardDTO>();
			
			// while 문을 사용하여 조회 결과에 대한 반복 작업 수행
			while(rs.next()) {
				// 1개 게시물 정보를 저장할 BoardDTO 객체 생성 후 조회 결과 저장
				BoardDTO board = new BoardDTO();
				board.setBoard_num(rs.getInt("board_num"));
				board.setBoard_name(rs.getString("board_name"));
				board.setBoard_pass(rs.getString("board_pass"));
				board.setBoard_subject(rs.getString("board_subject"));
				board.setBoard_content(rs.getString("board_content"));
				board.setBoard_file(rs.getString("board_file"));
				board.setBoard_real_file(rs.getString("board_real_file"));
				board.setBoard_re_ref(rs.getInt("board_re_ref"));
				board.setBoard_re_lev(rs.getInt("board_re_lev"));
				board.setBoard_re_seq(rs.getInt("board_re_seq"));
				board.setBoard_readcount(rs.getInt("board_readcount"));
				board.setBoard_date(rs.getDate("board_date"));
				
				// 1개 게시물 정보를 다시 전체 게시물 정보 저장 객체(articleList)에 추가
				articleList.add(board);
			}
			
		} catch (SQLException e) {
			System.out.println("SQL 구문 오류 발생! - selectArticleList()");
			e.printStackTrace();
		} finally {
			close(pstmt);
			close(rs);
		}

		
		return articleList;
	}

	

}

```
- qna_board_write.jsp 파일 수정 & qna_board_list.jsp 생성

```jsp
------------------------------------qna_board_write.jsp------------------------------------

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

------------------------------------qna_board_list.jsp------------------------------------

<%@page import="vo.BoardDTO"%>
<%@page import="vo.PageInfo"%>
<%@page import="java.util.ArrayList"%>
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%
// BoardListAction 클래스에서 request 객체에 저장한 객체 꺼내기(getAttribute())
// => "pageInfo" 와 "articleList" 라는 속성명을 사용하여 꺼내기
// => 단, 리턴타입이 Object 타입이므로 각 데이터타입으로 강제 형변환 필요
ArrayList<BoardDTO> articleList = (ArrayList<BoardDTO>)request.getAttribute("articleList");
PageInfo pageInfo = (PageInfo)request.getAttribute("pageInfo");
%>
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
	</table>
	<%=articleList %>
	</section>
	<section id="buttonArea">
		<input type="button" value="글쓰기" onclick="location.href='BoardWriteForm.bo'" />
	</section>
	<section id="pageList">
		<input type="button" value="이전"> 
		1 2 3
		<input type="button" value="다음">
	</section>
</body>
</html>


```

- context.xml의 driverClassName을 수정

```
<?xml version="1.0" encoding="UTF-8"?>
<Context>
	<Resource
		name="jdbc/MySQL" 
		auth="Container"
		type="javax.sql.DataSource"
		driverClassName="com.mysql.cj.jdbc.Driver"
		url="jdbc:mysql://localhost:3306/mvc_board3"
		username="root"
		password="1234"
		maxActive="500"
		maxIdle="100"
	/>
</Context>
```


## [ MVC_Board 프로젝트 비즈니스 로직 처리 과정 ]

```
--------------------------- 요청 방향 ------------------------->
서블릿 요청 -> FrontController -> Action 클래스 -> Service 클래스 -> DAO 클래스
<-------------------------- 응답 방향 --------------------------
```

### 글쓰기 동작 흐름
1. 서블릿 주소 "/BoardWriteForm.bo" 입력 시 BoardFrontController 에 의해 "/board/qna_board_write.jsp" 페이지로 이동
- (Dispatcher 방식으로 포워딩 = /BoardWriteForm.bo 주소가 유지됨)
2. qna_board_write.jsp 페이지에서 글쓰기 폼 출력 후 내용 입력받은 후 글쓰기 버튼 클릭 시 글쓰기 작업 요청을 위한 서블릿 주소("BoardWritePro.bo") 요청
3. 서블릿 주소 "/BoardWritePro.bo" 입력 시 비즈니스 로직 처리를 위해 BoardFrontController 에 의해 Action(BoardWriteProAction) 클래스의 인스턴스 생성 후 execute() 메서드 호출
4. BoardWriteProAction 클래스의 execute() 메서드에서 글쓰기 데이터 준비 작업 후 비즈니스 작업 요청을 위해 BoardWriteProService 클래스의 인스턴스 생성 및 registArticle() 메서드 호출
5. BoardWriteProService 클래스의 registArticle() 메서드에서 비즈니스 로직 처리를 수행하기 위한 준비 작업 및 BoardDAO 클래스의 insertArticle() 메서드 호출하여 INSERT 작업 수행
6. BoardWriteProService 클래스에서 BoardDAO 의 INSERT 작업이 완료된 후 결과값(insertCount)을 리턴받아 판별
- 실패 시 rollback, 성공 시 commit 작업 및 결과값(isWriteSuccess)을 true 로 변경한 후 결과값(isWriteSuccess)을 리턴
7. BoardWriteProAction 클래스에서 BoardWriteProService 로부터 리턴된    결과값(isWriteSuccess)을 리턴받아 판별
- 실패 시 자바스크립트를 사용하여 오류메세지 출력 및 이전페이지로 돌아가기
- 성공 시 ActionForward 객체를 사용하여 BoardList.bo 서블릿 주소를 Redirect 방식으로 설정
8. BoardFrontController 에서 BoardWriteProAction 클래스로부터 리턴받은 ActionForward 객체를 판별하여 포워딩
- "BoardList.bo" 서블릿 주소로 Redirect 방식 포워딩 수행

### 글목록 작업 흐름
1. 서블릿 주소 "/BoardList.bo" 입력 시 BoardFrontController 에 의해 Action(BoardListAction) 클래스의 인스턴스 생성 후 execute() 메서드 호출
2. BoardListAction 의 execute() 메서드에서 데이터 준비 작업 후 BoardListService 클래스 인스턴스 생성 및 getListCount() 메서드를 호출하여 전체 게시물 수 조회 요청
3. BoardListService 의 getListCount() 메서드에서 BoardDAO 의 selectListCount() 메서드를 호출하여 전체 게시물 목록의 갯수 조회 후 결과값(listCount) 리턴받아 해당 결과값을 다시 Action 클래스로 리턴
4. BoardListAction 에서 전체 게시물 조회 결과(listCount)를 리턴받아 저장
5. BoardListAction 클래스에서 BoardListService 클래스의 getArticleList() 메서드를 호출하여 전체 게시물 목록 조회 작업 요청
6. BoardListService 클래스의 getArticleList() 메서드에서 BoardDAO 의 selectArticleList() 메서드를 호출하여 전체 게시물 목록 조회 후 목록 조회를 저장한 ArrayList 객체를 리턴받아 다시 Action 클래스로 리턴
7. BoardListAction 클래스에서 리턴받은 ArrayList 객체를 request 객체에 저장한 후 ActionForward 객체를 사용하여 /board/qna_board_list.jsp 페이지로 포워딩(Dispatcher 방식)
