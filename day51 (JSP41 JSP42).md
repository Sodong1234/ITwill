# [오전수업] JSP 41차

### 정규표현식 연습
```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
    
    
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<script type="text/javascript">
	/*
	자바스크립트에서 정규표현식 사용 방법
	1. 정규표현식 패턴 객체 생성
		1) var 변수명 = new RegExp("패턴문자열");
		2) var 변수명 = /패턴문자열/;
		=> 두 가지 방법 중 한 가지 방법을 사용하여 패턴 문자열을 갖는 객체 생성하고
	2. 변수명.exec(검사할 데이터) 형태로 유효성 검사 
	   => 검사 결과를 true/false 형태로 취급 가능  
	*/
	function checkName(name) {
		var nameResult = document.getElementById("nameResult");  
		   
		// 정규표현식에 사용될 패턴 문자열을 변수 regex 에 저장
		// => 규칙 : 한글 2 ~ 5글자
// 		var regex = new RegExp("^[가-힣]{2,5}$");
		var regex = /^[가-힣]{2,5}$/;
// 		alert(regex : " : " + typeof(regex)); // object 타입 출력
		
		// 정규표현식에 따른 유효성 검사를 위해 if문 사용하여 id값 판별
		// => 결과를 HTML 형식으로 div 태그 내에 표시
// 		alert(regex.exec(name));
		if(regex.exec(name)) { // 정규표현식과 일치하는 데이터일 경우 
			nameResult.innerHTML = "사용 가능";
			nameResult.style.color = "GREEN";
		} else { // 정규표현식과 일치하지 않는 데이터일 경우
			nameResult.innerHTML = "사용 불가";
			nameResult.style.color = "RED";
		}
	}
	   
	function checkPhone1(phone1) {
		var regex = /^[0-9]{3,4}$/;
		if(!regex.exec(phone1)) {
			alert("전화번호 가운데 3~4자리 숫자 필수!");
		}
	}   
	
	function checkPhone2(phone2) {
		var regex = /^[0-9]{4}$/;
		if(!regex.exec(phone2)) {
			alert("전화번호 뒷자리 4자리 숫자 필수!");
		}
	}
	
	function checkPhone(phone) {
		// 전화번호 검증 양식 : xxx-xxxx-xxxx(하이픈 포함) 가능
		var regex = /^(010|011|051|02|031)-[0-9]{3,4}-[0-9]{4}$/;
		if(!regex.exec(phone)) {
			alert("전화번호 양식(xxx-xxxx-xxxx) 필수!");
		}
	}
	
	function checkEmail(email) {
		// 이메일 계정부분(@ 앞 부분) 규칙 체크
		// => 1. 계정(@ 앞부분) : 알파벳, 숫자, -, _, . 을 포함 할 수 있으며 1글자 이상
		// => 2. 도메인(@ 뒷부분) : 알파벳, 숫자, -을 포함 할 수 있으며 1글자 이상
		//	  단, 도메인은 최상위 도메인만 존재하거나(com, net 등)
		//	  서브도메인.최상위도메인 형식으로도 올 수 있음(co.kr, or.kr 등)
		// => 계정명@회사명.최상위도메인 또는 계정명@회사명.서브도메인(갯수 무관).최상위도메인
		var regex = /^[A-Za-z0-9-_.]+@[A-Za-z0-9-]+\.[A-Za-z0-9-]+(\.[A-Za-z0-9-]+)*$/;
		//			----- 계정 -----    -- 회사명 --  ------------ 도메인 -------------
// 		var regex = /^[\w-.]+@[\w-]+\.[\w-]+(\.[\w-]+)*$/;
		if(!regex.exec(email)) {
			alert("이메일 양식 틀림!")
		}
	}
</script>
</head>
<body>
	<h1>test.jsp - 정규표현식</h1>
	<form action="#">
		<!-- 이름 항목 입력 후 포커스 해제될 때 자바스크립트 함수 checkName() 호출(입력값 전달) -->
		이름 <input type="text" name="name" id="name" placeholder="한글 2~5글자" 
				   onblur="checkName(this.value)">
		<span id="nameResult"></span><br>
		전화번호
		<select>
			<option value="010">010</option>
			<option value="011">011</option>
		</select>
		- <input type="text" name="phone1" id="phone1" onblur="checkPhone1(this.value)">
		- <input type="text" name="phone2" id="phone2" onblur="checkPhone2(this.value)">
		<br>
		
		
		전화번호 <input type="text" name="phone" id="phone"
				onblur="checkPhone(this.value)"><br>
				
		이메일		
		<input type="text" name="email" id="email" onblur="checkEmail(this.value)">
		<br>
	</form>
</body>
</html>
```


---

# [오후수업] JSP 42차

## jQuery(제이쿼리)
- 클라이언트 사이드의 자바스크립트 라이브러리의 한 종류
- 오픈소스(= 무료)이며 크로스 플랫폼 지원됨
- 자바스크립트에서 사용하는 다양한 문법을 짧게 압축 가능
- 문서 객체 모델(Document Object Model = DOM)과 관련된 처리를 쉽게 구현
- 이벤트 연결을 쉽게 구현(= 일관된 방식으로 연결)
- AJAX 애플리케이션을 쉽게 개발 가능

< jQuery 사용법 >
1. www.jquery.com 사이트에서 jquery 라이브러리 다운로드(jquery-3.6.0.js) 필요
	- (다운로드 없이도 CDN 방식으로 서버에서 끌어다 사용 가능하지만 서버 장애 시 적용 불가능)
2. HTML 문서 내에서 <script> 태그를 사용하여 라이브러리 등록
```jsp	
   <script src="jquery-3.6.0.js"></script>
```	
3. load 이벤트 또는 ready 이벤트를 사용하거나 자바스크립트 함수 내에서 jquery 문장 적용
	- 주로, jquery 의 경우 페이지 로딩 시 동작하도록 load 또는 ready 이벤트에서 작성을 하며 ready 이벤트 방식을 좀 더 많이 사용 (ready 이벤트가 먼저 호출됨)
	- (추후 추가 예정)


### jQuery 연습
```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>

<script type="text/javascript">
	// body 태그의 onload 이벤트 : body 문서가 로딩 완료 시 자동으로 동작
	function load() {
		alert("load 이벤트");
	}
</script>
</head>
<body onload="load()"> <!-- body 영역 로딩 완료 시 load() 함수 호출됨 -->
	<script type="text/javascript">
		alert("1");
	</script>
	
	<h1>test.jsp - jQuery</h1>
	
	<script type="text/javascript">
		alert("2");
	</script>
</body>
</html>
```
```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<!-- jquery-3.6.0.js 라이브러리 등록 -->
<script src="../js/jquery-3.6.0.js"></script>
<script type="text/javascript">
	// window.jQuery = jQuery = $ 형식으로 식별자가 변경되어 왔으며, $ 를 가장 많이 사용
	// jQuery 에서 문서 로딩 관련 이벤트 처리 시 load 또는 ready 이벤트를 사용
	// jQuery 에서 document 객체(HTML) ready 이벤트 처리 방법
	// 1. jquery(selector).ready(함수) 형식으로 작성하는 방법
// 	function print() {
// 		alert("ready 이벤트 - 1");
// 	}
// 	jQuery(document).ready(print);
	// => 함수를 별도로 정의하고 ready() 함수 파라미터로 정의된 함수 전달하거나
	//    별도의 함수를 정의하지 않고 익명함수(anonymous function) 형태로 직접 정의 가능
// 	jQuery(document).ready(function()) { // 함수의 이름이 없는 익명함수를 ready() 함수 내에 정의(1회용) 
// 		alert("ready 이벤트 - 1");
// 	});
	
	
	
	
	// 2. $(selector).ready(함수) 형식으로 작성하는 방법
	//	  => ($(selector).이벤트(함수) 형식으로 주로 다른 객체에 대한 이벤트에서 많이 사용하는 형태
// 	$(document).ready(function() {
// 		alert("ready 이벤트 - 2");
// 	});
	
	// 3. $(함수) 형식으로 작성하는 방법(document.ready 이벤트일 경우 가장 많이 사용하는 형태)
	$(function() {
		alert("ready 이벤트 - 3")
	});
</script>
</head>
<body>
	<h1>test2.jsp - jQuery</h1>
</body>
</html>
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
	/*
	선택자(selector)
	- 문서 객체 내의 요소를 선택하는 용도로 사용되는 구성 요소
	- 자바스크립트에서 선택자를 사용하여 요소 선택 시
	  document.getElementBy("선택자") 또는 document.querySelector("선택자") 형태로 선택
	  => jQuery 는 $("선택자") 형태로 지정하므로 쉽고 편리하게 접근 가능
	  
	  < 선택자 종류 >
	1. 직접 선택자
	  1) 아이디(#) - 페이지 내에서 유일한 요소 => $("#id선택자명")
	  2) 클래스(.) - 페이지 내에서 중복될 수 있는 요소 => $(".class선택자명")
	  3) 태그 - 동일한 태그(복수개의 요소) 요소 => $("태그명")
	  4) 태그속성 - 동일한 태그 중 특정 속성 지정 => $("태그명[속성명]")
	  5) 태그속성값 - 동일한 태그 중 특정 속성의 값 지정 => $("태그명[속성명=값]")
	  
	< 선택자 활용 기본 문법 >
	$("선택자명 또는 *").메서드("속성명","속성값");
	*/

	// jQuery 를 활용하여 document 객체의 ready 이벤트를 통해 HTML 문서의 각종 속성 변경
	$(document).ready(function() {
		// HTML 문서 내의 전체 요소에 대해 CSS 속성을 변경
		// => 이 때, 전체 요소를 선택하기 위한 선택자로 * 사용 가능
		// => CSS 속성을 변경하기 위해서는 css() 메서드 호출하여 css("속성명", "속성값") 형태로 사용
		// 1. 전체 요소 글자색을 "RED" 로 변경
		$("*").css("color", "RED");
		
		// 2. id 선택자 중 "idSelector" 요소의 색상을 BLUE 로 변경
		$("#idSelector").css("color", "BLUE");
		
		// 3. class 선택자 중 "classSelector" 요소의 색상을 ORANGE 로 변경
		$(".classSelector").css("color","ORANGE");
		
		// 4. table 태그의 테두리 변경, 배경색 변경
// 		$("table").css("border","1px solid blue")
// 		$("table").css("background","yellow")

		// 동일한 대상에 복수개의 속성을 지정할 때 메서드를 연쇄적으로 호출도 가능
// 		$("table").css("border","1px solid blue").css("background","green")

		// 동일한 대상에 복수개의 속성을 지정할 때 블록으로 묶어서 사용 가능
		$("table").css({
		border: "3px solid blue",
		background: "gray"
		});
	});
	
	/*
	 < 선택자 종류 >
	2. 인접관계 선택자
		1) 자식 선택자(>) - 특정 선택자의 바로 하위에 있는 자식 요소를 선택 => $("부모선택자 > 자식선택자")
		2) 자식 또는 후손 선택자(공백) - 특정 선택자의 모든 하위에 있는 모든 요소를 선택
								   => $("부모선택자 자식선택자")
		3) 순서 선택자 - 특정 선택자를 기준으로 지정된 순서에 해당하는 요소를 선택
					  => $("선택자명:순서옵션")
		   ex) 첫 번째 태그요소 - 선택자명:first, 마지막 태그요소 - 선택자명:last
		   	   홀수번째 태그요소 - 선택자명:odd, 짝수번째 태그요소 - 선택자명:even
	*/
	$(function() {
		// table 태그 밑에 있는 tr 태그 배경색을 green 으로 변경
// 		$("table tr").css("background", "green");
		
		// tr 태그들 중 첫번째 tr 태그 배경색만 green 으로 변경하고 가운데 정렬
		$("tr:first").css("background", "green");
		$("tr:first").css("text-align", "center");
		
		// tr 태그들 중 홀수번째 태그 배경색을 skyblue 로 변경
		$("table tr").css("odd", "skyblue");
		$("table tr").css("even", "pink");
		
		// tr 태그들 중 짝수번째 태그 배경색을 pink 로 변경
		$("tr:odd").css("background", "skyblue");
		
		// tr 태그들 중 홀수번째 태그 배경색을 pink 로 변경(제목열도 짝수번째(0번) 이므로 변경됨)
		$("tr:even").css("background", "pink");
		
		// id 선택자 inputBox 의 하위요소 변경
		$("#inputBox > input").css("background","gray"); // input 태그 배경색 gray 로 변경
		$("#inputBox > input[type=text]").css("color","black"); // input 태그
		
		// id 선택자 inputBox 의 자식들 중 type 속성이 text 인 요소의 값 가져오기
		// => $("선택자").val() 메서드를 호출
		//	  전달인자 없으면 값 가져오기, 전달인자 있으면 값 설정하기
// 		alert($("#inputBox > input[type=text]").val());
		// 가져온 값을 다시 textarea 태그 중 readonly 속성이 적용된 요소에 출력
// 		var text = $("#inputBox > textarea[readonly]").val();
// 		$("#inputBox > textarea[readonly]").val(text);

		// id 선택자 divBox 의 자식 div 태그 색상을 blue 로 변경
		$("#divBox > div").css("color", "blue");
		
		// id 선택자 divBox2 의 자식 또는 후손 div 태그 색상을 blue 로 변경
		$("#divBox2 div").css("color", "blue");
		
	});
					  
	// 외부로부터 함수를 호출하여 jQuery 동작시킬 경우
	// => 주의! 외부에서 함수를 호출하기 때문에 ready 이벤트를 등록하지 않도록 해야함
	function showInputData() {
		// #inputBox 하위의 input 태그 입력값을 textarea 에 각각 출력
		var text = $("#inputBox > input[type=text]").val();
		$("#inputBox > textarea[readonly]").val(text);
		
		// 패스워드(type 속성값이 password 인 항목)는 #textarea 요소에 출력
		var passwd = $("#inputBox > input[type=password]").val();
		$("#textarea").val(text);
		
	}
</script>
</head>
<body>
	<h1>jQuery/test3.jsp</h1>
	<h1 id="idSelector">id 선택자</h1>
	<h1 class="classSelector">class 선택자</h1>
	<table>
		<tr><td>번호</td><td>제목</td></tr>
		<tr><td>1</td><td>제목1</td></tr>
		<tr><td>2</td><td>제목2</td></tr>
		<tr><td>3</td><td>제목3</td></tr>
		<tr><td>4</td><td>제목4</td></tr>
	</table>
	<div id="inputBox">
		<input type="text" value="admin">
		<input type="password" value="1234"><br>
		<input type="button" value="확인" onclick="showInputData()"><br>
		<textarea readonly="readonly"></textarea>
		<textarea id="textarea"></textarea>
	</div>
	<hr>
	<div id="divBox">
		<div>
			1번 div 태그
			<div>1-1번 div 태그</div>
			<div>1-2번 div 태그</div>
		</div>
		<span>span 태그</span>
		<div>2번 div 태그</div>
		<div>
			3번 div 태그
			<div>3-1번 div 태그</div>
			<div>3-2번 div 태그</div>
		</div>
	</div>
	<hr>
	<div id="divBox2">
		<div>
			1번 div 태그
			<div>1-1번 div 태그</div>
			<div>1-2번 div 태그</div>
		</div>
		<span>span 태그</span>
		<div>2번 div 태그</div>
		<div>
			3번 div 태그
			<div>3-1번 div 태그</div>
			<div>3-2번 div 태그</div>
		</div>
	</div>
	<hr>
	
</body>
</html>
```	
	
	
	
> 개인 프로젝트 회원가입 영역에 정규표현식 사용함.
>	개인 프로젝트용 홈페이지 구현작업 실시. repositories에 파일을 업로드해놓음.
	
