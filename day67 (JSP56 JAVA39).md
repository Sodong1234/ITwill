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

## 이벤트(Event)
- 컴포넌트(버튼 등)에서 사용자에 의해 어떤 상호작용이 일어나는 것
	- ex) 버튼 클릭, 마우스 이동, 키보드 입력, 체크박스 선택 등
- 이벤트가 발생했을 때 어떤 동작을 수행하기 위해서는 대상 컴포넌트와 이벤트를 처리하는 이벤트 리스너를 서로 연결해야함
	- 각 컴포넌트에 따라 서로 다른 리스너가 제공됨
		- ex) 버튼 클릭 이벤트 담당 : ActionListener 사용
	 	- 컴포넌트 객체의 addXXX() 메서드를 호출하여 리스너 객체를 파라미터로 전달하여 연결. 이 때, XXX은 담당 리스너 인터페이스(또는 클래스)이름
		- ex) btn.addActionListener(리스너 객체)

### 이벤트 처리(Event Handling)
- 컴포넌트에 특정 이벤트가 발생했을 때 수행할 동작을 지정하여 처리하는 것
	- XXXListener 인터페이스 또는 XXXAdapter 클래스가 제공됨
- 리스너 객체를 직접 구현하거나, 별도의 핸들러(Handler) 클래스를 정의하여 리스너 인터페이스(또는 어댑터 클래스)를 상속 받아 구현
- 리스너 인터페이스 등은 주로 java.awt.event 패키지 내에 위치함


**이벤트 처리 1단계**
```java
package event_handling;

import java.awt.event.WindowEvent;
import java.awt.event.WindowListener;

import javax.swing.JFrame;

public class Ex1 {

		 /* 
		 * < 이벤트 처리 5단계 >
		 * 1. 리스너 인터페이스를 구현하는 구현체 클래스(핸들러)를 정의
		 * 	  => 이벤트 발생 시 수행할 동작을 구현체 내부의 메서드에 기술하고
		 * 	     리스너 연결 시 구현체 객체 생성하여 addXXXListener() 메서드 파라미터로 전달
		 */
		public Ex1() {
			showFrame();
		}
		
		public void showFrame() {
			JFrame f = new JFrame("이벤트 처리-1");
			f.setBounds(600, 400, 300, 200);
			
			// 이벤트 처리 1단계. 리스너를 구현한 핸들러 클래스 정의하여 사용
			// 현재 프레임(JFrame 객체)에 WindowListener 구현체 연결
			// 1. 핸들러 객체(WindowListener 구현체) 생성
			MyWindowListener listener = new MyWindowListener();
			
			// 2. 이벤트 연결을 위한 대상 객체(JFrame)의 addXXXListener() 메서드를 호출하여
			//	  파라미터로 핸들어 객체를 전달 => 대상과 이벤트 리스너 연결이 완료됨
			f.addWindowListener(listener);
			
			f.setVisible(true);
			
			//=========================================
			// JFrame 객체 생성 후 MyWindowListener 리스너 연결
			JFrame f2 = new JFrame();
			f2.setBounds(800, 400, 300, 300);
			
//			MyWindowListener listener2 = new MyWindowListener();
//			f2.addWindowListener(listener2);
			// => 만약, 처리할 이벤트가 동일할 경우 새 객체를 생성하지 않고
			//    기존에 생성한 리스너 객체를 그대로 재사용 가능
			f2.addWindowListener(listener);
			
			f2.setVisible(true);
		}
		
		public static void main(String[] args) {
			new Ex1();
		}
		
	}

// 이벤트 처리 1단계
// 이벤트 처리를 위해 리스너 인터페이스를 구현하는 핸들러 클래스 별도로 정의
// 윈도우(프레임)에 대한 이벤트 처리 담당 리스너 : WindowListener 인터페이스
class MyWindowListener implements WindowListener {

	// WindowListener 인터페이스를 구현하는 MyWindowListener 클래스 정의
	// => 추상메서드 오버라이딩 필수!
	// => 각 메서드들은 이벤트 발생했을 때 해당 이벤트에 맞게 자동으로 호출됨
	//	  따라서, 이벤트 발생 시 처리할 동작을 기술
	
	@Override
	public void windowOpened(WindowEvent e) {
		// 맨 처음 프로그램이 실행될 때 프레임이 생성되어 표시되면 호출되는 메서드(1회)
		System.out.println("windowOpened");
	}

	@Override
	public void windowClosing(WindowEvent e) {
		System.out.println("windowClosing");
		System.exit(0); // 프로그램 종료(0 : 정상 종료, 0 이외의 값 : 비정상 종료)
	}

	@Override
	public void windowClosed(WindowEvent e) {
		System.out.println("windowClosed");
	}

	@Override
	public void windowIconified(WindowEvent e) {
		// 프레임의 최소화 버튼 클릭 시 호출되는 메서드
		System.out.println("windowIconified");
	}

	@Override
	public void windowDeiconified(WindowEvent e) {
		// 프레임의 최소화가 해제될 때 호출되는 메서드
		System.out.println("windowDeiconified");
	}

	@Override
	public void windowActivated(WindowEvent e) {
		// 프레임이 사용자와 상호작용이 가능한 상태가 될 때 호출되는 메서드
		System.out.println("windowActivated");
	}

	@Override
	public void windowDeactivated(WindowEvent e) {
		// 프레임이 사용자와 상호작용이 불가능한 상태가 될 때 호출되는 메서드
		// ex) 다른 프로그램에 가려진 상태가 될 때
		System.out.println("windowDeactivated");
	}
	
}
```

- 연습문제
```java
package event_handling;

import javax.swing.JButton;
import javax.swing.JFrame;

public class Test1 {

	public Test1() {
		showFrame();
	}
	
	public void showFrame() {
		/*
		 * 1. 300, 200 프레임 생성
		 * 2. "버튼" 이라는 텍스트를 갖는 버튼 객체 부착(CENTER영역)
		 * 3. 클릭 이벤트 처리를 위해 ActionListener 인터페이스를 구현 핸들어(MyActionListener) 정의
		 * 	  => 추상 메서드 오버라이딩
		 * 	  => 메서드 내에 "버튼 클릭" 문자열 출력하는 코드 기술
		 * 4. ActionListener 인터페이스 구현체(MyActionListener) 객체 생성
		 * 5. 버튼 객체의 addXXXListener() 메서드 호출하여 MyActionListener 객체 전달
		 * 
		 */
		
		JFrame f = new JFrame("이벤트처리 연습-1");
		f.setBounds(800, 400, 300, 200);
		
		// JButton 객체 생성 및 JFrame 객체에 부착
		JButton btn = new JButton("버튼");
		f.add(btn);
		
		// JButton 객체의 addXXXListener() 메서드 호출하여 연결
		// 1. 리스너 구현체 객체 생성
		MyActionListener listener = new MyActionListener();
		
		// 2. JButton 객체의 addActionListener() 메서드를 호출하여 구현체 객체 전달
		btn.addActionListener(listener);
		
		f.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
		f.setVisible(true);
		
		// ===========================================================
		
		JFrame f2 = new JFrame("버튼연습");
		f2.setBounds(300, 300, 300, 300);
		
		JButton btn2 = new JButton("버튼2");
		f2.add(btn2);
		
		btn2.addActionListener(listener);
	}
	
	public static void main(String[] args) {
		new Test1();
	}

}

// JButton 컴포넌트에 대한 이벤트 처리 담당 리스너 : ActionListener 인터페이스
class MyActionListener implements ActionListener {

	@Override
	public void actionPerformed(ActionEvent e) {
		// 버튼 클릭 시 자동으로 호출되는 메서드
		// => "버튼 클릭" 메세지 출력
		System.out.println("버튼 클릭!");
	}
	
}
```

**이벤트 처리 2단계**
```java
package event_handling;

import java.awt.event.WindowAdapter;
import java.awt.event.WindowEvent;

import javax.swing.JFrame;

public class Ex2 {
	/*
	 * < 이벤트 처리 5단계 >
	 * 2. 리스너 인터페이스 대신 어댑터 클래스 사용
	 * 	  - 추상메서드가 2개 이상인 인터페이스를 구현할 경우
	 * 		불필요한 메서드까지 구현해야하므로 코드가 길어짐
	 * 	  - 리스너 인터페이스를 미리 구현해놓은 어댑터 클래스를 상속받는 핸들러에서 
	 * 		원하는 메서드만 선택하여 오버라이딩이 가능하므로 코드가 간결해짐
	 * 	  - XXXListener 인터페이스 대응하는 XXXAdapter 클래스가 제공됨
	 * 	    단, 추상메서드가 하나뿐인 인터페이스는 어댑터클래스가 제공되지 않음
	 * 		ex) ActionListener 인터페이스의 추상메서드가 1개 이므로 ActionAdapter 제공 X
	 * 
	 */
	public Ex2() {
		showFrame();
	}
	
	public void showFrame() {
		JFrame f = new JFrame("이벤트처리-2");
		f.setBounds(600, 400, 300, 300);
		
		// 이벤트처리 2단계. 어댑터 클래스를 구현한 핸들러 클래스 정의하여 사용
		// 현재 프레임(JFrame 객체)에 WindowAdapter 구현체 연결
		// 1. 핸들러 객체(WindowAdapter 구현체) 생성
		MyWindowAdapter listener = new MyWindowAdapter();
		
		// 2. 이벤트 연결을 위한 대상 객체(JFrame)의 addXXXListener() 메서드를 호출하여
		//	  파라미터로 핸들러 객체를 전달 => 대상과 이벤트 리스너 연결이 완료됨
		f.addWindowListener(listener);
		
		
		f.setVisible(true);
	}
	
	public static void main(String[] args) {
		new Ex2();
	}

}

// 이벤트 처리 2단계
// 이벤트 처리를 위한 리스너 인터페이스 대신 어댑터 클래스를 구현하는 핸들러 클래스 별도로 정의
// 윈도우(프레임)에 대한 이벤트 처리 담당 리스너 : WindowListener 인터페이스
// => 이에 대한 어댑터클래스 : WindowAdapter 클래스 
class MyWindowAdapter extends WindowAdapter {
	
	// 상속받은 슈퍼클래스의 메서드 중 필요한 메서드만 선택적 오버라이딩
	// => windowClosing() 메서드 오버라이딩
	@Override
	public void windowClosing(WindowEvent e) {
		System.out.println("windowClosing");
		System.exit(0);
	}
}

```
```java
package event_handling;

public class Test2 {

	public Test2() {
		showFrame();
	}
	
	public void showFrame() {
		// 이벤트 처리 2단계.
		// ActionListener -> ActionAdapter
		// ActionListener는 추상메서드가 하나뿐이므로
		// 별도의 어댑터 클래스 (ActionAdapter)가 제공되지 않는다!
	}
	public static void main(String[] args) {
		new Test2();
	}

}

//class MyActionAdapter extends ActionAdapter {
//	
//}
```

**이벤트 처리 3단계**
```java
package event_handling;

import java.awt.event.WindowAdapter;
import java.awt.event.WindowEvent;

import javax.swing.JFrame;

public class Ex3 {

	public Ex3() {
		showFrame();
	}
	
	public void showFrame() {
		JFrame f = new JFrame("이벤트처리-3");
		f.setBounds(600, 400, 300, 300);
		
		/*
		 * < 이벤트 처리 5단계 >
		 * 3단계. 내부 클래스(Inner Class) 형태로 정의
		 * 리스너 인터페이스 또는 어댑터 클래스를 구현 시 내부클래스 형태로 구현하여 사용
		 * => 이벤트 리스너 구현체(핸들러 클래스)는 보통 GUI 클래스에서만 사용됨(= 전용클래스)
		 * 	  따라서, 별도의 클래스로 외부에 정의하기보다는 내부클래스 형태로 정의해서 사용
		 * 	  GUI 클래스 내부에 핸들러 클래스를 정의하면 내부클래스가 됨
		 * => 외부에 정의하는 방법과 클래스 모양은 동일하나 클래스 정의 위치만 달라짐
		 * 
		 * [ 위치에 따른 차이점 ]
		 * 1) 멤버 레벨에 정의하는 멤버 내부클래스 (인스턴스 내부 클래스)
		 * 	  => 멤버 변수의 성격과 동일한 클래스가 됨 (= 접근 범위가 멤버변수와 동일해짐)
		 * 2) 메서드 내부에 정의하는 로컬 내부 클래스
		 * 	  => 로컬 변수의 성격과 동일한 클래스가 됨 (= 접근 범위가 로컬변수와 동일해짐)
		 * 
		 */
		
		// 멤버레벨의 WindowAdapter
//		MyMemberWindowAdapter listener = new MyMemberWindowAdapter();
		
		// 로컬 내부 클래스 형태로 정의
		// => 로컬 변수와 동일한 범위에서만 접근 가능
		class MyLocalWindowAdapter extends WindowAdapter {

			@Override
			public void windowClosing(WindowEvent e) {
				System.out.println("windowClosing");
				System.exit(0);
			}
			
		}
		MyLocalWindowAdapter listener = new MyLocalWindowAdapter();
		
		f.addWindowListener(listener);
		f.setVisible(true);
		
		
	}
	
	public void showFrame2() {
		// 메서드가 달라지면 로컬 내부클래스는 접근이 불가능하므로
		// 여러 메서드에서 하나의 리스너를 공유하려면 멤버 내부클래스 형태로 정의 해야한다!
//		MyLocalWindowAdapter listener = new MyLocalWindowAdapter();		// 접근 불가!
		MyMemberWindowAdapter listener = new MyMemberWindowAdapter();	// 접근 가능!
		

		
	}
	
	public static void main(String[] args) {
		new Ex3();
	}

	// 이벤트 처리 3단계. 내부클래스(Inner Class) 형태로 정의
	// 멤버 내부클래스 형태로 정의
	// => 인스턴스 멤버와 동일한 위치에 정의하므로 인스턴스 내부 클래스라고도 함
	// => 인스턴스 내의 여러 메서드에서 공유 가능
	class MyMemberWindowAdapter extends WindowAdapter{

		@Override
		public void windowClosing(WindowEvent e) {
			System.out.println("windowClosing");;
			System.exit(0);
		}
		
	}
}

```

- 연습문제
```java
package event_handling;

import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;

import javax.swing.JButton;
import javax.swing.JFrame;

public class Test3 {

	public Test3() {
		showFrame();
	}
	
	public void showFrame() {
		JFrame f = new JFrame("이벤트처리 연습-3");
		f. setBounds(800, 400, 300, 300);
		
		// JButton 객체 생성 및 JFrame 객체에 부착
		JButton btn = new JButton("버튼");
		f.add(btn);
		
		// 이벤트 처리3. 내부 클래스 형태로 이벤트 처리
		// 1. 리스너 구현체 객체 생성
		MyMemberActionListener listener = new MyMemberActionListener();
		// 2. JButton 객체의 addACtionListener() 메서드를 호출하여 구현체 객체 전달
		btn.addActionListener(listener);
		
		f.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
		f.setVisible(true);
	}
	
	public static void main(String[] args) {
		new Test3();
	}

	
	// 멤버레벨에 JButton 컴포넌트에 대한
	// 이벤트 처리 담당 리스너 : ActionListener 인터페이스
	class MyMemberActionListener implements ActionListener {

		@Override
		public void actionPerformed(ActionEvent e) {
			System.out.println("버튼 클릭!");
		}
		
		
	}
	
	// 람다식 X
//	class MyMemberActionListener e -> System.out.println("버튼 클릭!");
	
}


```







