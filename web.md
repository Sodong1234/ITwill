> 18일 실시할 web 시험 대비 노트 작성. 시험 종료 후 삭제 예정




- 프론트엔드 개발(HTML, CSS, Javascript, jQuery)
	- 사용자가 보는 화면에 관련된 작업들
- 백엔드 개발(자바, JSP, MVC, 스프링)
	- 사용자의 눈에 보이지 않는 화면
- ex) (네이버 로그인창을 예시로) 뼈대는 HTML, 꾸미는 것은 CSS, 아이디 & 비밀번호 입력 필수 메시지만 띄우고 다음 단계로 진행 시키지 않는 것은 Javascript&jQuery, 서버는 백엔드



### HTML(Hyper Text Markup Language)
- 웹문서를 만드는 기본언어.
- Hyper Text : 문서를 서로 연결해주는 링크 + Markup : 표시한다
- HTML 기본기능 : 웹브라우저에 보여줄 내용에 마크업(표시하고) 문서끼지 링크하는 것.
- 세계표준화(웹 표준화)가 되어 있으며, 
```
https://w3c.or.kr/
https://www.w3.org/
https://www.w3schools.com/
```
등지에서 다양한 정보를 얻을 수 있음.
- 웹브라우저 - 웹페이지(HTML)을 실행하는 도구. 크롬, 엣지, 파이어폭스 등등이 사용됨.
- 웹편집기 - 메모장, 에디터(notepad++ 등), 이클립스, 비주얼스튜디오 코드(유료)


## table을 만드는 방법

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>html1/test4.html</title>
</head>
<body>
<h2>표만들기</h2>
<table border="1">
<tr><td>1행1열</td><td>1행2열</td></tr>
<tr><td>2행1열</td><td>2행2열</td></tr>
</table>
</body>
</html>
```

table를 만들때는 table, tr, td 코드를 사용함
- tr은 행(가로), td는 열(세로)
- table 옆에 붙이는 border는 선의 크기(개수)를 나타냄.



### 5행 4열 표 만들기
```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>html1/test4.html</title>
</head>
<body>
</table>
<h2>5행 4열 표 만들기</h2>
<table border="1">
<tr><td>용도</td><td>중량</td><td>개수</td><td>가격</td></tr>
<tr><td>선물용</td><td>3kg</td><td>11-16과</td><td>35000원</td></tr>
<tr><td>선물용</td><td>5kg</td><td>18-26과</td><td>52000원</td></tr>
<tr><td>가정용</td><td>3kg</td><td>11-16과</td><td>30000원</td></tr>
<tr><td>가정용</td><td>5kg</td><td>18-26과</td><td>47000원</td></tr>
</table>
</body>
</html>
```
![image](https://user-images.githubusercontent.com/95197594/151272313-3b7aec68-8e1b-4bf4-bac7-371fac784d62.png)


### 3행 3열 표 만든 후 표 합치기
```html

</table>
<h2>3행3열 표합치기</h2>
<table border = "1">
<tr><td>1행1열</td><td>1행2열1행3열</td></tr>
<tr><td>2행1열</td><td>2행2열</td><td>2행3열</td></tr>
<tr><td>3행1열</td><td>3행2열</td><td>3행3열</td></tr>
</table>
```
![1](https://user-images.githubusercontent.com/95197594/151276364-5c4091f9-d74d-4b47-b575-73ee579a0aa2.PNG)

그냥 합쳐버리면 표에 흔적이 남아있기 때문에 colspan 태그를 추가해주어야 함.

```html
<h2>3행3열 표합치기</h2>
<table border = "1">
<tr><td>1행1열</td><td colspan="2">1행2열1행3열</td></tr>
<tr><td>2행1열</td><td>2행2열</td><td>2행3열</td></tr>
<tr><td>3행2열</td><td>3행3열</td></tr>
```

![2](https://user-images.githubusercontent.com/95197594/151276369-98517a70-418b-41f3-8bd9-6b8591e3f81f.PNG)


2행1열과 3행1열을 합치고 싶을때는 rowspan 태그를 추가해주어야 함.

```html
<table border = "1">
<tr><td>1행1열</td><td colspan="2">1행2열1행3열</td></tr>
<tr><td rowspan="2">2행1열<br>3행1열</td><td>2행2열</td><td>2행3열</td></tr>
<tr><td>3행2열</td><td>3행3열</td></tr>
```

![3](https://user-images.githubusercontent.com/95197594/151276373-5cb81188-afac-43ff-81ca-978708b9a0b0.PNG)
![4](https://user-images.githubusercontent.com/95197594/151276375-c7e97aa6-1e82-472c-8748-365a36dc7be9.PNG)


### 5행 4열 표 합치기
```html
<h2>5행 4열 표 만들기</h2>
<table border="1">
<tr><td>용도</td><td colspan="3">중량 개수 가격</td></tr>
<tr><td rowspan="2">선물용</td><td>3kg</td><td>11-16과</td><td>35000원</td></tr>
<tr><td>5kg</td><td>18-26과</td><td>52000원</td></tr>
<tr><td rowspan="2">가정용</td><td>3kg</td><td>11-16과</td><td>30000원</td></tr>
<tr><td>5kg</td><td>18-26과</td><td>47000원</td></tr>
</table>
</body>
</html>
```

- ![5](https://user-images.githubusercontent.com/95197594/151277438-4c77b1bb-5f4b-40ba-9650-634c17f371d0.PNG)

## 양식(폼)태그
```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>html1/test5.html</title>
</head>
<body>
<h1>양식(폼)태그</h1>
<form action="test4.html" method="post">
<input type="submit" value="전송버튼">
</form>
</body>
</html>
```

![1](https://user-images.githubusercontent.com/95197594/151279302-614ff3ff-6772-4ed1-810d-a2d9668f7593.PNG)
- 전송버튼을 누르면 test4.html로 이동
- form 태그는 이동 / input 태그는 형태생성(텍스트, 버튼 등) 담당
  - form 태그 안에 입력한 데이터를 가지고 서버에 전송하면서 action에 저장된 페이지로 이동
- method는 데이터를 전송하는 방식
  - get 방식은 주소줄에 데이터가 보이면서 서버 전송 (get 방식은 따로 태그를 추가하지 않아도 자동으로 주소줄에 데이터가 보임)
  - post 방식은 주소줄에 데이터가 안 보이면서 서버 전송  
- value는 서버에 전송되어질 입력값


### 텍스트를 입력 가능한 상자 생성
```html
<body>
<h1>양식(폼)태그</h1>
<form action="test4.html" method="post">
아이디 : <input type="text" name="id" value="Lee"><br>
비밀번호 : <input type="password" name="pass"><br>
자기소개 : <textarea name="intro" rows="5" cols="10">안녕</textarea>
성별 : <input type="radio" name="gender" value="남">남성
       <input type="radio" name="gender" value="여">여성<br>
취미 : <input type="checkbox" name="hobby" value="여행">여행
       <input type="checkbox" name="game" value="게임">게임
       <input type="checkbox" name="health" value="운동">운동<br>
<input type="submit" value="전송버튼"><br>
목록상자 : <select name="sel">	     // name 뒤에 size 태그 추가시 넓게 보기 가능 ex) size="3". 하지만 많이 사용하지는 않음
	   <option value="목1">목록1</option>
	   <option value="목2">목록2</option>
	   <option value="목3">목록3</option>
	   </select>	   <br>
파일첨부 : <input type="file" name="file"><br>
히든태그 : <input type="hidden" name="hi" value="값"><br>
<input type="button" value="버튼"><br><br><br>	
<input type="submit" value="서버전송버튼"><br>	
<input type="image" src="1.jpg" width="100" height="100">	
<input type="reset" value="초기화"><br> 	
</form>
</body>
```
![캡처](https://user-images.githubusercontent.com/95197594/151293359-72cb1230-f72e-43cf-ae36-8eb037b0efe5.PNG)


- 이런 식으로 아이디, 비밀번호 입력상자 등등을 생성 가능.
- textarea 태그는 value 필요 없이 텍스트를 입력해놓으면 텍스트입력값이 형성됨
- rows, cols는 행/열 간격 설정

**사용한 input type 정리**
- text : 텍스트 입력
- password : 비밀번호 입력 (문자 입력시 글자가 비공개)
- intro : 넓은 텍스트입력란
- radio : 선택지 (하나만 선택)
- checkbox : 선택지 (다중선택)
- file : 첨부파일 업로드 가능 버튼
- hiddle : 본문에는 보이지 않지만 숨겨서 데이터를 넘길 수 있는 태그
- submit : 기능이 포함되어 있는 버튼 (form 태그에 있는 데이터들을 서버에 전송)
	- image : submit 버튼에 이미지 추가
- button : 기능이 없는 버튼 (자바스크립트와 제이쿼리에서 주로 사용)
- reset : 리셋버튼


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

- 픽셀 : 화을 이루는 빛(하나의 점), 해상도 : 빛(점의 개수)
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
	- 상대참조 : 열려져 있는 파일을 기준으로 다른 파일 찾는 방법
	- 절대참조 : 서버 / 기준
```
<img src="../html1/2.jpg">
<img src="../html1/img1/5.jpg">
<img src="../html1/img1/6.png">
```

## 다양한 하이퍼링크 연결법
```html
<body>
<h1>하이퍼링크(웹문서 연결)</h1>
<a href="http://www.naver.com">네이버 연결</a><br>
<!-- test6.html 같은 폴더에 있는 문서 test5.html -->
<a href="./test5.html">test5.html 연결</a><br>
<!-- html1 폴더의 test1.html 연결 -->
<a href="../html1/test1.html">html1 폴더에 test1.html 연결</a><br>
<!-- 이미지를 클릭했을 때 ex.html 연결 -->
<a href="ex1.html"><img src="4.jpg"></a><br>
<!-- 이미지 연결 -->
<a href="5.jpg" >5.jpg 연결</a><br>
<!-- 이미지 연결 => 다운로드 -->
<a href="5.jpg" download>5.jpg 다운로드</a><br>
<!-- 새 창에서 이미지 연결 -->
<a href="4.jpg" target="_blank">4.jpg 새 창에서 연결</a><br>
<!-- ex3.html을 새 창에서 연결 -->
<a href="./ex3.html" target="_blank">ex3.html 새 창에서 연결</a>
<!-- 순서 없는 목록, 목록1(ex4.html) 연결, 목록2(html1폴더-img1폴더 6.png 다운로드)연결 -->
<ul>
	<li><a href="ex4.html">ex4.html</a></li>
	<li><a href="../html1/img1/6.png" download>6.png 다운로드</a></li>
</ul>
<br>
<!-- 문서 내에서 표시이름 하이퍼링크 -->
<a href="#content1" id="menu">메뉴1</a>
<a href="#content2">메뉴2</a>
<a href="#content3">메뉴3</a>
<br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br>
<br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br>
<h1 id="content1">메뉴1</h1>
<br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br>
<br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br>
<a href="#menu">메뉴 위로 이동</a>
<h1 id="content2">메뉴2</h1>
<br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br>
<br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br>
<a href="#menu">메뉴 위로 이동</a>
<h1 id="content3">메뉴3</h1>
</body>
```

## 폼 양식

```html
<body>
<form action="test1.html" method="get">
<fieldset>
<legend>그룹이름1</legend>
<label>아이디 : </label>
<input type="text" name="id" value="아이디" size="10" maxlength="5" readonly><br>
<label>비밀번호 : </label>
<input type="password" name="pass" autofocus required><br>
<label>이메일 : (이메일 주소 형태 제어 @)</label>
<input type="email" name="email" placeholder="이메일형식 입력"><br>
</fieldset>

<fieldset>
<legend>그룹이름2</legend>
<label>검색 : (검색어 삭제)</label>
<input type="search" name="search"><br>
<label>웹주소 : (웹주소 형태 제어 http://)</label>
<input type="url" name="url"><br>
<label>연락처 : (모바일 숫자 자판)</label>
<input type="tel" name="tel"><br>
</fieldset>

<fieldset>
<legend>그룹이름3</legend>
<label>숫자 : (숫자만 입력, 범위 제어)</label>
<input type="number" name="number" min="10" max="20" step="2" value="10"><br>
<label>숫자 : </label>
<input type="range" name="range" min="1" max="5" value="1"><br>
<label>날짜 : (달력) </label>
<input type="date" name="date" min="2022-02-01" max="2022-02-27"><br>
<label>날짜(월) : </label>
<input type="month" name="month"><br>
<label>날짜(주) : </label>
<input type="week" name="week"><br>
<label>시간 : </label>
<input type="time" name="time"><br>
<label>날짜시간 : </label>
<input type="datetime-Local" name="datetime"><br>
</fieldset>

<fieldset>
<legend>기타</legend>
<label>라디오 박스 : (복수개 중 하나 선택)</label>
<input type="radio" name="ra" value="남" checked>남성
<input type="radio" name="ra" value="여">여성<br>
<label>체크 박스 : (복수개 다중 선택)</label>
<input type="checkbox" name="ch" value="1">체크1
<input type="checkbox" name="ch" value="2" checked>체크2
<input type="checkbox" name="ch" value="3">체크3<br>
<label>파일 업로드</label>
<input type="file" name="file"><br>
<label>숨겨서 데이터를 서버에 전송 : </label>
<input type="hidden" name="hi" value="값"><br>
<label>여러줄의 글을 입력 : value옵션 없음</label>
<textarea name="tx" rows="5" cols="10">값1234</textarea><br>
<label>목록 상자 (size="3" multiple)</label>
<select name="se">
	<option value="1">목록1</option>
	<option value="2" selected>목록2</option>
	<option value="3">목록3</option>
</select><br>
<label>데이터 목록</label>
<input type="text" name="p" list="pack">
<datalist id="pack">
	<option value="1">옵션1</option>
	<option value="2">옵션2</option>
	<option value="3">옵션3</option>
</datalist>
</fieldset>
<input type="submit" value="전송">
<!-- 이미지에 전송 기능 포함 -->
<input type="image" src="4.jpg">
<input type="reset" value="취소(초기화)">
<input type="button" value="모양버튼">
</form>
</body>
```

- readonly : 읽기만 가능, 수정 불가능.
- autofocus : 문서를 열면 최우선으로 포커스(커서 깜빡임)를 해당 항목에 지정
- required : 필수 입력 사항 
	- 입력하지 않으면 입력하라는 메세지를 표시하며 서버에 전송하지 못 하게 하는 기능
- placeholder : 해당 상자에 설명 붙이기 가능
- filedset : 그룹을 상자 모양으로 지정
- legend : 그룹의 이름 지정
- size : 텍스트 상자 크기 지정
- maxlength : 글자의 최대 입력 길이 지정
- search : 작성 문자를 X표를 눌러 삭제하는 기능 (잘 사용하지 않음)
- email : 이메일 형식으로 작성
  - 이메일 주소에 @를 포함하지 않을 경우 메세지 표시
- url : 웹 주소 형식으로 작성 
  - http:// 를 포함하지 않을 경우 메세지 표시
- tel : 웹 상에서는 변화가 없으나 모바일에서는 문자 자판이 아니라 숫자 자판이 나오게 됨
- number : 문자는 입력 불가, 숫자만 입력 가능
  - min과 max값을 지정하여 숫자의 최소값과 최대값 설정 가능
  - step값을 지정하여 화살표 한 번 클릭 당 표시 되는 숫자의 크기 지정 가능
  - 지정한 값의 범위를 벗어날 경우 메세지 표시
- range : 숫자를 자판 입력 방식이 아닌 마우스로 드래그 하는 방식
  - value값을 지정하여 시작 위치 설정 가능
- date : 달력 방식으로 날짜 설정 가능
  - min과 max값을 지정하여 지정한 범위에서만 날짜 선택 가능하게 설정 가능 
- month & week : 월 & 주 설정 기능
- time : 시간 설정 기능
- datetime-Local : 날짜와 시간을 모두 설정 가능
- checked : radio와 checkbox를 문서를 열 때부터 체크 되어 있게 설정 가능
- textarea : value 옵션이 없으나 꺽쇠로 태그를 닫고 작성한 문자가 value의 기능을 실행 
- select : selected 태그로 문서를 열 때부터 해당 항목이 체크 되어 있게 설정 가능
- datalist : 목록이 있는 텍스트 상자 기능
	- list와 id 태그로 목록 설정 가능
- button : 아무 기능이 없이 모양만 있는 버튼 

> - method를 get 방식으로 지정해놓으면 전송 시 주소창에 아이디와 비밀번호가 노출되므로 보통 post 방식을 사용함
> - 서버에 전송 되는 정보는 value값들이기 때문에 value가 필요한 항목들은 반드시 작성

![캡처](https://user-images.githubusercontent.com/95197594/153116025-502f55fb-2686-4230-980c-bd39b5d4d3db.PNG)


## 실습


![캡처](https://user-images.githubusercontent.com/95197594/153116680-a98bd54f-9bfe-45db-9798-40f240443e36.PNG)
```html
<body>

<h1>레드향 주문하기</h1>
<form action="a.jsp" method="post">
<fieldset>
<legend><b>배송 정보</b></legend>
<br>
<label>이름</label>
<input type="text" name="name" size="30"><br>
<label>배송 주소</label>
<input type="text" name="address" size="30"><br>
<label>이메일</label>
<input type="email" name="email" size="30"><br>
<label>연락처</label>
<input type="tel" name="tel" size="30"><br>
<label>배송 지정</label>
<input type="date" name="date" size="50">(주문일로부터 최소 3일 이후)<br>
<label>메모</label>
<textarea name="memo" rows="5" cols="50"></textarea>
</fieldset>
<input type="submit" value="주문하기">
<input type="reset" value="취소하기">
</form>

</body>
```
![캡처](https://user-images.githubusercontent.com/95197594/153123888-4be08d79-8fbc-4928-9004-3a2979216ba6.PNG)
```html
<body>

<h1>프런트엔드 개발자 지원서</h1><br>
HTML, CSS, Javascript에 대한 기술적 이해와 경험이 있는 분을 찾습니다.
<hr>
<form action="aaa.jsp" method="post">
<h4>개인정보</h4>
<ul>
	<li><label>이름</label><input type="text" name="name" size="20" placeholder="공백없이 입력하세요"></li> 
	<li><label>연락처</label><input type="tel" name="tel" size="20"></li>
</ul>

<h4>지원 분야</h4>
<ul>
	<li><input type="radio" name="rd" value="웹 퍼블리싱"><label>웹 퍼블리싱</label></li>
 	<li><input type="radio" name="rd" value="웹 애플리케이션 개발"><label>웹 애플리케이션 개발</label></li>
 	<li><input type="radio" name="rd" value="개발 환경 개선"><label>개발 환경 개선</label></li>
 </ul>
 
<h4>지원동기</h4>

<textarea name="ehdrl" rows="5" cols="30" placeholder="본사 지원 동기를 간략히 써 주세요."></textarea><br>
<input type="submit" value="접수하기">
<input type="reset" value="다시 쓰기">
 </form>


</body>
```

======================================================================================================================================================================
======================================================================================================================================================================
======================================================================================================================================================================
======================================================================================================================================================================
======================================================================================================================================================================
======================================================================================================================================================================


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
/* 		너비 */
/* 		width : 500px; 고정값 */
/* 		width : 50%; 브라우저 크기에 따라 가변값 */
		width : 500px;
/* 		높이 */
		height : 50px;
/* 		안여백 */
/* 		padding : 50px; */
/* 		padding : 10px 50px; */
		padding : 10px 50px 5px; 	/*시계방향*/
		padding-top : 50px;
/* 		padding-left & right & bottom 모두 적용 가능 */
/* 		밖여백 여백이 중복됨(중첩), auto시 가운데 정렬 */
/*		margin : 50px; auto */
		margin : 50px;
		
/* 		박스 모델 크기 계산 */
/* 		box-sizing : content-box; 내용기준, 기본값 계산 */
/* 		box-sizing : border-box; 내용, 안여백, 테두리선 두께 포함 계산 */
/* 		box-sizing : border-box; */
		
	}
	h2{
/* 		border-width : 1px; 전체  */
/* 		border-width : 1px 10px 위아래 왼오른쪽;  */
/* 		border-width : 1px 10px 5px 20px 위 오른쪽 아래 왼쪽;  */
/* 		border-bottom-width : 10px; */
/* 		border-left-width : 10px; */
 		border-right-width : 10px; 
		border-top-width : 10px;
		
		border-style : solid; 
/* 		border-style : solid dotted; */
/* 		border-style : solid dotted dashed double; */

		border-bottom-style : dashed;
/* 		border-left & right & top-style 모두 적용 가능 */


/* 		border-color : black */
 		border-color : black blue green red;  /* 시계방향 */ 
		border-bottom-color : green;
/* 		border-left & right & top-color 모두 적용 가능 */
	
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
![캡처](https://user-images.githubusercontent.com/95197594/155244475-279e20f7-900c-4463-b1fe-93ecdd3621c3.PNG)

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

### css 연습
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



> 18일 테스트 예정. ex2, test10, ex3, test13.html 파일 참조
