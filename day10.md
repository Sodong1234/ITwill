# [오전수업] web 3차
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

- 픽셀 : 화명을 이루는 빛(하나의 점), 해상도 : 빛(점의 개수)
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
```
<img src="../html1/2.jpg">
<img src="../html1/img1/5.jpg">
<img src="../html1/img1/6.png">
```


---


> 그 외 저번 시간에 학습했던 colspan과 rowspan을 이용한 표 합치기 복습 실시

---

# [오후수업]


