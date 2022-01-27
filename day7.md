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
- ![image](https://user-images.githubusercontent.com/95197594/151272313-3b7aec68-8e1b-4bf4-bac7-371fac784d62.png)


### 3행 3열 표 만든 후 표 합치기
```html

</table>
<h2>3행3열 표합치기</h2>
<table border = "1">
<tr><td>1행1열</td><td>1행2열1행3열</td></tr>
<tr><td>2행1열</td><td>2행2열</td><td>2행3열</td></tr>
<tr><td>3행1열</td><td>3행2열</td><td>3행3열</td></tr>
</table>
```
- ![1](https://user-images.githubusercontent.com/95197594/151276364-5c4091f9-d74d-4b47-b575-73ee579a0aa2.PNG)

그냥 합쳐버리면 표에 흔적이 남아있기 때문에 colspan 태그를 추가해주어야 함.

```html
<h2>3행3열 표합치기</h2>
<table border = "1">
<tr><td>1행1열</td><td colspan="2">1행2열1행3열</td></tr>
<tr><td>2행1열</td><td>2행2열</td><td>2행3열</td></tr>
<tr><td>3행2열</td><td>3행3열</td></tr>
```

- ![2](https://user-images.githubusercontent.com/95197594/151276369-98517a70-418b-41f3-8bd9-6b8591e3f81f.PNG)


2행1열과 3행1열을 합치고 싶을때는 rowspan 태그를 추가해주어야 함.

```html
<table border = "1">
<tr><td>1행1열</td><td colspan="2">1행2열1행3열</td></tr>
<tr><td rowspan="2">2행1열<br>3행1열</td><td>2행2열</td><td>2행3열</td></tr>
<tr><td>3행2열</td><td>3행3열</td></tr>
```

- ![3](https://user-images.githubusercontent.com/95197594/151276373-5cb81188-afac-43ff-81ca-978708b9a0b0.PNG)
- ![4](https://user-images.githubusercontent.com/95197594/151276375-c7e97aa6-1e82-472c-8748-365a36dc7be9.PNG)


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

- ![5](https://user-images.githubusercontent.com/95197594/151277438-4c77b1bb-5f4b-40ba-9650-634c17f371d0.PNG)

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

- ![1](https://user-images.githubusercontent.com/95197594/151279302-614ff3ff-6772-4ed1-810d-a2d9668f7593.PNG)
- 전송버튼을 누르면 test4.html로 이동
- form 태그는 이동 / input 태그는 형태생성(텍스트, 버튼 등) 담당
  - form 태그 안에 입력한 데이터를 가지고 서버에 전송하면서 action에 저장된 페이지로 이동
- method는 데이터를 전송하는 방식
  - get 방식은 주소줄에 데이터가 보이면서 서버 전송 (get 방식은 따로 태그를 추가하지 않아도 자동으로 주소줄에 데이터가 보임)
  - post 방식은 주소줄에 데이터가 안 보이면서 서버 전송  
- value는 서버에 전송되어질 입력값


### 텍스트를 입력 가능한 상자 생성
```html
<body>
<h1>양식(폼)태그</h1>
<form action="test4.html" method="post">
아이디 : <input type="text" name="id" value="Lee"><br>
비밀번호 : <input type="password" name="pass"><br>
자기소개 : <textarea name="intro" rows="5" cols="10">안녕</textarea>
성별 : <input type="radio" name="gender" value="남">남성
       <input type="radio" name="gender" value="여">여성<br>
취미 : <input type="checkbox" name="hobby" value="여행">여행
       <input type="checkbox" name="game" value="게임">게임
       <input type="checkbox" name="health" value="운동">운동<br>
<input type="submit" value="전송버튼"><br>
목록상자 : <select name="sel">	     // name 뒤에 size 태그 추가시 넓게 보기 가능 ex) size="3". 하지만 많이 사용하지는 않음
	   <option value="목1">목록1</option>
	   <option value="목2">목록2</option>
	   <option value="목3">목록3</option>
	   </select>	   <br>
파일첨부 : <input type="file" name="file"><br>
히든태그 : <input type="hidden" name="hi" value="값"><br>
<input type="button" value="버튼"><br><br><br>	
<input type="submit" value="서버전송버튼"><br>	
<input type="image" src="1.jpg" width="100" height="100">	
<input type="reset" value="초기화"><br> 	
</form>
</body>
```
![캡처](https://user-images.githubusercontent.com/95197594/151293359-72cb1230-f72e-43cf-ae36-8eb037b0efe5.PNG)


- 이런 식으로 아이디, 비밀번호 입력상자 등등을 생성 가능.
- textarea 태그는 value 필요 없이 텍스트를 입력해놓으면 텍스트입력값이 형성됨
- rows, cols는 행/열 간격 설정

**사용한 input type 정리**
- text : 텍스트 입력
- password : 비밀번호 입력 (문자 입력시 글자가 비공개)
- intro : 넓은 텍스트입력란
- radio : 선택지 (하나만 선택)
- checkbox : 선택지 (다중선택)
- file : 첨부파일 업로드 가능 버튼
- hiddle : 본문에는 보이지 않지만 숨겨서 데이터를 넘길 수 있는 태그
- submit : 기능이 포함되어 있는 버튼 (form 태그에 있는 데이터들을 서버에 전송)
	- image : submit 버튼에 이미지 추가
- button : 기능이 없는 버튼 (자바스크립트와 제이쿼리에서 주로 사용)
- reset : 리셋버튼

---

# [오후수업]
## 반복문(Loop)
- 특정 문장 또는 블럭을 주어진 조건에 따라 반복 실행하는 문
- for 문과 while문(do-while)
	- for 문과 while문은 문법적 구조는 다르지만, 동작방식이 동일함
		- 따라서, 상호 100% 전환이 가능 

### while문
- 주로 반복횟수를 알 수 없을 때 사용
- for문과 while문은 100% 상호 변환 가능

ex)
```java
		int i = 0;
		while (i < 10) {
			i++;
			System.out.println(i);
			
		}
		-----------------------------------------------		
		// (1, 2, 3, ..., 10 출력)
		
		i=1;
		while(i <= 10) {
			System.out.println(i);
			i++;
			
			
		}
		-----------------------------------------------
		// 2 4 6 8 10 출력
		
		i = 2;
		while (i <= 10) {
			System.out.println(i);
			i += 2;
		}
		
		-----------------------------------------------
		// 10 9 8 7 ... 2 1 로 출력
		i = 10;
		while (i >= 1) {
			System.out.println(i + " ");
			i--;
		}

```

문제 1. 구구단 출력 (예는 2단이라고 가정)
```
		   < 2단 >
		   2 * 1 = 2
		   2 * 2 = 4
		   2 * 3 = 6
		   ...
		   2 * 9 = 18
```

```java

나의 해답) 
		int a = 1;
		int b = 2;
		
		System.out.println("< 2단 >");
		while (a < 10 && b < 20)  {
			System.out.println("2 * " + a + " = " + b );
			a += 1;
			b += 2; 
		}
-----------------------------------------------
정답)
		int dan = 2;
		
		System.out.println("< " + dan + "단 >");
		int i = 1;
		while(i < 10) {
			System.out.println(dan + " * " + i + " = " + (dan * i));
			i++;
		}
```

문제 2. 1 ~ 10 까지 정수의 합 sum을 계산하여 출력 출력예시) "1 ~ 10 까지의 합 : XXX"
```java
		int sum = 0;
		int i = 1;
		while (i <= 10) {
			sum += i;
			i++;
		}
			System.out.println("1 ~ 10 까지의 합 : " + sum);
```

문제 3. 1 ~ 100 까지 짝수/홀수의 합을 출력
- int odd(홀수), int even(짝수)
- 출력예시) "1 ~ 100 까지 짝수의 합 : XX"
		 			 "1 ~ 100 까지 홀수의 합 : XX"
			
```java
		int odd = 0;
		int even = 0;
		int i = 1;

		while (i <= 100) {

			if (i % 2 == 0) {
				even += i;
			} else
				odd += i;


			i++;
		}
		System.out.println("1 ~ 100 까지 짝수의 합 : " + even );
		System.out.println("1 ~ 100 까지 홀수의 합 : " + odd );
```		

### for문
- 주로 반복횟수가 정해져 있을 때 (알 수 있을 때)


```
< 기본 문법 >
 
for(초기식(1); 조건식(2); 증감식(4)){
		// 조건식이 true 일 때 반복 실행할 문장들...
		실행문(3);
		}
		
 => 초기식(1) : 반복 횟수를 제어할 제어변수를 선언하고 필요에 따라 초기화 수행
 => 조건식(2) : 제어변수를 사용하여 반복문을 반복(종료)하는 조건식 지정
     				  (조건식 결과가 true일 동안 계속 반복 수행하고, false이면 종료)
 => 실행문(3) : 조건식 결과가 true일 동안 반복 실행할 문장을 기술
    				  (한 문장 또는 여러 문장)
 => 증감식(4) : 실행문 실행 후 조건식으로 이동하기 전 제어변수를 증감시킴
 
 => 초기식 -> 조건식 -> 실행문 -> 증감식 -> 조건식 -> 실행문 -> 증감식 ... 반복
 ```
 
 ex)
 
```java
		for(int i = 0; i < 10; i++) {
			System.out.println(i + " : Hello, World!");
		}
		System.out.println(i);
-----------------------------------------------

		// 2 ~ 10까지 2씩 증가하면서 i값을 출력
		for (int i = 2; i <= 10; i += 2) {
			System.out.println(i + " ");
		}
-----------------------------------------------
		// 10 ~ 1까지 1씩 감소하면서 i값 출력 (10 9 8 7 6 ... 2 1)
		for (int i = 10; i >= 1; i--) {
			System.out.println(i + " ");
		}
-----------------------------------------------		
				// for문에서 구성요소 제외 가능
		int i = 0;
		for(; i <= 10; i++) {
			System.out.println(i + " ");
		}
-----------------------------------------------		
//		// 만약, 제어변수 i의 값만 변경하려면 선언 코드만 제외하고 사용
//		for(i = 0; i <= 10; i++) {
//			if(i == 5) break;
//			System.out.println(i + " ");
//		}
-----------------------------------------------		
			// break;
- 반복문을 중단하고 싶을 때 사용
- 반복도중 특정 조건을 만족하면 반복문을 빠져나오고 싶은 경우

		int i = 0;
		while(true) {
			System.out.println(i);
			if(i==10) {
				break;
			}
			i++;
		}
-----------------------------------------------		
		// continue
- 특정 조건에서 아래 실행문을 건너뛰고 싶은 경우 사용
- 반복문을 빠져나가는 것이 아니라 건너뛰고 계속 반복한다

		int i = 0;
		for(i = 0; i <= 10; i++) {
			if(i % 2 == 1) { // 홀수이면
				continue;
			}
			System.out.println(i + " ");
		}
```

문제 1. 1 ~ 100 까지 for 문을 사용하여 짝수의 합(even), 홀수의 합(odd) 각각 누적 한 후 출력 
- 출력예시) 
	- 1 ~ 100 까지 짝수의 합 : XX
	- 1 ~ 100 까지 홀수의 합 : XX
```java
		int even = 0;
		int odd = 0;

		for (i = 1; i <= 100; i++) {
			if (i % 2 == 0) {
				even += i;
			} else
				odd += i;

		}
		System.out.println("1 ~ 100 까지 짝수의 합 : " + even);
		System.out.println("1 ~ 100 까지 홀수의 합 : " + odd);
```	

문제 2. 두 정수 a ~ b 까지 3의 배수의 합 
- 출력 : "X ~ X 까지 3의 배수의 합 : XX"
- int a = 10;, int b = 20;

```java
		int a = 10;
		int b = 20;
		int sum = 0;
		for (int i = a; i <= b; i++) {

			if (i % 3 == 0) {
				sum += i;
		}

	}
	System.out.println(a + " ~ " + b + " 까지의 3의 배수의 합: " + sum);
}
```

문제 3. 1 ~ 100 까지의 합을 계산하는데 합이 50을 넘을 경우 반복을 중단하고 그 때, 몇까지 더했는지 출력

```java
		int sum = 0;
		for (int i = 1; i <= 100; i++) {
				sum += i;
				
				if (sum >= 50) {
					System.out.println(i);
					break;
```					




