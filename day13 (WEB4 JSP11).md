# [오전수업] web 4차
> 하이퍼링크와 폼 양식의 보충 수업 실시
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


**간격 조정 등의 꾸미기는 CSS에서 다룰 예정**

---


# [오후수업] JSP 11차

