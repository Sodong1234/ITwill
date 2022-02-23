#[오전수업] WEB 6차
> WEB 5차에서 실시한 선 모양 추가분을 커밋해놓았음

## css 활용
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
