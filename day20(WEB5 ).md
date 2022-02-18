# [오전수업] WEB 5차
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


