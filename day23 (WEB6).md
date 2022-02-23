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
