# [오전수업] web 3차
## 간단한 회원가입 양식 만들기
```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>html1/test6.html</title>
</head>
<body>
<h1>회원가입</h1>
<form action="" method="get">
아이디<br><input type="text" name="id"><br>
비밀번호<br><input type="password" name="pd"><br>
비밀번호 재확인<br><input type="password" name="pd2"><br>
이름<br><input type="text" name="name"><br>
생년월일<br>
<input type="text" name="year" placeholder= "년">
<select name="month">
	<option value="목0">월</option>
	<option value="목1">1월</option>
	<option value="목2">2월</option>
	<option value="목3">3월</option>
	<option value="목4">4월</option>
	<option value="목5">5월</option>
	<option value="목6">6월</option>
	<option value="목7">7월</option>
	<option value="목8">8월</option>
	<option value="목9">9월</option>
	<option value="목10">10월</option>
	<option value="목11">11월</option>
	<option value="목12">12월</option>
</select>
<input type="text" name="day" placeholder="일"><br>
성별<br><select name="gender">
	<option value="목0">성별</option>
	<option value="목1">남성</option>
	<option value="목2">여성</option>
	<option value="목3">선택안함</option>
	</select><br>
	<br><br><br><br><input type="submit" value="가입하기">


</form>




</body>
</html>
```

![제목 없음](https://user-images.githubusercontent.com/95197594/152452513-0071237b-a4cc-4652-b32d-01bafe2eee62.png)

---

## 여러가지 효과 태그들
```html
<u></u> 태그 : 글자에 밑줄
<del></del> 태그 : 글자에 삭제선
<i></i> 태그 : 글자 기울임
<em></em> 태그 : 글자 기울임
<b></b> 태그 : 글자를 진하게
<strong></strong> 태그 : 글자를 진하게
<sup></sup> 태그 : 글자를 위첨자로
<sub></sub> 태그 : 글자를 아래첨자로

<blockquote></blockquote> 태그 : 인용문, 들여쓰기에 사용
<pre></pre> 태그 : 태그 안의 글자를 형식에 상관없이 보여주고 싶을 때

<div></div> 태그 : 큰 영역 지정 (웹문서 블록 지정)
<span></span> 태그 : 작은 영역(인라인) 지정 

<hr> 태그 : 수평선 
&nbsp; 태그 : 공백 
&lt; : 작은 꺽쇠
&gt; : 큰 꺽쇠
&copy; &amp; &quot; 등등 & 입력 후 ctrl+space키 누르면 다양한 특수문자 사용 가능
ㅁ + 한자키도 사용 가능

----------------------------------------------------------------------------------------------------------------------------


<p><u>껍질</u><sup>에</sup> <del>붉은</del> <i>빛이</i> <em>돌아</em> <b>레드향</b><sub>이라</sub> <strong>불린다</strong></p>
<blockquote>
인용문<br>
들여쓰기<br>
</blockquote>
<pre>
		원하는
				모양대로
							출력
</pre>

<div>큰 영역 지정 (웹문서 블록 지정)</div>
<span>작은 영역(인라인) 블록 지정</span>

<hr>
<h1>특수문자입력태그</h1>
공&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;백
&lt; 태그 특수 문자 &gt;
&copy; &amp; &quot;

<!-- & ctrl space -->

<!-- ♣ ㅁ 한자키 -->


```
![캡처](https://user-images.githubusercontent.com/95197594/152455620-6387e193-18fa-433e-84dc-8754bcdc0a6b.PNG)


---

## 여러가지 리스트들
```html
- <ol></ol> 태그 : 순서 있는 항목
- <ul></ul> 태그 : 순서 없는 항목
- <dl></dl> 태그 : 설명 목록
```

```html
<body>
<h1>순서있는 목록</h1>
<ol type = "a">
	<li>항목1</li>
	<li>항목2</li>
	<li>항목3</li>
</ol>

<h1>순서없는 목록</h1>
<ul type = "disc">
	<li>항목1</li>
	<li>항목2</li>
	<li>항목3</li>
</ul>
	
<h1>설명 목록</h1>
<dl>
	<dt>이름</dt>
	<dd>값</dd>
</dl>	

<dl>
	<dt>선물용 3kg</dt>
	<dd>소과 13 ~ 16과</dd>
	<dd>중과 10 ~ 12과</dd>
</dl>

</body>
```
<html>
<head>
<meta charset="UTF-8">
</head>
<body>
<h3>순서있는 목록</h3>
<ol type = "a">
	<li>항목1</li>
	<li>항목2</li>
	<li>항목3</li>
</ol>

<h3>순서없는 목록</h3>
<ul type = "disc">
	<li>항목1</li>
	<li>항목2</li>
	<li>항목3</li>
</ul>
	
<h3>설명 목록</h3>
<dl>
	<dt>이름</dt>
	<dd>값</dd>
</dl>	

<dl>
	<dt>선물용 3kg</dt>
	<dd>소과 13 ~ 16과</dd>
	<dd>중과 10 ~ 12과</dd>
</dl>

</body>
</html>

---

## 표 작성 태그
```html
<caption></caption> 태그 : 표 제목
<thead></thead> 태그 : 표 제목 영역 설정
<th></th> 태그 : 글자 굵게, 가운데 정렬
```
```html
<h2>상품구성</h2>
<table border="1" width=500>
<caption>선물용과 가정용 상품 구성</caption>

<thead>
<tr><th>용도</th><th>중량</th><th>갯수</th><th>가격</th></tr>
</thead>

<tbody>
<tr><td>선물용</td><td>3kg</td><td>11-16과</td><td>35000원</td></tr>
<tr><td>선물용</td><td>5kg</td><td>18-26과</td><td>52000원</td></tr>
<tr><td>가정용</td><td>3kg</td><td>11-16과</td><td>30000원</td></tr>
<tr><td>가정용</td><td>5kg</td><td>18-26과</td><td>47000원</td></tr>
</tbody>
</table>
```
<h2>상품구성</h2>
<table border="1" width=500>
<caption>선물용과 가정용 상품 구성</caption>

<thead>
<tr><th>용도</th><th>중량</th><th>갯수</th><th>가격</th></tr>
</thead>

<tbody>
<tr><td>선물용</td><td>3kg</td><td>11-16과</td><td>35000원</td></tr>
<tr><td>선물용</td><td>5kg</td><td>18-26과</td><td>52000원</td></tr>
<tr><td>가정용</td><td>3kg</td><td>11-16과</td><td>30000원</td></tr>
<tr><td>가정용</td><td>5kg</td><td>18-26과</td><td>47000원</td></tr>
</tbody>
</table>

---

## 이미지
- 웹에서 사용하는 이미지 형식 .jpg .gif .png 
	- jpg : 사진형태, 색상과 명암을 다양하게 표현
	- gif : 256색상 사용, 작은 아이콘, 작은 이미지 사용
	- png : 색상 다양하게 표현, 투명한 배경색, 웹에서 많이 사용

- 픽셀 : 화명을 이루는 빛(하나의 점), 해상도 : 빛(점의 개수)
- px : 이미지의 크기를 픽셀 지정, 고정값
- % : 이미지의 크기를 브라우저 크기에 따라서 변동
- alt : 이미지가 안 보일 때 이미지 설명
```
<img src="4.jpg" width="500px" height="500px">
<img src="5.jpg" width="50%" height="50%">
<img src="1.jpg" alt="이미지설명">
```
- 이미지는 현재 작성중인 파일과 같은 폴더에 이미지가 파일이 있어야 함
	- 다른 폴더에 존재할 경우 ..(상위폴더 표시) 경로를 지정해주어야 함 
	- 열려져 있는 파일을 기준으로 다른 파일을 찾아야 함 (상대참조라고 부름)
```
<img src="../html1/2.jpg">
<img src="../html1/img1/5.jpg">
<img src="../html1/img1/6.png">
```


---


> 그 외 저번 시간에 학습했던 colspan과 rowspan을 이용한 표 합치기 복습 실시

---

# [오후수업] 
> - 메서드 및 리턴문 마무리 수업. day8 커밋에 추가하였음
> - JSP 강사님의 자바 수업은 끝. 오늘부터 본격적 JSP 수업 시작

---
**시작 전 기본설정**
- Project Explorer 마우스 오른쪽 -> New -> Dynamic Project 생성 (강의실에서는 편의상 StudyJavascript로 이름 생성)
	- 생성시 Apache Tomcat v8.5 설정 되어 있어야 함.
		- 서버 실행 안 될 시 day1 커밋 참고하여 재설정

- Dynamic Project 새로 생성 -> Next -> Next -> web.xml 생성 체크하고 Finish 후 생성된 곳에서 src -> main -> webapp -> WEB-INF 안에 있는 web.xml을 복사하여 StudyJavascript의 WEB-INF 안에 붙여넣기 -> 방금 만든 Dynamic Project는 삭제

## 자바스크립트 개요
- 자바와 아무런 관계가 없는 스크립트 언어
- 별도의 준비나 컴파일(번역)없이 일반적인 문자 형태로 작성한 후 웹브라우저에서 바로 실행이 가능한 언어
- 개발자로부터 생성된 웹페이지에 사용자가 액션을 취했을 때 해당 액션에 대한 반응을 수행할 수 있도록 해주는 언어
	- ex) 회원가입 버튼 클릭 시 버튼에 대한 동작 처리 등
		- HTML 문서 내용 수정, 마우스 또는 키보드 등의 동작에 대한 반응
- 대부분의 웹브라우저에서 지원
- 객체 기반(객체 지향) 언어이며 무료로 공개되어 있고, 라이브러리 다양
- 자바스크립트에서 제공되는 내장객체들과 내장함수들을 사용하여 다양한 작업이 가능함
	- ex) document 객체, alert() 함수 등


> 자바스크립트를 사용하는 이유
> - JQuery 같은 라이브러리를 활용하여 좀 더 다양한 동작 처리 가능
> - 서버에 부하가 적게 걸림(= 자바스크립트 코드는 웹브라우저가 처리하므로)
> - 정적인 웹사이트 -> 동적인 웹사이트로 변환 가능

















### script 태그
- 자바스크립트 사용을 위해서는 <script> 태그를 사용하여 내부에 자바스크립트 코드를 기술해야함
```javascript
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<!-- 자바스크립트 사용을 위해서는 <script> 태그를 사용하여 내부에 자바스크립트 코드를 기술해야함-->
 <script type="text/javascript">
	 
 	// 자바스크립트 코드가 기술되는 위치

 </script>
</head>
<body>
	<h1>test.html</h1>
	<!-- head 태그 내에서 뿐만 아니라 body 태그 내에서도 script 태그 사용 가능함 -->
</body>
</html>
```	

### 주석
- 자바스크립트에서는 자바의 주석과 동일한 주석 사용 가능함
- 범위 주석도 사용 가능
```javascript
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>

 <script type="text/javascript">
 	// 자바스크립트에서는 자바의 주석과 동일한 주석 사용 가능함
 	/* 범위 주석도 사용 가능하다! */
 </script>
</head>
<body>
	<h1>test.html</h1>
	
</body>
</html>	
```

### alert 함수
- 특정 데이터를 웹브라우저 팝업 창에 출력하는 내장 함수
	
```javascript
html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>

 <script type="text/javascript">
 
 	// alert(데이터); 함수는 특정 데이터를 웹브라우저 팝업 창에 출력하는 내장 함수
 	alert("자바스크립트");
 	alert("alert 함수 호출됨");
 </script>
	
</head>
<body>
	<h1>test.html</h1>
	<!-- head 태그 내에서 뿐만 아니라 body 태그 내에서도 script 태그 사용 가능함 -->
	<script type="text/javascript">
		alert("이 곳은 body 태그 내부입니다");
	</script>
</body>
</html>
```

### 외부로부터 자바스크립트 파일을 불러오기
현재 쓰고 있는 html 파일(강의 중 사용하는 파일은 test.html) 과 같은 폴더에 New -> Javascript file 생성 -> 생성한 파일에  
```
alert("외부 자바스크립트 확인");
```
입력

다시 test..html로 복귀

```javascript
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>

 <script type="text/javascript">

 alert("alert 함수 호출됨");
 
 </script>
 
 <!-- 외부로부터 자바스크립트 파일을 불러오기 위해서는 script 태그를 사용하여
 src 속성을 통해외부 자바스크립트 파일을 지정해야 함 -->
 <script type="text/javascript" src="./test.js">
 
 	// src 속성이 포함된 script 태그 내의 자바스크립트 코드는 실행되지 않는다!
 	alert("이 코드는 실행이 될까요?"); // 실행되지 않음
	
	</script>
 
</head>
<body>
	<h1>test.html</h1>
	<script type="text/javascript">
		alert("이 곳은 body 태그 내부입니다");
	</script>
</body>
</html>
```

### 자바스크립트에서의 변수 사용법
```
< 기본 문법 >
데이터타입 변수명 = 값;
```
- 기본적인 문법은 자바의 변수 사용과 거의 동일함
- 자바의 데이터타입과 동일한 데이터타입을 사용하지는 않음(동일한 타입도 존재함)
	- ex) 문자와 문자열의 구분 없이 string 타입 사용(''와 "" 구분 없이 사용)
	- 숫자(number), 논리(boolean), 특수값 null 사용 가능
- 별도로 타입을 정하지 않고 일반적인(공통적인) 타입으로 변수 선언 가능함. 이 때, 저장되는 데이터에 따라 변수의 타입이 자동으로 결정됨
	- 공통 타입으로 사용되는 키워드는 var 또는 let 또는 const 중 하나 사용
- 변수명 지정 시 자바의 식별자 작성 규칙과 거의 동일함
- 특정 데이터 또는 변수의 타입 확인 방법 : typeof 키워드 사용


### 공통타입으로 변수 선언 및 초기화
```javascript
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<script type="text/javascript">

	
	// 공통타입으로 변수 선언 및 초기화
//	var num; // 데이터 저장하지 않으면 undefined 타입으로 변수 num 선언됨(타입 미정)
//	alert("num 의 타입 : " + typeof(num));
	
	var num = 10;
	alert("num 의 타입 : " + typeof(num)); // number 타입
	
	var str = "자바스크립트";
	alert("str 의 타입 : " + typeof(str)); // string 타입
	
	var b = true;
	alert("b 의 타입 : " + typeof(b)); // boolean 타입
	
	var v = null; // 특수한 값인 null 값도 저장 가능
	alert("v 의 타입 : " + typeof(v)); // boolean 타입
	
</script>
</head>
<body>

</body>
</html>
```
