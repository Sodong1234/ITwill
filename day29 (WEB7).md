# [오전수업] WEB 7차

css 연습
```css
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>css1/ex4.html</title>
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
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
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
