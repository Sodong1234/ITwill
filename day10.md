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


