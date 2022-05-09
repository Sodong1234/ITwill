# [오전, 오후수업] JSP 59, 60차



> 58차에서 실시한 json 관련 수업 추가진행

```

```


```jsp
-----------------------------test5_json.jsp-----------------------------
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
// 	});

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
				
				// 전달받은 JSON 객체를 문자열 형식으로 변환
// 				alert(JSON.stringify(data));
				
// 				$("#resultArea").html(JSON.stringify(data));
				
				$("#resultArea").html("<table border='1'>개인정보</table>");
				$("#resultArea > table").append("<tr><th>아이디</th><th>이름</th><th>나이</th><th>정보수신동의</th></tr>");
				// JSON 데이터가 대괄호[] 로 감싸져 있을 경우 배열 데이터로 취급되고
				// 해당 배열 내에서 중괄호{} 감싸져 있는 객체에 접근하여 객체의 속성 사용 가능
				// => 배열명[인덱스].속성
// 				$("#resultArea").html(data[0].id);
				
				// 파생된 JSON 객체의 배열 크기만큼 반복 작업 수행 => for문 사용
				for(let i = 0; i < data.length; i++) {
					// JSON 객체 내의 배열에서 i번 인덱스의 객체를 통해 각 속성 값을 변수에 저장
					let id = data[i].id;
					let name = data[i].name;
					let age = data[i].age;
					let agreeRcvSpam = data[i].agreeRcvSpam;
					
					// 저장된 변수값을 차례대로 td 태그를 통해 출력하기
					$("#resultArea > table").append(
							"<tr><td>" + id + "</td>" 
							+ "<td>" + name + "</td>"
							+ "<td>" + age + "</td>"
							+ "<td>" + agreeRcvSpam + "</td></tr>");
					
				}
			})
			.fail(function() {
				// 요청 실패 시 자동으로 호출되는 함수 정의
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

```jsp

-----------------------------json_data6.txt-----------------------------
[
		{
		"rnum":"1",
		"rank":"1",
		"rankInten":"0",
		"rankOldAndNew":"OLD",
		"movieCd":"20178501",
		"movieNm":"니 부모 얼굴이 보고 싶다",
		"openDt":"2022-04-27",
		"salesAmt":"210763970",
		"salesShare":"29.6",
		"salesInten":"2708600",
		"salesChange":"1.3",
		"salesAcc":"2548465120",
		"audiCnt":"21917",
		"audiInten":"686",
		"audiChange":"3.2",
		"audiAcc":"271607",
		"scrnCnt":"946",
		"showCnt":"3505"},
		{
		"rnum":"2",
		"rank":"2",
		"rankInten":"0",
		"rankOldAndNew":"OLD",
		"movieCd":"20212725",
		"movieNm":"신비한 동물들과 덤블도어의 비밀",
		"openDt":"2022-04-13",
		"salesAmt":"166666510",
		"salesShare":"23.4",
		"salesInten":"19113990",
		"salesChange":"13",
		"salesAcc":"11391483010",
		"audiCnt":"16740",
		"audiInten":"2068",
		"audiChange":"14.1",
		"audiAcc":"1106936",
		"scrnCnt":"996",
		"showCnt":"2841"}
]
-----------------------------test6_json.jsp-----------------------------
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
		$("#btnOk").on("click", function() {
			// json_data.txt 파일을 사용하여 AJAX 에서 JSON 객체 데이터 요청하기
			$.ajax({
				type: "GET",
				url: "json_data6.txt",
				dataType: "json" // 응답 데이터가 JSON 형식이므로 JSON 타입 지정
			})
			.done(function(data) { 
				$("#resultArea").html("<table border='1'><tr><th colspan='4'>일별 박스오피스</th></tr></table>");
				$("#resultArea > table").append(
						"<tr><th width='50'>순위</th>" 
						+ "<th width='500'>영화명</th>"
						+ "<th width='150'>개봉일</th>"
						+ "<th width='100'>누적관객수</th></tr>");
				
				// json_data6.txt 의 박스오피스 일부 정보를 추출하여 출력
				for(let i = 0; i < data.length; i++) {
					let rank = data[i].rank;
					let movieNm = data[i].movieNm;
					let openDt = data[i].openDt;
					let audiAcc = data[i].audiAcc;
					
					$("#resultArea > table").append(
							"<tr><td>" + rank + "</td>" 
							+ "<td>" + movieNm + "</td>"
							+ "<td>" + openDt + "</td>"
							+ "<td>" + audiAcc + "</td></tr>");
				}
			})
			.fail(function() {
				// 요청 실패 시 자동으로 호출되는 함수 정의
				$("#resultArea").html("요청 실패");				
			});
		});
	});
	
</script>
</head>
<body>
	<h1>test6_json.jsp - 일간 박스오피스 순위</h1>
	<input type="button" value="박스오피스 조회" id="btnOk"><br>
	<div id="resultArea"></div>
</body>
</html>
```

```jsp
-----------------------------json_data7.txt-----------------------------
- 10개의 영화 목록 추출. 생략함.
-----------------------------test7_data.jsp-----------------------------
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
		$("#btnOk").on("click", function() {
			// json_data.txt 파일을 사용하여 AJAX 에서 JSON 객체 데이터 요청하기
			$.ajax({
				type: "GET",
// 				url: "json_data7.txt",
				// 실제 영화 정보 API URL 을 사용하여 요청할 경우
				url: "https://kobis.or.kr/kobisopenapi/webservice/rest/boxoffice/searchDailyBoxOfficeList.json?key=f5eef3421c602c6cb7ea224104795888&targetDt=20220503",
				dataType: "json" // 응답 데이터가 JSON 형식이므로 JSON 타입 지정
			})
			.done(function(data) { 
				$("#resultArea").html("<table border='1'><tr><th colspan='4'>일별 박스오피스</th></tr></table>");
				$("#resultArea > table").append(
						"<tr><th width='50'>순위</th>" 
						+ "<th width='500'>영화명</th>" 
						+ "<th width='150'>개봉일</th>" 
						+ "<th width='100'>누적관객수</th></tr>");
				
				// json_data7.txt 의 박스오피스 일부 정보를 추출하여 출력
				// => 파라미터로 전달받은 data 변수가 JSON 객체(박스오피스 정보)를 저장하고 있으므로
				//    별도의 파싱 등이 추가로 불필요하며 즉시 JSON 객체 형태에서 가공 가능
				
				// 영화 일별 박스 오피스 정보 전체 파싱 후 각 데이터 추출
				// => 객체(data)명.키 의 형태로 접근하여 추출
				// 1. 전체 조회 결과에 해당하는 "boxOfficeResult" 키(Key) 추출
				let boxOfficeResult = data.boxOfficeResult;
// 				alert(JSON.stringify(boxOfficeResult));

				// 2. 추출된 전체 조회 결과에서 항목별 추출
				// => boxofficeType, showRange, dailyBoxOfficeList
				let boxofficeType = boxOfficeResult.boxofficeType;
				let showRange = boxOfficeResult.showRange;
				let dailyBoxOfficeList = boxOfficeResult.dailyBoxOfficeList;
// 				alert(JSON.stringify(boxofficeType) + "\n" + JSON.stringify(showRange) + "\n" + JSON.stringify(dailyBoxOfficeList))

				// 3. dailyBoxOfficeList 객체에 저장되어 있는 박스오피스 목록(영화 목록) 데이터 추출
				// => 배열([]) 형태의 객체이므로 배열 접근 방법을 통해 접근
// 				for(let i = 0; i < dailyBoxOfficeList.length; i++) {
					// JSON 객체 내의 배열에서 i번 인덱스의 객체를 통해 각 속성 값을 변수에 저장
// 					let rank = dailyBoxOfficeList[i].rank;
// 					let movieNm = dailyBoxOfficeList[i].movieNm;
// 					let openDt = dailyBoxOfficeList[i].openDt;
// 					let audiAcc = dailyBoxOfficeList[i].audiAcc;
					
					// 저장된 변수값을 차례대로 td 태그를 통해 출력하기
// 					$("#resultArea > table").append(
// 							"<tr><td>" + rank + "</td>" 
// 							+ "<td>" + movieNm + "</td>"
// 							+ "<td>" + openDt + "</td>" 
// 							+ "<td>" + audiAcc + "명</td></tr>"
// 					);

					// for..of 문 사용 시
				for(let boxOffice of dailyBoxOfficeList) {
					// JSON 객체 내의 배열에서 i번 인덱스의 객체를 통해 각 속성 값을 변수에 저장
					let rank = boxOffice.rank;
					let movieNm = boxOffice.movieNm;
					let openDt = boxOffice.openDt;
					let audiAcc = boxOffice.audiAcc;
					let movieCd = boxOffice.movieCd;
					
					// 저장된 변수값을 차례대로 td 태그를 통해 출력하기
					$("#resultArea > table").append(
							"<tr><td>" + rank + "</td>" 
							+ "<td><a href='test8_json.jsp?movieCd="+movieCd+"'>" + movieNm + "</a></td>"
							+ "<td>" + openDt + "</td>"
							+ "<td>" + audiAcc + "명</td></tr>"
					);
				}
			})
			.fail(function() {
				// 요청 실패 시 자동으로 호출되는 함수 정의
				$("#resultArea").html("요청 실패");				
			});
		});
	});
	
</script>
</head>
<body>
	<h1>test7_json.jsp - 일간 박스오피스 순위</h1>
	<input type="button" value="박스오피스 조회" id="btnOk">
	<br>
	<div id="resultArea"></div>
</body>
</html>
```

```jsp
-----------------------------test7_json_movie_detail.jsp-----------------------------
<%@ page language="java" contentType="text/html; charset=UTF-8"
	pageEncoding="UTF-8"%>
<%
// 파라미터로 전달받은 영화 코드(movieCd)값 가져오기
String movieCd = request.getParameter("movieCd");
%>	
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<script src="../js/jquery-3.6.0.js"></script>
<script type="text/javascript">
	$(function() {
//		alert("영화 코드 " + <%=movieCd%>);
		$("#btnOk").on("click", function() {
			// json_data.txt 파일을 사용하여 AJAX 에서 JSON 객체 데이터 요청하기
			$.ajax({
				type: "GET",
// 				url: "json_data7.txt",
				// 실제 영화 정보 API URL 을 사용하여 요청할 경우
				url: "https://www.kobis.or.kr/kobisopenapi/webservice/rest/movie/searchMovieInfo.json?key=f5eef3421c602c6cb7ea224104795888&movieCd=${param.movieCd}",
				dataType: "json" // 응답 데이터가 JSON 형식이므로 JSON 타입 지정
			})
			.done(function(data) { 
				$("#resultArea").html("<table border='1'><tr><th colspan='4'>영화 상세정보</th></tr></table>");
				$("#resultArea > table").append(
						"<tr><th width='500'>영화명</th>" 
						+ "<th width='150'>개봉일</th>" 
						+ "<th width='150'>감독</th>" 
						+ "<th width='150'>출연진</th></tr>");
				
				// 영화 상세 정보 전체 파싱 후 각 데이터 추출
				let movieInfo = data.movieInfoResult.movieInfo; // 하위 객체 직접 추출 가능
// 				alert(JSON.stringify(movieInfo));
					
					// 영화 상세 정보 추출
					let movieNm = movieInfo.movieNm;
					let movieNmEn = movieInfo.movieNmEn;
// 					let movieNm = movieInfo.movieNm + "(" + movieInfo.movieNmEn + ")";
					let openDt = movieInfo.openDt;
					
					// 영화 감독은 복수개의 객체일 수 있으므로 배열로 제공됨
					let directors = ""; // 초기값으로 널스트링 저장 필수!
					for(let director of movieInfo.directors) {
						directors += director.peopleNm + " ";
					}
					// 영화 배우는 복수개의 객체일 수 있으므로 배열로 제공됨 
					let actors = ""; // 초기값으로 널스트링 저장 필수!
					for(let actor of movieInfo.actors) {
						actors += actor.peopleNm + " ";
					}
					
					// 저장된 변수값을 차례대로 td 태그를 통해 출력하기
					$("#resultArea > table").append(
							"<tr><td>" + movieNm + "<br>" + movieNmEn + "</td>" 
							+ "<td>" + openDt + "</td>"
							+ "<td>" + directors + "</td>"
							+ "<td>" + actors + "</td></tr>");
			})
			.fail(function() {
				// 요청 실패 시 자동으로 호출되는 함수 정의
				$("#resultArea").html("요청 실패");				
			});
		});
	});
	
</script>
</head>
<body>
	<h1>test8_json.jsp - 상세 정보</h1>
	<input type="button" value="정보 조회" id="btnOk">
	<br>
	<div id="resultArea"></div>
</body>
</html>
```

```jsp
-----------------------------test8_data.jsp-----------------------------
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
		$("#btnOk").on("click", function() {
			// 선택된 날짜 가져오기
			let selectedDate = $("#date").val();

			// 날짜 미선택 시(널스트링) 날짜 선택 필수 메세지 출력 후 포커스 요청
			if(selectedDate == "") {
				alert("날짜 선택 필수!");
				$("#date").select();
				return;
			}
			
			// 가져온 날짜(yyyy-mm-dd)를 요청 형식에 맞게 yyyymmdd 로 변환
// 			selectedDate = selectedDate.replaceAll("-", ""); // "-" 문자열을 "" 로 대체(- 제거)
			let splitSelectedDate = selectedDate.split("-"); // "-" 문자열 기준 분리
			selectedDate = splitSelectedDate[0] + splitSelectedDate[1] + splitSelectedDate[2];
			alert(selectedDate); // 2022-05-04
			
			// json_data.txt 파일을 사용하여 AJAX 에서 JSON 객체 데이터 요청하기
			$.ajax({
				type: "GET",
// 				url: "json_data7.txt",
				// 실제 영화 정보 API URL 을 사용하여 요청할 경우
				url: "https://kobis.or.kr/kobisopenapi/webservice/rest/boxoffice/searchDailyBoxOfficeList.json?key=f5eef3421c602c6cb7ea224104795888&targetDt=" + selectedDate,
				dataType: "json" // 응답 데이터가 JSON 형식이므로 JSON 타입 지정
			})
			.done(function(data) { 
				$("#resultArea").html("<table border='1'><tr><th colspan='4'>일별 박스오피스</th></tr></table>");
				$("#resultArea > table").append(
						"<tr><th width='50'>순위</th>" 
						+ "<th width='500'>영화명</th>" 
						+ "<th width='150'>개봉일</th>" 
						+ "<th width='100'>누적관객수</th></tr>");
				
				// json_data7.txt 의 박스오피스 일부 정보를 추출하여 출력
				// => 파라미터로 전달받은 data 변수가 JSON 객체(박스오피스 정보)를 저장하고 있으므로
				//    별도의 파싱 등이 추가로 불필요하며 즉시 JSON 객체 형태에서 가공 가능
				
				// 영화 일별 박스 오피스 정보 전체 파싱 후 각 데이터 추출
				// => 객체(data)명.키 의 형태로 접근하여 추출
				// 1. 전체 조회 결과에 해당하는 "boxOfficeResult" 키(Key) 추출
				let boxOfficeResult = data.boxOfficeResult;
// 				alert(JSON.stringify(boxOfficeResult));

				// 2. 추출된 전체 조회 결과에서 항목별 추출
				// => boxofficeType, showRange, dailyBoxOfficeList
				let boxofficeType = boxOfficeResult.boxofficeType;
				let showRange = boxOfficeResult.showRange;
				let dailyBoxOfficeList = boxOfficeResult.dailyBoxOfficeList;
// 				alert(JSON.stringify(boxofficeType) + "\n" + JSON.stringify(showRange) + "\n" + JSON.stringify(dailyBoxOfficeList))

				// 3. dailyBoxOfficeList 객체에 저장되어 있는 박스오피스 목록(영화 목록) 데이터 추출
				// => 배열([]) 형태의 객체이므로 배열 접근 방법을 통해 접근
// 				for(let i = 0; i < dailyBoxOfficeList.length; i++) {
// 					// JSON 객체 내의 배열에서 i번 인덱스의 객체를 통해 각 속성 값을 변수에 저장
// 					let rnum = dailyBoxOfficeList[i].rnum;
// 					let rank = dailyBoxOfficeList[i].rank;
// 					let movieNm = dailyBoxOfficeList[i].movieNm;
// 					let openDt = dailyBoxOfficeList[i].openDt;
// 					let audiAcc = dailyBoxOfficeList[i].audiAcc;
					
// 					// 저장된 변수값을 차례대로 td 태그를 통해 출력하기
// 					$("#resultArea > table").append(
// 							"<tr><td>" + rank + "</td>" 
// 							+ "<td>" + movieNm + "</td>"
// 							+ "<td>" + openDt + "</td>" 
// 							+ "<td>" + audiAcc + "명</td></tr>"
// 					);
// 				}
				
				// for...of 문 사용 시
				for(let boxOffice of dailyBoxOfficeList) {
					// JSON 객체 내의 배열에서 i번 인덱스의 객체를 통해 각 속성 값을 변수에 저장
					let rnum = boxOffice.rnum;
					let rank = boxOffice.rank;
					let movieNm = boxOffice.movieNm;
					let openDt = boxOffice.openDt;
					let audiAcc = boxOffice.audiAcc;
					let movieCd = boxOffice.movieCd;
					
					// 저장된 변수값을 차례대로 td 태그를 통해 출력하기
					$("#resultArea > table").append(
							"<tr><td>" + rank + "</td>" 
							+ "<td><a href='test7_json_movie_detail.jsp?movieCd=" + movieCd + "'>" + movieNm + "</a></td>"
							+ "<td>" + openDt + "</td>" 
							+ "<td>" + audiAcc + "명</td></tr>"
					);
				}
				
			})
			.fail(function() {
				// 요청 실패 시 자동으로 호출되는 함수 정의
				$("#resultArea").html("요청 실패");
			});
		});
	});
	
</script>
</head>
<body>
	<h1>test8_json.jsp - 일간 박스오피스 순위</h1>
	<input type="date" id="date">
	<input type="button" value="박스오피스 조회" id="btnOk"><br>
	<div id="resultArea"></div>
</body>
</html>


```

## 자바 메일
> StudyJSP 프로젝트에 jsp14_java_mail 폴더 생성 후 mail_form.jsp 파일 생성
> Java 메일 jar (mail-1.4.7.jar) Build path 실시
> GoogleSMTPAuthenticator 클래스 생성
> WEB-INF 폴더에 application-data.properties 
> 개인정보 보호를 위해 패스워드는 xxxxxxxxxxxx로 입력함


```jsp
----------------------------mail_form.jsp----------------------------
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<h1>자바 메일 전송 폼</h1>
	<form action="mail_pro.jsp" method="post">
		<table border="1">
			<tr><td>보내는 사람</td><td><input type="text" name="sender"></td></tr>
			<tr><td>받는 사람</td><td><input type="text" name="receiver"></td></tr>			
			<tr><td>제목</td><td><input type="text" name="title"></td></tr>
			<tr><td>내용</td><td><textarea name="content" rows="20" cols="40"></textarea></td></tr>
			<tr><td colspan="2"><input type="submit" value="메일발송"></td></tr>						
		</table>
	
	</form>
</body>
</html>
----------------------------mail_pro.jsp----------------------------
<%@page import="jsp14_java_mail.GoogleSMTPAuthenticator"%>
<%@page import="javax.mail.Transport"%>
<%@page import="java.util.Date"%>
<%@page import="javax.mail.Message.RecipientType"%>
<%@page import="javax.mail.internet.InternetAddress"%>
<%@page import="javax.mail.Address"%>
<%@page import="javax.mail.internet.MimeMessage"%>
<%@page import="javax.mail.Message"%>
<%@page import="javax.mail.Session"%>
<%@page import="javax.mail.Authenticator"%>
<%@page import="java.util.Properties"%>
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%
request.setCharacterEncoding("UTF-8");

String sender = request.getParameter("sender");
String receiver = request.getParameter("receiver");
String title = request.getParameter("title");
String content = request.getParameter("content");

try {
	// ------- 메일 전송에 필요한 설정 작업 -------
	// 메일 전송 프로토콜 : SMTP(Simple Mail Tranfer Protocol) 
	// (참고. 메일 수신 프로토콜 : POP3)
	// 시스템(서버)의 속성 정보(서버 정보)를 java.util.Properties 객체로 리턴받기
	Properties properties = System.getProperties();
	// 메일 전송에 필요한 정보를 가져온 서버 속성 정보에 추가 - put() 메서드 활용
	// TLS(Transport Layer Security) 인증 관련 설정
	properties.put("mail.smtp.starttls.enable", "true");
	properties.put("mail.smtp.ssl.protocols", "TLSv1.2");
	// 메일 전송에 사용할 메일 서버 지정(Gmail, 네이버, 네이트, 아웃룩 등)
	properties.put("mail.smtp.host", "smtp.gmail.com"); // 구글(Gmail) 메일 발송 서버 주소 지정
	properties.put("mail.smtp.auth", "true"); // 인증 여부 설정(로그인 필요 시)
	properties.put("mail.smtp.port", "587"); // 메일 전송에 사용될 서비스 포트 설정(SMTP 포트)

	// 메일 서버에 대한 인증 정보를 관리하는 사용자 정의 클래스(GoogleSMTPAuthenticator)의 인스턴스 생성
// 	Authenticator authenticator = new GoogleSMTPAuthenticator(); // 업캐스팅
	// 외부 파일로부터 아이디, 패스워드를 가져오기 위해 request 객체 전달 시
	Authenticator authenticator = new GoogleSMTPAuthenticator(request); // 업캐스팅
	
	// 자바 메일의 기본 전송 단위를 javax.mail.Session 객체 단위로 관리
	// => Session 클래스의 getDefaultInstance() 메서드를 호출하여 파라미터로 서버 정보, 인증 정보 전달
//	    (주의! 변수명은 session 외의 다른 이름 사용 필수! => HttpSession 내장객체 변수명이 존재함)
	Session mailSession = Session.getDefaultInstance(properties, authenticator);

	// Message 정보를 관리할 javax.mail.internet.MimeMessage 객체 생성
	// => 파라미터로 서버 정보와 인증 정보를 관리하는 javax.mail.Session 객체 전달
	// => MimeMessage 객체를 사용하여 전송할 메일에 대한 정보 설정 가능
	Message mailMessage = new MimeMessage(mailSession);

	// 전송할 메일에 대한 정보 설정
	// 1. 발신자 정보 설정(발신자 정보를 관리하기 위한 InternetAddress 객체 생성)
	// => 단, 스팸 메일 정책으로 인해 상용 메일 사이트(구글 등)는 발신자 주소 변경 불가능
	Address sender_address = new InternetAddress(sender, "아이티윌"); // 두번째 파라미터는 변경 가능
	// 2. 수신자 정보 설정
	Address receiver_address = new InternetAddress(receiver);
	// 3. Message 객체를 통해 전송할 메세지 정보 설정
	// 1) 메일 헤더 정보 설정
	mailMessage.setHeader("content-type", "text/html; charset=UTF-8");
	// 2) 발신자 정보 설정
	mailMessage.setFrom(sender_address);
	// 3) 수신자 정보 설정(RecipientType 클래스의 TO 상수와 수신자 주소 전달)
	mailMessage.addRecipient(RecipientType.TO, receiver_address);
	// 4) 메일 제목 설정
	mailMessage.setSubject(title);
	// 5) 메일 본문 설정(본문과 본문의 컨텐츠타입 전달)
	mailMessage.setContent(content, "text/html; charset=UTF-8");
	// 6) 메일 전송 날짜 정보 설정(java.util.Date 객체를 활용하여 현재 시스템의 시각 정보 전달)
	mailMessage.setSentDate(new Date());

	// 4. 메일 전송
	// javax.mail.Transport 클래스의 static 메서드 send() 메서드 호출
	Transport.send(mailMessage);
	out.println("<h3>메일이 정상적으로 전송되었습니다!</h3>");
} catch(Exception e) {
	e.printStackTrace();
	out.println("<h3>SMTP 서버 설정 또는 서비스 문제 발생!</h3>");
}
%>    

----------------------------GoogleSMTPAuthenticator.----------------------------
package jsp14_java_mail;
import java.io.FileReader;
import java.util.Properties;

import javax.mail.Authenticator;
import javax.mail.PasswordAuthentication;
import javax.servlet.ServletContext;
import javax.servlet.http.HttpServletRequest;

// 메일 서버 인증을 위해 javax.mail.Authenticator 클래스를 상속받는 서브클래스 정의
public class GoogleSMTPAuthenticator extends Authenticator {
	// 인증 정보(아이디, 패스워드)를 관리할 javax.mail.PasswordAuthentication 클래스 타입 변수 선언
	PasswordAuthentication passwordAuthentication;

	
	// 기본 생성자 정의
//	public GoogleSMTPAuthenticator() throws Exception {
//		// 인증에 사용할 아이디와 패스워드 정보를 갖는 PasswordAuthentication 객체 생성
//		// => 구글 2단계 인증을 사용하지 않는 경우 계정명, 로그인 패스워드 전달
//		// => 구글 2단계 인증을 사용하는 경우 별도의 부가 작업 필요
//		// 	  (Gmail 의 경우 앱 비밀번호를 별도로 발급받아 패스워드 부분에 입력)
////		passwordAuthentication = new PasswordAuthentication("계정명", "비밀번호");
////		passwordAuthentication = new PasswordAuthentication("yys0507", "xxxxxxxxxxxxxxxx");
//	}
	
	// request 객체를 파라미터로 전달받는 GoogleSMTPAuthenticator 생성자 정의 
	public GoogleSMTPAuthenticator(HttpServletRequest request) throws Exception {
		// 아이디, 패스워드를 하드코딩하지 않고 외부 파일로부터 읽어들여 사용할 경우
		// => java.util.Properties 클래스 활용
		// 1. Properties 객체 생성
		Properties properties = new Properties();
		
		// 2. 아이디와 패스워드가 저장된 외부 파일(application-data.properties) 읽어오기
		//	  => (단, 파일이 위치한 폴더는 가상의 경로이므로 실제 경로를 알아내야한다!)
		// 2-1. WEB-INF 폴더의 실제 경로 알아내기
		ServletContext context = request.getServletContext(); // 톰캣 객체 가져오기
		String realPath = context.getRealPath("WEB-INF"); // 루트기준 WEB-INF 폴더의 실제 경로 알아내기
		// D:\workspace\workspace_JSP\.metadata\.plugins\org.eclipse.wst.server.core\tmp3\wtpwebapps\StudyJSP\WEB-INF
//		System.out.println(realPath); // 실제 프로젝트 업로드 된 서버에 따라 달라질 수 있음
		// 2-2. Properties 객체의 load() 메서드를 호출하여 파일 읽어오기 (FileNotFoundException 처리 필요)
		// => 파라미터로 FileReader 객체 생성하여 파일이 위치한 경로를 전달
		properties.load(new FileReader(realPath + "/application-data.properties")); 
		
		// 3. 읽어들인 파일로부터 원하는 키를 사용하여 데이터 가져오기
		// => Properties 객체의 getProperty() 메서드를 호출하여 키(속성명)를 파라미터로 전달
		String id = properties.getProperty("id");
		String passwd = properties.getProperty("passwd"); 
//		System.out.println(id + ", " + passwd);
		
		// 4. PasswordAuthentication 객체 생성(파라미터로 읽어들인 정보 전달)
		passwordAuthentication = new PasswordAuthentication(id, passwd);
		
	}

	
	// 인증 정보 객체를 외부로 리턴하는 getPasswordAuthentication() 메서드 오버라이딩
	// => 주의! 변수명이 달라질 수 있으므로 Getter 메서드를 직접 정의하는 것은 좋지 않다!
	@Override
	protected PasswordAuthentication getPasswordAuthentication() {
		return passwordAuthentication;
	}
	
	
}


----------------------------application-data.properties----------------------------
id=yys0507
passwd=xxxxxxxxxxxxxxxx
```

