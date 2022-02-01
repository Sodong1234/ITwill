> 인터넷 강의 "웹 표준에 맞는 HTML5 프로그래밍" 의 내용을 요약하여 간단히 정리함.



# 웹 프로그래밍 이해하기
## 웹 프로그래밍의 개요
### 웹 프로그래밍
- 하이퍼텍스트 프로토콜 (hypertext protocol)을 활용하여 월드 와이드 웹 (WWW: World Wide Web)을 통해 정보를 공유하는 환경을 구현하는 것
### HTML5
- 하이퍼텍스트 (Hyper text) : 텍스트, 이미지, 영상 같은 문서의 개체가 서로 연결되어 있는 것
- 마크업 언어 (Markup Language) : 태그(tag)를 이용하여 문서나 데이터의 구조를 명시하는 언어
### CSS3
- 캐스캐이딩 (Cascading) : 우선 순위에 따라 적용하는 것
- 스타일 시트 (Style Sheets) : 웹 페이지의 스타일(디자인)을 정의 하는 것
### javaScript
- 웹을 풍부하게 만들어주는 상대적으로 가벼운 프로그래밍 언어

## 웹 프로그래밍 관련 주요 구성 요소
- 웹 브라우저 : 사파리, 파이어폭스, 크롬, 엣지, 오페라 
- 웹 편집기
  - 텍스트 편집기 (Text Editor) : 노트패드++, 에디트 플러스, 울트라 에디트
  - 코드 에디터 (Code Editor) : 비주얼 스튜디오 코드, 서브라임 텍스트, 아톰, 브라켓, 드림위버 등
  - 웹 기반 코드 편집기 (Web based Code Editor), 통합개발환경 (Integrated Development Enviroment) : 코드팬, 라이브위브, 구름 IDE 등
- 네이티브 앱, 웹 앱, 하이브리드 앱
  - 네이티브 앱 : 모바일 기기에 최적화된 언어로 개발된 앱. 
    - java 언어로 만드는 안드로이드앱
    - C 언어로 개발된 아이폰 앱
  - 웹 앱 : 콘텐츠 영역은 HTML 기반의 웹 앱으로 제작, 최종 앱 배포에 필요한 패키징 처리만 아이폰&안드로이드 플랫폼 안에서 처리한 어플리케이션
  - 하이브리드 앱 : 모바일 웹+네이티브 앱. 모바일 웹의 특징을 가지면서 네이티브 앱의 장점도 가짐   
---

# 웹페이지 기본 문서 만들기
## HTML5의 기본 구성
- 시작과 끝이 없는 태그는 img, br, hr이 있고, 태그의 구조는 속성(Attribute)과 속성값(Value)임
- HTML5 문서태그 : html, head, title, body, base, meta, link, style, script, h1~h6, div, span, p, br, hr 등
## 텍스트와 링크 태그
- 제목 및 본문 태그 : h1~h6, p, br, hr, div, pre, 특수문자 등
- 글자 모양 태그 : b, strong, i, em, small, sub, sup, ins, del, mark 등
- 하이퍼링크 태그 : a, href, URL, 절대주소, 상대주소, 이미지에 링크 설정 등
  - a : 하이퍼링크를 의미
  - href: 파일의 경로(주소)
  - URL : 절대주소, 상대주소
## 미디어 태그
- 이미지 태그 : img, src, alt, width, height 등
- 오디오 태그 예시
```html
<audio controls="controls">
<source src="xxx.mp3" type="audio/mpeg"> 
</audio>
```
- 비디오 태그 예시
```html
<video width="640" controls="controls">
<source src="xxx.mp4" type="video/mp4”></video>
```                             
- Youtube 영상 등 외부 영상을 삽입할 때에는 <iframe> ……</iframe> 까지의 소스를 HTML 코드에 붙여 넣고, 동영상 주소 부분을 제외하고 모두 삭제
---
# 웹페이지 구조화하기
## 공간 분할 태그
- div : 블록 (Block) 형식으로 공간 분할 (태그 실행시 세로공간 차지)
- span : 인라인 (Inline)형식으로 공간 분할 (태그 실행시 가로공간 차지)
  - 공간 분할 태그 : CSS3를 활용해 레이아웃 구성할때 매우 유용함
## 시맨틱 태그
- Sementic : 의미론적인, 즉, 태그에 의미를 부여한다는 뜻
  - ex) 포털사이트에서 검색창 부분은 header 영역, 아래쪽의 사전&뉴스&증권&메일 등등을 모아놓은 탭은 nav영역, 그 아래쪽의 article 영역 등
- 시맨틱 태그는 웹사이트 별로 사용빈도가 상이함
## 리스트와 표
### 리스트 (list) 태그
```html
- <li> ol </li> : 순서가 있는 목록
- <li> ul </li> : 순서가 없는 목록
- <dt> dl </dt> : 정의 목록
- <dd> dl </dd> : 정의 설명
  - 정의 설명 태그는 태그 실행시 들여쓰기가 되어져 있음 
```
### 테이블 (table) 태그
```html
- <table></table>  : 표 삽입
- <tr></tr> : 표에 행(row 삽입)
- <th></th> : 표 제목 삽입
- <td></td> : 표 내용 삽입
```
### 입력양식(폼)
- 입력 양식 유형
  - 회원 가입 양식, 로그인&패스워드 양식, 질의응답 게시판 양식 등
- 입력 양식 작동 원리
  - 브라우저에서 서버로 요청 -> 서버에서 브라우저로 응답  
- 다양한 양식 (form)태그
```html  
<body>
<form action="test.html" method="post">
<input type="text">
<input type="password">
<input type="radio">
<input type="checkbox">
<input type="submit">
<select name="sel">
<option value="목1">목록1</option>
</select>	   <br>
<input type="file">
<input type="hidden">
<input type="button">
<input type="submit">	
<input type="image" src="1.jpg" width="100" height="100">	
<input type="reset">
</form>
</body>
```
- MDN과 Codepen을 활용한 태그 연습
  - MDN : https://developer.mozilla.org/en-US/
  - Codepen : https://codepen.io/
- html5와 CSS3 태그를 모두 외울 필요 없음. 이해한 후에 궁금한것들은 MDN 또는 w3schools에서 검색하고, 검색 결과를 Codepen과 같은 온라인 에디터사이트에서 확인
---
# CSS3를 활용한 시각적 효과 꾸미기 
## CSS의 개요
- CSS3의 개념 및 형식
  - HTML(Hypertext Markup Language) : 문서의 내용
  - CSS(Cascading Style Sheets) : 문서의 스타일(디자인)
- 선택자의 종류  
  - .class
  - #id
  - * (전체를 뜻함)
  - element
  - element, element, ...
## CSS3의 표현 방법
- 외부 스타일 시트 - CSS 문서를 별도로 두고 그 문서를 불러다 사용
- 내부 스타일 시트 - 직접 CSS 내용을 기술. <style></style> 태그 사용
- 인라인 스타일 시트 - 태그와 같은 라인에 CSS를 선언
  - ex)
```html
<body>
  <h1 style="color: blue; text-align: center;"><h1>
  <p style="colpr: red;"></p>
</body>
``` 
## CSS3의 단위, 색상, 글자 속성
- CSS3의 단위
  - 고정 크기 단위 (cm, mm, in, px, pt, pc)
  - 가변 크기 단위 (em, ex, ch, rem, vw ,vh, vmin, vmax, %)
- CSS3의 색상
  - 종류 (Background Color, Text Color, Border Color)
  - 표현 방법 (RGB, 16진수, HSL, RGBA, HSLA)
- CSS Text
  - Color, Alignment, Decoration, Transformation, Spacing
    - Color : 글자색. blue, #ff0000, rgb (255,0,0) 등
    - Alignment : 문자정렬방식. center, left, right, justify
    - Decoration : 문자꾸밈방식. none, overline, line-through, underline
    - Transformation : 문자변환방식(소문자->대문자, 대문자->소문자). uppercase, capitalize, lowercase 
    - Spacing : 글자간격. 50px, letter-spacing, ling-height, word-spacing, white-space
- CSS Font
  - font-family
    - 글꼴에 Serif인지 Sans-serif를 표기함
      - Serif : 꾸며진 글씨체
      - Sans-serif : 꾸며지지 않은 글씨체 
  - font-style
    - normal, italic, oblique 
  - font-weight
    - normal, bold 
  - font-size
    - px, em, %, rem     
---
# CSS3을 활용한 폼 꾸미기
## CSS BOX모델의 개념과 관련 요소
- CSS BOX모델 : HTML의 요소는 박스 모양으로 구성되며 마진(Margin), 보더(Border), 패딩(Padding), 콘텐츠(Content)로 구분
- CSS Padding, Borders, Margins 속성
  - Padding : Content와 Border와의 간격
  - Border : 전체 요소의 테두리
  - Margin : Border로부터 실제 Content가 표시될 수 있는 간격
- CSS Outline : 요소를 두드러지게 보이게하는 선을 지정
  - Outline의 종류 : dotted, dashed, solid, double, groove, ridge, inset, outset
  - 색상 지정도 가능
- CSS Display
- block : 박스형태 (div, h1~h6, p, form, header, footer, section)
- inline : 한 라인에 보여 줄 수 있는 형태 (span, a, img)
- display : 화면에 콘텐츠가 표현되는 방식 (none, block, inline, inline-block)
- visibility : 화면에 콘텐츠가 표현되는 방식 (visible, hidden, collapse)
## CSS 아이콘과 링크
- CSS Icons(아이콘)
  - Font awesome : script에 fontawsome JavaScript 불러온 후 아이콘의 이름을 명시하여 적용
  - Bootstrap : <head>안에 Bootstrap사이트 링크 삽입 후 아이콘의 이름을 명시하여 적용
  - google : <head>안에 google의 아이콘 사이트링크 삽입 후 아이콘의 이름을 명시하여 적용
- CSS Link(링크)
  - default : 파란색과 밑줄
  - CSS로 스타일변경 가능
## CSS 리스트, 주석, 이미지 갤러리
- Lists item Markers(표시자)
  - 다양한 표시자의 예 : circle, square, upper-roman, lower-alpha, URL로 직접 이미지 삽입 가능
- CSS Commentes (주석) : 코드의 혼동 방지, 편리한 유지 보수, 실제 코드에 영향을 주지 않음
```  
  - CSS : //(한 줄), /* */(여러 줄)
  - HTML : <!-- -->
  - JavaScript : //(한 줄), /* */(여러 줄)
```  
- Image Gallery
  - CSS로 이미지갤러리를 선언하면 각 이미지를 클릭했을 때 특정 이미지를 확대해서 나타나게 하는 것이 특징
---
# 다양한 레이아웃의 구성과 기능 
## 위치, 유동성, 부트스트랩  
- CSS position 속성 : 요소의 위치 결정 유형 지정
  - static : 정적 위치로 기본값
  - relative : position의 계산값이 상대적인 요소
  - fixed : 고정적인 위치
  - absolute : position의 계산값이 절대적 또는 고정적인 요소
  - sticky : position의 계산값이 sitcky(끈적한)인 요소. 평소에는 상대 위치 지정 요소, but 자신의 루트에서 지정한 임계값을 넘으면 fixed처럼 화면에 고정되는 요소
- CSS float 속성 : 다른 요소가 왼쪽 또는 오른쪽으로 배치되도록 함. 주로 이미지에 사용됨
- Bootstrap : 반응형 웹의 모바일 우선 프론트엔드 웹 개발을 위한 무료 오픈 소스 CSS 프레임 워크
## 플렉스 시스템과 그리드 시스템
- 플렉스 (flex) 시스템의 개념
  - 개념 : Flexbox (Flexible Box) 모듈로도 불림, Flexbox 인터페이스 내의 아이템 간 공간 배분과 강력한 정렬 기능을 제공하기 위한 1차원 레이아웃 모델
- 그리드 (grid) 시스템의 개념 및 속성
  - 개념 : 한번의 선언으로 명시적 속성들과 암시적 속성들을 빠르게 설정하는 방법
  - 명시적 속성 : grid-template-rows, grid-template-columns, grid-template-areas
  - 암시적 속성 : grid-auto-rows, grid-auto-columns, grid-auto-flow
## 반응형 웹과 미디어 쿼리
- 반응형 웹 디자인 (Responsive Web Design)의 개념과 특징
  - 개념 : 다양한 기기 (데스크탑, 노트북, 태블릿, 스마트폰 등)의 화면 크기에 상관없이 자연스럽게 보이도록 디자인 하는 것
  - 특징 : 모든 장치 (Device)에서 웹 페이지를 보기 좋게하고 HTML과 CSS만으로 구현 가능함 (프로그램도 자바스크립트도 아님)
- 미디어 쿼리 (media queries)의 개념과 특징
  - 개념 : @media(너비, 높이, 방향, 비트수, 해상도 등등)을 사용하여 미디어 별로 스타일을 지정하는 것  
---
# 화면설계 확인하기
## 와이어 프레임
- 와이어프레임(wireframe)의 개념
  - 웹사이트의 회로도 (schematic) 또는 청사진 (blueprint)라고도 불리우며, 웹사이트의 골격을 나타내는 시각적 가이드
- 와이어프레임(wireframe)의 구성요소
  - Wrap : 전체 박스
  - Header : 헤더
  - Slide : 이미지 슬라이드
  - Contents : 콘텐츠
  - Footer : 푸터
  - Tab Menu : 탭메뉴
  - Pop-up : 팝업창
- 와이어프레임(wireframe)은 가로형과 세로형이 있음
## 레이아웃 구현하기
- HTML 기본구성 입력과 CSS Reset하기
  - 웹에디터의 Emmet 기능을 활용하여 html 기본 구성을 완성함
  - HTML 태그에 대한 기본 CSS요소를 초기화
- HTML 기본 레이아웃 구현하기
  - header, slide, Main Contents, footer에 대한 HTML 영역을 구현
- CSS로 스타일 구현하기
  - header, slide, Main Contents, footer에 대한 CSS 스타일을 구현
- Header 영역 구조 꾸미기
  - ul, li, a에 대한 HTML 영역 구조를 제작함
- 메뉴 영역 꾸미기
  - ul, li, a에 대한 CSS 스타일을 제작
---
# 화면설계 UI 해석하기  
## 자바스크립트와 제이쿼리
- 자바스크립트 (JavaScript)의 개념 
  - 웹문서에 움직임(액션) 제공
- JQuery의 개념 및 특징
  - HTML의 클라이언트사이드(HTML5, CSS, 자바스크립트) 조작을 단순화 하도록 설계된 크로스플랫폼(사용하는 기기에 상관없이 사용 할 수 있는)의 자바스크립트 라이브러리
  - 자바스크립트 라이브러리로서 자바스크립트 프로그래밍을 단순화
  - 학습하기 쉬움
  - 웹프로그래밍 시간을 단축시킴
- JQuery의 버전
  - 3.x.x 버전 : 가장 최신 버전 (2020년 7월 현재) v3.5.1, Ajax를 지원
  - 2.x.x 버전 : IE 6~8 버전을 지원하지 않고, 1.x.x 버전이 간소화 되어 용량이 작음
  - 1.x.x 버전 : 구형 브라우저에 가장 안정적이고, 공공기관사이트를 작업한다면 이 버전 사용을 권장. 1.x.x 버전 중 가장 안정적인 버전은 JQuery-1.12.4
- JQuery의 사용법
  - Local 파일 사용 - 파일을 내부에서 가져오는 것
  - CDN(Content Delivery Network) 사용 - 파일을 외부에서 가져오는 것
## 가로형 & 세로형 메뉴 구현하기
- 와이어프레임에 제시된 지시사항 확인하기
  - 가로형 메뉴를 구현하기 위한 와이어프레임 확인하기 : 좌에서 우로 펼쳐지는 메뉴 구성
  - 세로형 메뉴를 구현하기 위한 와이어프레임 확인하기 : 위에서 아래로 내려오는 메뉴 구성
- HTML 기본 구성 입력하기
  - nav 태그 이용
  - ul, li 태그를 이용해서 각 메뉴 구성
  - 각 메뉴에 ul, li를 이용해서 서브 메뉴 구성
- CSS 작업하기
  - nav 전체 스타일링하기
  - 각 메뉴 스타일링, 마우스 오버 효과주기
  - 서브 메뉴 스타일링, 마우스 오버 효과주기
- 자바스크립트 작업하기
  - HTML 문서에 스크립트추가
  - 메뉴에 지정된 함수에 사용자 지정 효과주기
---
# UI 설계 확인하기
## 이미지 슬라이드(좌우)
- CSS로 구현하기
  - @keyframes를 이용함
  - margin-left로 좌측 좌표를 선언하여 좌우 슬라이드 구현 가능
- JavaScript로 구현하기
  - 지정 시간마다 반복되는 setlnterval() 함수를 활용
  - animate 메소드를 통해 지정 시간마다 요소의 left 속성을 할당하여 구현 할 수 있음
## 이미지 슬라이드(상하)
- CSS로 구현하기
  - @keyframes를 이용함
---
# UI를 통해 화면 구현하기
## CSS 레이어 팝업
- HTML과 CSS만으로 구현
- HTML에 모달 관련 클래스 이름을 명시
- CSS의 요소 : hover 특성을 이용하여 opacity 속성을 1로 할당함으로서 레이어 팝업을 구현
## JQuery 모달 팝업
- JQuery 파일을 연결함
- HTML에 모달 관련 클래스 이름을 명시함
- JavaScript 파일에서 요소 클릭 시, fadeIn(); 및 fadeOt(); 메소드를 호출함으로서 레이어 팝업을 구햔
## Bootstrap 모달 팝업
- bootstrap 파일을 연결
- 모달이 필요한 요소에 data-toggle="modal" 및 data-target="모달 팝업 아이디명"을 명시
- 모달 팝업이 되는 요소에 아이디를 입력
- bootstrap을 통해 자동으로 구현
## w3.css 이미지 팝업
- W3School에서 제공하는 적응형 CSS라이브러리
- 최신 반응형(Responsive)웹에 유용함
- 모든 브라우저, 모든 장치에 적용 가능
  - margin-top로 상단 좌표를 선언하여 상하 슬라이드 구현 가능
- JavaScript로 구현하기
  - 지정 시간마다 반복되는 setlnterval() 함수를 활용
  - animate 메소드를 통해 지정 시간마다 요소의 top 속성을 할당하여 구현 할 수 있음
## 페이드 인-아웃 구현하기
- CSS로 구현하기
  - @keyframes를 이용함
  - opacity로 투명도를 선언하여 페이드 인-아웃 구현 가능
- JavaScript로 구현하기
  - 지정 시간마다 호출하는 setTimout() 함수를 활용
  - style.display 속성을 'none', 'block' 으로 할당하여 페이드 인-아웃을 구현 할 수 있음
---
# 탭 메뉴를 통한 흐름 제어 구현하기
## CSS/W3.CSS를 활용한 탭 메뉴
- CSS를 활용한 탭 메뉴
  - HTML 작성하기 : styleseet 필요
  - CSS 작성하기 : 스타일과 animation효과로 탭 작동 구현하기
- W3.CSS를 활용한 탭 메뉴
  - HTML 작성하기 : CDN 방식으로 W3.CSS불러오기
  - JavaScript 작성하기 : 각 탭을 클릭하면 선택된 컨테이너의 콘텐츠가 보이도록 구현
## JavaScript를 활용한 탭 메뉴
- HTML 작성하기 : head에 stylesheet와 JavaScript 호출
- CSS 작성하기 : maring : 0;으로 reset하기
- JavaScript 작성하기 : 함수를 사용해서 작동 구현. document.getElementsBy를 사용해서 탭이 많아도 자동으로 JavaScript가 개수를 파악하고 구현
## JQuery를 활용한 탭 메뉴
- HTML 작성하기 : head에 stylesheet, 사용자 작성 JavaScript.
- CSS 작성하기 : 최초 실행시 나타나는 탭을 제외하고 모두 나타나지 않게 display : none으로 설정
- jQeury 작성하기 : 선택된 탭이 click을 통해 하나씩 표현 될 수 있도록 함수 작성
## BootStrap을 활용한 탭 메뉴
- Bootstrap: 반응형 웹의 모바일 우선 프론트엔드 웹 개발을 위한 무료 오픈 소스 CSS 프레임 워크
- HTML 작성하기 : head에 bootstrap에서 CSS, JQuery에서 JavaScript, bootstrap에서 JavaScript 호출
---
# 반응형 웹(RWD) 화면 구현하기
## Float 활용 화면 구현
- CSS Layout Float 실행결과
  - 브라우저 화면 크기에 따라 콘텐츠 변경 확인
- HTML 작성하기 : 화면 구성을 위한 영역 지정 (header, section, nav, article, footer 등) 
- CSS 작성하기 : @media를 사용한 반응형 웹 구현
## W3.CSS 활용 화면 구현
- W3.CSS Layout 실행결과 : W3 easy alignment를 사용해서 텍스트의 위치와 색을 구현
- HTML 작성하기 : W3schools.com을 호출하기
  - W3-container, W3-cell, W3-color 등
- CSS 작성하기 : 넓이, 색상 등 이미 HTML에 구현된 것 외의 스타일링 지정하기
## Grid 활용 화면 구현  
- HTML 작성하기 : div 태그를 이용한 영역 구성, 각 영역이름 지정
- CSS 작성하기 : display: grid를 사용. 그리드 영역에 지정된\이름만으로 화면 구성
- Grid 활용 화면 구현 실행 결과 : 별도의 코드 없이도 영역 구현
## Media query 활용 화면 구현
- 반응형 (Responsive) 화면 구현 실행결과 : 웹 페이지를 구성하는 영역이 화면의 크기에 따라 사용자의 편의에 맞게 변함
- HTML 작성하기
- CSS 작성하기 : @media로 반응형 웹 구현. max-width를 사용하여 디바이스 크기 지정 가능
---
# 요구사항에 맞추어 사이트 디자인하기
## 요구사항 살펴보기
- 웹디자인기능사 예시문제를 예시로 요구사항 명세 분석
  - 문제 개요, 와이어프레임, 영역별 지시사항
- Q-Net 사이트에서 다운로드 가능
## HTML로 구조 꾸미기
- HTML – 기본세팅 : 예시 문제를 통해 구성을 확인
  - head, A.header, B.Slide, C.Contetns, Modal, Footer, Footer with SNS
- 메뉴 영역인 Header 꾸미기 : ul과 li 태그를 사용한 메뉴 구성
- 이미지 슬라이드 영역인 Slide 꾸미기
  - 슬라이드에 사용할 이미지의 경로를 확인하고 img태그를 사용해서 삽입, alt에 해당 이미지에 대한 설명 추가
- 탭메뉴, 공지사항, 갤러리, 배너 영역인 Contents 꾸미기
  - 클릭을 통해 해당 페이지로 이동할 수 있도록 a 태그 사용과 경로 작성
  - 갤러리에 사용될 이미지를 ul과 li태그를 사용해서정렬
- 모달 팝업 영역인 모달 꾸미기
  - 팝업에 사용할 콘텐츠 작성, 닫기 버튼 구현
- 하단 메뉴 영역인 Footer 꾸미기
  - logo, copyright, 하단 메뉴 구성
## CSS로 모양 디자인하기
- HTML 구성요소의 스타일을 CSS로 디자인함
  - CSS 초기화
  - 메뉴들의 스타일링과 위치 속성 설정 : submenu같이 고정되어야하는 것은 absolute 지정
  - 이미지 슬라이드의 레이아웃의 크기보다 이미지가 클 경우 overflow: hidden으로 보이지 않도록 설정  
## JavaScript로 기능 더하기
- HTML 구성요소의 기능을 JavaScript로 구현함
  - 메뉴 : mouseover, mouseout을 통해 메뉴가 아래로 내려오고 다시 올라가도록 기능 구현
  - imageSlide가 사용자가 지정한 시간동안 페이드 인, 페이드 아웃의 방법으로 보여지도록 기능 구현
  - modal popup이 클릭이벤트를 통해 보여지고 닫기 버튼을 통해 사라지도록 기능 구현
---
# 포트폴리오 웹 프로그래밍
## 포트폴리오 사이트의 명세화
- 예시 사이트를 각 기능별 메뉴에 대해 구현 방법을 명세화
- 각 단계별로 코딩하기 보다는 기존 사이트를 활용해서 수정 및 보완의 방법으로 구현
## HTML로 구조 꾸미기
- CDN 방식으로 w3schools.com, google font, font-awesome 주소로 호출하기
- 사용자의 CSS, JavaScript 파일을 사용하여 직접 기능 구현
- 명세화에 따라 내용을 현행화하여 코딩
- W3.CSS를 사용함으로써 편리하게 화면 구성
## CSS로 모양 디자인하기
- HTML 요소에 대한 스타일을 CSS에서 꾸밈
- W3.CSS로 이미 지정된 스타일을제외한 스타일링
- parallax effect를 사용하여 스크롤을 내릴 때 자연스러운 이미지 변화 구현
- 각 요소별 클래스명을 따로 주어 특성별 스타일을 차별화
- @media를 이용한 반응형 웹 구현
## JavaScript로 기능 더하기
- 요소의 아이디나 클래스 선택자로 스타일 선언
- 이미지 갤러리를 클릭했을 때 모달 팝업과 같이 표현되도록 기능 설정
- 요소의 아이디나 클래스 선택자로 토글 기능을 구현

  
  

