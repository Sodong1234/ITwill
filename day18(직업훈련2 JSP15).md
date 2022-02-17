# [오전수업] 직업훈련 2차
---
# [오후수업] JSP 15차
## 이벤트
- 특정 대상(요소)에 어떤 사건(행위)이 발생하는 것(= 신호)
  - ex) 마우스 '클릭', 키보드 '키 누름', 마우스 '커서 오버', 대상을 '선택' 등
- 이벤트 발생 시 해당 이벤트에 대한 감지를 통해 이벤트 처리 해야함
  => 이벤트 발생 시 특정 동작을 수행하는 것을 이벤트 처리(Event Handling) 라고 함
  - ex) '버튼'을 '클릭' 하면 이벤트 발생 이라는 '메세지 출력'
- 이벤트 핸들링을 위해서는 이벤트 감지에 필요한 onXXX 속성을 대상 태그에 지정하고 해당 이벤트가 발생하면 수행할 작업을 속성값으로 지정해야함
  - ex) <button onclick="alert('이벤트 발생')"> => 버튼 클릭 시 "이벤트 발생" 메세지 출력됨
	  - click : 마우스 클릭, dblclick : 마우스 더블클릭
	  - mouseover : 마우스가 대상 위에 위치(= mouseenter 와 유사), mouseout : 마우스 대상 빠져나감
	  - focus : 대상이 선택됨(= 포커스가 위치), blur : 대상 선택이 해제됨(= 포커스 잃음)
	  - load : 대상 로딩 완료됨, unload : 대상 로딩 해제됨
	  - keydown : 키보드의 키 누름, keyup : 키보드의 키 뗌
	  - submit : 폼 데이터 전송, reset : 폼 데이터 초기화
  
  
### 이벤트 핸들러  
- 이벤트 발생 시 수행할 동작을 기술해 놓은 함수 또는 객체
- 이벤트가 발생되면 자동으로 onXXX 속성에 해당하는 함수 또는 객체 실행
---
- body 로딩 완료 메세지 출력하기	
```javascript

<title>Insert title here</title>
<script type="text/javascript">

	
	
1번방법.	
	// 현재 페이지의 body 영역 로딩이 완료되면 "body 로딩 완료!" 메세지 출력
// 	window.onload = function() {
// 		alert("body 로딩 완료!")
// 	}
	
</script>
</head>

2번방법.
<body onload="alert('body 로딩 완료!')" >
	<h1>자바스크립트 이벤트 - test11.html</h1>
</body>
```

- 버튼 클릭됨 메세지 출력하기
```javascript
<title>Insert title here</title>
<script type="text/javascript">
	  function clickButton() {
		  alert("버튼2 클릭됨!")
	  }
</script>
</head>
<body onload="alert('body 로딩 완료!')" >
	<h1>자바스크립트 이벤트 - test11.html</h1>
	
	<!-- "클릭" 버튼 클릭 시 alert() 함수를 사용하여 "버튼 클릭됨!" 출력 -->
	<input type="button" value="클릭" onclick="alert('버튼 클릭됨!')">
	
	<!-- "클릭2" 버튼 클릭 시 clickButton() 함수를 호출하여 "버튼2 클릭됨!" 출력 -->
	<input type="button" value="클릭2" onclick=clickButton();>
	
</body>
</html>
```  
### 다양한 이벤트 활용
```javascript
<script type="text/javascript">
	  function changeImage() {
		  // "img" 라는 id 선택자를 사용하여 해당 요소(태그 객체) 가져오기
		  var imgElem = document.getElementById("img")
		  // 전달받은 img 태그 객체의 src 속성값을 다른 이미지("away.jpg") 로 변경
		  imgElem.src = "./away.jpg"
	  }
	  
	  
	  
	  
	   function changeImage2() {
		  // "img" 라는 id 선택자를 사용하여 해당 요소(태그 객체) 가져오기
		  var imgElem2 = document.getElementById("img")
		  // 전달받은 img 태그 객체의 src 속성값을 원래 이미지("australian.jpg") 로 변경
		  imgElem2.src = "./australian.jpg"
	  }
	  
	  
	  
	  
	  // 이름 창에 대한 색상 변경 처리를 위해 색상명을 파라미터로 전달받는 함수 정의
	  function changeColor(color) {
		  var textElem = document.getElementById("name")
		  // 전달받은 텍스트 입력창의 배경색을 전달받은 색상명으로 변경
		  // => 객체명.style.background 속성 사용
		  textElem.style.background = color;
	  }
	  
	  
	  
	  // 나이 창에 대한 색상 변경 처리를 위해 나이 창 객체와 색상명을 파라미터로 전달받는 함수 정의
	  function changeColor2(elem, color) {
		  elem.style.background = color;
	  }
	  
	  
	  
	  // 테이블 행 마우스오버에 대한 색상 변경
	  function changeColorTr(elem, color) {
		  elem.style.background = color;
	  }
	  
	  
	  
	  // 테이블 셀 클릭 시 셀 명을 
	  function printTd(elem) {
		  alert("선택한 셀 : " + elem.innerHTML)
	  }
	  
	  
	  
	  // 함수 호출 시 this 지정할 경우 해당 태그 객체를 전달하고
	  // this. value 지정할 경우 해당 태그의 value 속성값을 전달
	  function checkId(id) { // 입력된 아이디가 전달됨
		  // "checkResult" id 선택자를 사용 하여 span 태그 객체 가져오기
		  var tagSpan = document.getElementById("checkResult")
		  // 입력받은 아이디(id 변수값)가 "admin" 인지 판별
		  if (id == "admin") {
			  // span 태그 영역에 "사용 불가능한 아이디" 텍스트 표시(텍스트 색상을 "RED" 로 지정)
			  tagSpan.innerHTML = "사용 불가능한 아이디"
			  tagSpan.style.color = "RED";
			  
		  } else {
			// span 태그 영역에 "사용 불가능한 아이디" 텍스트 표시(텍스트 색상을 "RED" 로 지정)
			  tagSpan.innerHTML = "사용 가능한 아이디"
			  tagSpan.style.color = "GREEN";
		  }
	  }
</script>
</head>
<body>
	<h1>자바스크립트 이벤트 - test11.html</h1>
	
	<!-- "클릭" 버튼 클릭 시 alert() 함수를 사용하여 "버튼 클릭됨!" 출력 -->
	<input type="button" value="클릭" onclick="alert('버튼 클릭됨!')">
	
	<!-- "클릭2" 버튼 클릭 시 clickButton() 함수를 호출하여 "버튼2 클릭됨!" 출력 -->
	<input type="button" value="클릭2" onclick=clickButton();>
	
	<!-- "마우스를 가져다 대세요" 버튼에 mouseover, mouseout 이벤트 처리 -->
	<input type="button" value="마우스를 가져다 대세요" onmouseover="alert('마우스 오버')" onmouseout="alert('마우스 아웃')">
	
	<!-- "이미지교체" 버튼 클릭 시 표시된 이미지를 다른 이미지로 교체 -->
	<img src="./australian.jpg" id="img" width="200" height="150"><br>
	<input type="button" value="이미지교체" onclick="changeImage()">
	
	<!-- 이름 입력창에 포커스가 위치하면 "SKYBLUE" 색상으로 변경, 포커스 잃으면 "WHITE"로 변경 -->
	<!-- 동일한 색상 변경 작업이므로 하나의 함수를 호출하며, 호출 시 색상명을 파라미터로 전달하여 구분 -->
	이름 : <input type="text" id="name" onfocus="changeColor('SKYBLUE')" onblur="changeColor('WHITE')">
	나이 : <input type="text" id="age" onfocus="changeColor2(this,'#FFCCCC')" onblur="changeColor2(this,'WHITE')">
	
	
	
	
	<table border="1">
		<tr>
			<th colspan="4">테이블연습</th>
		</tr>
		<!-- 테이블의 행(tr)에 마우스를 가져다 대면 changeColorTr() 함수 호출 -->
		<!-- 함수 호출 시 파라미터에 현재 tr 태그 객체와 변경할 색상을 전달 -->
		<!-- 마우스를 가져다 대면 '#FFCCCC' 색상으로 변경, 빼면 'WHITE' 로 변경 -->
		<tr onmouseover="changeColorTr(this, '#FFCCCC')"
			onmouseout="changeColorTr(this, '#FFFFFF')">
			<td>1-1</td>
			<td>1-2</td>
			<td>1-3</td>
			<td>1-4</td>
		</tr>
		<tr onmouseover="changeColorTr(this, '#FFCCCC')"
			onmouseout="changeColorTr(this, '#FFFFFF')">
			<td>2-1</td>
			<td>2-2</td>
			<td>2-3</td>
			<td>2-4</td>
		</tr>
		<tr onmouseover="changeColorTr(this, '#FFCCCC')"
			onmouseout="changeColorTr(this, '#FFFFFF')">
	<td>3-1</td>
		<td>3-2</td>
		<td>3-3</td>
		<td>3-4</td>
		</tr>
		</table>
		
		
		
		
		
		
		<table border="1">
		<tr>
			<th colspan="4">테이블연습</th>
		</tr>
		<!-- 테이블의 셀(td)에 마우스를 클릭하면 printTd() 함수 호출 -->
		<!-- 함수 호출 시 파라미터에 현재 td 태그 객체의 이름을 전달 -->
		<tr>
			<td>1-1</td>
			<td>1-2</td>
			<td>1-3</td>
			<td>1-4</td>
		</tr>
		<tr>
			<td>2-1</td>
			<td>2-2</td>
			<td>2-3</td>
			<td>2-4</td>
		</tr>
		<tr>
			<td onclick="printTd(this)">3-1</td>
			<td onclick="printTd(this)">3-2</td>
			<td onclick="printTd(this)">3-3</td>
			<td onclick="printTd(this)">3-4</td>
		</tr>
	</table>
	
	
	<!-- 아이디 입력란에 글자를 입력할 때마다 아이디 비교하여 결과를 span 영역에 출력하기 -->
	<!-- 함수 호출 시 파라미터로 입력된 텍스트를 전달하려면 this.value 사용(value 속성값 지정) -->
	아이디 : <input type="text" id="id" onkeyup="checkId(this.value)" placeholder="아이디 입력">
	<span id="checkResult"></span>
</body>
```

### 다양한 이미지 활용
```javascript
문서 내의 이미지(img 태그)에 접근하는 방법



<head>
<meta charset="UTF-8">
<title>test12.html</title>
<script type="text/javascript">
	/*
	문서 내의 이미지(img 태그)에 접근하는 방법
	1. document.getElementById() 함수 또는 document.getElementsByTagName() 함수 사용
	2. 이미지(img 태그)에 지정된 name 속성명으로 접근하는 방법
	   => document.name속성값.속성명 : 지정된 name속성값에 해당하는 이미지의 특정 속성에 접근
	   ex) document.img1.src : "img1" 이라는 name 속성값에 해당하는 태그의 src 속성에 접근
	3. 이미지(img 태그) 전체(= 복수개)를 배열 형태로 접근하는 방법
	   => document.images[인덱스].속성명 : 복수개의 이미지 중 인덱스에 해당하는 이미지 속성에 접근
	   ex) document.images[0].src : 첫번째 img 태그의 
	   
	*/
function func1() {
// 	document.write("src : " + document.img1.src + "<br>");
// 	document.write("width : " + document.img1.width + "<br>");
// 	document.write("height : " + document.img1.height + "<br>");
// 	document.write("border : " + document.img1.border + "<br>");

	var imgInfo = "src : " + document.img1.src + "\n"
				+ "width : " + document.img1.width + "\n"
				+ "height : " + document.img1.height + "\n"
				+ "border : " + document.img1.border + "\n";
	
	alert(imgInfo);
}



function func2() {
	// img1 이미지(1.jpg) 파일을 2.jpg 로 변경
	// 배열 형태로 접근할 경우
	document.images[0].src = "2.jpg";
	document.images[0].width = "300";
	document.images[0].height = "300";
	document.images[0].title = "2.jpg";
}


function changeImage(tagImg, img) {
	// this 미전달 시 : document.img2.xxx 속성으로 접근 가능
	// ex) document.img2.src = img;
	
	// this 전달 시 
	tagImg.src = img;
}


</script>
</head>
<body>
	<h1>test12.html</h1>
	<img src="1.jpg" name="img1" width="200" height="200" border="2" title="펭수" alt="펭수없음"><br>
	<input type="button" value="이미지 속성정보 출력" onclick="func1()">
	<input type="button" value="이미지 속성정보 변경" onclick="func2()"><br>
	<hr>
	<img src="2.jpg" name="img2" width="200" height="200" onclick="changeImage(this, '3.jpg')" onmouseover="changeImage(this, '4.jpg')" onmouseout="changeImage(this, '5.jpg')"><br>
	<!-- 2.jpg 이미지 클릭 시 3.jpg 로 변경, 마우스 가져다 대면 4.jpg, 마우스 빼면 5.jpg 로 변경 -->
</body>
</html>
```
### form 태그 활용
- 어떤 입력 데이터들(텍스트박스, 라디오버튼, 체크박스)을 하나의 묶음으로 관리하여 특정 페이지로 이동(포워딩) 시 파라미터 형태로 전달하는 역할의 태그
	- ex) 회원가입 페이지의 입력 내용을 하나의 form 태그로 처리
- 주로 input type="XXX" 형식의 입력 데이터 폼을 하나로 묶어주는 역할 수행
- input type="submit" 버튼을 사용하면, 일반 버튼과 달리 클릭 시 form 태그의 action 속성에 지정된 페이지로 이동하면서, 입력받은 데이터를 모두 파라미터로 전달
	- => 자바스크립트에서 submit() 함수를 통해 동일한 작업 처리 가능
- form 태그에 onsubmit 속성을(이벤트) 지정 시 submit 동작이 수행될 때 이벤트 처리 수행됨
	- => 이 때, true 또는 false 값을 리턴하면 페이지 이동 여부를 결정할 수 있음
	- ex) onsubmit="return checkForm()"
		- => checkForm() 함수를 호출하여 작업을 수행하고, true 또는 false 값을 리턴받아서 submit 작업을 실행할지 여부 결정할 수 있음

```javascript
[ 기본 문법 ]
	<form action="이동할 페이지의 URL" method="이동할 방식(메서드)" name="폼이름">
		// 폼 파라미터에서 입력받을 데이터들의 입력 태그
		<input type="submit" value="XXX">
	</form>
```
```javascript
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<script type="text/javascript">
	// form 태그 내의 각 요소에 접근하는 방법
	// document.form태그name속성값.접근할태그name속성값.속성명
	// => 속성명 : name, type, value 등
	// => 함수 : focus()  : 대상에 커서 요청
	// 			blur()   : 대상에서 커서 해제
	//			select() : 대상에 커서 요청(값 블럭지정)
function requestFocus() {
	// 폼 태그(name 속성값이 fr) 내의 아이디 입력받는 텍스트 박스(name 속성값이 id)에 접근
	document.fr.id.focus();
}



function print() {
		
	// 폼 태그에 입력된 데이터(아이디, 패스워드, 자기소개)를 가져와서 출력
	// => textarea 태그는 value 속성을 사용하지 않지만, 자바스크립트로 가져올 때는 document 객체를 통해 textarea 의 value 속성값 접근 가능
	
	alert("아이디 : " + document.fr.id.value + "\n" + "패스워드 : " + document.fr.passwd.value + "\n" + "자기소개 : " + document.fr.ta.value);
	}
	
	
	
function check() {
	// 폼 태그에 입력 항목 중에서 입력되지 않은 항목이 존재하는지 판별하는 방법
	// 1) 해당 태그의 value 속성값이 널스트링("")과 같은지 판별
	// 2) 해당 태그의  "" 에 대한 길이(length)가 0 인지 판별
	
// 	if(document.fr.id.value == "") { // 아이디가 입력되지 않은 경우
// 		alert("아이디를 입력하세요!"); // 경고메세지 출력
// 	} else if (document.fr.passwd.value == "") { // 패스워드가 입력되지 않은 경우
// 		alert("패스워드를 입력하세요!");
// 	} else if (document.fr.ta.value == "") { // 자기소개가 입력되지 않은 경우
// 		alert("자기소개를 입력하세요!");
// 	} else {
// 		document.fr.submit();
//  }
	
		if(document.fr.id.value.length == 0) { // 아이디가 입력되지 않은 경우
		alert("아이디를 입력하세요!"); // 경고메세지 출력
		// 아이디 입력 창에 커서 요청
		document.fr.id.focus();
		// 현재 함수 실행을 종료하고 빠져나가기(if문 외부의 submit() 함수가 실행되지 않도록 차단)
		return;
		
	} else if (document.fr.passwd.value.length == 0) { // 패스워드가 입력되지 않은 경우
		alert("패스워드를 입력하세요!");
		// 패스워드 입력 창에 커서 요청
		document.fr.passwd.focus();
		// 현재 함수 실행을 종료하고 빠져나가기(if문 외부의 submit() 함수가 실행되지 않도록 차단)
		return;		
	} else if (document.fr.ta.value.length == 0) { // 자기소개가 입력되지 않은 경우
		alert("자기소개를 입력하세요!");
		// 자기소개 입력 창에 커서 요청
		document.fr.ta.focus();
		// 현재 함수 실행을 종료하고 빠져나가기(if문 외부의 submit() 함수가 실행되지 않도록 차단)
		return;
	} 
		
	// 만약, 모든 항목에 대한 입력이 완료되었을 경우
	// 자바스크립트 함수에서 submit 기능 수행 할 수 있다! 
	// => 대상 폼 객체에 대해 submit() 함수 호출
	
	document.fr.submit();
	// => 주의! if문 외부에서 submit() 함수를 호출할 경우 입력값이 없을 경우에도 submit() 함수가 실행 될 수 있으므로
	//    입력값이 있을 경우에만 실행 되도록 수정해야함
	// 1) if문 마지막에 else 문을 통해 모든 값이 입력되면 submit() 함수 호출
	// 2) 각 if문마다 현재 함수를 종료하고 빠져나가도록 return 문 사용
}
	
	
	
</script>
</head>
<body>
	<!--
	form 태그
	- 어떤 입력 데이터들(텍스트박스, 라디오버튼, 체크박스)을 하나의 묶음으로 관리하여
	  특정 페이지로 이동(포워딩) 시 파라미터 형태로 전달하는 역할의 태그
	  ex) 회원가입 페이지의 입력 내용을 하나의 form 태그로 처리
	- 주로 input type="XXX" 형식의 입력 데이터 폼을 하나로 묶어주는 역할 수행
	- input type="submit" 버튼을 사용하면, 일반 버튼과 달리 클릭 시 
	  form 태그의 action 속성에 지정된 페이지로 이동하면서, 입력받은 데이터를 모두 파라미터로 전달
	  => 자바스크립트에서 submit() 함수를 통해 동일한 작업 처리 가능
	  - form 태그에 onsubmit 속성을(이벤트) 지정 시 submit 동작이 수행될 때 이벤트 처리 수행됨
	  => 이 때, true 또는 false 값을 리턴하면 페이지 이동 여부를 결정할 수 있음
	  ex) onsubmit="return checkForm()"
	  	  => checkForm() 함수를 호출하여 작업을 수행하고, true 또는 false 값을 리턴받아서
	  	  	 submit 작업을 실행할지 여부 결정할 수 있음
	
	[ 기본 문법 ]
	<form action="이동할 페이지의 URL" method="이동할 방식(메서드)" name="폼이름">
		// 폼 파라미터에서 입력받을 데이터들의 입력 태그
		<input type="submit" value="XXX">
	</form>   
	 -->
	<h1>test13.html - form 태그 이벤트</h1>
	<form action="./test13-2.html" name="fr">
		아이디 <input type="text" name="id" required="required">
		<input type="button" value="focus()" onclick="requestFocus()">
		<!-- 버튼 클릭 시 폼 태그의 요소에 접근하여 작업 직접 수행 가능 -->
		<input type="button" value="blur()" onclick="document.fr.id.blur">
		<input type="button" value="select()" onclick="document.fr.id.select">
		<br>
		<!-- 패스워드 <input type="text" name="passwd"> // 입력하는 패스워드가 노출됨 -->
		패스워드 <input type="password" name="passwd" required="required"> <!-- 입력하는 패스워드가 노출 X -->
		<br>
		<!-- textarea 태그는 input type="text" 태그와 달리 여러 줄 입력(줄바꿈) 가능한 태그 -->
		자기소개 <textarea rows="5" cols="20" name="ta" required="required">자기소개</textarea>
		<br>
		<input type="button" value="입력값 출력" onclick="print()">
		<input type="button" value="입력값 확인" onclick="check()">
		<input type="submit" value="가입">
	</form>
	
	
</body>
</html>
```
