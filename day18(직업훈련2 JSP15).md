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
- 다양한 이벤트 활용
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
