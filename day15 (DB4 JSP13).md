# [오전수업] DB 4차

> 저번 시간에 만들어놓은 테이블에서 추가 진행
```
+------------+-------------+------+-----+---------+-------+
| Field      | Type        | Null | Key | Default | Extra |
+------------+-------------+------+-----+---------+-------+
| stu_no     | int         | YES  |     | NULL    |       |
| last_name  | varchar(20) | YES  |     | NULL    |       |
| birth_date | date        | YES  |     | NULL    |       |
+------------+-------------+------+-----+---------+-------+
```

## DML (Data Manipulation Language) / 데이터 조작어
- 데이터를 입력(INSERT), 갱신(UPDATE), 삭제(DELETE) 할 수 있는 문법


### INSERT 문법
- 테이블에 데이터를 입력하는 문법



```
- INSERT INTO 테이블명 [(칼럼명1[, …])]
  VALUES (데이터1 [, …]);
  
- values절에서 숫자 데이터는 숫자로만 작성, 문자열 데이터는 ''로 묶어서 작성
- 컬럼에는 컬럼이 가지는 데이터타입의 데이터들로만 값을 채울 수 있다.
```
```
INSERT INTO student (stu_no, last_name) 입력
VALUES (1, 'hi'); 입력



- 입력된 데이터 확인
SELECT * FROM student; 입력

+--------+-----------+------------+
| stu_no | last_name | birth_date |
+--------+-----------+------------+
|      1 | hi        | NULL       |
+--------+-----------+------------+

---------------------------------------------------------------------------------------------------------------

INSERT INTO student (stu_no, last_name) 입력
VALUES (2, '6'); 입력


SELECT * FROM student; 입력

+--------+-----------+------------+
| stu_no | last_name | birth_date |
+--------+-----------+------------+
|      1 | hi        | NULL       |
|      2 | 6         | NULL       |
+--------+-----------+------------+


---------------------------------------------------------------------------------------------------------------


INSERT INTO student (last_name, stu_no) 입력
VALUES ('Kim', 'k'); 입력

ERROR 1366 (HY000): Incorrect integer value: 'k' for column 'stu_no' at row 1 INSERT INTO student (last_name, stu_no) 
// 타입에 맞지 않는 데이터를 입력시 에러 발생!


VALUES ('Kim', 3); 입력


SELECT * FROM student; 입력

+--------+-----------+------------+
| stu_no | last_name | birth_date |
+--------+-----------+------------+
|      1 | hi        | NULL       |
|      2 | 6         | NULL       |
|      3 | Kim       | NULL       |
+--------+-----------+------------+

---------------------------------------------------------------------------------------------------------------

- 컬럼의 목록을 생략하는 경우 테이블이 가진 컬럼의 순서와 갯수에 맞춰서 values절의 내용을 작성한다.
- 입력할 값이 없는 경우 NULL을 입력할 수 있다.

INSERT INTO student 입력
VALUES (4, 'bye', NULL); 입력

SELECT * FROM student; 입력

+--------+-----------+------------+
| stu_no | last_name | birth_date |
+--------+-----------+------------+
|      1 | hi        | NULL       |
|      2 | 6         | NULL       |
|      3 | Kim       | NULL       |
|      4 | bye       | NULL       |
+--------+-----------+------------+

---------------------------------------------------------------------------------------------------------------

- 함수와 같은 연산구조를 통한 값의 입력도 가능하다.

INSERT INTO student 입력
VALUES (5, 'Buy', now()); 입력

SELECT * FROM student; 입력

+--------+-----------+------------+
| stu_no | last_name | birth_date |
+--------+-----------+------------+
|      1 | hi        | NULL       |
|      2 | 6         | NULL       |
|      3 | Kim       | NULL       |
|      4 | bye       | NULL       |
|      5 | Buy       | 2022-02-11 |
+--------+-----------+------------+

---------------------------------------------------------------------------------------------------------------

INSERT INTO student 입력
VALUES (6, 'LEE', '2022-02-14'); 입력

SELECT * FROM student; 입력

+--------+-----------+------------+
| stu_no | last_name | birth_date |
+--------+-----------+------------+
|      1 | hi        | NULL       |
|      2 | 6         | NULL       |
|      3 | Kim       | NULL       |
|      4 | bye       | NULL       |
|      5 | Buy       | 2022-02-11 |
|      6 | LEE       | 2022-02-14 |
+--------+-----------+------------+


```

### UPDATE 문법
- 기존 데이터의 값을 갱신할 때 사용하는 문법

```
- UPDATE 테이블명
  SET 갱신컬럼 = 갱신할 값
  [WHERE 조건컬럼 = 조건값];
	
* WHERE절에 대한 내용은 SELECT 구문에서 다룰 예정

```
```
+--------+-----------+------------+
| stu_no | last_name | birth_date |
+--------+-----------+------------+
|      1 | hi        | NULL       |
|      2 | 6         | NULL       |
|      3 | Kim       | NULL       |
|      4 | bye       | NULL       |
|      5 | Buy       | 2022-02-11 |
|      6 | LEE       | 2022-02-14 |
+--------+-----------+------------+

student 테이블에서 stu_no 컬럼의 값이 3인 행에 대해서 last_name컬럼의 값을 'Gim'으로 갱신


UPDATE student 입력
SET last_name = 'GIM' 입력
WHERE stu_no = 3; 입력

SELECT * FROM student; 입력

+--------+-----------+------------+
| stu_no | last_name | birth_date |
+--------+-----------+------------+
|      1 | hi        | NULL       |
|      2 | 6         | NULL       |
|      3 | Gim       | NULL       |
|      4 | bye       | NULL       |
|      5 | Buy       | 2022-02-10 |
|      6 | LEE       | 2022-02-14 |
+--------+-----------+------------+
```

---

# [오후수업] JSP 14차

## 자바스크립트 내장 객체
- 자바 스크립트에서 미리 정의하여 제공하는 객체
- 별도의 선언 없어도 사용 가능


### window 객체 
- 자바스크립트의 최상위(루트) 객체
- 자바스크립트 전역(전체)에서 접근(사용) 가능한 객체
- 웹브라우저 창(browser window) 을 가리키는 용도로 사용되며 창에 대한 다양한 작업을 수행 할 수 있는 여러가지 함수, 속성 제공
  - window 객체 접근 시 객체명은 생략 가능

```javascript
function showWindowInfo() {
	//window 객체 내의 속성 사용 => 현재 창(window)에 대한 기본 정보 확인 및 제어도 가능
	alert("창 높이 : " + window.innerHeight + ", 창 너비 : " + window.innerWidth)
}
function openWindow() {
	// window 객체의 함수 호출(windiw.함수명())
	// window.open() 함수 : 새 창 열기
	// => window.open("새 창에서 표시할 URL", "창 이름", "창 옵션(크기, 위치 등)")
// 	window.open(); // 창 옵션 중 창 크기 생략 시 기존 창에 새로운 탭 열림
	window.open("", "", "width=300,height=300"); // 가로 300px, 세로 300px 크기의 새 창 열름
}

function openWindow2() {
	// window.open() 함수 첫 번째 파라미터로 URL 지정 시 해당 웹사이트가 새 창에서 열림
	window.open("http://www.naver.com", "", "width=600, height=300");
}

function closeWindow() {
	// window.close() 함수 : 현재 창 닫기
	window.close();
}

function openWindow3() {
	window.open("test3_new_window.html", "", "width=300, height=200")
}
</script>
</head>
<body>
	<h1>window 내장 객체</h1>
	<input type="button" value="창 크기 확인" onclick="showWindowInfo()"><br>
	<input type="button" value="새 창 열기" onclick="openWindow()"><br>
	<input type="button" value="새 창 열기2" onclick="openWindow2()"><br>
	<input type="button" value="현재 창 닫기" onclick="closeWindow()"><br>
	<input type="button" value="test3_new_window.html 열기" onclick="openWindow3()"><br>
  
  ------------------------------------test_new_wiondow.html----------------------------------------------
  // test3.html에서 현재 파일은 test3_new_window.html 파일을 사용하여 새 창 열어놓은 상태
	// 이 때, test3.html 파일은 부모창이고, test3_new_window.html 파일을 자식창이라고 했을 때
	// 자식창에서 부모창의 요소(객체)에 접근하는 방법 : window.opener 객체 사용
	function showMessage() {
		alert("자식창 메세지 출력")
	}
	
	function showMessage2() {
		// 부모창에서 alert() 함수를 호출하기
		window.opener.alert("자식창에서 요청한 메세지 출력")
		// => 주의! 부모창(test3.html)에 포커스가 위치해야하므로 부모창 클릭 필요
		//	  단, 포커스를 강제로 부모창으로 이동하거나 자식창을 닫으면 자동으로 출력됨
		
		// 현재 자식 창 닫기
		window.close();
	}
</script>
</head>
<body>
	<h1>test3.html에 의해 새로 열린 창</h1>
	<input type="button" value="자식창 메세지 출력" onclick="showMessage()"><br>
	<input type="button" value="부모창 메세지 출력" onclick="showMessage2()"><br>
------------------------------------test_new_wiondow.html----------------------------------------------
```


### window 객체의 하위 객체들 
1. document 객체
2. navigator, location, history 등
3. object, array, function 등

**[ loaction 객체 ]**
- 페이지 이동 관련 정보를 관리하는 객체(= 페이지 이동 관련 작업 담당)
	- window 객체의 하위 객체이므로 window.location 형식으로 접근해야하지만 보통 window 객체명읜 생략하고 location.xxx 형식으로 사용 가능
- 페이지 관련 속성(변수) 및 함수가 제공됨
- 주로 사용하는 속성 

```javascript
	function func1() {
		// URL 정보 확인을 위해 location 객체의 href 속성값 확인
		alert(location.href);

		//func3() 함수의 새로고침 기능 확인을 위해 임시로 배경색 변경
		document.body.style.background = "GRAY"
	}

	function func2() {
		// URL 정보 변경을 위해 location 객체의 href 속성값 변경
		// => 변경되는 주소로 새로운 요청 발생 = 해당 주소로 이동(포워딩)
		// 		location.href = "test3.html" // test3.html 페이지(파일)로 이동
		location.href = "http://www.naver.com"

	}

	function func3() {
		// URL 정보 새로고침을 위해 location 객체의 reload() 함수 호출
		// => URL 정보에 해당하는 페이지 다시 로딩 = 페이지 새로고침(F5키) 기능과 동일
		location.reload();
	}

	// 버튼 클릭 시 파라미터를 전달받은 func4() 함수 정의
	// => 전달받는 문자를 target 매개변수를 선언하여 저장
	function func4(target) {
		// 		alert(target)
		// 		location.href = "";
		
		
	// 전달받은 문자열이 "order" 일 경우 test4.order.html 페이지로 이동하고,
	// "cart" 일 경우 test4_cart.html 페이지로 이동
		if (target == "order") {
			location.href = "test4_order.html"
		} else if (target == "cart") {
			location.href = "test4_cart.html"
		}
		
		// 변수와의 결합을 통해 URL 생성도 가능
		location.href = "test4_" + target +".html"
	}

</script>
</head>
<body>
	<input type="button" value="URL 정보 확인" onclick="func1()">
	<br>
	<input type="button" value="URL 정보 변경" onclick="func2()">
	<br>
	<input type="button" value="URL 정보 새로고침" onclick="func3()">
	<br>
	<hr>
	<!-- 복수개의 버튼을 하나의 함수에서 구분하여 서로 다른 페이지로 이동 -->
	<!-- 즉시구매 : test4_order.html, 장바구니 : tes4_cart.html 로 이동 -->
	<!-- 함수 호출 시 파라미터에 해당 페이지를 구분하기 위한 정보를 각각 다르게 전달 -->
	<input type="button" value="즉시구매" onclick="func4('order')">
	<input type="button" value="장바구니" onclick="func4('cart')">
	<br>
	}
  
--------------------------------test4_xxxx.html-------------------------------------------
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<h1>test4_cart.html - 장바구니</h1>
  
  <h1>test4_order.html - 즉시구매</h1>
</body>
</html>
--------------------------------test4_xxxx.html-------------------------------------------
```

**[ history 객체 ]**
- 웹브라우저의 주소 방문 기록을 관리하는 객체
- 웹브라우저를 통해 접속했던 페이지 주소(URL) 정보를 저장하고 관리
	- 속성 : history.length 등
	- 함수 : history.back(), history.forward(), history.go() 등 

```javascript
function func1() {
		// 현재 웹브라우저에 저장된 방문기록 갯수 확인
		alert(history.length);
	}
	
	
	function func2() {
		// 웹브라우저에 저장된 페이지 목록 중 현재 페이지의 이전 페이지로 이동
		// = 뒤로 가기 => 1단계 이동
		history.back();
	}
	
	function func3() {
		// 웹브라우저에 저장된 페이지 목록 중 현재 페이지의 다음 페이지로 이동
		// = 앞으로 가기 => 1단계 이동
		history.forward();
}

	function func4() {
		// 현재 웹페이지에서 x번째 페이지로 이동 => go() 함수
		// 이 때, x번째 이전페이지로 이동하려면 음수값 전달
		history.go(-2);
		
}

	function func5() {
		// 현재 웹페이지에서 x번째 페이지로 이동 => go() 함수
		// 이 때, x번째 다음페이지로 이동하려면 양수값 전달
		history.go(2);
}

</script>
</head>
<body>
	<input type="button" value="방문 기록 갯수 확인" onclick="func1()"><br>
	<input type="button" value="이전 페이지" onclick="func2()"><br>
	<input type="button" value="다음 페이지" onclick="func3()"><br>
	<input type="button" value="2단계 이전 페이지" onclick="func4()"><br>
	<input type="button" value="2단계 다음 페이지" onclick="func5()"><br>
```

**[ document 객체 ]**
- HTML 문서 정보(내용)를 관리하는 객체
- 문서 정보 확인, 내용 변경 등의 작업 수행 가능
  - 속성 : title, bgColor, fgColor 등
  - 함수 : write() 등
```javascript
function func1() {
	// HTML 문서 제목(title 태그 내용) 확인
	alert(document.title)
}

function func2() {
	// HTML 문서 제목 변경(= 제목표시줄의 내용 변경 = title 속성값 변경)
	document.title = "변경된 타이틀";
}

function func3() {
	// HTML 문서 색상 정보 확인 (= bgColor, fgColor 속성 활용)
	alert("bgColor : " + document.bgColor + ", fgColor : " + document.fgColor)
}

function func4() {
	// HTML 문서 색상 정보 변경
	// => 속성값으로 "색상명" 또는 "#RGB코드값" 등 지정
	// 	  빛의 3원색 원리를 사용하여 16진수 RED값 2자리, GREEN값 2자리, BLUE값 2자리로 표현
	// => 즉, 값이 커질 수록 해당 색이 진해지며, 
	//	  모든 값을 최대치(FFFFFF)로 설정하면 흰색, 최소치(000000)로 설정하면 검정색

	// 1. 색상명을 지정하여 색상을 변경
//	document.bgColor = "RED"; // 배경색 "RED" (= 빨간색) 변경
// 	document.fgColor = "WHITE"; // 전경색(= 주로 텍스트) "WHITE" (= 흰색) 변경

	// 2. RGB 값을 지정하여 색상을 변경
	document.bgColor = "#FF0000"; // #FF0000 = RED 와 동일
	document.fgColor = "#FFFFFF"; // #FFFFFF = WHITE 와 동일 
	
	document.bgColor = "#FFFF00"; // #FFFF00 = YELLOW
	document.fgColor = "#FFFFFF"; // #0000FF = BLUE
}	

function func5(color) {
	// 파라미터로 전달받은 색상명을 사용하여 배경색(bgColor) 변경하기
	document.bgColor = color;
}

</script>
</head>
<body>
	<h1>document 객체</h1>
	<input type="button" value="문서 제목 출력" onclick="func1()"><br>
	<input type="button" value="문서 제목 변경" onclick="func2()"><br>
	<input type="button" value="문제 배경색 출력" onclick="func3()"><br>
	<input type="button" value="문제 배경색 변경" onclick="func4()"><br>
	<hr>
	
	<!-- 라디오버튼을 사용하여 "CYAN", "GRAY", "MAGENTA" 버튼 생성(name 속성값은 bgColor) -->
	<!-- 라디오버튼 클릭 이벤트 발생 시 func5() 함수 호출 -->
	<!-- 함수 파라미터로 색상명을 문자열로 전달하면, 함수에서 전달받아 해당 색으로 배경색 변경 -->
	<input type="radio" name="bgColor" onclick="func5('CYAN')">CYAN
	<input type="radio" name="bgColor" onclick="func5('GRAY')">GRAY
	<input type="radio" name="bgColor" onclick="func5('MAGENTA')">MAGENTA
	<!-- 이름을 직접 지정하지 않고 value 속성값을 함수에 전달할 경우 -->
	<!-- this 라는 값 지정 시 현재 태그를 의미하게 되고, this.value 지정 시 현재 태그의 value 속성값 -->
	<input type="radio" name="bgColor" value="SKYBLUE" onclick="func5(this.value)">SKYBLUE
	
</body>


------------------------------------------------------------------------------------------------------------------
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<script type="text/javascript">
	//document 객체는 HTML 문서 자체의 정보를 담고 있으며, 문서의 시작을 가리킴
	// = DOM(document Object Model, 문서 객체 모델)
	// => document 객체를 통해 문서 구성요소(Element)를 단계별로 접근 가능
	document.write("document : " + document + "<br>");
	// object HTMLHtmlElement => <html> 태그를 가리키는 객체
	document.write("document.documentElement : " + document.documentElement + "<br>")
	// object HTMLheadElement => <html> 태그를 가리키는 객체
	document.write("document.head : " + document.head + "<br>")
	
	// <HEAD> 태그 내의 자식 노드 갯수를 확인하는 경우
	document.write("document.head.childNodes.length : " + document.head.childNodes.length + "<br>")
	
	// for문을 사용하여 변수 i가 0 부터 자식 노드 갯수보다 작을 동안 반복하면서 자식노드 정보 출력
	// => HEAD 태그 내의 자식 노드 : META, TITLE, SCRIPT 태그와 함께 각 태그의 줄바꿈으로 인해
	//							 TEXT 노드(#TEXT) 가 추가되어 있음(= 총 3개)
	//							 (#TEXT, META, #TEXT, TITLE, #TEXT, SCRIPT = 총 6개)					
	for(var i = 0; i < document.head.childNodes.length; i++) {
		document.write(document.head.childNodes[i] + " : " + document.head.childNodes[i].nodeName + "<br>")
	}
	
	// object HTMLBodyElement : <BODY> 태그를 가리키는 객체
	document.write("document.body : " + document.body + "<br>")
</script>
</head>
<body>
	<h1>test7.html</h1>
	<div>목록 시작</div>
	<ul>
		<li>항목1</li>
		<li>항목2</li>
	</ul>
	<div>목록 끝</div>
	<hr>
	<script type="text/javascript">
	for(var i = 0; i < document.body.childNodes.length; i++) {
		alert(document.body.childNodes[i] + " : " + document.body.childNodes[i].nodeName + "<br>")
	}
	</script>
	
	<!-- 이 부분부터는 body 태그 내의 자식 노드 중 script 노드보다 아래쪽에 위치하므로
	script 태그 실행 시점에서는 아직 로딩되기 전의 요소.
	따라서, for문을 통해 body 태그 자식 노드 접근 시 대상 요소에 포함되지 않는다! -->
	<h1>body 태그 자식 노드 접근 후</h1>
</body>
</html>
------------------------------------------------------------------------------------------------------------------	
```
