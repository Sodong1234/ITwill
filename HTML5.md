> 인터넷 강의 "웹 표준에 맞는 HTML5 프로그래밍" 의 내용을 정리함 



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
  - CSS : //(한 줄), /* */(여러 줄)
  - HTML : <!-- -->
  - JavaScript : //(한 줄), /* */(여러 줄)
- Image Gallery
  - CSS로 이미지갤러리를 선언하면 각 이미지를 클릭했을 때 특정 이미지를 확대해서 나타나게 하는 것이 특징
---
# 다양한 레이아웃의 구성과 기능  
