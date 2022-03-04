# [오전수업] WEB 7차

css 연습
```css

<style type="text/css">
#tot {
/* 	너비 800px 가운데 정렬 위아래 0 */
	width: 800px;
	margin: 0px auto;
}
p{
/* 줄간격 20px, 글자크기 12px */
	line-height: 20px;
	font-size: 12px;
}
#a1 {
/* 너비 350px, 높이 200px, 밖여백 10px, 안여백 10px, 테두리선 1px 실선, #ccc */
	width: 350px;
	height: 200px;
	margin: 10px;
	padding: 10px;
	border: 1px solid #ccc;
	float: left;
}
#footer{
/* 너비 100%, 높이 50px, 배경색 #222, 어울림 해제 */
	width: 100%;
	height: 50px;
	background-color: #222;
	clear: left;
}
#footer p{
/* 글자 흰색, 글자크기 15pt, 글자 가운데, 줄 간격 50px */
	color: white;
	font-size: 15pt;
	text-align: center;
	line-height: 50px;
}
</style>
</head>
<body>
	<div id="tot">
<h2>강아지 용품 준비하기</h2>
<div id="a1">
<h3>강아지 집</h3>
<p>강아지가 편히 쉴 수 있는 포근한 집이 필요합니다. 강아지의 집은 강아지가 다 큰 후에도 계속 쓸 수 있는 집으로 구입하세요.집을 구입하실 때는 박음질이 잘 되어 있는지, 세탁이 간편한 제품인지 꼭 확인하시고 고르시는 것이 좋습니다.</p>
</div>

<div id="a1">
<h3>강아지 먹이</h3>
<p>강아지의 먹이는 꼭 어린 강아지용으로 나와있는 사료를 선택하세요. 강아지들은 사람에 비해 성장속도가 8배정도 빠르답니다. 따라서 강아지에게는 성장속도에 맞는 사료를 급여하셔야 합니다. 사람이 먹는 음식을 먹게 되면 양념과 향신료에 입맛이 익숙해지고, 비만이 될 가능성이 매우 높아집니다. 강아지용 사료는 생후 12개월까지 급여하셔야 합니다.</p>
</div>

<div id="a1">
<h3>밥그릇, 물병</h3>
<p>밥그릇은 쉽게 넘어지지 않도록 바닥이 넓은 것이 좋습니다.물병은 대롱이 달린 것으로 선택하세요. 밥그릇에 물을 주게 되면 입 주변에 털이 모두 젖기 때문에 비위생적이므로 대롱을 통해서 물을 먹을 수 있는 물병을 마련하시는 것이 좋습니다.</p>
</div>

<div id="a1">
<h3>이름표, 목줄</h3>
<p>강아지를 잃어버릴 염려가 있으니 산책할 무렵이 되면 이름표를 꼭 목에 걸어주도록 하세요. 그리고 방울이 달린 목걸이를 하고자 하실 때는 신중하셔야 합니다. 움직일 때마다 방울이 딸랑 거리면 신경이 예민한 강아지들에게는 좋지 않은 영향을 끼칠 수 있기 때문입니다.</p>
</div>

<div id="footer">
<p>boxmodel 연습하기</p>
</div>

</div>
	
</body>
</html>
```
![캡처](https://user-images.githubusercontent.com/95197594/156678113-3669d868-4d3b-43bd-b770-f91f5fc85c7b.PNG)

```css

<style type="text/css">
div{
/* 	background-color: yellow; */
/* 	background-color: #0000ff; */
/* 	background-color: rgb(255,0,0); */
/* 	배경이미지 */
	background-image: url("dot.png");
/* 	background-repeat: repeat-x; */
/* 	background-repeat: repeat-y; */
/* 	이미지 반복 */
 	background-repeat: no-repeat;
/* 	위치 
	수평left center right 
	수직top conter bottom */
/* 	background-position: 50% 50%; */
	background-position: right center;
/* 	이미지 크기 */
	background-size: 50px 50px;
/* 	이미지 고정 */
	background-attachment: fixed;
	
	width: 300px;
	height: 500px;
	border: 20px dotted black;
}

</style>
</head>
<body>
<h1>제목제목제목제목제목제목제목제목제목제목제목제목제목제목제목제목제목제목제목제목제목제목</h1>

<div>
<h2>레드향</h2>
<p>껍질에 붉은빛이 돌아 <span class="sp">레드향</span>이라 불린다</p>
<p>비타민Cvvvv와 비타민 Pbbbbb가 풍부해<br>
혈액순환, 감기예방 등에 좋은 것으로 알려져 있다</p>
</div>
	
</body>
</html>
```
![캡처](https://user-images.githubusercontent.com/95197594/156681092-d9dffd1f-0cd0-41f6-ac2f-1b970b886af3.PNG)

```css

<style type="text/css">
/* ul 대상 스타일 없앰 */
/* li 왼쪽으로 어울림 */
/* a 밑줄 없애기, 안여백 위아래 10px 좌우20px, 배경색 #ccc  */
ul{
	list-style-type: none;
}
li{
	float: left;
}
a{
	text-decoration: none;
	padding: 10px 20px;
	background-color: #ccc;
}
a:link, a:visited {
	color: black;
}
a:hover {
	color: white;
	background-color: black;
}
</style>
</head>
<body>
<div>
	<ul>
		<li><a href="ex1.html">메뉴1</a></li>
		<li><a href="ex2.html">메뉴2</a></li>
		<li><a href="ex3.html">메뉴3</a></li>
		<li><a href="ex4.html">메뉴4</a></li>
	</ul>
</div>

</body>
</html>
```
![캡처](https://user-images.githubusercontent.com/95197594/156682268-885ea36a-683a-4fbf-b12e-2e29d2621f69.PNG)

```css

<style type="text/css">
*{
	margin: 0px;
	padding: 0px;
}
body{
	background-color: #ccc;
	padding: 20px;
}
form{
/* 배경색 흰색, 테두리선 1px 실선 #222, 테두리 둥글게 5px */
/* 안여백 20px, 너비 400px, 밖여백 상하30px 가운데정렬 */
	background-color: white;
	border: 1px solid #222;
	border-radius: 5px;
	padding: 20px; 
	width: 400px;
	margin: 30px auto;
}
fieldset {
/* 	테두리선 1px 실선 #ccc, 밖아래여백 30px */
	border : 1px solid #ccc;
	margin-bottom: 30px;
}
legend {
/* 	폰트크기 16px, 폰트굵기 bold, 안 왼쪽여백 5px, 안 아래여백 10px */
	font-size: 16px;
	font-weight: bold;
	padding-left: 5px;
	padding-bottom: 10px;
}
li{
/* 줄간격 30px, 리스트스타일 none, 안 여백 5px 10px, 밖아래여백 2px */
	line-height: 30px;
	list-style: none;
	padding: 5px 10px;
	margin-bottom: 2px;
}
label {
/* 왼쪽으로 배치, 폰트크기 13px, 너비 110px */
	float: left; font-size: 13px; width: 110px;
}
/* class="btn" */
.btn{
/* 테두리 1px 실선 #222, 테두리 둥글게 20px, 글자크기 16px, 글자간격 1px, 밖여백 상하 0px 가운데정렬, 안여백 7px 25px */
	border : 1px solid #222;
	border-radius: 20px;
	font-size: 16px;
	letter-spacing: 1px;
	margin: 0px auto;
	padding: 7px 25px;
	display: block;
} 


</style>
</head>
<body>
<form action="a.jsp" method="get">
	<fieldset>
		<legend>로그인 정보</legend>
		<ul>
			<li><label>아이디</label>
			    <input type="text" name="id" autofocus required></li>
			<li><label>비밀번호</label>
			    <input type="password" name="pwd1"  required></li>
			<li><label>비밀번호 확인</label>
			    <input type="password" name="pwd2"  required></li>
			<li><label>회원 등급</label>
			    <input type="text" name="level" value="준회원" readonly></li>
		</ul>
	</fieldset>
	<fieldset>
		<legend>개인정보</legend>
		<ul>
			<li><label>이름</label>
			    <input type="text" name="name" placeholder="5자미만 공백없이" required></li>
			<li><label>메일 주소</label>
			    <input type="email" name="email" placeholder="abcd@domain.com" required></li>
			<li><label>연락처</label>
			    <input type="tel" name="tel"></li>
		</ul>
	</fieldset>
	<fieldset>
		<input type="submit" value="제출" class="btn">
	</fieldset>
</form>

</body>
</html>
```
```css
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<style type="text/css">
*{
	margin: 0px;
	padding: 0px;
}
body{
	background-color: #ccc;
	padding: 20px;
}
form{
/* 배경색 흰색, 테두리선 1px 실선 #222, 테두리 둥글게 5px */
/* 안여백 20px, 너비 400px, 밖여백 상하30px 가운데정렬 */
	background-color: white;
	border: 1px solid #222;
	border-radius: 5px;
	padding: 20px; 
	width: 400px;
	margin: 30px auto;
}
fieldset {
/* 	테두리선 1px 실선 #ccc, 밖아래여백 30px */
	border : 1px solid #ccc;
	margin-bottom: 30px;
}
legend {
/* 	폰트크기 16px, 폰트굵기 bold, 안 왼쪽여백 5px, 안 아래여백 10px */
	font-size: 16px;
	font-weight: bold;
	padding-left: 5px;
	padding-bottom: 10px;
}
li{
/* 줄간격 30px, 리스트스타일 none, 안 여백 5px 10px, 밖아래여백 2px */
	line-height: 30px;
	list-style: none;
	padding: 5px 10px;
	margin-bottom: 2px;
}
label {
/* 왼쪽으로 배치, 폰트크기 13px, 너비 110px */
	float: left; font-size: 13px; width: 110px;
}
/* class="btn" */
.btn{
/* 테두리 1px 실선 #222, 테두리 둥글게 20px, 글자크기 16px, 글자간격 1px, 밖여백 상하 0px 가운데정렬, 안여백 7px 25px */
	border : 1px solid #222;
	border-radius: 20px;
	font-size: 16px;
	letter-spacing: 1px;
	margin: 0px auto;
	padding: 7px 25px;
	display: block;
} 
input:not([type=submit]){
/* 테두리선 1px 실선 #ccc, 테두리선 둥글게 3px, 글자 크기 13px, 안여백 5px, 너비 200px */
	border : 1px solid #ccc;
	border-radius: 3px;
	font-size: 13px;
	padding: 5px;
	width: 200px;
}
/* 마지막 fieldset 다른스타일 적용 */
fieldset:last-of-type {
	border: none;
	margin-bottom: 0px;
}
/* required 조건 테두리선 1px 실선 빨간색 */
input[required] {
	border: 1px solid red;
}
/* input태그 readonly 조건 테두리 none */
input[readonly] {
	border-style: none;
}

</style>
</head>
<body>
<form action="a.jsp" method="get">
	<fieldset>
		<legend>로그인 정보</legend>
		<ul>
			<li><label>아이디</label>
			    <input type="text" name="id" autofocus required></li>
			<li><label>비밀번호</label>
			    <input type="password" name="pwd1"  required></li>
			<li><label>비밀번호 확인</label>
			    <input type="password" name="pwd2"  required></li>
			<li><label>회원 등급</label>
			    <input type="text" name="level" value="준회원" readonly></li>
		</ul>
	</fieldset>
	<fieldset>
		<legend>개인정보</legend>
		<ul>
			<li><label>이름</label>
			    <input type="text" name="name" placeholder="5자미만 공백없이" required></li>
			<li><label>메일 주소</label>
			    <input type="email" name="email" placeholder="abcd@domain.com" required></li>
			<li><label>연락처</label>
			    <input type="tel" name="tel"></li>
		</ul>
	</fieldset>
	<fieldset>
		<input type="submit" value="제출" class="btn">
	</fieldset>
</form>

</body>
</html>
```
![캡처](https://user-images.githubusercontent.com/95197594/156688942-fd252993-a8e2-429e-ab19-9b84c575ca93.PNG)

```css
<title>Insert title here</title>
<style type="text/css">
/* 웹문서 구조 만드는 태그 : 시맨틱 태그 */
/* header, nav, main, article, section, aside, footer */
*{
	margin: 0px;
	padding: 0px;
	box-sizing: border-box;
}
a{
	text-decoration: none; 
}
ul{
	list-style-type: none;
}
/* id="container" 밖여백 가운데, 너비 1200px, 배경 흰색 */
#container{
	width: 1200px; 
	margin: 0px auto; 
	background-color: white;
}
header{
/* 	너비 100%, 높이 100px, 배경색 #045 */
	width: 100%;
	height: 100px;
	background-color: #045;
}
/* id="logo" 너비 250px, 높이 100px, 줄간격 100px, 안왼쪽여백 20px */
#logo{
	width: 250px;
	height: 100px;
	line-height: 100px;
	padding-left: 20px;
	float: left;
}
#logo h1{
	font-weight: 700px; 
	font-size: 40px; 
	color: white;
}
nav{
	width: 900px; 
	height: 100px;
	padding-top: 40px;
	float: right;
}
/* id="topMenu" */
#topMenu{
	height: 60px;
}
#topMenu li{
	float: left;
}
#topMenu li a{
	font-size: 1.1em;
	color: white;
	font-weight: 600;
	padding: 20px 60px;
}
#topMenu li a:hover {
	color: #1fa8f8;
}
/* --------------------- */
footer{
	width: 1200px; 
	height: 100px;
	border-top: 2px solid #222;
}
#bottomMenu{
	width: 100%;
	height: 20px;
	margin-left: 60px;
}
#bottomMenu ul{
	margin-top: 15px;
}
#bottomMenu ul li{
	float: left;
	padding: 5px 20px;
	border-right: 1px solid #ddd;
}
#bottomMenu ul li:last-of-type{
	border-right: none;
}
#bottomMenu ul li a{
	font-size: 15px;
	color: #666;
}
#bottomMenu ul li a:hover{
	color: orange;
}
/* -------------------------------------------- */
main{
	width: 1000px;
	margin: 50px auto;
}
main section{
	margin-top: 60px;
}
main section h2{
	font-size : 1.5em;
}
main div{
	margin-top: 20px;
}
main h3{
	font-size: 1.2em;
}
.detail p{
	line-height: 2.0;
	color: #222;	
}
.detail img{
	margin-right: 50px;
	float: left;
}
.schedule h3{
	font-size: 1.2em;
}
.schedule ul li{
	line-height: 2;
}
</style>
</head>
<body>
<div id="container">
<header>
<div id="logo"><a href="ex7.html"><h1>Dream Jeju</h1></a></div>
<nav>
	<ul id="topMenu">
		<li><a href="ex1.html">단체 여행</a></li>
		<li><a href="ex2.html">맞춤 여행</a></li>
		<li><a href="ex3.html">갤러리</a></li>
		<li><a href="ex4.html">문의하기</a></li>
	</ul>
</nav>
</header>
<main class="contents">
	<section id="healing">
		<h2>몸과 마음이 치유되는 섬</h2>
		<div class="detail">
			<img src="healing.jpg">
			<p>쉼의 공간으로 안내합니다.</p>
			<p>탁 트인 바다, 시원한 바람에 몸을 맡기고 뚜벅뚜벅 오름을 오르고 올렛길을 걷다보면 온전히 나에게 집중할 수 있습니다.</p>
		</div>
		<div class="schedule">
			<h3>상세일정</h3>
			<ul>
				<li>여행 기간 : 2박 3일</li>
				<li>여행 일정 : (여행 일정은 상담을 통해 결정 및 조정 가능합니다)</li>
			</ul>
		</div>
	</section>
	
	<section id="activity">
		<h2>다양한 액티비티가 기다리는 섬</h2>
		<div class="detail">
			<img src="activity.jpg">
			<p>모험과 스릴이 넘치는 레저의 천국으로 안내합니다.</p>
			<p>둘러보기만 하는 여행을 하셨나요?</p>
			<p>하늘을 날며 시원한 바다를 내려다보는 패러글라이등과 투명한 물빛 속을 여행하는 스킨스쿠버... 아름다운 제주 해안도로를 씽씽 전동바이크나 전동킥보드로 달려보세요. 시원한 바다를 가까이에서 느낄 수 있는 요트 체험과 배낚시도 빼놓을 수 없겠죠?</p>
		</div>
	</section>
</main>
<footer>
	<section id="bottomMenu">
		<ul>
			<li><a href="ex1.html">회사 소개</a></li>
			<li><a href="ex2.html">개인정보처리방침</a></li>
			<li><a href="ex3.html">여행약관</a></li>
			<li><a href="ex4.html">사이트맵</a></li>
		</ul>
	</section>
</footer>

</div>

</body>
</html>
```
![캡처](https://user-images.githubusercontent.com/95197594/156695784-2005721b-a2cf-4d7d-bcac-2bd4472ea0c3.PNG)

> 12일 테스트 예정. ex2, test10, ex3, test13.html 파일 참조


# [오후수업] JSP 24차

## jsp - mysql 연동 연습

```jsp
-------------------------------------jdbc_connect_test3_form2-------------------------------------
<%@page import="java.sql.PreparedStatement"%>
<%@page import="java.sql.Connection"%>
<%@page import="java.sql.DriverManager"%>
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<h1>정보 입력 시스템</h1>
	<form action="jdbc_connect_test3_pro2.jsp">
		번호 : <input type="text" name="idx"><br>
		아이디 : <input type="text" name="id"><br>
		패스워드 : <input type="password" name="passwd"><br>
		<input type="submit" value="저장">	
	</form>
</body>
</html>
-------------------------------------jdbc_connect_test3_pro2-------------------------------------
<%@page import="java.sql.PreparedStatement"%>
<%@page import="java.sql.Connection"%>
<%@page import="java.sql.DriverManager"%>
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<%
	// 번호(idx), 아이디(id), 패스워드(passwd) 파리미터 가져와서 출력
	
// 	String idx = request.getParameter("idx");
	// String 타입 데이터를 int 타입으로 변환해야하는 경우 
	// Integer.parseInt(문자열데이터);
	// => 지정된 문자열데이터를 int 타입으로 변환하여 리턴해줌
	// => 주의! 문자열이 정수 형태가 아닐 경우 오류 발생!
	int idx = Integer.parseInt(request.getParameter("idx"));
	String id = request.getParameter("id");
	String passwd = request.getParameter("passwd");
	
	// 1단계, 2단계 작업을 통해 MySQL 연결
	String driver = "com.mysql.jdbc.Driver";
	String url = "jdbc:mysql://localhost:3306/study_jsp3";
	String user = "root";
	String password = "1234";
	
	// 1단계
	Class.forName(driver);
	out.println("<h3>드라이버 로드 성공!</h3>");
	// 2단계
	Connection con = DriverManager.getConnection(url, user, password);
	out.println("<h3>DB 연결 완료!</h3>");
	
	// 3단계. SQL 구문 작성 및 전달
	// => 주의! 문자 데이터는 반드시 작은따옴표('')로 둘러싸야한다!
// 		ex) INSERT INTO test2 VALUES (1,'admin','1234');
// 		ex) INSERT INTO test2(idx,id,passwd) VALUES (1, 'admin', '1234');
// 	String sql = "INSERT INTO test2 VALUES (" + idx + ",'" + id + "','" + passwd + "')";
// 	PreparedStatement pstmt = con.prepareStatement(sql);

	// ------------------------------------------------------------------------------
	// SQL 구문을 사용하여 외부로부터 입력받은 데이터를 처리할 때
	// SQL 구문에 전달되는 데이터를 조작하여 개발자가 의도하지 않은 구문을 실행하도록 할 수 있음
	// 이러한 공격 기법을 SQL 삽입 공격(Injection Attack) 이라고 한다.
	// => 이를 예방하기 위해서는 전달되는 데이터에 대한 검증을 수행하여
	//	  올바른 데이터인지 판별하고, SQL 구문에 직접적인 결합이 불가능하도록 해야함
	// => PreparedStatement 객체를 사용하고, 파라미터 대신 만능문자(?)를 사용하여
	//	  데이터가 위치할 공간을 표시한 후, 차후에 별도로 데이터를 검증 후 전달해야함
	// 1) SQL 구문 작성할 때 데이터가 위치할 공간을 만능문자(?) 만으로 표시(실제 데이터 전달 X)
	//	  => 문자 데이터를 표시할 때에도 작은따옴표('') 없이 만능문자만으로 표시
	String sql = "INSERT INTO test2 VALUES(?,?,?)";
	// 2) Connection 객체를 통해 SQL 구문을 전달하여 preparedStatement 객체 리턴받기
	PreparedStatement pstmt = con.prepareStatement(sql);
	// 3) PreparedStatement 객체의 setXXX() 메서드를 호출하여 만능문자 부분에 데이터 교체
	//	  => setXXX() 메서드의 XXX 은 전달할 데이터의 자바 데이터타입명 명시
	//	     ex) idx 컬럼의 데이터타입은 int, 자바 데이터타입도 int 이므로 setInt() 메서드 호출
	//	  기본 문법 : pstmt.setInt(parameterIndex, x)
	//	  => 첫 번째 파라미터(int parameterIndex) : 만능문자(?)의 순서번호(1부터 시작)
	pstmt.setInt(1, idx); // 1번 만능문자(?)를 정수형 변수 idx 값 교체
	// 주의사항! 만능문자(파라미터) 갯수에 맞게 setXXX() 메서드로 데이터를 설정하지 않으면
	// => java.sql.SQLException: No value specified for parameter 2
	//	  (2번 파라미터(만능문자)에 값이 없다!)
	pstmt.setString(2, id); // 2번 만능문자를 String 타입 변수 id 값으로 교체
	pstmt.setString(3, passwd);
	// -------------------------------------------------------------------------------
	
	// 4단계. SQL 구문 실행
	int insertCount = pstmt.executeUpdate();
	out.println("<h3>추가 완료</h3>");
	%>
</body>
</html>
```


```jsp
-------------------------------------jdbc_insert_form.jsp-------------------------------------
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<form action="jdbc_insert_pro.jsp" method="post">
	 	<table border="1">
	 		<tr>
	 			<td>학번</td>
	 			<td><input type="text" name="no"></td>
	 		</tr>
	 		<tr>
	 			<td>이름</td>
	 			<td><input type="text" name="name"></td>
	 		</tr>
	 		<tr>
	 			<td>나이</td>
	 			<td><input type="text" name="age"></td>
	 		</tr>
	 		<tr>
	 			<td>성별</td>
	 			<td>
	 				<input type="radio" name="gender" value="남">남
	 				<input type="radio" name="gender" value="여">여
	 			</td>
	 		</tr>
	 	</table>	 
	 		<input type="submit" value="저장">	
	 </form>
</body>
</html>

-------------------------------------jdbc_insert_pro.jsp-------------------------------------

<%@page import="java.sql.PreparedStatement"%>
<%@page import="java.sql.DriverManager"%>
<%@page import="java.sql.Connection"%>
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<%
	// => 주의! 한글 데이터가 포함되므로 UTF-8 설정 필수!
	request.setCharacterEncoding("UTF-8");
	
	// 번호, 이름, 나이, 성별 파라미터 가져와서 test3 테이블에 추가(INSERT)
	int no = Integer.parseInt(request.getParameter("no"));
	String name = request.getParameter("name");
	int age = Integer.parseInt(request.getParameter("age"));
	String gender = request.getParameter("gender");
	
	String Driver= "com.mysql.jdbc.Driver";
	String url = "jdbc:mysql://localhost:3306/study_jsp3";
	String user = "root";
	String password = "1234";
	
	// 1단계
	Class.forName(Driver);
	out.println("<h3>드라이버 로딩 성공!</h3>");
	
	// 2단계
	Connection con = DriverManager.getConnection(url, user, password);
	out.println("<h3>로그인 성공!</h3>");
	
	// 3단계
	String sql = "INSERT INTO test3 VALUES(?,?,?,?)";
	
	PreparedStatement pstmt = con.prepareStatement(sql);
	
	pstmt.setInt(1, no);
	pstmt.setString(2, name);
	pstmt.setInt(3, age);
	pstmt.setString(4, gender);
	
	// 4단계
	int insertCount = pstmt.executeUpdate();
	out.println("<h3>추가 완료</h3>");
	%>
	
</body>
</html>
```

### delete 구문
```jsp
-------------------------------------jdbc_delete_form.jsp-------------------------------------

<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<h1>jdbc_delete_form.jsp</h1>
	<form action="jdbc_delete_pro.jsp" method="post">
	 	<table border="1">
	 		<tr>
	 			<td>학번</td>
	 			<td><input type="text" name="no" placeholder="삭제할 학번 입력" required="required"></td>
	 		</tr>
	 		<tr>
	 			<td>이름</td>
	 			<td><input type="text" name="name" placeholder="삭제할 이름 입력" required="required"></td>
	 		</tr>
	 	</table>	 
	 		<input type="submit" value="삭제">	
	 </form>
</body>
</html>

-------------------------------------jdbc_delete_pro.jsp-------------------------------------

<%@page import="java.sql.PreparedStatement"%>
<%@page import="java.sql.DriverManager"%>
<%@page import="java.sql.Connection"%>
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<%
	request.setCharacterEncoding("UTF-8");
	
	int no = Integer.parseInt(request.getParameter("no"));
	String name = request.getParameter("name");
	
	// 학번(no)과 이름(name) 이 일치하는 레코드를 찾아 삭제(DELETE) 작업 수행
	// => INSERT 작업과 SQL 구문만 다르고 동작 방식은 동일함
	String Driver = "com.mysql.jdbc.Driver";
	String url = "jdbc:mysql://localhost:3306/study_jsp3";
	String user = "root";
	String password = "1234";
	
	// 1단계
	Class.forName(Driver);
	out.println("<h3>드라이버 로딩 성공!</h3>");
		
	// 2단계
	Connection con = DriverManager.getConnection(url, user, password);
	out.println("<h3>로그인 성공!</h3>");
	
	// 3단계
	String sql = "DELETE FROM test3 WHERE no=? AND name=?";
	PreparedStatement pstmt = con.prepareStatement(sql);
	
	pstmt.setInt(1, no);
	pstmt.setString(2, name);
	
	int deleteCount = pstmt.executeUpdate();
	out.println("<h3>삭제 완료</h3>");
	%>
	
</body>
</html>
```

### update 구문

-------------------------------------jdbc_update_form.jsp-------------------------------------

<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<h1>jdbc_delete_form.jsp</h1>
	<form action="jdbc_update_pro.jsp" method="post">
	 	<table border="1">
	 		<tr>
	 			<td>검색할 학번</td>
	 			<td><input type="text" name="search_no" placeholder="검색할 학번 입력" required="required"></td>
	 		</tr>
	 		<tr>
	 			<td>검색할 이름</td>
	 			<td><input type="text" name="search_name" placeholder="검색할 이름 입력" required="required"></td>
	 		</tr>
	 		<tr>
	 			<td>변경할 학번</td>
	 			<td><input type="text" name="no" placeholder="변경할 학번 입력" required="required"></td>
	 		</tr>
	 		<tr>
	 			<td>변경할 이름</td>
	 			<td><input type="text" name="name" placeholder="변경할 이름 입력" required="required"></td>
	 		</tr>
	 	</table>	 
	 		<input type="submit" value="변경">	
	 </form>
</body>
</html>
-------------------------------------jdbc_update_pro.jsp-------------------------------------

<%@page import="java.sql.PreparedStatement"%>
<%@page import="java.sql.DriverManager"%>
<%@page import="java.sql.Connection"%>
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<%
	request.setCharacterEncoding("UTF-8");
	
	int search_no = Integer.parseInt(request.getParameter("search_no"));
	String search_name = request.getParameter("search_name");
	int no = Integer.parseInt(request.getParameter("no"));
	String name = request.getParameter("name");
	
	// 학번(no)과 이름(name) 이 일치하는 레코드를 찾아 삭제(DELETE) 작업 수행
	// => INSERT 작업과 SQL 구문만 다르고 동작 방식은 동일함
	String Driver = "com.mysql.jdbc.Driver";
	String url = "jdbc:mysql://localhost:3306/study_jsp3";
	String user = "root";
	String password = "1234";
	
	// 1단계
	Class.forName(Driver);
	out.println("<h3>드라이버 로딩 성공!</h3>");
		
	// 2단계
	Connection con = DriverManager.getConnection(url, user, password);
	out.println("<h3>로그인 성공!</h3>");
	
	// 3단계
	// 학번(search_no)과 이름(search_name)이 일치하는 레코드를 찾아 
	// 전달받은 학번(no)과 이름(name)으로 변경(UPDATE)
	String sql = "UPDATE test3 SET no=?,name=? WHERE no=? AND name=?";
	PreparedStatement pstmt = con.prepareStatement(sql);
	
	pstmt.setInt(1, no); // 변경할 번호
	pstmt.setString(2, name); // 변경할 이름
	pstmt.setInt(3, search_no); // 검색할 번호
	pstmt.setString(4, search_name); // 검색할 이름
	
	int updateCount = pstmt.executeUpdate();
	out.println("<h3>수정 완료</h3>");
	%>
	
</body>
</html>
```	
