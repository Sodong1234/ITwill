# [오전수업] JAVA 41차

> 테스트 실시

---

# [오후수업] JSP 58차

> - JSP 30차에서 사용한 jsp11_board 패키지의 JdbcUtil, MemberDAO, MemberDTO 를 기본패키지에 복사하고, ajax 패키지에 다시 옮겨넣기 실시
> - jsp11_board 폴더의 join_form.jsp 복사 후 파일명을 test4_join_form.jsp로 변경 실시
> - 개인프로젝트에서 사용한 check_id, check_id_pro, join.jsp 복사 실시, check_id_pro를 test4_chec_id_pro로 이름 변경 실시


```jsp
-----------------------------------test3.jsp-----------------------------------
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<script src="../js/jquery-3.6.0.js"></script>
<script type="text/javascript">
	$(function() {
// 		$("#btnOk").on("click", function() {
// 			// 폼 파라미터를 test2_result.jsp 로 전송
// 			let sendData = $("form").serialize();
			
// 			$.ajax({
// 				type: "GET",
// 				url: "test3_result.jsp",
// 				data: sendData,
// 				dataType: "text",
// 				success: function(msg) {
// 					$("#resultArea").html(msg);
// 				}
// 			});
// 		});

		// 패스워드 입력창에서 커서가 빠져나가면 로그인 작업 요청
		$("#passwd").on("blur", function() {
			// 폼 파라미터를 test3_result.jsp 로 전송
			let sendData = $("form").serialize();
			
			$.ajax({
				type: "GET",
				url: "test3_result.jsp",
				data: sendData,
				dataType: "text",
				success: function(msg) { // 요청에 대한 처리 성공 시
					$("#resultArea").html(msg);
				},
				error: function(xhr, textStatus, errorThrown) { // 요청에 대한 처리 실패 시(에러)
					$("#resultArea").html(
							"xhr = " + xhr + "<br>" 
							+ "textStatus = " + textStatus + "<br>" 
							+ "errorThrown = " + errorThrown);
				}
			});
		});
	});
</script>
</head>
<body>
	<h1>AJAX - test3.jsp</h1>
	<form>
		<table border="1">
			<tr>
				<td>아이디</td><td><input type="text" name="id" id="id"></td>
			</tr>
			<tr>
				<td>패스워드</td><td><input type="password" name="passwd" id="passwd"></td>
			</tr>
			<tr>
				<td colspan="2"><input type="button" value="확인" id="btnOk">
			</tr>
		</table>
	</form>
	<div id="resultArea"></div>
</body>
</html>
-----------------------------------test3_result.jsp-----------------------------------
<%@page import="ajax.MemberDAO"%>
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<h1>test3_result.jsp</h1>
	<!-- test3.jsp 페이지로부터 AJAX 를 통해 전달받은 파라미터(id, passwd)를 사용하여 로그인 -->
	<!-- MemberDAO 클래스의 login() 메서드를 통해 로그인 판별 작업 요청 -->
	<!-- 리턴값 판별 : true 이면 "로그인 성공" 출력, 아니면 "로그인 실패!" 출력 -->
	<%
	String id = request.getParameter("id");
	String passwd = request.getParameter("passwd");
	
	// MemberDAO 인스턴스 생성
	MemberDAO memberDAO = new MemberDAO();
	boolean isLoginSuccess = memberDAO.login(id, passwd);
	
	if(isLoginSuccess) {
		out.println("<h3>로그인 성공! " + id + " 님, 환영합니다!</h3>");
	} else {
		out.println("<h3>로그인 실패!");		
	}
	%>
	
	
</body>
</html>

```

```jsp
-----------------------------------join.jsp-----------------------------------
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>member/join.jsp</title>
<link href="../css/default.css" rel="stylesheet" type="text/css">
<link href="../css/subpage.css" rel="stylesheet" type="text/css">
<script type="text/javascript">
	// 아이디 중복확인 여부와 패스워드 검사, 패스워드 확인 결과를 저장할 전역변수 선언
	var checkIdResult = false, checkPassResult = false, checkRetypePassResult = false
	
	function checkPass(pass) {
		// 패스워드 검사를 위한 정규표현식 패턴 작성 및 검사 결과에 따른 변수값 변경
		var spanElem = document.getElementById("checkPassResult");
		
		// 정규표현식 패턴 정의
		var lengthRegex = /^[A-Za-z0-9!@#$%]{8,16}$/; // 길이 및 사용 가능 문자 규칙
		var engUpperRegex = /[A-Z]/; // 대문자 규칙
		var engLowerRegex = /[a-z]/; // 소문자 규칙
		var numRegex = /[0-9]/; // 숫자 규칙
		var specRegex = /[!@#$%]/; // 특수문자 규칙
		
		var count = 0; // 각 규칙별 검사 결과를 카운팅 할 변수
		
		if(lengthRegex.exec(pass)) { // 패스워드 길이 및 사용 가능 문자 규칙 통과 시
// 			spanElem.innerHTML = "사용 가능한 패스워드";
// 			spanElem.style.color = "GREEN";
// 			checkPassResult = true;

			// 각 규칙별 검사 후 해당 항목이 포함되어 있을 경우 카운트 증가
			if(engUpperRegex.exec(pass)) count++;
			if(engLowerRegex.exec(pass)) count++;
			if(numRegex.exec(pass)) count++;
			if(specRegex.exec(pass)) count++;
			
			switch (count) {
				case 4 : 
					spanElem.innerHTML = "사용 가능한 패스워드(안전)";
					spanElem.style.color = "GREEN";
					checkPassResult = true;
					break;
				case 3 : 
					spanElem.innerHTML = "사용 가능한 패스워드(보통)";
					spanElem.style.color = "YELLOW";
					checkPassResult = true;
					break;
				case 2 : 
					spanElem.innerHTML = "사용 가능한 패스워드(위험)";
					spanElem.style.color = "ORANGE";
					checkPassResult = true;
					break;
				default:
					spanElem.innerHTML = "영문자,숫자,특수문자 중 2가지 이상 조합 필수!";
					spanElem.style.color = "RED";
					checkPassResult = false;
			}
			
		} else {
// 			spanElem.innerHTML = "사용 불가능한 패스워드";
			spanElem.innerHTML = "영문자,숫자,특수문자 조합 8 ~ 16자리 필수";
			spanElem.style.color = "RED";
			checkPassResult = false;
		}
		
	}

	function checkRetypePass(pass2) {
		/*
		함수에서 pass 와 pass2 의 항목 비교하여 일치하면 "패스워드 일치"(초록색) 표시하고
		아니면 "패스워드 불일치"(빨간색) 표시
		=> 패스워드 일치 시 checkRetypePassResult 를 true, 아니면 false 로 변경
		*/
		var pass = document.fr.pass.value;
		var spanElem = document.getElementById("checkRetypePassResult");
		
		if(pass == pass2) {
			spanElem.innerHTML = "패스워드 일치";
			spanElem.style.color = "GREEN";
			checkRetypePassResult = true;
		} else {
			spanElem.innerHTML = "패스워드 불일치";
			spanElem.style.color = "RED";
			checkRetypePassResult = false;
		}
	}
	
	function openCheckIdWindow() {
		window.open("check_id.jsp","","width=400,height=250");
	}
	
	function checkSubmit() {
		// 아이디 중복 확인 작업을 통해 아이디를 입력받았고(checkIdResult == true),
		// 정규표현식을 통해 패스워드 규칙이 올바르고(checkPassResult == true),
		// 패스워드 확인을 통해 두 패스워드가 동일하면(checkRetypePassResult == true) 이면
		// true 리턴, 아니면 false 리턴
		// => 아아디 중복 확인 작업과 패스워드 비교 작업 시 성공 여부를 저장할 변수 필요
		//    (이 변수는 다른 메서드에서도 접근해야하므로 함수 외부에 전역변수로 선언해야함)
		if(checkIdResult == false) { // 아이디 중복확인을 수행하지 않았을 경우
			// alert() 함수를 통해 "중복 확인 필수!" 메세지 출력 후 아이디 창에 포커스 요청
			alert("아이디 중복확인 필수!");
			document.fr.id.focus();
			return false; // 현재 함수 종료
		} else if(checkPassResult == false) { // 패스워드 확인을 수행하지 않았을 경우
			// alert() 함수를 통해 "올바른 패스워드 입력 필수!" 메세지 출력 후 아이디 창에 포커스 요청
			alert("올바른 패스워드 입력 필수!");
			document.fr.pass.focus();
			return false; // 현재 함수 종료
		} else if(checkRetypePassResult == false) { // 패스워드 확인을 수행하지 않았을 경우
			// alert() 함수를 통해 "패스워드 확인 필수!" 메세지 출력 후 아이디 창에 포커스 요청
			alert("패스워드 확인 필수!");
			document.fr.pass2.focus();
			return false; // 현재 함수 종료
		}
		
		
	}
</script>
</head>
<body>
	<div id="wrap">
		<!-- 헤더 들어가는곳 -->
		<jsp:include page="../inc/top.jsp"></jsp:include>
		<!-- 헤더 들어가는곳 -->
		  
		<!-- 본문들어가는 곳 -->
		  <!-- 본문 메인 이미지 -->
		  <div id="sub_img_member"></div>
		  <!-- 왼쪽 메뉴 -->
		  <nav id="sub_menu">
		  	<ul>
		  		<li><a href="#">Join us</a></li>
		  		<li><a href="#">Privacy policy</a></li>
		  	</ul>
		  </nav>
		  <!-- 본문 내용 -->
		  <article>
		  	<h1>Join Us</h1>
		  	<form action="joinPro.jsp" method="post" id="join" name="fr" onsubmit="return checkSubmit()">
		  		<fieldset>
		  			<legend>Basic Info</legend>
		  			<label>User Id</label>
		  			<input type="text" name="id" class="id" id="id" required="required" readonly="readonly" placeholder="중복체크 버튼 클릭">
		  			<input type="button" value="dup. check" class="dup" id="btn" onclick="openCheckIdWindow()"><br>
		  			<!-- 버튼 클릭 시 새 창 띄우기(크기 : 350 * 200, 파일 : check_id.jsp) -->
		  			
		  			<label>Password</label>
		  			<input type="password" name="pass" id="pass" required="required" placeholder="영문,숫자,특수문자 8~16글자"
		  						onkeyup="checkPass(this.value)">			
		  			<span id="checkPassResult"><!-- 패스워드 규칙 판별 결과 표시 영역 --></span><br>
		  			
		  			<label>Retype Password</label>
		  			<input type="password" name="pass2" required="required" onkeyup="checkRetypePass(this.value)">
		  			<span id="checkRetypePassResult"><!-- 패스워드 일치 여부 표시 영역 --></span><br>
		  			<!-- pass2 항목에 텍스트 입력할 때마다 자바스크립트의 checkRetypePass() 함수 호출 -->
		  			
		  			<label>Name</label>
		  			<input type="text" name="name" id="name" required="required"><br>
		  			
		  			<label>E-Mail</label>
		  			<input type="email" name="email" id="email" required="required"><br>
		  		</fieldset>
		  		
		  		<fieldset>
		  			<legend>Optional</legend>
		  			<label>Post code</label>
		  			<input type="text" name="post_code" id="post_code" readonly="readonly" placeholder="검색 버튼 클릭">
		  			<input type="button" value="search" class="dup"><br>
		  			<label>Address</label>
		  			<input type="text" name="address1" id="address1" readonly="readonly">
		  			<input type="text" name="address2" id="address2" placeholder="상세주소 입력"><br>
		  			<label>Phone Number</label>
		  			<input type="text" name="phone" ><br>
		  			<label>Mobile Phone Number</label>
		  			<input type="text" name="mobile" ><br>
		  		</fieldset>
		  		<div class="clear"></div>
		  		<div id="buttons">
		  			<input type="submit" value="Submit" class="submit">
		  			<input type="reset" value="Cancel" class="cancel">
		  		</div>
		  	</form>
		  </article>
		  
		  
		<div class="clear"></div>  
		<!-- 푸터 들어가는곳 -->
		<jsp:include page="../inc/bottom.jsp"></jsp:include>
		<!-- 푸터 들어가는곳 -->
	</div>
</body>
</html>


-----------------------------------test4_join_form.jsp-----------------------------------


<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<script src="../js/jquery-3.6.0.js"></script>
<script type="text/javascript">
	$(function() {
		$("#id").on("blur", function() {
			// 아이디 값을 사용하여 test4_check_id_pro.jsp 페이지로 전달하여
			// MemberDAO의 checkId() 메서드를 통해 ID 중복 판별 후 결과 출력
			// => 일치하는 아이디 존재할 경우 : "이미 사용중인 아이디" 출력하고
			// 	  아니면 "사용 가능한 아이디" 출력
			$.ajax({
				type: "GET",
				url: "test4_check_id_pro.jsp",
				data: {
					id: $("#id").val()
				},
				dataType: "text",
				success: function(msg) { // 요청에 대한 처리 성공 시
					$("#resultArea").html(msg);
				}
			});
		});
	});
</script>
</head>
<body>
	<h1>회원 가입</h1>
	<form action="test4_result.jsp" method="post">
		<table>
			<tr>
				<th>아이디</th>
				<td>
					<input type="text" name="id" id="id" required="required">
					<div id="resultArea"></div>
				</td>
			</tr>
			<tr>
				<th>패스워드</th>
				<td>
					<input type="password" name="passwd" required="required">
				</td>
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

-----------------------------------check_id.jsp-----------------------------------

<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%
// id 파라미터 값을 가져와서 id 변수에 저장
String id = request.getParameter("id");

// duplicate 파라미터 값을 가져와서 duplicate 변수에 저장
// => 만약, 새 창을 연 상태일 경우 파라미터 자체가 없으므로 null 값이 리턴되고
//    check_id_pro.jsp 페이지를 통해 확인을 한 경우 "true" 또는 "false" 값이 리턴됨
String isDuplicate = request.getParameter("duplicate");
%>    
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<script type="text/javascript">
	function useId(id) {
		// 자식창(나)에서 부모창(나를 연 창)을 제어하려면 window.opener.xxx 형태로 제어
		// => xxx 은 원래 해당 요소에 접근하는 문법 그대로 사용
		// 1. 사용 가능한 아이디를 부모창의 아이디 입력란에 표시
		window.opener.document.fr.id.value = id;
		// 2. 부모창의 전역변수 checkIdResult 값을 true 로 변경
		window.opener.checkIdResult = true;
		// 3. 자식창 닫기
		window.close();
	}
	
	function checkId(id) {
		var divCheckIdResult = document.getElementById("checkIdResult");
		
		// 정규표현식을 통해 id 규칙 판별
		var regex = /^[A-Za-z0-9]{4,16}$/;
		if(!regex.exec(id)) { // 규칙에 맞지 않는 아이디일 경우
			divCheckIdResult.innerHTML = "4 ~ 16자리 영문자,숫자 조합!";
			divCheckIdResult.style.color = "RED";
		} else {
			divCheckIdResult.innerHTML = "";
		}
	}
	
	// onsubmit 을 통해 아이디 검증을 수행할 경우
	function checkId2() {
		var divCheckIdResult = document.getElementById("checkIdResult");
		var id = document.getElementById("id").value;
		
		// 정규표현식을 통해 id 규칙 판별
		var regex = /^[A-Za-z0-9]{4,16}$/;
		if(!regex.exec(id)) { // 규칙에 맞지 않는 아이디일 경우
			divCheckIdResult.innerHTML = "4 ~ 16자리 영문자,숫자 조합!";
			divCheckIdResult.style.color = "RED";
			return false; // submit 동작 취소
		} else {
			divCheckIdResult.innerHTML = "";
			return true; // submit 동작 수행
		}
	}
</script>
</head>
<body>
	<h1>아이디 중복 체크</h1>
	<form action="check_id_pro.jsp" onsubmit="return checkId2()">
		<!-- id 파라미터가 존재할 경우 id 입력란에 id 값 표시 -->
		<input type="text" name="id" id="id" <%if(id != null) { %> value="<%=id %>" <%} %>required="required"  placeholder="영문,숫자 4~16글자"">
		<input type="submit" value="중복확인">
	</form>
	<div id="checkIdResult">
	<!-- 중복체크 결과 표시 위치 -->
	<%if(isDuplicate != null && isDuplicate.equals("true")) {%>
		<br>이미 사용중인 아이디입니다.
	<%} else if(isDuplicate != null && isDuplicate.equals("false")) { %>
		<br>사용 가능한 아이디입니다.<br>
		<!-- 버튼 클릭 시 useId() 함수 호출(파라미터로 자바의 변수 id 값을 문자로 전달) -->
		<input type="button" value="아이디 사용" onclick="useId('<%=id%>')">
	<%} %>
	</div>
</body>
</html>

-----------------------------------test4_check_id_pro.jsp-----------------------------------

<%@page import="ajax.MemberDAO"%>
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%
// AJAX 를 사용하여 전달받은 id 가져와서 변수에 저장 후
// MemberDAO 객체의 checkId() 메서드를 호출하여 아이디 중복 여부 확인 작업 요청
// => 파라미터 : 아이디(id)     리턴타입 : boolean(isDuplicate)

String id = request.getParameter("id");

MemberDAO memberDAO = new MemberDAO();
boolean isDuplicate = memberDAO.checkId(id);

if(isDuplicate) { // 중복
	out.println("이미 사용중인 아이디");
} else {
	out.println("사용 가능한 아이디");
}
%>
```

## json(JavaScript Object Notation)
- 자바스크립트로부터 파생되어 만들어진 포맷으로 데이터를 키-값 의 한 쌍 또는 배열 등을 활용하여 텍스트 데이터를 활용하도록 하는 포맷
- 프로그래밍 언어에 독립적으로 다양한 언어를 통해 데이터 처리 가능함
- 숫자(Number), 문자열(String), 논리(Boolean), 배열(Array), 객체(Object) 등 표현

### 표현 방법
1) 객체 표현 방법
- 중괄호를 사용하여 객체의 속성을 "이름":값 형태로 표현함
```
{"name":"홍길동", "age":20}
```

2) 배열 표현 방법
- 대괄호는 사용하여 배열의 각 요소를 표현하며, 배열 내의 객체는 다시 중괄호{} 로 표현
```
[1, {"age":20}, 3]
```

** 사람 1명의 데이터를 저장하는 JSON 객체 표현법 **
```
{
  "이름":"홍길동",
  "나이":20
  "성별":"남",
  "취미":["게임","등산"]
}
```

** 3개의 객체를 갖는 배열을 JSON 으로 표현 **
```
[
	{"id":"admin","name":"관리자","age":0,"agreeRcvSpam":false},
	{"id":"hong123","name":"홍길동","age":20,"agreeRcvSpam":true},
	{"id":"leess","name":"이순신","age":44,"agreeRcvSpam":false}
]
```

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<script src="../js/jquery-3.6.0.js"></script>
<script type="text/javascript">
	// 자바스크립트 형식의 객체 생성
	let jsObject = {
		name : "홍길동",
		age : 20
	};
	// => 자바스크립트의 객체에 접근 시 변수명.속성명 형태로 접근
	
	// JSON 객체의 stringify() 함수
	// - 파라미터로 전달된 자바스크립트 객체를 JSON 형식의 문자열로 변환
// 	$(function() {
// 		$("#btnOk").on("click", function() {
// 			// jsObject 객체를 JSON 형식으로 변환
// 			// {"name":"홍길동", "age":20} 출력됨
// 			alert(JSON.stringify(jsObject));
// 		});
// 	})

	$(function() {
		$("#btnOk").on("click", function() {
			// json_data.txt 파일을 사용하여 AJAX 에서 JSON 객체 데이터 요청하기
			$.ajax({
				type: "GET",
				url: "json_data.txt",
				dataType: "json" // 응답 데이터가 JSON 형식이므로 JSON 타입 지정
			})
			.done(function(data) { 
				// 요청 성공 시 자동으로 호출되는 함수 정의
				// => 요청에 성공했을 경우 함수 파라미터에 응답 결과(JSON 객체)가 전달됨
// 				alert(data);
				
				// 전달받은 객체를 문자열 형식으로 변환
				alert(JSON.stringify(data));
				
// 				$("#resultArea").html(data);
			})
			.fail(function() {
				$("#resultArea").html("요청 실패");				
			});
		});
	});
</script>
</head>
<body>
	<h1>test5_json.jsp</h1>
	<input type="button" value="Javascript -> JSON" id="btnOk"><br>
	<div id="resultArea"></div>
</body>
</html>
```








