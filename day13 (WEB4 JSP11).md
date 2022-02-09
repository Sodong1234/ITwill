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


