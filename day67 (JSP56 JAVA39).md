# [오전수업] JSP 56차

- controller 폴더의 MemberFrontController.java 파일 수정
```java
package action;

import java.io.PrintWriter;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;

import svc.MemberLoginProService;
import vo.ActionForward;
import vo.MemberDTO;

public class MemberLoginProAction implements Action {

	@Override
	public ActionForward execute(HttpServletRequest request, HttpServletResponse response) throws Exception {
		System.out.println("MemberLoginProAction");
		ActionForward forward = null;
		
		// 폼 파라미터 가져와서 MemberDTO 객체에 저장
		MemberDTO member = new MemberDTO();
		member.setId(request.getParameter("id"));
		member.setPasswd(request.getParameter("passwd"));
//		System.out.println(member.toString());
		
		// MemberLoginProService 클래스의 loginMember() 메서드를 호출하여 로그인 판별 요청
		// => 파라미터 : MemberDTO 객체(member)    리턴타입 : boolean(isMember)
		MemberLoginProService service = new MemberLoginProService();
		boolean isMember = service.loginMember(member);
		
		// 로그인 판별 작업 요청 결과에 따른 판별 작업 수행
		if(!isMember) { // 로그인 실패
			response.setContentType("text/html; charset=UTF-8");
			PrintWriter out = response.getWriter();
			out.println("<script>");
			out.println("alert('아이디 또는 패스워드 틀림!')");
			out.println("history.back()");
			out.println("</script>");
		} else { // 등록 성공
			// request 객체의 getSession() 메서드를 호출하여 HttpSesson 객체를 얻어오기
			HttpSession session = request.getSession();
			// HttpSession 객체의 setAttribute() 메서드를 호출하여 세션 아이디값 저장(속성명 : sId)
			session.setAttribute("sId", request.getParameter("id"));
			
			// ActionForward 객체를 통해 메인페이지 포워딩 설정
			forward = new ActionForward();
			forward.setPath("./");
			forward.setRedirect(true);
		}
		
		return forward;
	}

}

```

- action 폴더의 MemberCheckIdDuplicateAction, MemberJoinProAction, MemberLoginProAction, MemberLogoutAction 파일 수정 및 생성

```java
------------------------------------MemberCheckIdDuplicateAction.java------------------------------------
package action;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import svc.MemberCheckIdDuplicateService;
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
		MemberCheckIdDuplicateService service = new MemberCheckIdDuplicateService();
		boolean isDuplicate = service.isDuplicateId(id);
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

------------------------------------MemberJoinProAction.java------------------------------------
package action;

import java.io.PrintWriter;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import svc.BoardWriteProService;
import svc.MemberJoinProSerivce;
import vo.ActionForward;
import vo.BoardDTO;
import vo.MemberDTO;

public class MemberJoinProAction implements Action {

	@Override
	public ActionForward execute(HttpServletRequest request, HttpServletResponse response) throws Exception {
		System.out.println("MemberJoinProAction");
		
		ActionForward forward = new ActionForward();
		
		MemberDTO member = new MemberDTO();
		member.setName(request.getParameter("name"));
		member.setId(request.getParameter("id"));
		member.setPasswd(request.getParameter("passwd"));
		member.setEmail(request.getParameter("email1") + "@" + request.getParameter("email2"));
		member.setGender(request.getParameter("gender"));
		
		
		
		// MemberJoinProService 클래스의 registMember() 메서드를 호출하여 회원 등록 요청
		// => 파라미터 : MemberDTO 객체(member)		리턴타입 : boolean(isRegistSuccess)
		MemberJoinProSerivce service = new MemberJoinProSerivce();
		boolean isRegistSuccess = service.registMember(member);
		
		// 등록 작업 요청 결과에 따른 판별 작업 수행
		if(!isRegistSuccess) { // 등록 실패
			response.setContentType("text/html; charset=UTF-8");
			PrintWriter out = response.getWriter();
			out.println("<script>");
			out.println("alert('회원 가입 실패!')");
			out.println("history.back()");
			out.println("</script>");
		} else { // 등록 성공
			// ACtionForward 객체를 통해 메인페이지 포워딩 설정
			forward = new ActionForward();
			forward.setPath("./");
			forward.setRedirect(true);
		}
		
		
		return forward;
	}

}

------------------------------------MemberLoginProAction.java------------------------------------
package action;

import java.io.PrintWriter;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;

import svc.MemberLoginProService;
import vo.ActionForward;
import vo.MemberDTO;

public class MemberLoginProAction implements Action {

	@Override
	public ActionForward execute(HttpServletRequest request, HttpServletResponse response) throws Exception {
		System.out.println("MemberLoginProAction");
		ActionForward forward = null;
		
		// 폼 파라미터 가져와서 MemberDTO 객체에 저장
		MemberDTO member = new MemberDTO();
		member.setId(request.getParameter("id"));
		member.setPasswd(request.getParameter("passwd"));
//		System.out.println(member.toString());
		
		// MemberLoginProService 클래스의 loginMember() 메서드를 호출하여 로그인 판별 요청
		// => 파라미터 : MemberDTO 객체(member)    리턴타입 : boolean(isMember)
		MemberLoginProService service = new MemberLoginProService();
		boolean isMember = service.loginMember(member);
		
		// 로그인 판별 작업 요청 결과에 따른 판별 작업 수행
		if(!isMember) { // 로그인 실패
			response.setContentType("text/html; charset=UTF-8");
			PrintWriter out = response.getWriter();
			out.println("<script>");
			out.println("alert('아이디 또는 패스워드 틀림!')");
			out.println("history.back()");
			out.println("</script>");
		} else { // 등록 성공
			// request 객체의 getSession() 메서드를 호출하여 HttpSesson 객체를 얻어오기
			HttpSession session = request.getSession();
			// HttpSession 객체의 setAttribute() 메서드를 호출하여 세션 아이디값 저장(속성명 : sId)
			session.setAttribute("sId", request.getParameter("id"));
			
			// ActionForward 객체를 통해 메인페이지 포워딩 설정
			forward = new ActionForward();
			forward.setPath("./");
			forward.setRedirect(true);
		}
		
		return forward;
	}

}

------------------------------------MemberLogoutAction.java------------------------------------
package action;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;

import vo.ActionForward;

public class MemberLogoutAction implements Action {

	@Override
	public ActionForward execute(HttpServletRequest request, HttpServletResponse response) throws Exception {
		System.out.println("MemberLogoutAction");
		ActionForward forward = null;
		
		// 세션 객체 얻어오기
		HttpSession session = request.getSession();
		
		
		// 세션 초기화
		session.invalidate();
		
		forward = new ActionForward();
		forward.setPath("./");
		forward.setRedirect(true);
		
		return forward;
	}

}

```
- svc 폴더의 MemberCheckIdDuplicateService, MemberJoinProService, MemberLoginProService 파일 생성 및 수정
```java
------------------------------------MemberCheckIdDuplicateService.java------------------------------------
package svc;

import static db.jdbcUtil.close;
import static db.jdbcUtil.getConnection;

import java.sql.Connection;

import dao.MemberDAO;

public class MemberCheckIdDuplicateService {
	// 아이디 중복 확인을 위한 패스워드 조회 작업을 요청하는 isDuplicateId() 메서드 정의
	public boolean isDuplicateId(String id) {
		boolean isDuplicate = false;
		
		Connection con = getConnection();
		
		MemberDAO memberDAO = MemberDAO.getInstance();
		
		memberDAO.setConnection(con);
		
		// MemberDAO 의 isDuplicateId() 메서드를 호출하여 아이디 중복 판별
		// => 파라미터 : 아이디(id)
		//    리턴타입 : boolean(isDuplicate)
		isDuplicate = memberDAO.isDuplicateId(id);
		
		close(con);
		
		return isDuplicate;
	}
}


------------------------------------MemberJoinProService.java------------------------------------
package svc;

import java.sql.Connection;

import dao.MemberDAO;
import static db.jdbcUtil.*;
import vo.MemberDTO;

public class MemberJoinProSerivce {
	// 회원 등록 작업 요청을 위한 registMember() 메서드 ㅓㅈㅇ의
	public boolean registMember(MemberDTO member) {
		System.out.println("MemberJoinProService - registArticle()");

		boolean isRegistSuccess = false;

		Connection con = getConnection();

		MemberDAO memberDAO = MemberDAO.getInstance();

		memberDAO.setConnection(con);

		// MemberDAO 의 insertMember() 메서드를 호출하여 회원 등록 작업 수행
		// => 파라미터 : MemberDTO 객체(member)		리턴타입 : int(insertCount)
		int insertCount = memberDAO.insertMember(member);

		
		if (insertCount > 0) {
			commit(con);
			isRegistSuccess = true;
		} else {
			rollback(con);
		}

		close(con);

		return isRegistSuccess;
	}
}
------------------------------------MemberLoginProService.java------------------------------------
package svc;

import java.sql.Connection;

import dao.MemberDAO;
import vo.MemberDTO;

import static db.jdbcUtil.*; 

public class MemberLoginProService {

	public boolean loginMember(MemberDTO board) {
		System.out.println("MemberLoginProService");
		boolean isLoginWriter = false;
		
		Connection con = getConnection();
		MemberDAO memberDAO = MemberDAO.getInstance();
		memberDAO.setConnection(con);
		
		isLoginWriter = memberDAO.isMember(board);
		
		close(con);
		
		return isLoginWriter;
	}


}

```

- join_form.jsp, index.jsp 파일 수정

```jsp
------------------------------------join_form.jsp------------------------------------
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
			document.fr.email2.readonly = false; // 입력창 잠금 해제
			document.fr.email2.focus(); // 커서 요청
		} else { // 다른 도메인 선택 시
			document.fr.email2.readonly = true; // 입력창 잠금
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
	<form action="MemberJoinPro.me" name="fr" method="POST" onsubmit="return checkForm()">
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

------------------------------------index.jsp------------------------------------
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%
// 세션 객체에 저장된 세션 아이디("sId") 가져와서 변수에 저장
String sId = (String)session.getAttribute("sId");
%>   
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<script type="text/javascript">
	function confirmLogout() {
		if(confirm("로그아웃 하시겠습니까?")) {
			// MemberLogout.me 서블릿 주소 요청
			location.href = "./MemberLogout.me";
		}
	}
</script>
</head>
<body>
	<header>
		<div align="right">
			<!-- 세션 아이디가 null 또는 ""일 경우 로그인, 회원가입 링크 표시 -->
			<%if(sId == null || sId.equals(""))  { %>
				<h5><a href="./MemberLoginForm.me">로그인</a> | <a href="./MemberJoinForm.me">회원가입</a></h5>
			<%} else { %>
				<h5>
					<a href="./MemberInfo.me"><%=sId %></a>님 | 
					<a href="javascript:void(0)" onclick="confirmLogout()">로그아웃</a>
					<!--
					하이퍼링크 클릭 시 자바스크립트를 실행해야할 경우
					href 속성에 # 또는 javascript:void(0) 를 지정하면 페이지 갱신 없이 실행됨
					-->
<!-- 						<a href="#" onclick="confirmLogout()">로그아웃</a> -->
				</h5>
			<%} %>
		</div>
	</header>
	<h1>MVC_Board 메인페이지</h1>
	<h3><a href="./BoardWriteForm.bo">글쓰기</a></h3>
	<h3><a href="./BoardList.bo">글목록</a></h3>
</body>
</html>
```

- dao 폴더의 emberDAO.java 파일 수정

```java
package dao;

import java.sql.Connection;
import java.sql.Date;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;
import java.util.ArrayList;

import static db.jdbcUtil.*;

import vo.BoardDTO;
import vo.MemberDTO;

public class MemberDAO {
	// ------------ 싱글톤 디자인 패턴 구현 ---------------
	private static MemberDAO instance = new MemberDAO();

	private MemberDAO() {
	}

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

	public int insertMember(MemberDTO member) {
		int InsertCount = 0;

		PreparedStatement pstmt = null;

		try {
			String sql = "INSERT INTO member VALUES(null,?,?,?,?,?,now())";
			pstmt = con.prepareStatement(sql);
			pstmt.setString(1, member.getName());
			pstmt.setString(2, member.getId());
			pstmt.setString(3, member.getPasswd());
			pstmt.setString(4, member.getEmail());
			pstmt.setString(5, member.getGender());
			InsertCount = pstmt.executeUpdate();

		} catch (SQLException e) {
			System.out.println("구문 오류! - insertMember()");
			e.printStackTrace();
		} finally {
			close(pstmt);
		}

		return InsertCount;
	}

	public boolean isDuplicateId(String id) {
		boolean isDuplicate = false; // true : 중복, false : 중복 아님

		PreparedStatement pstmt = null;
		ResultSet rs = null;

		try {
			String sql = "SELECT * FROM member WHERE id=?";
			pstmt = con.prepareStatement(sql);
			pstmt.setString(1, id);
			rs = pstmt.executeQuery();

			if (rs.next()) { // 입력받은 아이디가 존재할 경우(= 중복)
				isDuplicate = true;
			}
		} catch (SQLException e) {
			System.out.println("isDuplicateId() 메서드 오류!");
			e.printStackTrace();
		} finally {
			close(rs);
			close(pstmt);
		}

		return isDuplicate;
	}

	// 로그인 판별 작업 수행을 위한 isMember() 메서드 정의
	public boolean isMember(MemberDTO member) {
		boolean isMember = false;

		PreparedStatement pstmt = null;
		ResultSet rs = null;

		try {
//			// Statement 객체는 Connection 객체와 연결 시 SQL 구문을 미리 전달하지 않음
//			Statement stmt = con.createStatement();
//			// SQL 구문 작성 시 파라미터(?) 사용이 불가능하므로 데이터는 문자열 결합으로 전달함
//			// => 데이터 부분에 SQL 구문을 전달하면 구문으로 취급되어 SQL 삽입 공격이 가능해진다
//			String sql = "SELECT * FROM member "
//					+ "WHERE id='" + member.getId() + "' AND passwd='" + member.getPasswd() + "'";
//			rs = stmt.executeQuery(sql);

			// PreparedStatement 객체는 Connection 객체와 연결 시 SQL 구문을 미리 전달하여
			// 컴파일만 해 놓고 차후에 데이터를 전달하여 결합(Bind) 후 실행하는 방식을 사용
			// => 따라서, 데이터 부분에 SQL 구문을 전달하더라도 구문으로 취급되지 않고 데이터로만 취급됨
			String sql = "SELECT * FROM member WHERE id=? AND passwd=?";
			pstmt = con.prepareStatement(sql);
			// => 이 시점에서 이미 실행될 SQL 구문의 형태는 확정됨(컴파일 완료)
			// => setXXX() 메서드는 단순히 데이터 결합 역할만 수행함(SQL 구문으로 동작하지 못함)
			pstmt.setString(1, member.getId());
			pstmt.setString(2, member.getPasswd());
			rs = pstmt.executeQuery();

			if (rs.next()) { // 로그인 성공 시
				isMember = true;
			}
		} catch (SQLException e) {
			System.out.println("isMember() 메서드 오류!");
			e.printStackTrace();
		} finally {
			close(rs);
			close(pstmt);
		}

		return isMember;
	}

}
```
---

# [오후수업] JAVA 39차
