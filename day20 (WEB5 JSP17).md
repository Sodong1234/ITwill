# [오전수업] WEB 5차
## CSS(Cascading Style Sheets)
- html : 웹문서의 뼈대 역할
  - 텍스트, 문단, 이미지, 하이퍼링크, 목록상자, 표, 폼태그 등 
- CSS(Cascading Style Sheets) : html 문서의 꾸미기 담당
  - 텍스트, 이미지, 배경의 크기 배치, 디자인 등

### 적용방법
  1. 태그에 바로 스타일 적용
  2. head 안의 문서 내 스타일 적용
  3. 외부파일로 스타일 적용
```html
<h2>레드향</h2>
<p>껍질에 붉은빛이 돌아 레드향이라 불린다</p>
<p>비타민C와 비타민 P가 풍부해
혈액순환, 감기예방 등에 좋은 것으로 알려져 있다</p>
```

```css
1. 태그에 바로 스타일 적용
<p style="color: red;">껍질에 붉은빛이 돌아 레드향이라 불린다</p>
```

```css
2. head 안의 문서 내 스타일 적용
사용법 ex :

<style type="text/css">
	태그대상{
		속성:값;
		글자색:색;
		}
</style>



<style type="text/css">
		h2 {
		color: blue;
		background-color: skyblue;
		}
</style>
```
```css
3. 외부파일로 스타일 적용
사용법 : 
<head>태그 내부에 <link rel="stylesheet" href="xxx.css"> 추가


New -> CSS파일 생성
---------------------------------css1.css 내부---------------------------------

 		h2 { 
 			color: red; 
 			background-color: pink; 
 		} 

---------------------------------css1.css 내부---------------------------------

<link rel="stylesheet" href="css1.css">
```
![1](https://user-images.githubusercontent.com/95197594/154608195-d0138ed0-6fa4-4027-90c7-08fd4549d73a.PNG)

### css 활용
```css
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<style type="text/css">
	/*	태그대상{
		속성:값;
		글자색:색;
	} */
/* 	대상 : 전체대상 *, 아이디는 #, 클래스는 .,  태그대상, 이름(표시) 조건 태그[속성=값] */
	*{
		color: green;
	}
	h2 {
		color: red;
	}
	#hh {
		color: purple;
	}
/* 	id="hhh" 표시에는 파란색 */
	#hhh {
		color: blue;
	}
/* 	class="rd" 표시는 빨간색 */
	.rd{
		color: red;
	}
/*  	id="p1" 배경색  */
	#p1{
		background-color: pink;
	}
/* 	class="p2" 배경색 */
	.p2{
		background-color: yellow;
	}
/* 	id="cont" 스타일 적용 */
	#cont{
		border: 1px solid black;
		width: 500px;
		padding : 20px;
	}
	
</style>
</head>
<body>
<div id="cont">
<h2>레드향</h2>
<p id="p1">껍질에 붉은빛이 돌아 <span class="rd">레드향</span>이라 불린다</p>
<p class="p2">비타민C와 비타민 P가 풍부해
혈액순환, 감기예방 등에 좋은 것으로 알려져 있다</p>
<h2 id="hh">일정</h2>
<h2 id="hhh">먹거리</h2>
</div>

</body>
</html>
```
![2](https://user-images.githubusercontent.com/95197594/154608202-bb507cfa-7c3c-4bee-8c2c-9081c6fcc6c6.PNG)

```css
<style type="text/css">
/* 속성 : 글꼴관련 스타일 */
*{
/* 글꼴(글자체) */
	font-family: 궁서체;
/* 글자크기
   절대크기(고정)
   상대크기(변동) %
   pt(포인트), px(픽셀)
 */
	font-size: 10pt;
/* 글꼴 스타일 */
	font-style: normal;
/* 글꼴 굵기 */
	font-weight:bold;
}


/* 	p태그 글자 스타일 기울임, 글자 굵기 bold 글자크기 20px */
	p {
	font-style: italic;
	font-weight: bold;
	font-size: 20px;
	}
	

/* 	h2 글꼴 바탕 글자크기 3em */
	h2 {
	font-family: 바탕체;
	font-size:3em;
	}

</style>
</head>
<body>
<h2>레드향</h2>
<p>껍질에 붉은빛이 돌아 레드향이라 불린다</p>
<p>비타민C와 비타민 P가 풍부해
혈액순환, 감기예방 등에 좋은 것으로 알려져 있다</p>
<h2>일정</h2>
<h2>먹거리</h2>
</body>
</html>
```
![3](https://user-images.githubusercontent.com/95197594/154608205-b4a6f5fd-2639-4090-90cb-3c6969a56618.PNG)
```css
<style type="text/css">
h2{
/* 글자색 영문 10진수 16진수 RG #RRGGBB */
/* 99 => 9*10 + 9*1 */
/* ff => f*16 + f*1 => 15*16 + 15*1 => 255 */
/* 	color: blue; */
/* 	color: rgb(255,0,0); */
/* 	color: #ff0000; */
/* 투명도 */
   color: rgba(255,0,0,0.5); 
}

p{
/* 글자정렬 */
	text-align: center;
/* 	줄간격 (위, 아래, 가운데 정렬) */
	line-height: 50px;
/* 	글자 간 간격 */
	letter-spacing: 0.5em;
/* 	단어 간 간격 */
	word-spacing: 50px;
/* 	텍스트 영문자 대문자, 소문자, 대소문자 변경 */
	text-transform: capitalize;
}

.sp{
/* 텍스트 줄표시*/
	text-decoration: underline;
/* 	그림자 text-shadow: 가로 세로 번짐정도 색상 */
	text-shadow: 5px 10px 3px gray;
}

</style>
</head>
<body>
<h2>레드향</h2>
<p>껍질에 붉은빛이 돌아 <span class="sp">레드향</span>이라 불린다</p>
<p>비타민Cddddd와 비타민 Pbbbbb가 풍부해
혈액순환, 감기예방 등에 좋은 것으로 알려져 있다</p>
<h2>일정</h2>
<h2>먹거리</h2>
</body>
</html>
```
![4](https://user-images.githubusercontent.com/95197594/154608210-a156c3ee-5a7c-4aa5-8369-63b6b759d374.PNG)
```css
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<style type="text/css">
/* 목록 스타일 */
.book1{
	list-style-type: circle;
}

.book2{
/* 	목록 위치 안으로 inside(들여쓰기), 기본 outside */
	list-style-position: inside;
/* 	목록 블릿 대신에 이미지 사용 */
	list-style-image: url("dot.png")
}
</style>
</head>
<body>
<h1>도서 시리즈</h1>
<ul class="book1">
	<li>html 시리즈</li>
	<li>java 시리즈</li>
	<li>jsp 시리즈</li>	
</ul>

<ol class="book2">
	<li>html 시리즈</li>
	<li>java 시리즈</li>
	<li>jsp 시리즈</li>	
</ol>
</body>
</html>
```
![5](https://user-images.githubusercontent.com/95197594/154608238-e53b8533-ad97-4d64-8e8a-ea708e5ea6ff.PNG)

```css
<style type="text/css">
/* ul태그에 목록상자 스타일 점 없게 적용 */
ul{
	list-style-type: none;
}
/* a 하이퍼링크 밑줄 없게 적용, 글자 파란색 */
a{
	text-decoration: none;
	color: rgb(0,0,255);
}
li{
	border: 1px solid #222;
	padding: 20px;
	margin: 5px;
}
div{
	width: 300px;
</style>
</head>
<body>
<div>
<ul>
	<li><a href="test5.html">회사 소개</a></li>
	<li><a href="test5.html">도서</a></li>
	<li><a href="test5.html">자료실</a></li>
	<li><a href="test5.html">동영상 강의</a></li>
</ul>
</div>
</body>
</html>
```
![6](https://user-images.githubusercontent.com/95197594/154613296-420bf982-facf-4fb2-8e4a-aec8bcfb7f20.PNG)

```css
<title>Insert title here</title>
<style type="text/css">
	table {
/* 		표제목 위치 */
		caption-side: bottom;
/* 		칸여백 */
/* 		border-spacing:10px */
/* 테이블 선, td칸 선 합치기, 기본 separate; */
		border-collapse:collapse;

		border: 1px solid black;
		width: 500px;
	}
	td, th{
/* 		border: 2px dotted blue; */
		border: 1px solid black;
		padding: 10px;
/* 		글자 가운데 정렬 */
		text-align: center;
		background-color: silver;
	}
</style>
</head>
<body>
<h2>표 합치기 연습</h2>
<table>
<caption>상품 구성</caption>
<tr><td>용도</td><td colspan="3">중량 개수 가격</td>                      </tr>
<tr><td rowspan="2">선물용</td><td>3kg</td><td>11-16과</td><td>35000원</td></tr>
<tr>                         <td>5kg</td><td>18-26과</td><td>52000원</td></tr>
<tr><td rowspan="2">가정용</td><td>3kg</td><td>11-16과</td><td>35000원</td></tr>
<tr>                          <td>5kg</td><td>18-26과</td><td>47000원</td></tr>
</table>

</body>
</html>
```
![7](https://user-images.githubusercontent.com/95197594/154613310-d558612b-71d8-46f8-9f2e-b39782279fba.PNG)

```css
<style type="text/css">

/* 	th, td 테두리선 1px 실선 검정, 안 여백 10px */
/* th 배경색 #eee */

table {
/* 	table 태그에 테두리선 1px 실선 검정
	테이블 제목 아래, 테이블 선 합치기 */

		border: 1px solid black;
		caption-side: bottom;
		border-collapse: collapse;
}

th, td {
/* 	th, td 테두리선 1px 실선 검정, 안 여백 10px */
		border: 1px solid black;
		padding: 10px;
}

th {
/* th 배경색 #eee */
	background-color: #eee;
}

</style>
</head>
<body>
<table>
<caption>2019 국민 독서실태</caption>
<tr><th>구분</th><th>성인</th><th>독서자</th></tr>
<tr><th>종이책</th><td>6.1권</td><td>11.8권</td></tr>
<tr><th>전자책</th><td>1.2권</td><td>7.1권</td></tr>
<tr><th>오디오북</th><td>0.2권</td><td>5.5권</td></tr>
</table>

</body>
</html>
```
![8](https://user-images.githubusercontent.com/95197594/154613314-8576e524-24bc-43e1-a504-ee3aae0b0248.PNG)
```css
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<style type="text/css">
	*{
/* h2, p, div		문단태그 전체영역(블럭 영역) */
/* span, strong		요소영역(인라인 영역) */
/* border: 1px solid black; */
	}
	p{
/* 		선모양 */
		border-width: 5px;
		border-style: dotted;
		border-color: green;
	}

</style>
</head>
<body>
<h2>레드향</h2>
<p>껍질에 붉은빛이 돌아 <span>레드향</span>이라 불린다</p>
<p>비타민C와 비타민 P가 풍부해
혈액순환, 감기예방 등에 좋은 것으로 알려져 있다</p>
<h2>일정</h2>
<h2>먹거리</h2>
</body>
</html>
```
![캡처](https://user-images.githubusercontent.com/95197594/154614163-e5f5d078-1856-4e10-8905-2d20422cb1c4.PNG)


# [오후수업] JSP 17차
## 테스트

```javascript
E-Mail 셀렉트박스 항목
1) 직접입력
2) naver.com
3) nate.com
4) gmail.com

직업 셀렉트박스 항목
1) 항목을 선택하세요
2) 개발자
3) DB엔지니어
4) 관리자
5) 기타

가입 버튼 - submit
초기화 버튼 - reset
돌아가기 버튼 - button(이전페이지로 돌아가기)
----------------------------------------------
1. ID 중복확인 버튼 클릭 시 새 창(check_id.html) 띄우기
=> 단, 입력된 ID 텍스트의 길이가 4 ~ 8글자 사이일 때만 새 창을 띄우고
    아니면, "4~8글자만 사용가능합니다" 메세지 출력하기(alert() 함수 사용)
=> 새 창 띄울 때 ID중복확인 버튼 우측 빈공간에 "중복확인완료" 표시하기

2. 비밀번호 입력란에 키를 누를때마다 비밀번호 길이 체크하기
=> 체크 결과를 비밀번호 입력창 우측 빈공간에 표시하기
=> 비밀번호 길이 체크를 통해 8 ~ 16글자 사이이면 "사용 가능한 패스워드"(파란색) 표시,
    아니면, "사용 불가능한 패스워드"(빨간색) 표시

3. 비밀번호확인 입력란에 키를 누를때마다 비밀번호와 같은지 체크하기
=> 체크 결과를 비밀번호확인 입력창 우측 빈공간에 표시하기
=> 비밀번호와 비밀번호확인 입력 내용이 같으면 "비밀번호 일치"(파란색) 표시,
    아니면, "비밀번호 불일치"(빨간색) 표시

4. 주민번호 숫자 입력할때마다 길이 체크하기
=> 주민번호 앞자리 입력란에 입력된 숫자가 6자리이면 뒷자리 입력란으로 커서 이동시키기
=> 주민번호 뒷자리 입력란에 입력된 숫자가 7자리이면 뒷자리 입력란에서 커서 제거하기

5. 이메일 도메인 선택 셀렉트 박스 항목 변경 시 선택된 셀렉트 박스 값을 
   이메일 두번째 항목(@ 기호 뒤)에 표시하기
   단, 직접입력 선택 시 표시된 도메인 삭제하기

6. 취미의 "전체선택" 체크박스 체크 시 취미 항목 모두 체크, 
   "전체선택" 해제 시 취미 항목 모두 체크 해제하기
-------------------------------------------------------------------------------------------------
7. submit 버튼 클릭 시 필수 조건들이 만족할 경우에만 다음 페이지로 이동하기(submit())
    - 아이디 중복 확인 버튼을 통해 아이디 중복 확인을 수행하고(버튼 클릭으로 대체(임시))
    - 비밀번호와 비밀번호확인 두 개가 일치하고
    - 주민번호 앞자리와 뒷자리가 모두 정상적으로 입력되고
    - 이메일이 정상적으로 입력됐을 경우
    => 위의 모든 조건이 만족할 경우 true 리턴하여 submit 작업을 수행하고
       아니면, false 를 리턴하여 submit 작업을 취소
    => 단, 현재 함수에서 다른 작업의 수행여부를 확인하려면
         해당 작업 수행 후 전역변수를 사용하여 값을 변경해 놓아야함





<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<script type="text/javascript">
	// submit 작업을 수행하기 전 각 작업의 완료 여부를 체크할 변수(전역변수) 선언
	var isCheckDuplicateId = false; // ID 중복확인 체크 변수
	var isCheckConfirmPasswd = false; // 패스워드 중복확인 체크 변수
	

	// 1. ID 중복확인 버튼 클릭 시 새 창 띄우기
	function checkDuplicateId() {
		// 입력받은 ID 값 가져와서 4 ~ 8자리 일 경우에만 새 창 띄우기
		var id = document.fr.id.value;
// 		alert(id);
		
		if(id.length >= 4 && id.length <= 8) { // 유효한 아이디일 경우
			window.open("check_id.html", "check", "width=300,height=200");
			document.getElementById("checkIdResult").innerHTML = "중복확인완료";
			
			// 중복확인 완료 표시를 위해 isCheckDuplicateId 변수값을 true 로 변경
			isCheckDuplicateId = true;
		} else { // 유효하지 않은 아이디일 경우
			alert("4~8글자만 사용 가능합니다.");
			document.getElementById("checkIdResult").innerHTML = "";
			
			isCheckDuplicateId = false;
		}
	}
	
	// 2. 비밀번호 입력란에 키를 누를때마다 비밀번호 길이 체크하기
	// => 체크 결과를 비밀번호 입력창 우측 빈공간에 표시하기
	// => 비밀번호 길이 체크를 통해 8 ~ 16글자 사이이면 "사용 가능한 패스워드"(파란색) 표시,
    // 아니면, "사용 불가능한 패스워드"(빨간색) 표시
	function checkPasswd(passwd) {
		// span 태그 영역(checkPasswdResult)
		var span_checkPasswdResult = document.getElementById("checkPasswdResult");
		
		// 입력된 패스워드 길이 체크
		if(passwd.length >= 8 && passwd.length <= 16) {
			span_checkPasswdResult.innerHTML = "사용 가능한 패스워드";
			span_checkPasswdResult.style.color = "BLUE";
		} else {
			span_checkPasswdResult.innerHTML = "사용 불가능한 패스워드";
			span_checkPasswdResult.style.color = "RED";
		}
	}
	
	// 3. 비밀번호확인 입력란에 키를 누를때마다 비밀번호와 같은지 체크하기
	// => 체크 결과를 비밀번호확인 입력창 우측 빈공간에 표시하기
	// => 비밀번호와 비밀번호확인 입력 내용이 같으면 "비밀번호 일치"(파란색) 표시,
	//    아니면, "비밀번호 불일치"(빨간색) 표시
	function checkConfirmPasswd(confirmPasswd) {
		// 패스워드 입력란에 입력된 패스워드를 가져오기
		var passwd = document.fr.passwd.value;
		
		var span_checkConfirmPasswdResult = document.getElementById("checkConfirmPasswdResult");
		
		// 패스워드 일치 여부 판별
		if(passwd == confirmPasswd) { // 일치할 경우
			span_checkConfirmPasswdResult.innerHTML = "비밀번호 일치";
			span_checkConfirmPasswdResult.style.color = "BLUE";
			
			// 패스워드 일치 여부 확인을 위해 isCheckConfirmPasswd 변수값을 true 로 변경
			isCheckConfirmPasswd = true;
		} else { // 일치하지 않을 경우
			span_checkConfirmPasswdResult.innerHTML = "비밀번호 불일치";
			span_checkConfirmPasswdResult.style.color = "RED";

			isCheckConfirmPasswd = false;
		}
	}
	
	// 4. 주민번호 숫자 입력할때마다 길이 체크하기
	// => 주민번호 앞자리 입력란에 입력된 숫자가 6자리이면 뒷자리 입력란으로 커서 이동시키기
	// => 주민번호 뒷자리 입력란에 입력된 숫자가 7자리이면 뒷자리 입력란에서 커서 제거하기
	function checkJumin1(jumin1) {
		if(jumin1.length == 6) {
			document.fr.jumin2.focus(); // 커서 요청(포커스 요청)
		}
	}
	
	function checkJumin2(jumin2) {
		if(jumin2.length == 7) {
			document.fr.jumin2.blur(); // 커서 제거(포커스 해제)
		}
	}
	
	// 5. 이메일 도메인 선택 셀렉트 박스 항목 변경 시 선택된 셀렉트 박스 값을 
	//    이메일 두번째 항목(@ 기호 뒤)에 표시하기
	//    단, 직접입력 선택 시 표시된 도메인 삭제하기
	function selectDomain(domain) {
		document.fr.email2.value = domain;
		
		// 만약, "직접입력" 항목 선택 시 도메인 입력창(email2)에 커서 요청(옵션)
		if(domain == "") { // "직접입력" 선택 시 domain 변수에 "" 값이 전달됨
			document.fr.email2.disabled = false; // 입력창 잠금 해제
			document.fr.email2.focus(); // 커서 요청
		} else { // 다른 도메인 선택 시
			document.fr.email2.disabled = true; // 입력창 잠금
		} 
	}
	
	// 6. 취미의 "전체선택" 체크박스 체크 시 취미 항목 모두 체크, 
	//    "전체선택" 해제 시 취미 항목 모두 체크 해제하기
	function checkAll(isChecked) {
		// 전체선택 체크 항목값(isChecked)에 따라 취미 항목 체크 또는 체크 해제
		if(isChecked) { // isChecked == true 와 동일한 조건식
			// 복수개의 동일한 name 값을 갖는 체크박스는 배열로 관리되므로 배열 인덱스 활용
			document.fr.hobby[0].checked = true;
			document.fr.hobby[1].checked = true;
			document.fr.hobby[2].checked = true;
		} else {
			document.fr.hobby[0].checked = false;
			document.fr.hobby[1].checked = false;
			document.fr.hobby[2].checked = false;
		}
	}
	
	function checkForm() {
		/*
		7. submit 버튼 클릭 시 필수 조건들이 만족할 경우에만 다음 페이지로 이동하기(submit())
	    - 아이디 중복 확인 버튼을 통해 아이디 중복 확인을 수행하고(버튼 클릭으로 대체(임시))
	    - 비밀번호와 비밀번호확인 두 개가 일치하고
	    - 주민번호 앞자리와 뒷자리가 모두 정상적으로 입력되고
	    - 이메일이 정상적으로 입력됐을 경우
	    => 위의 모든 조건이 만족할 경우 true 리턴하여 submit 작업을 수행하고
	       아니면, false 를 리턴하여 submit 작업을 취소
	    => 단, 현재 함수에서 다른 작업의 수행여부를 확인하려면
	       해당 작업 수행 후 전역변수를 사용하여 값을 변경해 놓아야함
	    */
	    // 아이디와 패스워드는 전역변수에 저장된 boolean 타입 값을 확인하고
	    // 주민번호, 이메일은 해당 입력값 자체를 가져와서 확인
	    if(!isCheckDuplicateId) { // 아이디 중복확인을 수행하지 않은 경우(또는 중복인 경우)
	    	alert("아이디 중복확인 필수!");
	    	document.fr.id.focus();
	    	// 더 이상 작업이 진행되지 않고, submit 동작을 취소하기 위해 false 값 리턴
	    	return false; // 리턴값이 폼태그의 onsubmit="return checkForm()" 부분으로 전달되어
	    	// false 일 때 return false 가 되어 submit 동작이 취소됨(생략 시 true 리턴됨)
	    } else if(!isCheckConfirmPasswd) {
	    	alert("패스워드 확인 필수!");
	    	document.fr.passwd2.focus();
	    	return false;
	    } else if(document.fr.jumin1.value.length != 6) {
	    	alert("주민번호 앞자리 6자리 필수!");
	    	document.fr.jumin1.focus();
	    	return false;
	    } else if(document.fr.jumin2.value.length != 7) {
	    	alert("주민번호 뒷자리 7자리 필수!");
	    	document.fr.jumin2.focus();
	    	return false;
	    } else if(document.fr.email1.value.length == 0) {
	    	alert("이메일 계정 입력 필수!");
	    	document.fr.email1.focus();
	    	return false;
	    } else if(document.fr.email2.value.length == 0) {
	    	alert("이메일 도메인 입력 필수!");
	    	document.fr.email2.focus();
	    	return false;
	    }
	    
	    // 모든 조건을 통과했을 경우 submit 동작 실행
// 	    return true; // 생략 가능
	}
</script>
</head>
<body>
	<h1>회원 가입</h1>
	<form action="a.html" name="fr" onsubmit="return checkForm()">
		<table border="1">
			<tr><td>이름</td><td><input type="text" name="name" required="required"></td></tr>
			<tr>
				<td>ID</td>
				<td>
					<input type="text" name="id" placeholder="4 ~ 8글자 사이 입력" required="required">
					<input type="button" value="ID중복확인" onclick="checkDuplicateId()">
					<span id="checkIdResult"></span>
				</td>
			</tr>
			<tr>
				<td>비밀번호</td>
				<td>
					<input type="password" name="passwd" onkeyup="checkPasswd(this.value)" placeholder="8 ~ 16글자 사이 입력" required="required">
					<span id="checkPasswdResult"></span>
				</td>
			</tr>
			<tr>
				<td>비밀번호확인</td>
				<td>
					<input type="password" name="passwd2" onkeyup="checkConfirmPasswd(this.value)" required="required">
					<span id="checkConfirmPasswdResult"></span>
				</td>
			</tr>
			<tr>
				<td>주민번호</td>
				<td>
					<input type="text" name="jumin1" onkeyup="checkJumin1(this.value)" required="required"> -
					<input type="password" name="jumin2" onkeyup="checkJumin2(this.value)" required="required">
				</td>
			</tr>
			<tr>
				<td>E-Mail</td>
				<td>
					<input type="text" name="email1" required="required">@
					<input type="text" name="email2" required="required">
					<select name="emailDomain" onchange="selectDomain(this.value)">
						<option value="">직접입력</option>
						<option value="naver.com">naver.com</option>
						<option value="nate.com">nate.com</option>
						<option value="daum.net">daum.net</option>
					</select>
				</td>
			</tr>
			<tr>
				<td>직업</td>
				<td>
					<select name="job" required="required">
						<option value="">항목을 선택하세요</option>
						<option value="개발자">개발자</option>
						<option value="DB엔지니어">DB엔지니어</option>
						<option value="관리자">관리자</option>
						<option value="기타">기타</option>
					</select>
				</td>
			</tr>
			<tr>
				<td>성별</td>
				<td>
					<input type="radio" name="gender" value="남">남
					<input type="radio" name="gender" value="여">여
				</td>
			</tr>
			<tr>
				<td>취미</td>
				<td>
					<input type="checkbox" name="hobby" value="여행">여행
					<input type="checkbox" name="hobby" value="독서">독서
					<input type="checkbox" name="hobby" value="게임">게임
					<!-- 전체선택 체크박스 클릭 시 체크 상태(true, false)를 함수에 전달(함수 이름과 name 속성 다르게!) -->
					<input type="checkbox" name="check_all" value="전체선택" 
										onclick="checkAll(this.checked)">전체선택
				</td>
			</tr>
			<tr>
				<td>가입동기</td>
				<td><textarea name="content" cols="40" rows="10"></textarea></td>
			</tr>
			<tr>
				<td colspan="2">
					<input type="submit" value="가입">
					<input type="reset" value="초기화">
					<input type="button" value="돌아가기" onclick="history.back()">
				</td>
			</tr>
		</table>
	</form>
</body>
</html>
```








