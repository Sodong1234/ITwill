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

공&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;백
&lt; 태그 특수 문자 &gt;


```



---




