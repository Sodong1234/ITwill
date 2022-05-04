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
-----------------------------test8_data.jsp-----------------------------

