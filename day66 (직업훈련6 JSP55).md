# [오전수업] 직업훈련 6차

---

# [오후수업] JSP 55차

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
	<header>
		<div align="right">
			<h5><a href="./MemberLoginForm.me">로그인</a> | <a href="./MemberJoinForm.me">회원가입</a></h5>
		</div>
	</header>
	<h1>MVC_Board 메인페이지</h1>
	<h3><a href="./BoardWriteForm.bo">글쓰기</a></h3>
	<h3><a href="./BoardList.bo">글목록</a></h3>
</body>
</html>
```
- member 폴더의 join_form.jsp 파일 수정 & check_id.jsp 파일 생성
```jsp
----------------------------------------join_form.jsp----------------------------------------
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<script type="text/javascript">
	// submit 작업을 수행하기 전 각 작업의 완료 여부를 체크할 변수(전역변수) 선언
	let isCheckConfirmPasswd = false; // 패스워드 중복확인 체크 변수
	

	// 1. ID 중복확인 버튼 클릭 시 새 창 띄우기
	function checkDuplicateId() {
		window.open("MemberCheckId.me", "check", "width=400,height=200");
	}
	
	// 2. 비밀번호 입력란에 키를 누를때마다 비밀번호 길이 체크하기
	// => 체크 결과를 비밀번호 입력창 우측 빈공간에 표시하기
	// => 비밀번호 길이 체크를 통해 8 ~ 16글자 사이이면 "사용 가능한 패스워드"(파란색) 표시,
    // 아니면, "사용 불가능한 패스워드"(빨간색) 표시
	function checkPasswd(passwd) {
		// span 태그 영역(checkPasswdResult)
		var span_checkPasswdResult = document.getElementById("checkPasswdResult");
		
		// 입력된 패스워드 길이 체크
		if(passwd.length >= 8 && passwd.length <= 16) {
			span_checkPasswdResult.innerHTML = "사용 가능한 패스워드";
			span_checkPasswdResult.style.color = "BLUE";
		} else {
			span_checkPasswdResult.innerHTML = "사용 불가능한 패스워드";
			span_checkPasswdResult.style.color = "RED";
		}
	}
	
	// 3. 비밀번호확인 입력란에 키를 누를때마다 비밀번호와 같은지 체크하기
	// => 체크 결과를 비밀번호확인 입력창 우측 빈공간에 표시하기
	// => 비밀번호와 비밀번호확인 입력 내용이 같으면 "비밀번호 일치"(파란색) 표시,
	//    아니면, "비밀번호 불일치"(빨간색) 표시
	function checkConfirmPasswd(confirmPasswd) {
		// 패스워드 입력란에 입력된 패스워드를 가져오기
		var passwd = document.fr.passwd.value;
		
		var span_checkConfirmPasswdResult = document.getElementById("checkConfirmPasswdResult");
		
		// 패스워드 일치 여부 판별
		if(passwd == confirmPasswd) { // 일치할 경우
			span_checkConfirmPasswdResult.innerHTML = "비밀번호 일치";
			span_checkConfirmPasswdResult.style.color = "BLUE";
			
			// 패스워드 일치 여부 확인을 위해 isCheckConfirmPasswd 변수값을 true 로 변경
			isCheckConfirmPasswd = true;
		} else { // 일치하지 않을 경우
			span_checkConfirmPasswdResult.innerHTML = "비밀번호 불일치";
			span_checkConfirmPasswdResult.style.color = "RED";

			isCheckConfirmPasswd = false;
		}
	}
	
	// 4. 주민번호 숫자 입력할때마다 길이 체크하기
	// => 주민번호 앞자리 입력란에 입력된 숫자가 6자리이면 뒷자리 입력란으로 커서 이동시키기
	// => 주민번호 뒷자리 입력란에 입력된 숫자가 7자리이면 뒷자리 입력란에서 커서 제거하기
	function checkJumin1(jumin1) {
		if(jumin1.length == 6) {
			document.fr.jumin2.focus(); // 커서 요청(포커스 요청)
		}
	}
	
	function checkJumin2(jumin2) {
		if(jumin2.length == 7) {
			document.fr.jumin2.blur(); // 커서 제거(포커스 해제)
		}
	}
	
	// 5. 이메일 도메인 선택 셀렉트 박스 항목 변경 시 선택된 셀렉트 박스 값을 
	//    이메일 두번째 항목(@ 기호 뒤)에 표시하기
	//    단, 직접입력 선택 시 표시된 도메인 삭제하기
	function selectDomain(domain) {
		document.fr.email2.value = domain;
		
		// 만약, "직접입력" 항목 선택 시 도메인 입력창(email2)에 커서 요청(옵션)
		if(domain == "") { // "직접입력" 선택 시 domain 변수에 "" 값이 전달됨
			document.fr.email2.disabled = false; // 입력창 잠금 해제
			document.fr.email2.focus(); // 커서 요청
		} else { // 다른 도메인 선택 시
			document.fr.email2.disabled = true; // 입력창 잠금
		} 
	}
	
	// 6. 취미의 "전체선택" 체크박스 체크 시 취미 항목 모두 체크, 
	//    "전체선택" 해제 시 취미 항목 모두 체크 해제하기
	function checkAll(isChecked) {
		// 전체선택 체크 항목값(isChecked)에 따라 취미 항목 체크 또는 체크 해제
		if(isChecked) { // isChecked == true 와 동일한 조건식
			// 복수개의 동일한 name 값을 갖는 체크박스는 배열로 관리되므로 배열 인덱스 활용
			document.fr.hobby[0].checked = true;
			document.fr.hobby[1].checked = true;
			document.fr.hobby[2].checked = true;
		} else {
			document.fr.hobby[0].checked = false;
			document.fr.hobby[1].checked = false;
			document.fr.hobby[2].checked = false;
		}
	}
	
	function checkForm() {
		/*
		7. submit 버튼 클릭 시 필수 조건들이 만족할 경우에만 다음 페이지로 이동하기(submit())
	    - 아이디 중복 확인 버튼을 통해 아이디 중복 확인을 수행하고(버튼 클릭으로 대체(임시))
	    - 비밀번호와 비밀번호확인 두 개가 일치하고
	    - 주민번호 앞자리와 뒷자리가 모두 정상적으로 입력되고
	    - 이메일이 정상적으로 입력됐을 경우
	    => 위의 모든 조건이 만족할 경우 true 리턴하여 submit 작업을 수행하고
	       아니면, false 를 리턴하여 submit 작업을 취소
	    => 단, 현재 함수에서 다른 작업의 수행여부를 확인하려면
	       해당 작업 수행 후 전역변수를 사용하여 값을 변경해 놓아야함
	    */
	    // 아이디는 중복 확인 결과(중복확인 통과한 아이디)값이 입력되어 있지 않은지 확인하고
	    // 패스워드는 전역변수에 저장된 boolean 타입 값을 확인하고
	    // 주민번호, 이메일은 해당 입력값 자체를 가져와서 확인
	    if(document.fr.id.value == "") { // 아이디 중복확인을 수행하지 않은 경우(또는 중복인 경우)
	    	alert("아이디 중복확인 필수!");
	    	document.fr.id.focus();
	    	// 더 이상 작업이 진행되지 않고, submit 동작을 취소하기 위해 false 값 리턴
	    	return false; // 리턴값이 폼태그의 onsubmit="return checkForm()" 부분으로 전달되어
	    	// false 일 때 return false 가 되어 submit 동작이 취소됨(생략 시 true 리턴됨)
	    } else if(!isCheckConfirmPasswd) {
	    	alert("패스워드 확인 필수!");
	    	document.fr.passwd2.focus();
	    	return false;
	    } else if(document.fr.jumin1.value.length != 6) {
	    	alert("주민번호 앞자리 6자리 필수!");
	    	document.fr.jumin1.focus();
	    	return false;
	    } else if(document.fr.jumin2.value.length != 7) {
	    	alert("주민번호 뒷자리 7자리 필수!");
	    	document.fr.jumin2.focus();
	    	return false;
	    } else if(document.fr.email1.value.length == 0) {
	    	alert("이메일 계정 입력 필수!");
	    	document.fr.email1.focus();
	    	return false;
	    } else if(document.fr.email2.value.length == 0) {
	    	alert("이메일 도메인 입력 필수!");
	    	document.fr.email2.focus();
	    	return false;
	    }
	    
	    // 모든 조건을 통과했을 경우 submit 동작 실행
// 	    return true; // 생략 가능
	}
</script>
</head>
<body>
	<h1>회원 가입</h1>
	<form action="MemberJoinPro.me" name="fr" onsubmit="return checkForm()">
		<table border="1">
			<tr><td>이름</td><td><input type="text" name="name" required="required"></td></tr>
			<tr>
				<td>ID</td>
				<td>
					<input type="text" name="id" placeholder="중복확인 버튼 클릭" readonly="readonly" required="required">
					<input type="button" value="ID중복확인" onclick="checkDuplicateId()">
					<span id="checkIdResult"></span>
				</td>
			</tr>
			<tr>
				<td>비밀번호</td>
				<td>
					<input type="password" name="passwd" onkeyup="checkPasswd(this.value)" placeholder="8 ~ 16글자 사이 입력" required="required">
					<span id="checkPasswdResult"></span>
				</td>
			</tr>
			<tr>
				<td>비밀번호확인</td>
				<td>
					<input type="password" name="passwd2" onkeyup="checkConfirmPasswd(this.value)" required="required">
					<span id="checkConfirmPasswdResult"></span>
				</td>
			</tr>
			<tr>
				<td>E-Mail</td>
				<td>
					<input type="text" name="email1" required="required">@
					<input type="text" name="email2" required="required">
					<select name="emailDomain" onchange="selectDomain(this.value)">
						<option value="">직접입력</option>
						<option value="naver.com">naver.com</option>
						<option value="nate.com">nate.com</option>
						<option value="daum.net">daum.net</option>
					</select>
				</td>
			</tr>
			<tr>
				<td>성별</td>
				<td>
					<input type="radio" name="gender" value="남">남
					<input type="radio" name="gender" value="여">여
				</td>
			</tr>
			<tr>
				<td colspan="2">
					<input type="submit" value="가입">
					<input type="reset" value="초기화">
					<input type="button" value="돌아가기" onclick="history.back()">
				</td>
			</tr>
		</table>
	</form>
</body>
</html>
----------------------------------------check_id.jsp----------------------------------------
<%@ page language="java" contentType="text/html; charset=UTF-8"
	pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<script type="text/javascript">
	// MemberCheckId.me 서블릿 주소 뒤에 id 파라미터나 isDuplicate 파라미터가 있을 경우
	// (http://localhost:8080/MVC_Board/MemberCheckId.me?id=asdfa&isDuplicate=false)
	// 아이디 중복 확인 작업을 끝낸 후에 현재 페이지로 돌아왔다(새로고침)는 의미이다.
	// 따라서, 중복 확인 작업이 끝난 후 돌아왔을 때
	// 1) 아이디 입력창에 검사 완료된 아이디를 표시하고
	// 2) 검사 결과 출력 영역(div 태그)에 검사 결과를 표시
	// 만약, 주소 뒤에 파라미터가 없을 경우 처음 열린 페이지이므로 수행할 작업이 아무것도 없음
	window.onload = function() { // body 영역 로딩 완료 시 수행할 익명 함수 정의
		// body 태그 속성으로 <body onload="check()"> 형식으로 사용 가능
		// 1. URL 파라미터에서 id 파라미터가 존재하는지 확인
		<%if(request.getParameter("id") != null) {%>
			let id = "<%=request.getParameter("id")%>";
			document.fr.id.value = id; // 아이디 입력란에 검사 완료된 아이디 표시
			
			// 2. 검사 결과 값을 boolean 타입으로 변환하여 저장하기
			let isDuplicate = "<%=request.getParameter("isDuplicate")%>";
// 			alert(isDuplicate + ", " + typeof(isDuplicate));

			if(isDuplicate == "true") { // 아이디 중복일 경우
				document.getElementById("checkIdResult").innerHTML = "이미 사용중인 아이디";
				document.getElementById("checkIdResult").style.color = "RED";
			} else { // 아이디가 중복이 아닐 경우
				let btn = "<input type='button' value='아이디 사용' onclick='useId()'>";
				
				document.getElementById("checkIdResult").innerHTML = "사용 가능한 아이디<br>" + btn;
				document.getElementById("checkIdResult").style.color = "GREEN";
			}
			
		<%}%>
	}
	
	function useId() {
		// 부모창(join_form.jsp)의 ID 입력란에 중복 확인 완료된 ID 값 표시
		window.opener.document.fr.id.value = document.fr.id.value;
		
		// 현재 자식창(check_id.jsp) 닫기
		window.close();
	}
	
	function checkId() {
		// 입력된 아이디 가져와서 id 변수에 저장
		let id = document.fr.id.value;

		if (id.length >= 4 && id.length <= 8) { // 아이디 규칙이 적합할 경우
			// 아이디 중복 확인을 위한 비즈니스 로직 처리를 위해 CheckIdDuplicate.me 요청
			// => 요청 URL 에 입력된 id 값을 id 라는 파라미터명으로 전달
			location.href = "CheckIdDuplicate.me?id=" + id;
			// http://localhost:8080/MVC_Board/CheckIdDuplicate.me?id=admin
		} else { // 아이디 규칙이 적합하지 않을 경우
			alert("4~8글자만 사용 가능합니다.");
			document.getElementById("checkIdResult").innerHTML = "";
			document.fr.id.select(); // 입력항목 포커스 요청 및 블럭지정
		}
	}
	// 			document.getElementById("checkIdResult").innerHTML = "중복확인완료";
</script>
</head>
<body>
	<h1>ID 중복 체크</h1>
	<form action="" name="fr">
		<input type="text" name="id" placeholder="4 ~ 8글자 문자, 숫자 조합 필수!">
		<input type="button" value="중복확인" onclick="checkId()"><br>
		<div id="checkIdResult"></div>
	</form>
</body>
</html>
```
- controller 폴더에 MemberFrontController.java 파일 생성
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
import action.BoardReplyFormAction;
import action.BoardReplyProAction;
import action.BoardWriteProAction;
import action.MemberCheckIdDuplicateAction;
import action.MemberJoinProAction;
import vo.ActionForward;

@WebServlet("*.me")
public class MemberFrontController extends HttpServlet {

	protected void doProcess(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {
		System.out.println("MemberFrontController");

		// POST 방식에 대한 한글 처리
		request.setCharacterEncoding("UTF-8");

		// 서블릿 주소 추출
		String command = request.getServletPath();
		System.out.println("command : " + command);

		// Action 클래스의 공통 타입(슈퍼클래스)인 Action 인터페이스 타입 변수 선언
		Action action = null;
		// 포워딩 정보를 관리하는 ActionForward 타입 변수 선언
		ActionForward forward = null;

		if (command.equals("/MemberJoinForm.me")) {
			forward = new ActionForward();
			forward.setPath("./member/join_form.jsp");
			forward.setRedirect(false);
		} else if (command.equals("/MemberCheckId.me")) {
			forward = new ActionForward();
			forward.setPath("./member/check_id.jsp");
			forward.setRedirect(false);
		} else if (command.equals("/CheckIdDuplicate.me")) {
			// 비즈니스 로직 처리를 위해 BoardListAction 클래스의 execute() 메서드 호출
			action = new MemberCheckIdDuplicateAction();
			try {
				forward = action.execute(request, response);
			} catch (Exception e) {
				e.printStackTrace();
			}
		} else if (command.equals("/MemberJoinPro.me")) {
			action = new MemberJoinProAction();
			try {
				forward = action.execute(request, response);
			} catch (Exception e) {
				e.printStackTrace();
			}
		} else if (command.equals("/MemberLoginForm.me")) {
			forward = new ActionForward();
			forward.setPath("./member/login_form.jsp");
			forward.setRedirect(false);
		} else if (command.equals("/MemberLoginPro.me")) {

		}
		// ----------------------------------------------------
		// ActionForward 객체에 저장된 포워딩 정보에 따른 포워딩 작업 수행
		if (forward != null)

		{ // ActionForward 객체가 비어있지 않을 경우
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

- action 폴더에 MemberCheckIdDuplicateAction.java 파일 생성
```java
package action;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import vo.ActionForward;

public class MemberCheckIdDuplicateAction implements Action {

	@Override
	public ActionForward execute(HttpServletRequest request, HttpServletResponse response) throws Exception {
		System.out.println("MemberCheckIdDuplicateAction");
		
		ActionForward forward = null;
		
		// URL 파라미터로 전달받은 id 가져와서 저장
		String id = request.getParameter("id");
		
		// MemberCheckIdDuplicateService 의 checkId() 메서드
		// -> MemberDAO 의 checkId() 메서드를 통해 중복 여부 판별
		// => 파라미터 : id	리턴타입 : boolean(isDuplicate)
		boolean isDuplicate = false;
		
		// ActionForward 객체를 사용하여 MemberCheckId.me 서블릿 주소 요청
		// => URL 파라미터로 검사한 아이디와 검사 결과를 전달
		// => 주소 변경을 위해 Redirect 방식 포워딩
		forward = new ActionForward();
		forward.setPath("MemberCheckId.me?id=" + id + "&isDuplicate=" + isDuplicate);
		forward.setRedirect(true);
		// ex) http://localhost:8080/MVC_Board/MemberCheckId.me?id=admin&isDuplicate=false
		
		
		return forward;
	}

}
```

- vo 폴더의 MemberDTO.java 파일 수정
```java
package vo;

import java.sql.Date;

public class MemberDTO {
	/*
	 * member 테이블 정의
	 * --------------------------------
	   번호(idx) - INT, PK, AI
	   이름(name) - VARCHAR(20), NN
	   아이디(id) - VARCHAR(8), UN, NN
	   패스워드(passwd) - VARCHAR(16), NN
	   E-Mail(email) - VARCHAR(50), UN, NN
	   성별(gender) - VARCHAR(1), NN
	   가입일(date) - DATE, NN
	   --------------------------------
	CREATE TABLE member (
		idx INT PRIMARY KEY AUTO_INCREMENT,
		name VARCHAR(20) NOT NULL,
		id VARCHAR(8) UNIQUE NOT NULL,
		passwd VARCHAR(16) NOT NULL,
		email VARCHAR(50) UNIQUE NOT NULL,
		gender VARCHAR(1) NOT NULL,
		date DATE NOT NULL
	);
	 */
	private int idx;
	private String name;
	private String id;
	private String passwd;
	private String email;
	private String gender;
	private Date date; // java.sql 패키지
	
	public int getIdx() {
		return idx;
	}
	public void setIdx(int idx) {
		this.idx = idx;
	}
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
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
	public String getEmail() {
		return email;
	}
	public void setEmail(String email) {
		this.email = email;
	}
	public String getGender() {
		return gender;
	}
	public void setGender(String gender) {
		this.gender = gender;
	}
	public Date getDate() {
		return date;
	}
	public void setDate(Date date) {
		this.date = date;
	}
	
	@Override
	public String toString() {
		return "MemberDTO [idx=" + idx + ", name=" + name + ", id=" + id + ", passwd=" + passwd + ", email=" + email
				+ ", gender=" + gender + ", date=" + date + "]";
	}
	
	
	
}
```

- dao 폴더에 MemberDAO.java 파일 생성
```java
package dao;

import java.sql.Connection;
import java.sql.Date;
import java.sql.PreparedStatement;
import java.sql.SQLException;

import static db.jdbcUtil.*;
import vo.MemberDTO;

public class MemberDAO {
	// ------------ 싱글톤 디자인 패턴 구현 ---------------
	private static MemberDAO instance = new MemberDAO();
	
	private MemberDAO() {}

	public static MemberDAO getInstance() {
		return instance;
	}
	// -------------------------------------------------------
	// 외부에서 Connection 객체를 전달받아 멤버변수에 저장
	private Connection con;

	public void setConnection(Connection con) {
		this.con = con;
	}
	// -------------------------------------------------------

	public int Join(MemberDTO member) {
		int InsertCount = 0;
		
		PreparedStatement pstmt = null;
		
		try {
			
			String sql ="INSERT INTO member VALUES(?,?,?,?,?,?,now())";
			pstmt = con.prepareStatement(sql);
			pstmt.setInt(1, member.getIdx());
			pstmt.setString(2, member.getName());
			pstmt.setString(3, member.getId());
			pstmt.setString(4, member.getPasswd());
			pstmt.setString(5, member.getEmail());
			pstmt.setString(6, member.getGender());
			
			InsertCount = pstmt.executeUpdate();
		} catch (SQLException e) {
			System.out.println("구문 오류! - Join()");
			e.printStackTrace();
		} finally {
			close(pstmt);
		}

		return InsertCount;
	}
	
	
}
```
