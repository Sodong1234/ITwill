# [오전수업] JSP 41차

### 정규표현식 연습
```java
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

> 개인 프로젝트 회원가입 영역에 정규표현식 사용함.
