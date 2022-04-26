# [오전수업] JAVA 38차

## Java Swing
- 자바에서 GUI 구현을 위해 제공되는 API
- 이전의 Java AWT(Active Window Toolkit)를 업그레이드하여 제공하는 패키지
  - AWT API와 Swing API를 조합하여 사용
- java.awt 패키지와 avax.swing 패키지의 각종 클래스 및 인터페이스 활용

---

*자주 사용되는 용어*
1. 컨테이너 : 여러 컴포넌트를 하나로 묶어서 부착 가능한 객체
- 프레임과 패널을 주로 사용
- JFrame 클래스와 JPanel 클래스로 구현
2. 컴포넌트 : 화면을 구성하는 각각의 요소
- 버튼, 체크박스, 라디오버튼, 텍스트필드, 텍스트에어리어
- 컨테이너에 부착하여 사용
- 각 컴포넌트에서 사용자로부터 어떤 동작이 발생하면 해당 동작을 처리하는 이벤트 처리 필요
>
- 창(윈도우)을 생성하기 위해서는 Window 계열 객체 생성 필요
  - 주로 JFrame 클래스를 사용
  - JFrame 객체를 생성하거나 JFrame 클래스를 상속받는 서브클래스를 정의하여 창 생성
    - ex) class Ex1 extends JFrame {} JFrame f = new JFrame();


### 화면을 표시하는 기능을 수행할 showFrame() 메서드
- JFrame 클래스의 각종 메서드를 사용하여 윈도우(창) 설정
1. setSize(가로픽셀, 세로픽셀) => 가로픽셀 * 세로픽셀 크기 지정
    - setLocation(가로좌표, 세로좌표) => 가로(x), 세로(y) 좌표 위치 지정
2. setTitle("제목") => 제목표시줄(타이틀)에 표시할 내용 지정
3. setDefaultCloseOperation() => 닫기 버튼 클릭 시 수행할 동작 지정
  - JFrame.XXX_ON_CLOSE 상수를 사용하여 닫기 버튼 클릭 시 수행할 동작 지정
    - EXIT_ON_CLOSE : 닫기 버튼 클릭 시 프로그램 종료 (따른 창도 모두 닫힘)
		- DISPOSE_ON_CLOSE : 닫기 버튼 클릭 시 현재 창만 종료 (닫힘)
		- HIDE_ON_CLOSE : 닫기 버튼 클릭 시 현재 창 숨김(종료X => 다시 표시 가능)
		- DO_NOTHING_ON_CLOSE : 닫기 버튼 클릭 시 아무 작업도 안함

4. setVisible(true or false) : 현재 프레임 표시 여부 결정(true : 표시, false : 숨김)

```java
package swing;

import javax.swing.JFrame;

public class Ex1 extends JFrame {
	
	public Ex1() {
		showFrame();
	}
	
	// 화면을 표시하는 기능을 수행할 showFrame() 메서드 정의
	public void showFrame() {
	
		setSize(300, 200);	// 가로 300픽셀, 세로 200픽셀 크기 설정
		setLocation(300, 350);	// 창이 생성될 좌표설정
		setTitle("JFrame을 상속받아 생성");	// 제목표시줄 타이틀 설정
		setDefaultCloseOperation(DO_NOTHING_ON_CLOSE);
	
		setVisible(true);
		
		// -----------------------------------------------------------------------
		// JFrame 객체를 직접 생성하여 프레임 생성
		JFrame f = new JFrame("JFrame 으로 생성한 프레임");
		f.setSize(600, 400);
		f.setLocation(600, 600);
		f.setTitle("타이틀 설정");
		f.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
		f.setVisible(true);
		
	}

	public static void main(String[] args) {
		new Ex1();
	}

}
```

연습문제
```java
package swing;

import javax.swing.JFrame;

/*
 * 400 * 400 크기의 창 생성
 * 제목 : JFrame 생성 연습
 * 닫기 버튼 클릭 시 프로그램 종료
 * 화면 표시 설정 : true
 * 
 * 
 */

public class Test1 extends JFrame {

	public Test1() {
		showFrame();
	}
	
	public void showFrame() {
		// FRame 객체 생성 version
//		JFrame f = new JFrame("JFrame 생성 연습");
//		f.setSize(400, 400);
//		f.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
//		f.setVisible(true);
		
		// JFrame 상속 받는 version
		setTitle("JFrame 생성연습");
		setSize(400, 400);
		setDefaultCloseOperation(EXIT_ON_CLOSE);
		setVisible(true);
	}
	
	public static void main(String[] args) {

	}

}
```

```java
package swing;

import java.awt.Dimension;
import java.awt.Point;
import java.awt.Rectangle;

import javax.swing.JFrame;

public class Ex2 {

	public Ex2() {
		showFrame();
	}
	
	public void showFrame() {
		JFrame f = new JFrame();
//		f.setSize(600, 400);
		
		// 크기를 지정하는 setSize() 파라미터에 Dimension 객체 전달 가능하며
		// Dimension 객체 사용 시 크기를 지정하는 값을 관리할 수 있음
		// => 여러 컨테이너나 컴포넌트의 크기가 일정할 때 하나로 관리할 수 있음
//		Dimension d = new Dimension(600, 400);
//		f.setSize(d);
		
//		f.setLocation(800, 300);
		
		// 위치 좌표를 지정하는 setLocation() 파라미터에 Point 객체 전달 가능하며
		// Point 객체 사용 시 좌표를 지정하는 값을 관리 가능
		// => 여러 컨테이너나 컴포넌트의 위치가 일정할 때 하나로 관리할 수 있음
//		Point p = new Point(800, 300);
//		f.setLocation(p);
		
		// --------------------------------------------------------------
		// setBounds() 메서드를 사용하면 좌표와 크기를 동시에 설정 가능
		// 1) setBounds(x, y, width, height) : x좌표, y좌표, 가로크기, 세로크기 순으로 전달
//		f.setBounds(800, 300, 600, 400);
		
		// 2) setBounds(Rectangle r) : 좌표와 크기를 관리하는 Rectangle 객체 전달 가능
		// => Rectangle 객체 생성 시 파라미터로 x, y, width, height 전달 하거나 
		//	  Point p, Dimension d 객체 전달도 가능
//		Point p = new Point(800, 300);
//		Dimension d = new Dimension(600, 400);
//		Rectangle r = new Rectangle(p, d);
		
		Rectangle r = new Rectangle(800, 300, 600, 400);
		
		f.setBounds(r);
		
		f.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
		f.setVisible(true);
	}
	
	public static void main(String[] args) {
		new Ex2();
	}

}
```

```java
package swing;

import javax.swing.JButton;
import javax.swing.JFrame;

public class Ex3 extends JFrame {

	public Ex3() {
		showFrame();
	}
	
	public void showFrame() {
		setBounds(600, 400, 300, 300);
		
		// 버픈 컴포넌트(JButton)를 생성하여 프레임(JFrame = 현재 객체)에 부착
		JButton btn = new JButton("버튼");
		add(btn);
		
//		btn.addActionListener(new ActionListener() {
//			
//			@Override
//			public void actionPerformed(ActionEvent e) {
//				System.out.println("버튼클릭");
//			}
//		});
		
		// 람다식
		
		btn.addActionListener(e -> System.out.println("버튼클릭"));
		
		setDefaultCloseOperation(EXIT_ON_CLOSE);
	}
	
	public static void main(String[] args) {
		new Ex3();
	}

}
```


연습문제
```java
package swing;

import java.awt.Rectangle;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;

import javax.swing.JButton;
import javax.swing.JFrame;

/*
 * JFrame 객체 2개 생성
 * - 1번 크기와 위치 (800, 400, 300, 200)
 * - 2번 크기와 위치 (200, 200, 500, 500)
 * 
 * JButton 객체 2개 생성 (버튼1, 버튼2)
 * 각각 프레임에 부착
 * 이벤트 : "버튼X 클릭됨!" 이라고 출력 (람다식)
 * 
 */
public class Test3 {

	public Test3() {
		showFrame();
	}

	public void showFrame() {
		// JFream 객체 생성
		JFrame f1 = new JFrame("프레임1");
		JFrame f2 = new JFrame("프레임2");
		
		// JFrame 크기와 위치 설정
		f1.setBounds(800, 400, 300, 200);
		f2.setBounds(200, 200, 500, 500);

		
		// JButton 객체 생성
		JButton btn1 = new JButton("버튼1");
		JButton btn2 = new JButton("버튼2");
		
		// JFrame 객체에 JButton 객체 부착
		f1.add(btn1);
		f2.add(btn2);
		
		// JButton 객체에 이벤트 연결
//		btn1.addActionListener(new ActionListener() {
//			
//			@Override
//			public void actionPerformed(ActionEvent e) {
//				System.out.println("버튼1 클릭됨!");
//			}
//		});
//		
//		btn2.addActionListener(new ActionListener() {
//			
//			@Override
//			public void actionPerformed(ActionEvent e) {
//				System.out.println("버튼2 클릭됨!");
//			}
//		});
		
		// JButton 객체에 이벤트 연결(람다식 version)
//		btn1.addActionListener(e -> System.out.println(btn1.getText() + " 클릭됨!"));
//		btn2.addActionListener(e -> System.out.println(btn2.getText() + " 클릭됨!"));

		ActionListener action = e -> System.out.println(((JButton)e.getSource()).getText() + " 클릭됨!");
		btn1.addActionListener(action);
		btn2.addActionListener(action);


		
		f1.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
		f2.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
		f1.setVisible(true);
		f2.setVisible(true);


	}
	

	public static void main(String[] args) {
		new Test3();
	}

}

```



---

# [오후수업] JSP 54차

- action 폴더의 BoardModifyProAction.java 파일 수정 & BoardReplyFormAction.java, BoardReplyProAction.java 생성
```java
-----------------------------------------------BoardModifyProAction.java-----------------------------------------------
package action;

import java.io.PrintWriter;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import svc.BoardModifyProService;
import vo.ActionForward;
import vo.BoardDTO;

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
		if(!isArticleWriter) { // 패스워드가 일치하지 않을 경우(= 수정 권한 없음)
			response.setContentType("text/html; charset=UTF-8");
			PrintWriter out = response.getWriter();
			out.println("<script>");
			out.println("alert('수정 권한이 없습니다!')");
			out.println("history.back()");
			out.println("</script>");
		} else { // 패스워드가 일치할 경우(= 수정 권한 있음)
			// 수정 가능할 경우 BoardModifyProService 에서 - modifyArticle() 메서드 호출하여 수정
			// => 파라미터 : BoardDTO 객체(article)
			// => 리턴타입 : boolean(isModifySuccess)
			BoardDTO article = new BoardDTO();
			article.setBoard_num(board_num);
			article.setBoard_name(board_name);
			article.setBoard_subject(board_subject);
			article.setBoard_content(board_content);
			
			boolean isModifySuccess = service.modifyArticle(article);
			
			// 수정 완료 결과 판별 후 실패 시 자바스크립트로 "수정 실패!" 출력 후 이전페이지,
			// 성공 시 BoardDetail.bo 로 포워딩 요청
			if(!isModifySuccess) {
				response.setContentType("text/html; charset=UTF-8");
				PrintWriter out = response.getWriter();
				out.println("<script>");
				out.println("alert('수정 실패!')");
				out.println("history.back()");
				out.println("</script>");
			} else {
				// 수정 성공 시(isModifySuccess 가 true) BoardList.bo 페이지 포워딩 설정
				// => 페이지 번호를 URL 에 포함시켜 포워딩
				forward = new ActionForward();
				forward.setPath("BoardList.bo?page=" + request.getParameter("page"));
				forward.setRedirect(true);
			}
		}
		return forward;
	}

}

-----------------------------------------------BoardReplyFormAction.java-----------------------------------------------

package action;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import svc.BoardDetailService;
import vo.ActionForward;
import vo.BoardDTO;

public class BoardReplyFormAction implements Action {

	@Override
	public ActionForward execute(HttpServletRequest request, HttpServletResponse response) throws Exception {
		System.out.println("BoardReplyFormAction");
		
		ActionForward forward = null;
		
		// 전달받은 파라미터(글번호) 가져오기
		int board_num = Integer.parseInt(request.getParameter("board_num"));
		
		// BoardDetailService 클래스의 getArticle() 메서드를 호출하여
		// 원본 게시물 1개 정보 가져오기 
		// => 파라미터 : 글번호(board_num)	 리턴타입 : BoardDTO(article)
		BoardDetailService service = new BoardDetailService();
		BoardDTO article = service.getArticle(board_num);
		
		// request 객체에 전달할 객체 지정
		request.setAttribute("article", article);
		
		// ActionForward 객체를 통해 board/qna_board_reply.jsp 페이지 설정
		forward = new ActionForward();
		forward.setPath("./board/qna_board_reply.jsp");
		forward.setRedirect(false); // Dispatcher 방식
		
		return forward;
		
	}

}

-----------------------------------------------BoardReplyProAction.java-----------------------------------------------

package action;

import java.io.PrintWriter;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import svc.BoardModifyProService;
import svc.BoardReplyProService;
import vo.ActionForward;
import vo.BoardDTO;

public class BoardReplyProAction implements Action {

	@Override
	public ActionForward execute(HttpServletRequest request, HttpServletResponse response) throws Exception {
		System.out.println("BoardReplyProAction");
		
		ActionForward forward = null;
		
		BoardDTO article = new BoardDTO();
		article.setBoard_num(Integer.parseInt(request.getParameter("board_num")));
		article.setBoard_name(request.getParameter("board_name"));
		article.setBoard_pass(request.getParameter("board_pass"));
		article.setBoard_subject(request.getParameter("board_subject"));
		article.setBoard_content(request.getParameter("board_content"));
		article.setBoard_re_ref(Integer.parseInt(request.getParameter("board_re_ref")));
		article.setBoard_re_lev(Integer.parseInt(request.getParameter("board_re_lev")));
		article.setBoard_re_seq(Integer.parseInt(request.getParameter("board_re_seq")));
//		System.out.println(article);
		
		// BoardReplyProService 의 replyArticle() 메서드를 호출하여 답글 등록 작업 요청
		// => 파라미터 : BoardDTO 객체(article)   리턴타입 : boolean(isReplySuccess)
		BoardReplyProService service = new BoardReplyProService();
		boolean isReplySuccess = service.replyArticle(article);
		
		// 답글 등록 작업 요청 처리 결과 판별
		// => 실패 시 자바스크립트를 사용하여 "답글 등록 실패!" 출력 후 이전페이지
		// => 성공 시 BoardList.bo 페이지로 포워딩(페이지 번호 전달)
		if(!isReplySuccess) {
			response.setContentType("text/html; charset=UTF-8");
			PrintWriter out = response.getWriter();
			out.println("<script>");
			out.println("alert('답글 등록 실패!')");
			out.println("history.back()");
			out.println("</script>");
		} else {
			forward = new ActionForward();
			forward.setPath("BoardList.bo?page=" + request.getParameter("page"));
			forward.setRedirect(true); // Redirect 방식
		}
		
		return forward;
	}

}

```

- svc 폴더의 BoardModifyProService.java 파일 수정 & BoardReplyProService.java 생성

```java
-----------------------------------------------BoardModifyProService.java-----------------------------------------------
package svc;

import static db.jdbcUtil.close;
import static db.jdbcUtil.commit;
import static db.jdbcUtil.getConnection;
import static db.jdbcUtil.rollback;

import java.sql.Connection;

import dao.BoardDAO;
import vo.BoardDTO;

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

	// 글 수정 작업을 요청하는 modifyArticle() 메서드 정의
	public boolean modifyArticle(BoardDTO article) {
		boolean isModifySuccess = false;
		
		Connection con = getConnection();
		
		BoardDAO boardDAO = BoardDAO.getInstance();
		
		boardDAO.setConnection(con);
		
		// BoardDAO 의 updateArticle() 메서드 호출하여 글 수정 작업 수행
		// => 파라미터 : BoardDTO 객체		리턴타입 : int(updateCount)
		int updateCount = boardDAO.updateArticle(article);
		
		// updateCount 가 0보다 크면 commit, 아니면 rollback 작업 수행
		if(updateCount > 0) {
			commit(con);
			// isModifySuccess 를 true 로 변경
			isModifySuccess = true;
		} else {
			rollback(con);
		}
		
		close(con);
		
		return isModifySuccess;
	}
}


-----------------------------------------------BoardReplyProService.java-----------------------------------------------
package svc;

import static db.jdbcUtil.*;

import java.sql.Connection;

import dao.BoardDAO;
import vo.BoardDTO;

public class BoardReplyProService {
	// 답글 등록 작업을 요청하는 replyArticle() 메서드 정의
	public boolean replyArticle(BoardDTO article) {
		boolean isReplySuccess = false;
		
		Connection con = getConnection();
		
		BoardDAO boardDAO = BoardDAO.getInstance();
		
		boardDAO.setConnection(con);
		
		// BoardDAO 의 insertReplyArticle() 메서드 호출하여 답글 등록 작업 수행
		// => 파라미터 : BoardDTO 객체    리턴타입 : int(insertCount)
		int insertCount = boardDAO.insertReplyArticle(article);
		
		// insertCount 가 0보다 크면 commit, 아니면 rollback 작업 수행
		if(insertCount > 0) {
			commit(con);
			// isReplySuccess 를 true 로 변경
			isReplySuccess = true;
		} else {
			rollback(con);
		}
		
		close(con);
		
		return isReplySuccess;
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

	// 글 수정 작업을 수행하는 updateArticle() 메서드 정의
	public int updateArticle(BoardDTO article) {
		int updateCount = 0;

		PreparedStatement pstmt = null;
		try {
			String sql = "UPDATE board " 
					+ "SET board_name=?,board_subject=?,board_content=? " 
					+ "WHERE board_num=?";
			pstmt = con.prepareStatement(sql);
			pstmt.setString(1, article.getBoard_name());
			pstmt.setString(2, article.getBoard_subject());
			pstmt.setString(3, article.getBoard_content());
			pstmt.setInt(4, article.getBoard_num());

			updateCount = pstmt.executeUpdate();
		} catch (SQLException e) {
			System.out.println("SQL 구문 오류 발생! - updateArticle()");
			e.printStackTrace();
		} finally {
			close(pstmt);
		}

		return updateCount;
	}

	// 답글 등록 작업을 수행하는 insertReplyArticle() 메서드 정의
	public int insertReplyArticle(BoardDTO article) {
		int insertCount = 0;

		PreparedStatement pstmt = null, pstmt2 = null;
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

			/*
			 * 답변글 등록을 위한 계산
			 * 1) 원본글 번호(board_num)
			 * 2) 새 글 번호(num)
			 * 3) 참조글번호(board_re_ref)
			 * 4) 순서번호(board_re_seq)
			 * 5) 들여쓰기레벨(board_re_lev)
			 * +-----------+-----------+------------------+------------+------------+------------+
			 * | board_num |새글번호num| board_subject      |board_re_ref|board_re_seq|board_re_lev|
			 * +-----------+-----------+------------------+------------+------------+------------+
			 * |        76 |        X  | 수정-사진입니다.    |         76 |          0 |          0 |
			 * |        74 |        X  | 제목74           |         74 |          0 |          0 |
			 * |        73 |        X  | 제목73           |         73 |          0 |          0 |
			 * +-----------+-----------+------------------+------------+------------+------------+
			 * 
			 * ex) 76번 게시물에 대한 답글 등록 => 등록할 새 글 번호가 100번이라고 가정
			 * => 76번에 대한 답글이므로 원본글(76번)의 참조글번호(board_re_ref)를 
			 *    새 글(100번)의 참조글번호(board_re_ref)로 사용
			 * => 원본글의 참조글번호(board_re_ref)가 동일한 게시물의 순서번호(board_re_seq)보다
			 *    큰 게시물들의 순서번호를 모두 + 1 씩 증가시킨 후
			 *    새 글의 순서번호는 원본글의 순서번호 + 1 값을 지정	
			 * => 새 글의 들여쓰기 레벨(board_re_lev)은 원본글의 들여쓰기 레벨 + 1 값을 지정
			 * +-----------+-----------+------------------+------------+------------+------------+
			 * | 원본글번호   |새글번호num  | board_subject    |board_re_ref|board_re_seq|board_re_lev|
			 * +-----------+-----------+------------------+------------+------------+------------+
			 * |        76 |        X  | 수정-사진입니다.     |         76 |          0 |          0 |
			 *          76        100    Re:수정-사진입.               76            1            1
			 * |        74 |        X  | 제목74           |          74 |          0 |          0 |
			 * |        73 |        X  | 제목73           |          73 |          0 |          0 |
			 * +-----------+-----------+------------------+------------+------------+------------+
			 * 
			 * ex) 76번 게시물에 대한 두번째 답글 등록 => 등록할 새 글 번호가 101번이라고 가정
			 * => 76번에 대한 답글이므로 원본글(76번)의 참조글번호(board_re_ref)를 
			 *    새 글(100번)의 참조글번호(board_re_ref)로 사용
			 * => 원본글과 참조글번호(ref)가 동일한 게시물이 존재(100번)하므로
			 *    100번 게시물의 순서번호(seq)를 1 증가시킨 후 
			 *    새 글의 순서번호(seq)는 원본글의 순서번호 + 1 로 설정
			 * => 새 글의 들여쓰기 레벨(lev)은 원본글의 들여쓰기레벨 + 1 로 설정
			 * +-----------+-----------+------------------+------------+------------+------------+
			 * | 원본글번호|새글번호num| board_subject         |board_re_ref|board_re_seq|board_re_lev|
			 * +-----------+-----------+------------------+------------+------------+------------+
			 * |        76 |        X  | 수정-사진입니다.     |         76 |          0 |          0 |
			 *          76        101    Re:수정-사진입22              76           1            1
			 *          76        100    Re:수정-사진입.               76       1 -> 2            1
			 * |        74 |        X  | 제목74            |          74 |          0 |          0 |
			 * |        73 |        X  | 제목73            |          73 |          0 |          0 |
			 * +-----------+-----------+------------------+------------+------------+------------+
			 * 
			 * 
			 * ex) 101번 게시물에 대한 두번째 답글 등록 => 등록할 새 글 번호가 110번이라고 가정
			 * => 101번에 대한 답글이므로 원본글(101번)의 참조글번호(board_re_ref)를 
			 *    새 글(110번)의 참조글번호(board_re_ref)로 사용
			 * => 원본글과 참조글번호(ref)가 동일한 게시물이 존재하므로
			 *    해당 게시물들 중 원본글의 순서번호보다 큰 게시물(109, 100번)들의 
			 *    순서번호(seq)를 모두 1씩 증가시킨 후 
			 *    새 글의 순서번호(seq)는 원본글의 순서번호 + 1 로 설정
			 * => 새 글의 들여쓰기 레벨(lev)은 원본글의 들여쓰기레벨 + 1 로 설정
			 * +-----------+------------------+------------+------------+------------+
			 * |새글번호num| board_subject      |board_re_ref|board_re_seq|board_re_lev|
			 * +-----------+------------------+------------+------------+------------+
			 * |       76  | 수정-사진입니다.     |         76 |          0 |          0 |
			 *        101    Re:수정-사진입2               76            1            1
			 *        110      Re:Re:수정-사22            76            2            2
			 *        109      Re:Re:수정-사진            76        2 -> 3            2
			 *        100    Re:수정-사진입.               76        3 -> 4            1
			 * |       74  | 제목74           |          74 |          0 |          0 |
			 *        200      Re:제목74                 74            1            1
			 * |       73  | 제목73           |          73 |          0 |          0 |
			 * +-----------+------------------+------------+------------+------------+
			 */
			
			// 기존 답글들에 대한 순서 번호 증가 작업 처리
			// => 원본글의 참조글번호(board_re_ref)와 같고
			//    원본글의 순서번호(board_re_seq)보다 큰 레코드들의
			//    순서번호(board_re_seq)를 1씩 증가시키기
			sql = "UPDATE board SET board_re_seq=board_re_seq+1 "
					+ "WHERE board_re_ref=? AND board_re_seq>?";
			pstmt2 = con.prepareStatement(sql);
			pstmt2.setInt(1, article.getBoard_re_ref()); // 참조글번호
			pstmt2.setInt(2, article.getBoard_re_seq()); // 순서번호
			pstmt2.executeUpdate();
			
			// 답글(새글) 등록 작업 처리
			// 전달받은 데이터(BoardDTO 객체)를 사용하여 board 테이블 INSERT 작업 수행
			sql = "INSERT INTO board VALUES (?,?,?,?,?,?,?,?,?,?,?,now())";
			pstmt2 = con.prepareStatement(sql);
			pstmt2.setInt(1, num); // 새 글 번호
			pstmt2.setString(2, article.getBoard_name());
			pstmt2.setString(3, article.getBoard_pass());
			pstmt2.setString(4, article.getBoard_subject());
			pstmt2.setString(5, article.getBoard_content());
			pstmt2.setString(6, ""); // 답글에 파일 등록을 허용하지 않으므로
			pstmt2.setString(7, ""); // 파일명은 모두 널스트링("")으로 처리
			// -----------------------------------------------------------------------------
			// 주의해야할 부분(답글 관련 처리)
			// 답글에 사용될 참조글 번호(board_re_ref)는 원본글의 참조글번호와 동일하게 지정
			pstmt2.setInt(8, article.getBoard_re_ref());
			// 들여쓰기레벨(board_re_lev)과 순서번호(board_re_seq)는 원본글의 값 + 1 
			pstmt2.setInt(9, article.getBoard_re_lev() + 1);
			pstmt2.setInt(10, article.getBoard_re_seq() + 1);
			// -----------------------------------------------------------------------------			
			pstmt2.setInt(11, 0); // board_readcount
			
			insertCount = pstmt2.executeUpdate();
		} catch (SQLException e) {
			System.out.println("SQL 구문 오류 발생! - insertReplyArticle()");
			e.printStackTrace();
		} finally {
			close(rs);
			close(pstmt);
			close(pstmt2);
		}
		
		return insertCount;
	}
	
}
```
- controller 폴더의 BoardFrontController.java 파일 수정

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
import action.BoardReplyFormAction;
import action.BoardReplyProAction;
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
		} else if (command.equals("/BoardReplyForm.bo")) {
			// 비즈니스 로직 처리를 위해 BoardReplyFormAction 클래스의 execute() 메서드 호출
			action = new BoardReplyFormAction();
			try {
				forward = action.execute(request, response);
			} catch (Exception e) {
				e.printStackTrace();
			}
		} else if (command.equals("/BoardReplyPro.bo")) {
			// 비즈니스 로직 처리를 위해 BoardReplyFormAction 클래스의 execute() 메서드 호출
			action = new BoardReplyProAction();
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

- qna_board_reply.jsp 파일 생성
```jsp
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
	#replyForm {
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
	
	<!-- 게시판 답글 작성 -->
	<section id="ReplyForm">
		<h1>게시판 답글 작성</h1>
		<form action="./BoardReplyPro.bo" name="boardForm" method="post">
			<!-- input type="hidden" 사용하여 글번호(board_num)와 페이지번호(page) 전달 -->
			<input type="hidden" name="board_num" value="<%=article.getBoard_num()%>">
			<input type="hidden" name="page" value="<%=request.getParameter("page")%>">
			<!-- 답글 관련 원본글 정보를 담는 board_re_ref, board_re_lev, board_re_seq 도 전달 -->
			<input type="hidden" name="board_re_ref" value="<%=article.getBoard_re_ref()%>">
			<input type="hidden" name="board_re_lev" value="<%=article.getBoard_re_lev()%>">
			<input type="hidden" name="board_re_seq" value="<%=article.getBoard_re_seq()%>">
			
			<table>
				<tr>
					<td class="td_left"><label for="board_name">글쓴이</label></td>
					<td class="td_right">
						<input type="text" name="board_name" required="required" />
					</td>
				</tr>
				<tr>
					<td class="td_left"><label for="board_pass">비밀번호</label></td>
					<td class="td_right"><input type="password" name="board_pass" required="required" /></td>
				</tr>
				<!-- 답글 작성 시 원본글의 제목, 내용은 표시 -->
				<tr>
					<td class="td_left"><label for="board_subject">제목</label></td>
					<td class="td_right"><input type="text" name="board_subject" value="RE:<%=article.getBoard_subject() %>" required="required" /></td>
				</tr>
				<tr>
					<td class="td_left"><label for="board_content">내용</label></td>
					<td class="td_right">
						<textarea id="board_content" name="board_content" cols="40" rows="15" required="required">
						-------- 원본 글 내용 --------
						<%=article.getBoard_content() %>
						</textarea>
					</td>
				</tr>
			</table>
			<section id="commandCell">
				<input type="submit" value="답글등록">&nbsp;&nbsp;
				<input type="reset" value="다시쓰기">&nbsp;&nbsp;
				<input type="button" value="취소" onclick="history.back()">
			</section>
		</form>
	</section>
	
</body>
</html>
```

- member폴더에 join_form.jsp, login_form.jsp 생성

```jsp
---------------------------------------------join_form.jsp---------------------------------------------
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
	<form action="MemberJoinPro.me" method="post" name="joinForm">
		<table border="1">
			<tr>
				<td>이름</td>
				<td><input type="text" name="name" required="required" size="20"></td>
			</tr>
			<tr>
				<td>성별</td>
				<td>
					<input type="radio" name="gender" value="남">남&nbsp;&nbsp;
					<input type="radio" name="gender" value="여">여
				</td>
			</tr>
			<tr>
				<td>나이</td>
				<td><input type="text" name="age" required="required" size="10"></td>
			</tr>
			<tr>
				<td>E-Mail</td>
				<td>
					<input type="text" name="email1" required="required" size="10">@
					<input type="text" name="email2" required="required" size="10">
					<select name="selectDomain">
						<option value="">직접입력</option>	
						<option value="naver.com">naver.com</option>
						<option value="nate.com">nate.com</option>
					</select>
				</td>
			</tr>
			<tr>
				<td>아이디</td>
				<td>
					<input type="text" name="id" required="required" size="20" 
						placeholder="4-16자리 영문자,숫자 조합">
					<span id="checkIdResult"><!-- 자바스크립트에 의해 메세지가 표시될 공간 --></span>
				</td>
			</tr>
			<tr>
				<td>패스워드</td>
				<td>
					<input type="password" name="passwd" required="required" size="20" 
						placeholder="8-20자리 영문자,숫자,특수문자 조합">
					<span id="checkPasswdResult"><!-- 자바스크립트에 의해 메세지가 표시될 공간 --></span>
				</td>
			</tr>
			<tr>
				<td colspan="2" align="center">
					<input type="submit" value="회원가입">
					<input type="button" value="취소" onclick="history.back()">
				</td>
			</tr>
		</table>
	</form>
</body>
</html>
---------------------------------------------login_form.jsp---------------------------------------------
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<h1>로그인</h1>
	<form action="MemberLoginPro.me" method="post">
		<table>
			<tr>
				<td>아이디</td>
				<td><input type="text" name="id" required="required" size="20"></td>
			</tr>
			<tr>
				<td>패스워드</td>
				<td><input type="password" name="passwd" required="required" size="20"></td>
			</tr>
			<tr>
				<td colspan="2" align="center">
					<input type="submit" value="로그인">
					<input type="button" value="회원가입" onclick="location.href='MemberJoinForm.me'">
				</td>
			</tr>
		</table>
	</form>
</body>
</html>
```
