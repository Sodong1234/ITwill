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

