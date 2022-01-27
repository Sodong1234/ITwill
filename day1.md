# 시작에 앞서 설치 및 세팅작업 실시.

## 설치
### 1. notepad++ 설치
https://notepad-plus-plus.org/downloads/v8.2/ 에서 설치.


### 2. JDK 설치 & 환경 변수 편집
학원에서는 17버전이 아닌 8 버전(jdk-8u321-windows-x64) 사용.
시스템 환경 변수 편집 - 환경 변수 - 시스템 변수에서 새로만들기 후
변수 이름을 JAVA_HOME / 변수 값을 C:\Program Files\Java\jdk1.8.0_321(설치시 기본경로) 로 지정.
다음 환경 변수의 Path 항목에서 새로만들기 - %JAVA_HOME%\bin 값 추가 - 맨 위로 이동.
제대로 설치가 됐다면 cmd에서 java -version / javac -version 명령어로 확인 가능.


### 3. 웹서버(웹애플리케이션서버) 설치
- 아파치톰캣(대표적으로 구글&네이버처럼 페이지를 서비스하는 서버. 내가 만든 사이트를 전세계 사람들에게 보여주기 위해 설치.) 
- https://tomcat.apache.org/ (수업에서는 8버전 사용.) - 32-bit/64-bit Windows Service Installer (pgp, sha512) 설치. 
- 포트(접속하는 약속된 통로)는 8080. 포트 없으면 전 세계 사람들이 다 들어올 수 있음.
- 설치 후 
	- 서버 start - 웹브라우저 : http://localhost:8080/ . 현재단계에서는 본인만 볼 수 있음. 추후 조별과제시 이 경로 사용할 예정. 
	- 서버 stop 누르면 사이트에 연결 할 수 없다고 뜸. 네이버에서도 자기 서버에서 스탑하면 연결 할 수 없다고 뜰 것!


### 4. 이클립스 (개발도구) 설치 
자바 전문 에디터. 하지만 웹에서도 사용하여 겸으로 이클립스 사용.
https://www.eclipse.org/ - eclipse-jee-2021-12-R-win32-x86_64 압축 풀기
  1) 환경설정(UTF-8한글처리)
  - 세계 어디서 실행을 하든 한글이 깨지지 않도록 설정.
  
		Window - Preferences - General - Workspace - Text file encoding - Other(UTF-8)
			   	     - Web - CSS Files - ISO 10646/Unicode(UTF-8)
					   - HTML Files - ISO 10646/Unicode(UTF-8)
					   - JSP Files - ISO 10646/Unicode(UTF-8)

  2) 톰캣서버 가져오기&서버환경설정
File - New - Other - Server - Server - Apache -	 Tomcat v8.5 Server - Next - Browse - C:\Program Files\Apache Software Foundation\Tomcat 8.5 - Next - Finish
화면 하단의 Servers - 톰캣 더블클릭 - Ports - Tomcat admin port를 5005로 설정(학원에서만 사용할 포트임)
*만약 하단에 Servers 탭이 보이지 않을 경우 상단 바 Window - Show view - Server를 선택.

  3) 프로젝트(작업) 만들기
File - New - Other - Web - Dynamic Web Project - Next - 프로젝트네임 WebProject - Next - Next - web.xml 체크 (위에 자동 설정 되어있는 Contxt root는 프로젝트 폴더, Content directory는 내용 폴더임) - Finish

  4) 서버와 프로젝트 연결
화면 하단의 Servers - 톰캣 마우스오른쪽 - Add and Remove - Available의 WebProject를 Configured로 이동(Add 버튼) - Finish





---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------


*오전 JSP / 오후 WEB 진행 예정이었으나 첫째 날 오전은 OT로 인하여 메모장으로 Hello World!를 출력하는 작업만 공부.


# [오전 수업]

명령어는 다음과 같았음.
```java
public class Hello {

	public static void main(String[] args) {

		System.out.println("Hello World!");

	}

}
```

본 명령어를 메모장에 입력 후, 저장 시 파일 명을 Hello.java 로 변경.
cmd에서 이것을 javac 명령어를 통하여 컴파일 -> java Hello 명령어로 Hello World!를 확인 할 수 있었음.








# [오후 수업]
프론트엔드 개발(HTML, CSS, Javascript, jQuery). 사용자가 보는 화면에 관련된 작업들.
백엔드 개발(자바, JSP, MVC, 스프링). 사용자의 눈에 보이지 않는 화면. 
간단한 예시 - (네이버 로그인창을 예시로) 뼈대는 HTML, 꾸미는 것은 CSS, 아이디입력해라, 비번입력해라 메시지만 띄우고 다음 단계로 진행 시키지 않는 것은 Javascript&jQuery, 서버는 백엔드.



그 중 오늘 배운 것은 HTML 중에서 기초적인 몇 가지임.
*HTML(Hyper Text Markup Language), 웹문서를 만드는 기본언어.
-Hyper Text : 문서를 서로 연결해주는 링크 + Markup : 표시한다
HTML 기본기능 : 웹브라우저에 보여줄 내용에 마크업(표시하고) 문서끼지 링크하는 것.
세계표준화(웹 표준화)가 되어 있으며, 
https://w3c.or.kr/
https://www.w3.org/
https://www.w3schools.com/
등지에서 다양한 정보를 얻을 수 있음.

웹브라우저 - 웹페이지(HTML)을 실행하는 도구. 크롬, 엣지, 파이어폭스 등등이 사용됨.
웹편집기 - 메모장, 에디터(notepad++ 등), 이클립스, 비주얼스튜디오 코드(유료)


*간단한 웹페이지 만들기*
1) 메모장
 -다음과 같이 작성.
```html
<html>
<head>
<title>html title</title>
</head>
<body>
html body
</body>
</html>
```
test1.html 저장 후 실행시키면 타이틀 html title, 본문 내용 html body의 웹페이지가 만들어짐.



2) notepad++
 -다음과 같이 작성.
```html
<html>
<head>
<title>html title2</title>
</head>
<body>
html body2
</body>
</html>
```
test2.html 저장 후 실행시키면 타이틀 html title2, 본문 내용 html body2의 웹페이지가 만들어짐.
노트패드는 약속된 마크업의 경우 파란색으로 글씨가 표시. 라인도 보여줌(x라인에서 오류가 날 경우 좀 더 직관적으로 확인 가능). 메모장보다 편리.



3) 이클립스
  WebProject 폴더에서 src - main - webapp 폴더 마우스오른쪽 - New - html File - 파일명 test.html - Next - html5(웹 표준. 학원에서는 웹 표준에 맞춰 사용) - Finish

- 메모장, 에디터는 마크업을 모두 다 직접 작성했어야 했지만 이클립스는 기본적인 뼈대가 모두 작성이 되어 있음.
- 또한 작성 후 저장, 실행을 통하여 결과물을 확인 할 수 있는 메모장&에디터와 달리 이클립스는 편집하면서 바로 실행, 확인이 가능하여 편리함.
- 실행은 파일(여기서는 test.html) - 오른쪽버튼 - run as - run on server - tomcat 8.5 선택 및 아래 Always use .. this project 체크 - next - finish 누르면 브라우저가 자동으로 실행. 주소도 톰캣 서버로 열림.
	- or 위에 작업표시줄 화면에서 초록색 화살표 버튼 - run as - run on server 
 	- or ctrl+F11.
 
 
 
 
 
 * 여러가지 마크업들. 이클립스 사용함.
 ```html
-html에서 엔터키 효과를 내고 싶다면 <br>
-body에 제목을 붙이고 싶다면 <h1>제목</h1>
-문단 구별은 <p></p>
-글자기울이기는 <i></i>		  // italic
-네이버 하이퍼링크를 넣고 싶다면 <a href="http://www.naver.com">네이버 하이퍼링크</a>
-아까 이클립스에서 만든 웹문서 하이퍼링크를 넣고 싶다면 <a href="test.html">test.html 웹문서 하이퍼링크</a>
-이미지를 추가하고싶으면 같은 폴더 안에(지금같은 경우는 html1 폴더 안에) 있어야함. 폴더 안에 없으면 실행시켰을때 이미지 깨져서 보임.
<img src="1.jpg"> <img src="2.jpg"> <img src="3.jpg">
img 태그 하나당 이미지 하나. 여러개 안됨
이미지 파일은 jpg, gif, png만 지원.
이미지에 하이퍼링크 걸기 가능.

-목록을 추가하고싶으면 
1. 순서 있는 목록(숫자) : 
<ol>
	<li>목록1</li>
	<li>목록2</li>
</ol>

 순서 있는 목록(영어)
<ol type="a">				// order list
	<li>목록1</li>
	<li>목록2</li>
</ol>



2. 순서 없는 목록
<ul>
	<li>목록1</li>
	<li>목록2</li>
</ul>

순서 있는 목록(모양)
<ol type="square">
	<li>목록1</li>
	<li>목록2</li>
</ol>

```

- 주석은 ctrl+shift+c
- html은 기본 뼈대, 좀 더 사이트를 이쁘게 만들고 싶으면 css 활용해야함.
