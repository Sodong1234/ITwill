# [오전수업] WEB 6차
> WEB 5차에서 실시한 선 모양 추가분을 커밋해놓았음

## css 선 스타일 및 화면배치 연습
```css
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>css1/test8.html</title>
<style type="text/css">
/* 전체 대상으로 테두리선을 기준으로 계산 */
/* id="box1"의 너비 100px, 높이 100px  
테두리선 종류 실선, 너비 thick, 선색 파란색
밖여백 상하 20px, 좌우 50px 
안여백 위 50px */

/* id="box2"의 너비 100px, 높이 100px 
선 스타일 dotted, 선 두께 10px 50px, 선 색 red blue
밖여백 가운데 정렬, 상하 20px*/

/* id="box3" 너비 100px, 높이 100px 
선 스타일 dashed, 선 두께 5 10px 20 30px
선 색 red blue green purple
밖여백 20px*/


*{
	   box-sizing : border-box;
}

#box1{ width : 100px;
	   height : 100px;
	   border-style: solid;
	   border-width : thick;
	   border-color : blue;
	   margin: 20px 50px;
	   padding-top : 50px;
}

#box2{ width : 100px;
	   height : 100px;
	   border-style : dotted;
	   border-width : 10px 5px;
	   border-color : red blue;
	   margin : 20px auto;
}

#box3{ width : 100px;
	   height : 100px;
	   border-style : dashed;
	   border-width : 5px 10px 20px 30px;
	   border-color : red blue green purple;
	   margin : 20px; 	    
}

#box4{ width : 100px;
	   height : 50px;
	   border : 1px solid black;
/* 	   모서리 둥글게 */
/* 	   border-radius : 20px;  */
	   border-radius : 50%;
	   border-bottom-left-radius : 20px;
	   border-bottom-right-radius : 20px; 
	   border-top-left-radius : 10px;
	   border-top-right-radius : 10px; 
	   
/* 	   그림자 */
/* 	   box-shadow : 수평, 수직, 흐림정도, 번짐정도, 색상  */
	   box-shadow : 2px -2px 5px 2px purple; 
}

/* id="img1" 너비 100px, 높이 100px, 모서리 둥글게 50%
박스 그림자 5px 5px 15px 5px blue*/

#img1{ width : 100px;
	   height : 100px;
	   border-radius : 50%;
	   box-shadow : 5px 5px 15px 5px blue;
}
</style>
</head>
<body>
<div id="box1">내용1</div>
<div id="box2">내용2</div>
<div id="box3">내용3</div>
<div id="box4"></div>
<img src="./1.jpg" id="img1">
</body>
</html>
```
![캡처](https://user-images.githubusercontent.com/95197594/155251366-690b02f3-9c52-4db5-bd1d-10dbfda0f298.PNG)

```css
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<style type="text/css">
*{
	box-sizing: border-box;
}
ul {
	list-style-type: none;
}
ul li{
	border : 1px solid black;
/* 	안 여백 20px */
	padding : 20px;
/* 	밖 여백 상하 0px, 좌우 20px */
	margin : 0px 20px;
/* 	h2, p, div : 문단태그 전체영역(블랙 영역) */
/* 	span, strong : 요소영역(인라인 영역) */
/* 	배치 display:block : 전 영역 지정
	    display:inline : 영역 하나하나 지정
	    display:inline-block : 영역 하나하나 지정 (inline과는 조금 다름) */
	display : inline-block;
}
span {
	border : 1px solid black;
	display : block;
}

div *{
	border : 1px solid black;
}
img{
/* 	이미지 글자 어울리게 배치 */
	float : left;
	margin-right : 20px;
}
/* id="bb" 어울림 해제 */
#bb{
/*  claer : left; clear : right; clear : both; */
	clear : both;
}
</style>
</head>
<body>
<span>요소영역(인라인)</span>
<ul>
	<li>menu 1</li>
	<li>menu 2</li>
	<li>menu 3</li>
	<li>menu 4</li>
</ul>
<div>
<img src="2.jpg">
<p>가나다라마바가나다라마바가나다라마바가나다라마바가나다라마바가나다라마바</p>
<p>가나다라마바가나다라마바가나다라마바가나다라마바가나다라마바가나다라마바</p>
<p>가나다라마바가나다라마바가나다라마바가나다라마바가나다라마바가나다라마바</p>
<p>가나다라마바가나다라마바가나다라마바가나다라마바가나다라마바가나다라마바</p>
<p id="bb">안녕하세요</p>
</div>
</body>
</html>
```
![캡처](https://user-images.githubusercontent.com/95197594/155253299-ce8e05a9-494e-4a15-b19e-93444c43744b.PNG)

```css
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<style type="text/css">
*{
/* 	border : 1px solid black; */
/* 	브라우저 기본여백 안여백0 밖여백0 */
	padding : 0px; margin : 0px; 
/* 	테두리선 기준으로 계산 */
	box-sizing : border-box; 
}
/* id="aaa" 너비 1200px, 가운데 정렬 밖여백 상하 20 좌우 가운데 */
#aaa{
	width : 1200px;
	margin : 20px auto;
/* 	background-color : yellow;  */
}
/* id="header" 너비 100%, 높이120px */
#header{
	width : 100%; height : 120px;
	background-color : purple; 
}
/* id="leftbar" 너비 300px, 높이 600px */
#leftbar{
	width : 300px; height : 600px;
	background-color : pink;
/* 	다음에 나오는 내용 어울림 */
	float : left;
}

/* id="bbb" 너비 900px, 높이 600px */
#bbb{
	width : 900px; height : 600px;
	background-color : blue;
/* 	다음에 나오는 내용 어울림 */
	float : left;
}
/* id="footer" 너비 100%, 높이 100px */
#footer{
	width : 100%; height : 100px;
	background-color : green;
/* 	어울림 해제 */
	clear : left; 
}

</style>
</head>
<body>
<div id="aaa">
	<div id="header">사이트 제목</div>
	<div id="leftbar">왼쪽메뉴</div><div id="bbb">본문</div>
	<div id="footer">푸터</div>
</div>

</body>
</html>
```
![캡처](https://user-images.githubusercontent.com/95197594/155256343-a30f71e2-51e4-4070-b752-20437f038176.PNG)

```css
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<style type="text/css">
	*{
	margin : 0px; padding : 0px;
	box-sizing : border-box;
}
	#aaa{
	width : 1200px;
	margin : 20px auto;
}
	#header{
		width : 100%; height : 120px;
		background-color : yellow;
		border : 1px solid black;
	}
	#leftbar{
		width : 250px; height : 600px;
		background-color : pink;
		float : left;
		border : 1px solid black;
	}
	#bbb{
		width : 800px; height : 600px;
		float : left;
		background-color : blue;
		border : 1px solid black;
	}
	#rightbar{
		width : 150px; height : 600px;
		float : left;
		background-color : skyblue;
	}
	#footer{
		width : 100%; height : 100px;
		clear : left;
		background-color : green;
	}

</style>
</head>
<body>
<div id="aaa">
	<div id="header">사이트제목</div>
	
	<div id="leftbar">왼쪽메뉴 너비 250</div>
	<div id="bbb">본문 너비 800</div>
	<div id="rightbar">오른쪽바 너비 150</div>
	
	<div id="footer">푸터</div>
</div>

</body>
</html>
```
![캡처](https://user-images.githubusercontent.com/95197594/155258670-1bf7a44a-e5b1-4fc3-a14c-ef5ba2465efa.PNG)

---

# [오후수업] JSP 19차

## form 태그
### action 속성
- submit 시 폼 태그 내의 파라미터(= 폼파라미터)를 모두 request 객체에 저장한 후 action 속성에 지정된 페이지(또는 파일)로 포워딩(= 이동)
	- ex) <form action="requestPro1.jsp"> : submit 버튼 클릭 시 requestPro1.jsp 페이지로 이동
### method 속성
- 서버가 수행해야할 동작을 지정하는 속성
- GET 방식
	- 폼 태그 내에 method="get" 명시 또는 method 속성 생략하거나 하이퍼링크를 사용하여 이동하거나 URL을 직접 입력하여 이동할 경우 폼 파라미터가 URL에 함께 포함되어 전달됨(= URL에 파라미터값 노출되며, 전송 파라미터 데이터의 길이 제한이 있으나, POST 방식에 비해 전송속도가 빠름
- POST 방식
	- 폼 태그 내에 method="post" 명시하여 이동할 경우 폼 파라미터가 URL에 포함되지 않고 body 영역에 포함되어 전달되므로 URL에 파라미터값 노출이 없고, 전송 파라미터 데이터 길이 제한도 없음
	
### Form 데이터를 전달하기
- requestForm1.jsp 페이지에서 데이터 입력 후 submit 버튼 클릭 시 form 태그 내의 데이터(=폼 파라미터) 가 모두 내장 객체인 request 객체에 저장되며 지정된 웹페이지 또는 파일(requestPro1.jsp)로 이동하면서 request 객체(데이터 포함)도 함께 전달됨
	- 요청 관련 모든 정보는 request 객체가 관리(= 자동으로 생성되는 객체 = 내장 객체)
	- request 객체의 뱐수 또는 메서드를 통해 요청 정보 사용 가능
		- request.변수명 또는 request.메서드명()
	- 요청받은 request 객체에 저장된 폼 파라미터 데이터를 가져오는 방법
	   1) request.getParameter("파라미터명"); => 단일 항목 파라미터 가져오기(String 타입 리턴)
	   2) request.getParameterValues("파라미터명"); => 복수 항목 파라미터 가져오기(String[] 타입 리턴)
	- 주의! 전달받은 파라미터가 존재하지 않을 경우 (= 지정한 파라미터명이 없을 경우) null 값이 리턴되고, 파라미터명은 존재하지만 데이터가 없을 경우 널스트링("") 값이 리턴됨
	   - 단, 일반 파라미터의 경우 데이터 미입력 시 널스트링이 전달되지만 라디오버튼, 체크박스 등은 데이터 미입력 시 파라미터 자체가 전달되지 않으므로 null 값 전달됨
	   - 특히, 체크박스는 배열 형태로 접근하므로 null 값을 통해 배열에 접근하려하면 NullPointerException 이라는 예외(= 오류)가 발생하여 코드 실행이 불가능해짐

```jsp
-----------------------------------------requestForm1.jsp-----------------------------------------
	<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<h1>requestForm1.jsp - request 객체 예제</h1>
	
	<!-- 
	폼 작성
	이름 입력 (name 속성값 "name" 필수항목)
	나이 입력 (name 속성값 "age' 필수 항목
	성별 입력 (name 속성값 "gender" 남(value 속성 "male", 여(value 속성 "female"))
	취미 선택 (name 속성값 "hobby", 독서(value="독서"), 운동(value="운동"), 게임(value="게임") - 체크
	submit 버튼("전송")
	 -->
	 
	 <form action="requestPro1.jsp" method="get">
<!-- 	 <form action="requestPro1.jsp" method="get"> -->
	 <form action="requestPro1.jsp" method="post">
	 이름 입력<input type="text" name="name"><br>
	 나이 입력<input type="text" name="age"><br>
	 성별 선택<input type="radio" name="gender" value="male">남
	 		<input type="radio" name="gender" value="female">여<br>
	 취미 선택<input type="checkbox" name="hobby" value="독서">독서
	 		<input type="checkbox" name="hobby" value="운동">운동
	 		<input type="checkbox" name="hobby" value="게임">게임<br>
	 <input type="submit" value="전송">	 
	 </form>
</body>
</html>
		
-----------------------------------------requestPro1.jsp-----------------------------------------
		
<%@ page language="java" contentType="text/html; charset=UTF-8"
	pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<h1>requestPro1.jsp - request 객체 처리 예제</h1>
	<%
	
	// -----------------------------------------------------------------------
	// 만약, requestForm1.jsp 페이지에서 POST 방식으로 요청(method="post")했을 경우
	// 파라미터로 전달되는 데이터 중 한글 데이터가 포함되어 있으면 깨질 수 있다!
	// 따라서, 응답페이지(ex. requestPro1.jsp 페이지)에서 별도의 한글 처리가 필요함
	// => request 객체의 setCharacterEncoding() 메서드를 호출하여 "UTF-8" 타입 지정
	request.setCharacterEncoding("UTF-8"); // 유니코드 방식(UTF-8)으로 변경
	// => 반드시 한글 파라미터 가져오기 전 인코딩 타입 변경을 먼저 수행해야한다!
	// -----------------------------------------------------------------------

	// 1. 폼 파라미터 중 파라미터명(name 속성값)이 "name" 인 파라미터의 데이터 가져오기
	// => String 타입 변수 strName 에 저장
	String strName = request.getParameter("name");
	// 스크립틀릿 내에서 변수에 저장된 데이터를 웹페이지에 출력할 경우 out.print() 메서드 사용
	// 	out.print("이름 : " + strName + "<br>");

	// 2. 폼 파라미터 중 파라미터명(name 속성값)이 "age" 인 파라미터의 데이터 가져오기
	String strAge = request.getParameter("age");
	// 	out.print("나이 : " + strAge + "<br>");

	// 3. 폼 파라미터 중 파라미터명(name 속성값)이 "gender" 인 파라미터의 데이터 가져오기
	String strGender = request.getParameter("gender");
	// 	out.print("성별 : " + strGender + "<br>");

	// 4. 폼 파라미터 중 파라미터명(name 속성값)이 "hobby" 인 파라미터의 데이터 가져오기
	// => 주의! checkbox 처럼 복수개의 데이터가 하나의 파라미터명으로 전달될 경우 
	// 	  getParameter() 메서드를 사용하면 하나의(첫번째) 데이터만 가져올 수 있음
	// => 따라서, getParameter() 메서드 대신 getParameterValues() 메서드를 호출하여
	//	  복수개의 데이터를 String[] 타입으로 리턴받아 처리해야함
	// 	String strHobby = request.getParameter("hobby");
	// 	out.print("취미 : " + strHobby + "<br>"); // 복수개 체크 시에도 하나의 값만 출력됨

	// 파라미터명이 "hobby"인 취미를 가져와서 String[] 타입 strHobbies에 저장
	String[] strHobbies = request.getParameterValues("hobby");
	// 	out.print(strHobbies); // 주의! 미체크 시 null 값 전달 될 수 있음
	
	// 배열의 첫번째 데이터 출력
	// 	out.print("취미 : " + strHobbies[0] + "<br>");
	// => null 값이 전달될 경우 배열 인덱스 접근이 불가능하므로 오류가 발생할 수 있음
	%>


	<table border="1">
		<tr>
			<td>이름</td>
			<td><%=strName%></td>
		</tr>
		<tr>
			<td>나이</td>
			<td><%=strAge%></td>
		</tr>
		<tr>
			<td>성별</td>
			<td><%=strGender%></td>
		</tr>
		<tr>
			<td>취미</td>
			<td>
				<!-- strHobbies 배열 내의 인덱스가 고정이 아니므로 0 ~ 2번 인덱스 무조건 접근 시 오류 --> <%-- 			<%=strHobbies[0] %>,<%=strHobbies[1] %>,<%=strHobbies[2] %> --%>
				<!-- 자바 코드 for문을 활용하여 배열 내의 모든 인덱스 차례대로 접근해야함 --> <!-- 주의! null 값이 전달될 경우 length 속성 접근근 시에도 오류 발생! -->
				<!-- 따라서, if문을 사용하여 배열이 null이 아닐 때만 반복문 사용 --> 
				<%
 				if (strHobbies != null) {
 					for (int i = 0; i < strHobbies.length; i++) {
 						out.println(strHobbies[i]);
 					}
 				} else {
//  					out.println("없음");
				%>
					<!-- 자바스크립트를 사용하여 "취미 선택 필수!" 메세지 출력 후 이전페이지로 돌아가기 -->
					<script type="text/javascript">
						alert("취미 선택 필수!");
						history.back();
					</script>
				<%
 				}
 				%>
			</td>
		</tr>
	</table>
</body>
</html>
```	
		
```jsp
-----------------------------------------requestForm2.jsp-----------------------------------------		
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<h1>requestPro2.jsp - 로그인</h1>
	<!-- 
	아이디(속성명 id), 패스워드(속성명 passwd)를 입력받고,
	"로그인" 버튼(타입 submit) 클릭 시 requestPro2.jsp 페이지로 이동(POST 방식)
	 -->
<form action="requestPro2.jsp" method="post">
		<table border="1">
			<tr>
				<td>아이디</td>
				<td><input type="text" name="id"></td>
			</tr>
			<tr>
				<td>패스워드</td>
				<td><input type="passwd" name="passwd"></td>
			</tr>
			<tr>
				<td colspan="2"><input type="submit" value="로그인"></td>
			</tr>
		</table>
	</form>

</body>
</html>
-----------------------------------------requestPro2.jsp-----------------------------------------	
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<h1>requestPro2.jsp - 로그인 처리</h1>
	<%
	// POST 방식 한글 처리(아이디, 패스워드에 한글 미포함시 생략 가능 )
// 	request.setCharacterEncoding("UTF-8");
	
	// 폼 파라미터로 전달받은 아이디, 패스워드 가져오기
	String id = request.getParameter("id");
	String passwd = request.getParameter("passwd");
	%>
	<h3>아이디 : <%=id %></h3>
	<h3>패스워드 : <%=passwd %></h3>
	
	<!-- 아이디가 "admin" 이고, 패스워드가 "1234" 일 경우 현재 웹페이지에 "로그인 성공!" 을 출력
	아니면, 자바스크립트를 사용하여 "로그인 실패" 출력 후 이전페이지로 돌아가기
	=> 주의! 자바코드에서 문자열 비교 시 동등비교연산자(==) 보다 equals() 메서드 사용을 권장!
	   원본 문자열 equals(비교할 문자열) => 결과값이 boolean 타입으로 리턴됨
	-->
	
	<%if(id.equals("admin") && passwd.equals("1234")) { %>
		<%-- 아이디와 패스워드가 일치할 경우(= 로그인 성공일 경우) --%>
		<h3>로그인 성공!</h3>
	<%} else { %>
		<%-- 아이디 또는 패스워드가 일치하지 않을 경우(= 로그인 실패일 경우) --%>
		<script type="text/javascript">
			alert("로그인 실패!");
			history.back();
		</script>
	<%} %>
</body>
</html>
```		
