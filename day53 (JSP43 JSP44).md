# [오전수업] JSP 43차
> 42차에 수업한 test3 파일의 추가분을 커밋해놓았음.

### Jquery css
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
	css("속성명", "값") 형식으로 호출 시 해당 속성값을 변경할 수 있으며
	css("속성명") 형식으로 호출 시 해당 속성에 대한 속성값을 가져올 수 있다.
	=> 단, 해당 선택자 요소가 복수개일 경우 가장 첫번째 요소에 대한 것만 가져옴 
	*/
	$(function() {
		// h3 태그 중 마지막 요소의 color 속성값을 red로 변경
		$("h3:last").css("color","red");

		var h3Color = $("h3").css("color"); // 첫번째 h3 태그의 color 속성값을 가져와서 저장
// 		alert(h3Color);	// rgb(0,0,0) 출력됨 => BLACK 색상값
		
		// h3 태그 중 첫번째 요소의 color 속성값을 blue 로 변경 후 값 출력
		$("h3:first").css("color","blue");
// 		alert($("h3").css("color"));

		// css() 함수 내에서 구현되는 익명 함수
		// => $("선택자").css("속성명", function() {})
		// 1) $("선택자").css("속성명", function(index) {}) 
		// 	  => 해당 선택자에 대한 지정된 속성값에 반복 접근
		//	  	 (index 파라미터로 요소의 인덱스 전달됨)
// 		$("h3").css("color", function(index) {
// 			alert(index); // h3 태그에 차례대로 접근하면서 해당 요소의 인덱스(0, 1, 2) 출력
// 		});
		
		// 2) $("선택자").css("속성명", function(index, value) {}) 
		// 	  => 해당 선택자에 대한 지정된 속성값에 반복 접근
		//	  	 첫번째 파라미터 (index) : 해당 요소의 인덱스(0부터 시작) 차례대로 전달(= 1씩 증가)
		//		 두번째 파라미터(value) : 각 인덱스에 해당하는 요소의 지정된 속성값 차례대로 전달
// 		$("h3").css("color", function(index, value) {
// 			alert(index + ", " + value);
// 		});
		
		// ------------------------------------------------------------------------------
		var colorValue = ["green", "magenta", "skyblue"]; // 글자 색상을 저장하는 배열 생성
		var backgroundColorValue = ["yellow", "black", "red"]; // 배경색 색상을 저장하는 배열 생성 
			
		$("h3").css("color", function(index) {
			// 익명함수 내에서 return 문을 사용하여 특정 값 리턴 시 해당 요소의 속성값 변경 가능
// 			return "blue";	// 모든 h3 태그의 color 속성값을 "blue" 로 변경

			// colorValue 배열 내에 저장된 색상을 index 를 활용하여 접근한 뒤 각 요소의 색상 변경
			return colorValue[index]; // index 값이 0, 1, 2 가 전달되므로 배열의 인덱스로 활용 가능
			// => 결과적으로 제목1, 제목2, 제목3 에 해당하는 h3 태그의 color 속성이
			//			  green, magenta, skyblue 값으로 차례대로 변경됨
		});
		
		// css() 함수를 호출하여 "h3" 태그의 "background-Color" 속성값을 배열 값을 사용하여 변경
// 		$("h3").css("background-color", function(index) {
// 			return backgroundColorValue[index];
// 		});
		
		// 동일한 대상에 복수개의 속성을 변경하는 경우
		// $("선택자").css({속성1:함수(index) {}, 속성2:함수(index) {}});
		$("h3").css({
			// 변경할 속성명:함수(){} 형태로 구현하며, 각 속성은 콤마(,)로 구분
			color:function(index) {
				return colorValue[index];
			},
			background:function(index) {
				return backgroundColorValue[index];
			}
		});
		
	});
	
	
</script>
</head>
<body>
	<h1>jQuery/test4.jsp</h1>
	<h3>제목1</h3>
	<h3>제목2</h3>
	<h3>제목3</h3>
</body>
</html>
```
### Jquery click 이벤트
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
	$(document).ready(function() {
		// 버튼 등의 특정 대상에 대한 이벤트(기본적인 이벤트)
		// => $("선택자").click(function() {});
		
		// 버튼 중 첫번째 버튼에 대한 이벤트 처리
// 		$("input[type=button]:first").click(function() {
// 			alert("시계표시 버튼 클릭됨!")
// 		});
		
		/*
		[ 특정 요소에 대한 조작 함수 ]
		1. html()
			- HTML 문서 내에서 지정된 범위(선택자)안의 모든 요소를 가져오거나 추가하는 함수
			- 해당 요소 내의 태그까지 모두 가져오며, 주석도 포함됨	
		2. text()
			- HTML 문서 내에서 지정된 범위(선택자)안의 모든 텍스트를 가져오거나 추가하는 함수
			- html() 함수와 달리 태그를 제외한 텍스트만 가져옴
			- 단, 브라우저마다 줄바꿈이나 공백 등이 달라질 수 있음
		3. empty()
			- 지정된 선택자 내의 요소 제거
		4. remove()
			- 지정된 선택자 내의 요소와 선택자 자체도 제거
		*/
		
		// 첫번째 h3 태그의 요소(태그안에 포함된 내용) 가져와서 출력 
// 		var h3Element = $("h3").html(); // "제목1" 텍스트 가져오기
// 		alert(h3Element);

		// id 선택자 h3_2 요소 내용 가져와서 출력
// 		var h3Element = $("#h3_2").html(); 
// 		alert(h3Element);
		// => 제목2&nbsp;<span>제목2-1</span>&nbsp;<span>제목2-2</span>
		
		// id 선택자 h3_2 요소 텍스트 가져와서 출력(공백 적용됨)
// 		var h3Element = $("#h3_2").text(); 
// 		alert(h3Element);
		// => 제목2 제목2-1 제목2-2
		
		// -----------------------------------------------------------------
		// html() 함수를 호출하여 #h3_1 선택자의 요소 내용을 변경(태그 적용됨)
		// => 자바스크립트 기본 : document.getElementById("id선택자명").innerHTML = 변경할 값
		//	  jQuery : $("id선택자명").html(변경할값);
		$("#h3_1").html("<i>html() 함수로 변경한 제목1</i>");
		
		// text() 함수를 호출하여 #h3_2 선택자의 요소 내용을 변경(태그도 텍스트로 취급됨)
		$("#h3_2").text("<i>html() 함수로 변경한 제목1</i>");
		// -----------------------------------------------------------------
		// html() 함수 등도 함수 내부에 익명함수 구현을 통해 요소 반복 가능
		// => $("선택자").html(function()) {}
		// => $("선택자").html(function(index)) {}
		// => $("선택자").html(function(index, oldHtml)) {}
		
		// 모든 h3 태그의 텍스트 변경
// 		$("h3").html(function(index, oldHtml) {
// 			alert(index + "번 요소 : " +oldHtml); // 각 인덱스에 해당하는 번호와 요소 내용 차례대로 출력
// 		});
		
		// 모든 h3 태그의 요소 변경
		$("h3").html(function(index, oldHtml) {
			return index + "번 요소 : " + oldHtml;
		});		
		// -----------------------------------------------------------------
		// empty() 함수를 사용하여 선택자 h3_2 내의 요소 제거
		$("#h3_2").empty();
		
		// remove() 함수를 사용하여 선택자 h3_2 내의 요소 및 선택자 자체 제거
		$("#h3_2").remove();
		
		// id 선택자 ta 항목(textarea) 내부에 id 선택자 wrap 항목의 모든 요소(태그 포함) 출력
// 		var wrapElement = $("wrap").html();
// 		$("#ta").html(wrapElement);
		$("#ta").html($("#wrap").html());
			
	});
	
	// 서버 현재 시각을 "시:분:초" 형식으로 리턴하는 함수 정의
	function getTime() {
		var now = new Date(); // Fri Apr 08 2022 12:09:07 GMT+0900 (한국 표준시) 형식
		// 임의로 표시되는 형식을 변경하기 위해서 각 데이터를 따로 가져와야함
		var hour = now.getHours();
		var min = now.getMinutes();
		var sec = now.getSeconds();
		
		return hour + "시" + min + "분" + sec + "초";
	}
	
	$(function() {
		// setInterval() 함수 내에서 반복되는 작업에 대해 중지 신호로 사용할 변수 선언
		var isStop = false; // true 일 경우 중지 신호, false 일 경우 동작 신호
		
		// btnShowClock 버튼 클릭 시 timer 요소 내부 항목 표시
		$("#btnShowClock").click(function() {
			
			
			// isStop 변수값을 false 로 초기화(나중에 중지된 후 다시 시작을 위해)
			isStop = false;
			
			// id 선택자 timer 요소 내부에 getTime() 함수 리턴값을 표시
			$("#timer").html(getTime());
			
			// 표시된 시각 정보를 1초마다 갱신
			// => setInterval() 함수 필요
			// < 기본 문법 > setInterval(f, ms)
			// => setInterval(function() {수행할 작업}, 갱신간격);
			setInterval(function() {
				// 갱신 조건 : 타이머 중지 신호(isStop)이 false 일 때
				// 중지 조건 : 타이머 중지 신호 isStop)이 true 일 때 => clearInterval() 함수 필요
				if(!isStop) { // 갱신
				$("#timer").html("<h3>" + getTime() + "</h3>");
				} else { // 중지
					// setInterval() 함수를 통해 반복되는 작업 중지 시 clearInterval() 함수 호출
					// => setInterval() 함수 내에서 clearInterval() 함수 파라미터로
					//	  this 지정 시 setInterval() 함수에 구현된 익명객체 자체를 전달 = 자기 자신 중지
					clearInterval(this);
				}
			
			}, 1000); // 1000ms = 1000밀리초 = 1초
		});
		
		// btnStopClock 버튼 클릭 시 timer 반복 중지 신호 전달
		$("#btnStopClock").click(function() {
			// 타이머 중단을 위해 isStop 변수값을 true 로 변경
			isStop = true;
		});
		
		// btnHideClock 버튼 클릭 시 timer 요소 내부 항목 제거
		$("#btnHideClock").click(function() {
			// timer 선택자 태그 자체는 남겨두기 위해 empty() 함수 사용하여 제거
			$("#timer").empty();
			
			// 타이머 중단을 위해 isStop 변수값을 true 로 변경
			isStop = true;
		});
	});

</script>
</head>
<body>
	<h1>jQuery/test5.jsp</h1>
	<div id="wrap">
		<!-- 제목을 출력하는 공간 -->
		<h3 id="h3_1">제목1</h3>
		<h3 id="h3_2">제목2&nbsp;<span>제목2-1</span>&nbsp;<span>제목2-2</span></h3>
		<h3 id="h3_3">제목3</h3>
	</div>
	<textarea id="ta" rows="5" cols="50"></textarea>
	<hr>
	<input type="button" value="시계 표시" id="btnShowClock">
	<input type="button" value="시계 정지" id="btnStopClock">  
	<input type="button" value="시계 제거" id="btnHideClock">
	<div id="timer">
	<!-- 시계가 표시될 공간 -->
	</div>
	
</body>
</html>
```

---

# [오후수업] JSP 44차

### 특정 요소에 대한 조작 함수 - 2
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
[ 특정 요소에 대한 조작 함수 - 2 ]
1. append()
	- 지정한 내용을 선택자 내부의 '끝 부분' 에 추가
	- appendTo() 함수와 문장 구조의 차이만 다르고 기능은 동일
	- $("선택자").append("추가할내용")
	  $("추가할내용").appendTo("선택자")
2. prepend()
	- 지정한 내용을 선택자 내부의 '첫 부분' 에 추가
	- prependTo() 함수와 문장 구조의 차이만 다르고 기능은 동일
	- $("선택자").prepend("추가할내용")
	  $("추가할내용").prependTo("선택자")
3. after()
	- 지정한 내용을 선택자 요소의 '뒤'에 삽입
	- insertAfter() 함수와 문장 구조의 차이만 다르고 기능은 동일
	- $("선택자").after("삽입할내용")
	  $("삽입할내용").insertAfter("선택자")
4. before()
	- 지정한 내용을 선택자 요소의 '앞'에 삽입
	- insertBofore() 함수와 문장 구조의 차이만 다르고 기능은 동일
	- $("선택자").before("삽입할내용")
	  $("삽입할내용").insertBefore("선택자")
*/
	$(function() {
		$("#wrap_append").append("<div>append() 로 삽입한 div 태그</div>");
		// => #wrap_append 영역 [내부]의 가장 마지막 요소로 삽입
// 		$("<div>appendTo() 로 삽입한 div 태그</div>").appendTo("#wrap_append");
		
		$("#wrap_prepend").prepend("<div>prepend() 로 삽입한 div 태그</div>");
		// => #wrawp_prepend 영역 [내부]의 가장 처음 요소로 삽입
// 		$("<div>prependTo() 로 삽입한 div 태그</div>").prependTo("#wrap_prepend");
		
		$("#wrap_after").after("<div>after() 로 삽입한 div 태그</div>");
		// => #wrawp_after 영역 [외부] 기준으로 뒤에 삽입
// 		$("<div>insertAfter() 로 삽입한 div 태그</div>").insertAfter("#wrap_after");
		
		$("#wrap_before").before("<div>before() 로 삽입한 div 태그</div>");
		// => #wrawp_before 영역 [외부] 기준으로 앞에 삽입
// 		$("<div>insertBefore() 로 삽입한 div 태그</div>").insertBefore("#wrap_before");
		
		// 결과 확인을 위해 textarea(#ta) 에 id 선택자 wrap 의 내용을 모두 출력(html() 또는 val())
		$("#ta").val($("#wrap").html());
		
	});

</script>
</head>
<body>
	<h1>jQuery/test6.jsp</h1>
	<div id="wrap">
		<div id="wrap_append">
			<div>
				<div>div 태그1</div>
				<div>div 태그2</div>
			</div>		
		</div>
		<hr>
		<div id="wrap_prepend">
			<div>
				<div>div 태그1</div>
				<div>div 태그2</div>
			</div>		
		</div>
		<hr>
		<div id="wrap_after">
			<div>
				<div>div 태그1</div>
				<div>div 태그2</div>
			</div>		
		</div>
		<hr>
		<div id="wrap_before">
			<div>
				<div>div 태그1</div>
				<div>div 태그2</div>
			</div>		
		</div>
	</div>	
	<!-- 결과 확인을 위한 태그 출력용 textarea -->
	<textarea id="ta" rows="20" cols="100"></textarea>
	
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
1. eq(인덱스)
	- 선택자를 포함하여 형제자매 요소 탐색
	- 파라미터로 전달하는 인덱스 값은 0부터 시작하며, 인덱스에 해당하는 순서에 위치한 요소 리턴
	- 음수값 전달 시 뒤에서부터 탐색
	
2. attr("HTML속성명")
	- 선택자의 지정된 해당 속성 값을 가져오거나 추가
	- HTML 태그 속성값 자체를 가져오며 상태에 따라 변하지 않음
	ex) 체크박스 checked="checked" 속성값을 가져올 때 활용(단, 체크 상태가 변해도 checked 값 전달됨)

3. prop("Javascript속성명")
	- 선택자의 지정된 속성 값에 대한 상태를 가져오거나 추가
	- 속성값 자체를 다루지 않고 해당 속성에 대한 상태(true/false) 리턴
	- attr() 함수와 유사하나 HTML 태그의 상태에 따라 결과값이 변함(자바스크립트의 상태 사용)
	ex) 체크박스 checked="checked" 속성값을 가져올 때 활용
		=> 단, 체크박스 체크 시 true, 체크 해제 시 false 리턴
4. is()
	- 선택자 입력 값과 관련된 상태 확인 후 일치 여부(true/false) 리턴
	- 선택자의 실행 결과가 태그 확인용
	- prop() 함수 리턴값을 boolean 타입으로 비교하는 것과 동일
*/
	$(function() {
// 		$("input[type=button]").click(function() {
// 			alert("버튼 클릭!");
// 		});

		// 가상선택자를 사용하여 버튼 클릭 이벤트 등록 시 => $(":button") 활용
		$(":button").click(function() {
			// 선택자 요소에 eq() 함수를 사용하여 지정된 요소들 중 인덱스에 해당하는 순서의 요소 지정
			// => 요소 지정 후 attr() 또는 prop() 함수를 사용하여 해당 요소의 속성에 접근
			// 홍길동에 해당하는 두번째 체크박스(1번 인덱스)에 대한 값 확인
			var attr1 = $("input[type=checkbox]").eq(1).attr("checked");
			// => 실제 체크 여부와 관계없이 HTML 속성에 checked="checked" 이므로 항상 checked 리턴
			var prop1 = $("input[type=checkbox]").eq(1).prop("checked");
			// => 체크 상태에 따라 true 또는 false 가 바뀌어 리턴
			
			// 이순신에 해당하는 두번째 체크박스(1번 인덱스)에 대한 값 확인
			var attr2 = $("input[type=checkbox]").eq(2).attr("checked");
			// => 체크박스 태그 내에 checked 속성이 명시되어 있지 않으므로 undefined 리턴
			var prop2 = $("input[type=checkbox]").eq(2).prop("checked");
			// => 체크 상태에 따라 true 또는 false 가 바뀌어 리턴
			
			// 두번째, 세번째 체크박스(1번, 2번 인덱스)에 대한 checked 속성값 상태 확인
			var is1 = $("input[type=checkbox]").eq(1).is(":checked");
			var is2 = $("input[type=checkbox]").eq(2).is(":checked");
			
			
			// 값 확인을 위해 id 선택자 result 영역에 표시
			$("#result").html(
				"attr1 = " + attr1 + ", prop1 = " + prop1 + "<br>" +
				"attr2 = " + attr2 + ", prop2 = " + prop2 + "<br>" +
				"is1 = " + is1 + ", is2 = " + is2 + "<br>"
			);
		});
	});
</script>
</head>
<body>
	<h1>jQuery/test7.jsp</h1>
	<table border="1">
        <tr>
            <th><input type="checkbox" id="allCheck"></th>
            <th>번호</th>
            <th>이름</th>
        </tr>
        <tr>
            <td><input type="checkbox" id="check1" name="check" checked="checked" value="1"></td>
            <td>1</td>
            <td>홍길동</td>
        </tr>
        <tr>
            <td><input type="checkbox" name="check" value="2"></td>
            <td>2</td>
            <td>이순신</td>
        </tr>
        <tr>
            <td><input type="checkbox" name="check" value="3"></td>
            <td>3</td>
            <td>강감찬</td>
        </tr>
        <tr>
        	<td colspan="3">
        		<input type="button" value="확인"><br>
        		<div id="result">결과 확인 위치</div>
        	</td>
        </tr>
    </table>
</body>
</html>
```

### submit 함수
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
	$(function() {
		/*
		submit() 함수
		- submit 동작에 대한 이벤트 처리
		- 자바스크립트에서 submit 이벤트 처리 방법과 동일
		- form 태그를 선택자로 지정하여 submit() 함수를 호출하여
		  submit 함수 내에 익명함수를 구현하여 submit 버튼 클릭 시 수행할 동작 지정
		*/
		$("form").submit(function() {
			// 콤보박스(select) 의 option 태그들 중 첫번째 option 태그의 selected 속성 확인
			// => '선택하세요' 항목이 선택되어 있을 경우 true, 아니면 false 리턴하기 위해 prop() 함수 활용
// 			alert($("#selectBox > option").eq(0).prop("selected"));
// 			alert($("#selectBox > option:first").prop("selected"));
// 			alert($("#selectBox > option[value=선택하세요]").prop("selected"));

// 			alert($("input[name=id]").val()); // 아이디 입력값 가져와서 출력

			// if문을 사용하여 '선택하세요' 항목이 선택되어 있을 경우(true)
			// => alert() 함수를 통해 "항목 선택 필수!" 출력 후 false 값 리턴(submit 동작 취소)
			// 아니면 true 값 리턴(submit 동작 수행)
			if($("#selectBox > option").eq(0).prop("selected")) { // true 일 경우
				alert("항목 선택 필수!");
				$("#selectBox > option").eq(0).focus();
				return false;
			} else if($("input[name=id]").val() == "") {
				alert("아이디 입력 필수!");
				$("input[name=id]").focus();
				return false;
			} else if($("input[name=password]").val() == "") {
				alert("패스워드 입력 필수!");
				$("input[name=password]").focus();
				return false;
			} else { // false 일 경우
				return true;
			}
		});
	});
</script>
</head>
<body>
	<h1>jQuery/test8.jsp</h1>
	<div id="wrap">
		<form action="test8_result.jsp">
			<div id="inputBox">
				아이디 : <input type="text" name="id"><br>
				패스워드 : <input type="password" name="password">
			</div>
			<select id="selectBox" name="subject">
				<option value="선택하세요">선택하세요</option>
				<option value="자바">자바</option>
				<option value="JSP">JSP</option>
				<option value="스프링">스프링</option>
			</select>
			<br>
			<input type="submit" value="전송">
		</form>
	</div>
	
</body>
</html>
```
### on 함수
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
	$(function() {
		/*
		DOM 객체에 대한 탐색 및 접근을 통해 조작하는 공통적인 이벤트
		=> on("이벤트명", "함수") 형식을 사용
		
		1. 클릭 동작에 대한 이벤트 : on("click", 함수)
		- 특정 대상 클릭 시 동작하는 이벤트
		- $("선택자").click(함수) 이벤트와 동일함
		- 클릭 대상에 제한 없음(버튼 외에 어떤 요소도 사용 가능)
		*/
		
		// 확인 버튼에 click 이벤트 연결
		// 1) click() 함수 사용 시
// 		$("#btnOk").click(function() {
// 			alert("확인 버튼 클릭!");
// 		});
		
		// 2) on() 함수 사용 시
		$(":button").on("click", function() {
// 			alert("확인 버튼 클릭! - on()");
			// 아이디, 패스워드 값을 textarea 에 출력
			var id = $("input[name=id]").val()
			var password = $("input[name=password]").val() 
			var subject = $("#selectBox > option:selected").val();
			$("#resultArea").html( 
				"아이디 : " + id + "\n" + 
				"패스워드 : " + password + "\n" +
				"선택과목 : " + subject
			);
		});
		
		// 버튼 외의 항목도 클릭 이벤트 처리 가능
		$("#clickDiv").on("click", function() {
			alert("div 태그 클릭됨!");
		});
		
		// 아이디 입력란에 키보드를 눌러 데이터 입력 시 처리 이벤트 = "keyup"
		$("input[name=id]").on("keyup", function() {
			var id = $("input[name=id]").val();
			$("#spanId").html(id);
		});
		
		// 특정 요소에 포커스가 주어질 때(focus)와 포커스가 해제될 때(blur) 동작하는 이벤트
// 		$("#resultArea").on("focus", function() {
// 			$("#resultArea").html("textarea focus in").css("background", "skyblue");
// 		});
		
// 		$("#resultArea").on("blur", function() {
// 			$("#resultArea").html("textarea focus out").css("background", "white");
// 		});
		
// 		$("#resultArea").on("focus", function() {
// 			$("#resultArea").html("textarea focus in").css("background", "skyblue");
// 		});
		
		// 복수개의 이벤트를 하나의 함수로 처리할 경우(이벤트명을 차례대로 기술 - 공백으로 구분)
// 		$("#resultArea").on("focus blur", function() {
// 			$("#resultArea").html("포커스 상태 변경");
// 		});
		
		// 복수개의 이벤트를 각각의 함수로 처리할 경우(on() 함수 내에서 {이벤트명:함수()} 형태로 작성)
		$("#resultArea").on({
			// 이벤트명: function() {수행할 동작},
			// 이벤트명: function() {수행할 동작}...
			focus: function() {
	 			$("#resultArea").html("textarea focus in").css("background", "skyblue");
			},
			blur: function() {
	 			$("#resultArea").html("textarea focus out").css("background", "white");
			},
			mouseenter: function() {
	 			$("#resultArea").html("textarea mouse enter").css("background", "orange");
			},
			mouseleave: function() {
	 			$("#resultArea").html("textarea mouse leave").css("background", "yellow");
			}
		});
		
		// off() 함수를 사용하면 등록된 이벤트 제거 가능(= binding 된 이벤트를 unbindimg)
		// => $("선택자").off("이벤트명")
		// 버튼 클릭 시 확인 버튼 동작을 수행하지 못하도록 이벤트 제거
		$("#btnStopEvent").on("click", function() {
			$("#btnOk").off("click").val("버튼 클릭 불가능 처리됨");
		});
		
	});
</script>
</head>
<body>
	<h1>jQuery/test8.jsp</h1>
	<div id="wrap">
		<div id="inputBox">
			아이디 : <input type="text" name="id"><span id="spanId"></span><br>
			패스워드 : <input type="password" name="password">
		</div>
		<select id="selectBox" name="subject">
			<option value="선택하세요">선택하세요</option>
			<option value="자바">자바</option>
			<option value="JSP">JSP</option>
			<option value="스프링">스프링</option>
		</select>
		<br>
		<textarea id="resultArea" rows="5"></textarea>
		<br>
		<input type="button" value="확인" id="btnOk">
		<input type="button" value="취소" id="btnCancel">
		<input type="button" value="확인 버튼 클릭 기능 중지" id="btnStopEvent">
	</div>
	====<div id="clickDiv">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</div>====
	
</body>
</html>
```
### each 함수
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
	$(function() {
		$("table").css("width", "300px").css("text-align", "center");
		
		
		// on("change", 함수) 이벤트를 통한 체크박스 상태 변경 처리
		// => 특정 대상의 '상태가 변하면' 동작하는 이벤트
		
		// each() 함수
		// => 지정된 대상에 대한 반복 접근 수행하는 함수
		// => 함수 내부에 익명함수 구현을 통해 인덱스와 요소를 전달받을 수 있음
		//	  $("선택자").each(function(index, item) {})
		// 	  => 각 요소에 접근하여 해당 요소의 인덱스와 요소 자체를 전달받음
		
		// #allCheck 체크박스 체크 상태에 따라 다른 체크박스 체크 또는 해제
		$("#allCheck").on("change", function() {
			// 전체선택 체크박스에 대한 체크 상태 판별(prop() 또는 is() 활용 => true/false 리턴)
			if($("#allCheck").is(":checked")) { // $("#allCheck").prop("checked") 동일함
// 				alert("전체선택 체크됨!");
				// 체크박스에 대한 전체 요소 반복 - each() 함수 활용
				$(":checkbox").each(function (index, item) {
// 					alert(index + " : " + $(item).prop("checked"))
					// 전체선택 체크박스를 제외한 나머지 체크박스를 체크(= 체크상태를 true 로 변경)
					// => prop() 함수의 파라미터로 "속성명" 과 함께 값을 전달 시 속성값 변경 가능
					// index 값이 0 보다 클 경우에만 체크 수행(전체선택 제외시킴
					if(index > 0) {
					$(item).prop("checked", true);
					}
				});
			} else {
// 				alert("전체선택 체크 해제됨!");
				// 체크박스 전체를 체크 해제
				$(":checkbox").each(function (index, item) {
					$(item).prop("checked", false);
				});
			}
		});
				
		// -----------------------------------------------------
		// target 콤보박스의 change 이벤트 바인딩
		$("#selectBox").on("change", function() {
			alert("selectBox 변경됨! - " + $("#selectBox").val());
		});
		
		// searchType 콤보박스의 change 이벤트 바인딩
		$("#searchType").on("change", function() {
			alert("searchType 변경됨! - " + $("#searchType").val());
		});
		
		// searchKeyword 인풋박스의 change 이벤트
		// => 데이터 입력받는 인풋박스의 change 이벤트는 입력이 끝난 후 포커스 해제될 때 동작
		//	  (= blur 이벤트와 유사하지만, 내용 변경이 있을 때만 동작함)
		$("#searchKeyword").on("change", function() {
			alert("searchKeyword 변경됨! - " + $("searchKeyword").val())
		});
	});
</script>
</head>
<body>
	<h1>jQuery/test10.jsp</h1>
	<table border="1">
		<tr>
			<td colspan="3">
				<select id="selectBox" name="target">
					<option value="전체">전체</option>
					<option value="VIP">VIP</option>
					<option value="일반">일반</option>
				</select>
			</td>
		</tr>
        <tr>
            <th><input type="checkbox" id="allCheck"></th>
            <th>번호</th>
            <th>이름</th>
        </tr>
        <tr>
            <td><input type="checkbox" id="check1" name="check" checked="checked" value="1"></td>
            <td>1</td>
            <td>홍길동</td>
        </tr>
        <tr>
            <td><input type="checkbox" name="check" value="2"></td>
            <td>2</td>
            <td>이순신</td>
        </tr>
        <tr>
            <td><input type="checkbox" name="check" value="3"></td>
            <td>3</td>
            <td>강감찬</td>
        </tr>
       <tr>
			<td colspan="3">
				<select id="searchType" name="target">
					<option value="name">이름</option>
					<option value="id">아이디</option>
				</select>
				<input type="text" id="searchKeyword" name="search" placeholder="검색어 입력">
			</td>
		</tr>
    </table>
</body>
</html>
```
