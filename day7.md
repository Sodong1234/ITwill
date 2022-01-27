# [오전수업]

## table을 만드는 방법

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>html1/test4.html</title>
</head>
<body>
<h2>표만들기</h2>
<table border="1">
<tr><td>1행1열</td><td>1행2열</td></tr>
<tr><td>2행1열</td><td>2행2열</td></tr>
</table>
</body>
</html>
```

table를 만들때는 table, tr, td 코드를 사용함
- tr은 행(가로), td는 열(세로)
- table 옆에 붙이는 border는 선의 크기(개수)를 나타냄.



### 5행 4열 표 만들기
```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>html1/test4.html</title>
</head>
<body>
</table>
<h2>5행 4열 표 만들기</h2>
<table border="1">
<tr><td>용도</td><td>중량</td><td>개수</td><td>가격</td></tr>
<tr><td>선물용</td><td>3kg</td><td>11-16과</td><td>35000원</td></tr>
<tr><td>선물용</td><td>5kg</td><td>18-26과</td><td>52000원</td></tr>
<tr><td>가정용</td><td>3kg</td><td>11-16과</td><td>30000원</td></tr>
<tr><td>가정용</td><td>5kg</td><td>18-26과</td><td>47000원</td></tr>
</table>
</body>
</html>
```
![image](https://user-images.githubusercontent.com/95197594/151272313-3b7aec68-8e1b-4bf4-bac7-371fac784d62.png)


### 3행 3열 표 만든 후 표 합치기
```html

</table>
<h2>3행3열 표합치기</h2>
<table border = "1">
<tr><td>1행1열</td><td>1행2열</td><td>1행3열</td></tr>
<tr><td>2행1열</td><td>2행2열</td><td>2행3열</td></tr>
<tr><td>3행1열</td><td>3행2열</td><td>3행3열</td></tr>
</table>
```
![1](https://user-images.githubusercontent.com/95197594/151276364-5c4091f9-d74d-4b47-b575-73ee579a0aa2.PNG)

그냥 합쳐버리면 표에 흔적이 남아있기 때문에 colspan 태그를 추가해주어야 함.

```html
<h2>3행3열 표합치기</h2>
<table border = "1">
<tr><td>1행1열</td><td colspan="2">1행2열1행3열</td></tr>
<tr><td>2행1열</td><td>2행2열</td><td>2행3열</td></tr>
<tr><td>3행2열</td><td>3행3열</td></tr>
```

![2](https://user-images.githubusercontent.com/95197594/151276369-98517a70-418b-41f3-8bd9-6b8591e3f81f.PNG)


2행1열과 3행1열을 합치고 싶을때는 rowspan 태그를 추가해주어야 함.

```html
<table border = "1">
<tr><td>1행1열</td><td colspan="2">1행2열1행3열</td></tr>
<tr><td rowspan="2">2행1열<br>3행1열</td><td>2행2열</td><td>2행3열</td></tr>
<tr><td>3행2열</td><td>3행3열</td></tr>
```

![3](https://user-images.githubusercontent.com/95197594/151276373-5cb81188-afac-43ff-81ca-978708b9a0b0.PNG)
![4](https://user-images.githubusercontent.com/95197594/151276375-c7e97aa6-1e82-472c-8748-365a36dc7be9.PNG)


### 5행 4열 표 합치기
```html
<h2>5행 4열 표 만들기</h2>
<table border="1">
<tr><td>용도</td><td colspan="3">중량 개수 가격</td></tr>
<tr><td rowspan="2">선물용</td><td>3kg</td><td>11-16과</td><td>35000원</td></tr>
<tr><td>5kg</td><td>18-26과</td><td>52000원</td></tr>
<tr><td rowspan="2">가정용</td><td>3kg</td><td>11-16과</td><td>30000원</td></tr>
<tr><td>5kg</td><td>18-26과</td><td>47000원</td></tr>
</table>
</body>
</html>
```

![5](https://user-images.githubusercontent.com/95197594/151277438-4c77b1bb-5f4b-40ba-9650-634c17f371d0.PNG)

## 양식(폼)태그
```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>html1/test5.html</title>
</head>
<body>
<h1>양식(폼)태그</h1>
<form action="test4.html" method="post">
<input type="submit" value="전송버튼">
</form>
</body>
</html>
```

![1](https://user-images.githubusercontent.com/95197594/151279302-614ff3ff-6772-4ed1-810d-a2d9668f7593.PNG)
- 전송버튼을 누르면 test4.html로 이동
- form 태그는 이동 / input 태그는 형태생성(텍스트, 버튼 등) 담당
  - form 태그 안에 입력한 데이터를 가지고 서버에 전송하면서 action에 저장된 페이지로 이동
- method는 데이터를 전송하는 방식
  - get 방식은 주소줄에 데이터가 보이면서 서버 전송 (get 방식은 따로 태그를 추가하지 않아도 자동으로 주소줄에 데이터가 보임)
  - post 방식은 주소줄에 데이터가 안 보이면서 서버 전송  
- value는 입력값
### 텍스트를 입력 가능한 상자 생성
```html
<body>
<h1>양식(폼)태그</h1>
<form action="test4.html" method="post">
아이디 : <input type="text" name="id" value="Lee"><br>
비밀번호 : <input type="password" name="pass"><br>
자기소게 : <textarea name="intro" rows="5" cols="10">안녕</textarea>
<input type="submit" value="전송버튼"><br>
</form>
</body>
```
![2](https://user-images.githubusercontent.com/95197594/151279935-ee306df4-e8b6-45bb-a1fe-95a262b71f37.PNG)
- 이런 식으로 아이디, 비밀번호 입력상자 등등을 생성 가능.


