# [오전수업] JAVA 10차
> JAVA 9차에서 실시한 복습문제 이어서 실시

3. 구구단을 단보다 곱하는 수가 작거나 같은 경우까지만 출력하는 프로그램 만들기 (break문 사용)

```java
		for (int dan = 2; dan < 10; dan++) {
			System.out.println("< " + dan + "단 >");
			for (int i = 1; i < 10; i++) {
				if (dan < i) {
					break;
				}
				System.out.println(dan + " * " + i + " = " + (dan * i));

			}
			System.out.println();
		}
```

4. 반복문을 사용하여 다음 모양을 출력하는 프로그램 만들기
```java
      *			// i = 3일 때, 좌우공백 : 3개, 별 : 1개
    * * *		// i = 2일 때, 좌우공백 : 2개, 별 : 3개
  * * * * *		// i = 1일 때, 좌우공백 : 1개, 별 : 5개
* * * * * * *		// i = 0일 때, 좌우공백 : 0개, 별 : 7개



int n = 4; // n을 층 수로 생각
		

n = 4일 때, i = 3, 별의갯수 = 1
n = 4일 때, i = 2, 별의갯수 = 3
n = 4일 때, i = 1, 별의갯수 = 5
n = 4일 때, i = 0, 별의갯수 = 7
2씩 곱하고 계산하였을때,
8 - 6 = 2
8 - 4 = 4
8 - 2 = 6
8 - 0 = 8
별의 갯수가 -1씩 적은 것을 확인 할 수 있음.

		
		
		for(int i = n-1; i >= 0; i--) { // 행
			
			for(int j = 0; j < i; j++) {System.out.print(" ");} // 좌공백
			for(int j = 1; j <= (n*2) - (i*2) - 1; j++) {System.out.print("*");} // 별
			for(int j = 0; j < i; j++) {System.out.print(" ");} // 우공백
			
			System.out.println();
		}
```

5. 반복문과 조건문을 사용하여 다음 모양을 출력하는 프로그램 만들기
```java
  
      *			// i = 0일 때, 좌우공백 : 3개, 별 : 1개
    * * *		// i = 1일 때, 좌우공백 : 2개, 별 : 3개
  * * * * *		// i = 2일 때, 좌우공백 : 1개, 별 : 5개
* * * * * * *		// i = 3일 때, 좌우공백 : 0개, 별 : 7개
  * * * * *		// i = 4일 때, 좌우공백 : 1개, 별 : 5개
    * * *		// i = 5일 때, 좌우공백 : 2개, 별 : 3개
      *			// i = 6일 때, 좌우공백 : 3개, 별 : 1개
-----------------------------------------------------------------------------------      
package ch04;

public class Ex5 {

	public static void main(String[] args) {
      
      
      
		int line = 7; // 홀수만 가능
		int space = line / 2; // 공백
		int star = 1;
		for (int i = 1; i < line; i++) { // 행

			for (int j = 0; j < space; j++) {
				System.out.print(" ");
			} // 좌공백
			for (int j = 0; j < star; j++) {
				System.out.print("*");
			} // 별
			for (int j = 0; j < space; j++) {
				System.out.print(" ");
			} // 우공백
			System.out.println();
			if (i < line / 2) {
				space = space - 1;
				star = star + 2;
			} else {
				space = space + 1;
				star = star - 2;
			}

		}

		System.out.println("------------------------------");
		// method 버전
		line = 7;
		space = line/2;
		star = 1;
		
		for(int i = 0; i < line; i++) {
			
			printSpace(space);
			printStar(star);
			printSpace(space);
			System.out.println();
			
			if(i < line/2) {
				space -= 1;
				star += 2;
			} else {
				space += 1;
				star -= 2;
			}
		}
		
	}
	
	// 공백을 찍을 메서드
	public static void printSpace(int 공백갯수) {
		for(int i=0; i<공백갯수; i++) {
			System.out.print(" ");
		}
	}
	
	// 별찍을 메서드
	public static void printStar(int 별갯수) {
		for(int i=0; i<별갯수; i++) {
			System.out.print("*");
		}
	}

}
```
6. 거스름돈 계산
```java
N이 32850일 경우,
50000원 : 0개
10000원 : 3개
5000원 : 0개
1000원 : 2개
500원 : 1개
100원 : 3개
50원 : 1개
10원 : 0개
배열, 메서드 사용
-----------------------------------------------------------------------------------   

public class Test1 {

	public static void main(String[] args) {

		
		int N = 32850;
		int[] money = { 50000, 10000, 5000, 1000, 500, 100, 50, 10 };
		
		
		
		
		// while문 (나의 해답)
//		int i = 0;
//		while(i < money.length) {
//			
//			System.out.println(money[i] + "원 : " + (N / money[i]) + "개");
//			N %= money[i];
//			i++;
//		}

		
		
		
		
		// for문
//		for(int i = 1; i < money.length; i++) {
////			int cnt = N / money[i]; // 메서드로 변경
//			int cnt = change(money[i], N);
//			N %= money[i];
//				System.out.println(money[i] + "원 : " + cnt + "개");
//		}

		
		
		
		
		// 거스름돈 갯수를 저장할 배열 cnt[]를 리턴받는 메서드 version
		
		int [] cnt = change(money, N);
		for(int i = 0; i < cnt.length; i++) {
			System.out.println(money[i] + "원 : " + cnt[i] + "개");
		}
		
		
		
		

	}
	// 거스름돈만 계산해서 리턴하는 메서드
	public static int change(int money, int N) {
		return N / money;
	}	
		// money 배열과 기준이 되는 금액(N)을 전달받아 cnt[] 배열을 리턴하는 메서드 작성
	public static int[] change(int[] money, int N) {
			int cnt[] = new int[money.length];
			
			for(int i = 0; i < money.length; i++) {
				cnt[i] = N / money[i];
				N %= money[i];
			}
			return cnt;
		}
	}


```
7. n개의 숫자가 입력되면 n개의 숫자를 왼쪽으로 하나씩 돌려서 출력하시오
```java
입력예시)
5
1 2 3 4 5
출력 예시)
1 2 3 4 5 
2 3 4 5 1
3 4 5 1 2
4 5 1 2 3
5 1 2 3 4 
-----------------------------------------------------------------------------------   

public class Test2 {

	public static void main(String[] args) {


		int[] arr = { 1, 2, 3, 4, 5 };

		// 기본적인 중첩 for문 정답
		for (int i = 0; i < arr.length; i++) {

			// 배열에 요소를 우측으로 출력
			for (int j = 0; j < arr.length; j++) {
				System.out.print(arr[j] + " ");
			}

			System.out.println();

			// 배열의 요소 이동하는 부분 -> for문으로
//			int temp = arr[0];
//			arr[0] = arr[1];
//			arr[1] = arr[2];
//			arr[2] = arr[3];
//			arr[3] = arr[4];
//			arr[4] = temp;

			// 배열 한 바퀴 돌림 (rotation)
			int temp = arr[0];
			for (int j = 0; j < arr.length - 1; j++) {
				arr[j] = arr[j + 1];
			}
			arr[arr.length - 1] = temp;

		}
		System.out.println("==============================================");
		// method로 변경
		for(int i = 0; i < arr.length; i++) {
			printArray(arr);
			rotation(arr);
		}
	}

	// 배열에 요소를 우측으로 출력
	public static void printArray(int[] arr) {
		for (int i = 0; i < arr.length; i++) {
			System.out.print(arr[i] + " ");
		}

		System.out.println();
	}

	// 배열 한 바퀴 돌림 (rotation)
	public static void rotation(int[] arr) {
		int temp = arr[0];
		for (int i = 0; i < arr.length - 1; i++) {
			arr[i] = arr[i + 1];
		}
		arr[arr.length - 1] = temp;
	}
}
```

# [오후수업] JSP 14차
## 자바스크립트에서의 배열(Array)
### 배열 생성 방법 2가지
1. 객체 생성 방법을 통한 배열 사용(new Array(배열크기))
```javascript	
var arr1 = new Array(3); // 3개의 저장 공간을 갖는 배열 생성
// => 자바 코드 : int[] arr 1 = new int[3]; 과 동일한 코드(타입은 다를 수 있음)
//	  (타입은 다를 수 있음 => 자바스크립트의 배열은 데이터타입 미정)
// 	document.write("arr1 배열의 타입 : " + typeof(arr1)); // object 타입
```

2. 객체 생성 대신 리터럴을 통한 데이터 초기화
```javascript
// => 대괄호[] 를 사용하여 데이터(요소)를 직접 전달하여 배열 생성(많이 사용)
var arr2 = [1, 2, 3]; // 3개의 배열 공간에 정수 1, 2, 3 을 차례대로 저장
```	

```javascript
	// fruitsArr 배열에 "사과", "딸기", "바나나", 데이터 초기화
	var fruitsArr = ["사과", "딸기", "바나나"]
	
	// document.write() 함수를 사용하여 fruitsArr 배열 내의 모든 요소를 하나씩 출력
	document.write(fruitsArr[0] + ", " + fruitsArr[1] + ", " + fruitsArr[2] + "<br>")
	// 만약, 배열 내의 모든 요소를 한꺼번에 출력할 경우 배열명만으로도 출력 가능
	document.write(fruitsArr + "<br>")
	
	// 배열 요소에 접근하여 데이터 저장
	fruitsArr[0] = "오렌지"; // 0번 인덱스(첫번째 요소) 데이터 변경
	document.write(fruitsArr + "<br>")
	
	// 배열명.length 속성을 사용하여 배열 크기 확인 가능
	document.write("fruitsArr 배열 크기 : " + fruitsArr.length + "<br>")	
	document.write("<hr>")
```
### 반복문을 사용하여 배열의 각 요소에 접근
1. 일반적인 for문(배열의 인덱스 활용)을 사용하여 접근
- 제어변수 i값을 0부터 마지막 인덱스(배열크기-1)까지 1씩 증가하며 차례대로 접근
```javascript
for(var i = 0; i < fruitsArr.length; i++) {
		document.write(i + "번째 요소 : " + fruitsArr[i] + "<br>")
	}
```

2. for...of문 사용(가장 많이 사용)
- 배열 내의 요소를 자동으로 차례대로 받아와서 사용(단, 인덱스 지정 불가)
	- (자바의 향상된 for문(foreach 문) 과 동일한 방식)
```javascript
	// 기본 문법 : for(변수선언 of 배열명) {}
	// => 우변의 배열로부터 데이터를 하나씩 꺼내서 좌변의 선언된 변수에 저장
	for(var element of fruitsArr) { // fruitsArr 배열의 데이터를 하나씩 차례대로 element 변수에 저장
		document.write("for...of문 사용한 배열 요소 : " + element + "<br>")	
	}
```
3. for...in 문 사용
객체(배열)의 key(배열에서는 인덱스)를 차례대로 받아와서 사용
- 주로 객체를 대상으로 사용하며, 배열 대상으로는 잘 사용하지 않음
```javascript
// 기본 문법 : for(변수선언 in 배열명) {}
	// => 우변의 배열로부터 데이터가 위치한 인덱스 번호를 좌변에 선언된 변수에 저장
	for(var index in fruitsArr) {
		document.write("for...in문 사용한 배열 요소 : " + fruitsArr[index] + "<br>")			
	}
```

### 자바의 배열과 다른 점
1. 생성된 기존 배열에 새로운 요소 추가 가능
- 기존 배열 크기와 상관없이 추가되는 데이터에 따라 공간 확장됨
```javascript
document.write("현재 fruitsArr 배열 크기 : " + fruitsArr.length + "<br>")
	fruitsArr[3] = "레몬"; // 0 ~ 2번까지만 존재하는 배열의 3번 인덱스에 데이터 추가됨
	document.write("추가 후 fruitsArr 배열 크기 : " + fruitsArr.length + "<br>")
	document.write("추가 후 fruitsArr 배열 데이터 : " + fruitsArr + "<br>")
```

2. 배열의 length 속성은 변경 가능한 속성이므로 배열의 크기를 강제로 변경 가능
```javascript
fruitsArr.length = 10;
	document.write("크기 변경 후 fruitsArr 배열 크기 : " + fruitsArr.length + "<br>")	// 10 출력
	
	// 만약, 배열을 초기화 할 경우 배열 크기를 0으로 지정
	fruitsArr.length = 0;
	document.write("초기화 후 fruitsArr 배열 데이터 : " + fruitsArr+ "<br>")
```

3. 배열에 저장되는 요소들의 데이터타입에 제약이 없음
- 정수, 문자, 논리타입 뿐만 아니라 객체, 배열, 함수까지도 저장 가능
```javascript
var arrAllType = ["홍길동", 3.14, true, function() { alert("배열 내의 함수"); }];
	// for...of 문 사용하여 arrAllType 배열 내의 모든 데이터 출력
	for(var item of arrAllType) { // 우변의 배열 데이터 하나씩 좌변의 변수에 저장 반복
		document.write("배열 내의 요소 : " + item + ", 데이터타입 : " + typeof(item) + "<br>")
```

4. 배열에 요소를 전달하여 생성 시 마지막 요소 끝에도 콤마(,) 사용 가능
- 차후 데이터(요소) 추가/삭제 시 마지막 요소에 대한 콤마 구분 불필요
- Trailing Comma 라고 함
```javascript
var names = [
			"홍길동",
			"이순신",
			"강감찬",
			"김태희",
			"전지현",
			"송혜교",
	]
```


### 배열을 다루는 연산자 및 함수(배열명.함수() 형태로 호출)
1. delete 연산자(delete 배열명[인덱스]) : 지정된 인덱스의 배열 요소 삭제
```javascript
delete names[2]; // names 배열의 2번 인덱스 요소 ("강감찬") 삭제
	document.write("요소 삭제 후 names : " + names + "<br>")
	// => 주의! 데이터가 있던 인덱스 자체는 그대로 유지됨(배열 출력 시 데이터 공간은 , 로 표시됨)
```

2. splice(인덱스, 요소갯수) 함수 : 지정된 인덱스부터 요소갯수만큼 배열 요소 삭제
```javascript
	names.splice(1, 2);	// 1번 인덱스부터 2개의 요소 삭제
	document.write("요소 삭제 후 names : " + names + "<br>")
	// => 데이터 삭제 시 해당 공간 자체를 삭제
```

3. slice(시작인덱스, 끝인덱스) : 시작인덱스 ~ 끝인덱스-1 까지 부분 배열 추출
- 추출된 배열은 배열 형태로 변수에 저장 또는 사용 가능
```javascript
var arr = [1, 2, 3, 4, 5];
	var arrSlice = arr.slice(1, 3); // 1 부터 3-1 까지의 인덱스 요소 추출하여 배열로 리턴
	document.write("slice() 함수 호출 후 : " + arrSlice + "<br>")
```

---

## onload 이벤트
```javascript
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<script type="text/javascript">
	// onload 이벤트
	// window 객체의 onload 속성에 특정 함수를 선언하는 함수 선언문을 대입할 경우
	// 현재 문서(HTML)의 본문(body 영역)이 로딩 완료 된 후 onload 이벤트가 동작하며,
	// onload 이벤트에 대입되어 있는 함수가 자동으로 호출됨
	// 이벤트 등록 방법 1. 자바스크립트에서 window.onload 형식으로 이벤트 등록
	// => window.onload = function() { 실행할 함수 코드...	}
	// 이벤트 등록 방법 2. body 태그에 onload 속성을 통해 이벤트 등록
	// => <body onload="호출할함수()">
	
// 	window.onload = function() { // 함수의 이름을 지정하지 않는 익명 함수 선언
// 		alert("이 함수는 언제 실행될까요?")
// 		// 위치상으로는 가장 먼저 실행될 자바스크립트 코드이지만,
// 		// onload 이벤트에 등록된 함수이므로 body 영역의 모든 코드가 실행 된 후 자동으로 실행됨
		
// 		// 따라서, 이 함수 내에서는 body 태그 영역의 요소에 접근도 가능함
// 		// => body 태그 내의 내용이 모두 로딩된 후이므로 body의 모든 요소에 접근 및 제어가 가능한 시점이라는 의미
// 		alert("window.onload 이벤트의 document.body : " + document.body)
// 		// => body 태그에 해당하는 객체가 이미 생성된 후 (body가 로딩된 후) 이므로
// 		//	  document.body 에 접근 시 HTMLBodyElement 객체가 출력됨(객체가 존재함)
		
// 		// 하나의 함수 내에서 body 영역의 다양한 제어가 가능
// 		document.body.style.background = "GRAY";
		
// 	}
	
	
	function func1() {
		alert("이 함수는 언제 실행될까요?")
		document.body.style.background = "GRAY";
	}
	
</script>



<!-- 기본적으로 HTML 및 자바스크립트 코드는 위에서 아래로 차례대로 번역 후 실행됨 -->
<script type="text/javascript">
	alert("작업 - 1"); // 첫번째로 실행되는 스크립트
	alert("작업-1 의 document.body : " + document.body) // null 값 출력됨(body 존재하지 않음)

</script>
</head>
<body onload="func1()"><!-- body 영역 로딩 완료 후 onload 이벤트에 의해 func1() 함수 호출됨 -->
	<h1>test9-2.html</h1>
	<script>alert("작업 - 2")</script> <!-- 두번째 스크립트 -->
	<!-- 세번째 스크립트 -->
	<script>
	alert("작업 - 3");
	alert("작업-3 의 document.body : " + document.body)
	</script> <!-- 세번째 스크립트 -->
	<h1>body 태그 내의 모든 요소 로딩 완료!</h1>
</body>
</html>
```
## getElementById(), getElementsByTagName() 함수
- 특정 태그에 접근하는 함수
```javascript
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<script type="text/javascript">
	/*
	document 객체의 getElementById() 함수
	- 태그 내에 지정된 id 속성값(id 선택자)이 지정된 특정 태그에 접근하기 위해
	  해당 태그의 객체를 얻어오는 함수
	  => 지정된 id에 해당하는 태그의 요소를 객체 형태로 리턴
	- 함수 파라미터로 태그에 지정된 id 선택자의 속성값을 문자열로 전달
	
	< 기본 문법 >
	 document.getLelmentById("id선택자이름");
	 ex) document.getElementById("header"); => "header" 라는 id 속성값을 갖는 태그를 객체로 리턴
	 ---------------------------------------------------------------------------------------------
	 document 객체의 getElementsByTagName() 함수
	 - 특정 태그 전체를 한꺼번에 접근하기 위해 해당 태그의 객체를 얻어오는 함수
	   => 지정된 태그와 일치하는 태그의 요소를 모두 객체 형태로 리턴
	 - 함수 파라미터로 태그 이름을 문자열로 전달
	 - 주의! 리턴받은 객체는 복수개의 객체가 HTMCollection 타입으로 저장되어 있으며
	  접근을 위해서는 반복문(주로 for...of)을 사용하여 접근해야함
	  => 반복문을 통해 저장된 태그 객체 하나씩 별도로 접근 
	 
	 < 기본 문법 >
	   document.getElementsByTagName("태그명");
	   ex) document.getElementsByTagName("h1"); => h1 태그를 모두 객체로 리턴
	 */
// 	 var idHeader1 = document.getElementById("header1")
// 	alert(idHeader1); // null 출력됨("header1" 이름을 갖는 태그 로딩되기 전 = 객체 없음)
// 	idHeader1.style.background = "RED";
	 //	=> 주의! 일반 자바스크립트 형태로 실행 시 body 영역 로딩 전에 실행될 경우 실행 불가능!
	
	 
	 // onload 이벤트를 통해 함수 내에서 body 영역에 접근해야함
	window.onload = function() {
		 var idHeader1 = document.getElementById("header1") // header1 이라는 속성값의 태그 지정
// 		 alert(idHeader1); // HTMLHeadingElement 객체 출력됨(= 객체 존재함)
		 // 따라서, 지정된 객체(태그)에 접근하여 속성 등을 변경하는 작업 수행 가능
		 idHeader1.style.background = "RED"; // 지정된 객체의 background 속성을 "RED" 로 변경
		 
		 // "header2" 라는 이름의 id 선택자를 지정하여 해당 객체를 변수에 저장한 후
		 // background 속성을 "SKYBLUE" 로 변경
		 
		 var idHeader2 = document.getElementById("header2")
		 idHeader2.style.background = "SKYBLUE"
		 
		 // "header2" 선택자에 해당하는 태그의 내용(텍스트) 변경
		 // => 해당 객체의 innerHTML 속성에 변경할 내용을 지정
		 idHeader2.innerHTML = "innerHTML 속성을 변경";
		 
		 // -------------------------------------------------------------------------
		 // h2 태그를 지정하여 변수 tagsH2 에 저장
		 var tagsH2 = document.getElementsByTagName("h2")
// 		 alert(tagsH2 + ", " + typeof(tagsH2)) // HTMLCollection 객체 출력됨
// 		 tagsH2.style.background = "GREEN" // 잘못된 코드
		 for(var tagH2 of tagsH2) { // 저장된 태그 객체를 하나씩 꺼내서 변수에 저장
			 tagH2.style.background = "GREEN"
		 }
	}
	
	 
	 function func1() {
// 		 alert("수업 시작")
		 // id 속성값이 "status" 인 div 태그 객체 가져오기(tagDiv 에 저장)
		 var tagDiv = document.getElementById("status")
		 tagDiv.innerHTML = "JSP 수업 상태 : 시작됨!";
		 // div 태그 객체의 innterHTML 속성에 "JSP 수업 상태 : 시작됨!" 값 저장(= 변경)
	 }
	 
 	function func2() {
// 		 alert("수업 종료")		 
// 		 id 속성값이 "status" 인 div 태그 객체 가져오기(tagDiv 에 저장)
		 var tagDiv = document.getElementById("status")
		 tagDiv.innerHTML = "JSP 수업 상태 : 종료됨!";
		 // div 태그 객체의 innterHTML 속성에 "JSP 수업 상태 : 종료됨!" 값 저장(= 변경)
	 }
	 
</script>
</head>
<body>
	<h1>test10.html</h1>
	<h3 id="header1">Header - 1</h3>
	<h3 id="header2">Header - 2</h3>
	<h2 id="header3">Header - 3</h2>
	<h2 id="header4">Header - 4</h2>
	<hr>
	<div id="status">JSP 수업 상태</div>
	<!-- [수업 시작] 버튼 클릭 시 func1() 함수 호출, [수업 종료] 버튼 클릭 시 func2() 함수 호출 -->
	<input type="button" value ="수업 시작" onclick = "func1()">
	<input type="button" value ="수업 종료" onclick = "func2()">
</body>
</html>
```
## setInterval() 함수
- 주기적 반복 함수
```javascript
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<script type="text/javascript">
	// body 태그 요소가 로딩 완료된 후 호출될 함수 선언
	window.onload = function() {
		// clock1 과 clock2 에 해당하는 id 속성값을 지정하여 해당 객체를 가져와서 변수에 저장
		
		var clock1 = document.getElementById("clock1")
		var clock2 = document.getElementById("clock2")
// 		alert(clock1 + ", " + clock2)
		
		// Date 객체를 활용하여 현재 서버 시각 정보 얻어오기
		var now = new Date().toString();
		clock1.innerHTML = now;
		
		/*
		자바스크립트 내에서 어떤 작업(함수 등)을 주기적으로 반복하기 위해서는
		setInterval() 함수를 호출하여 반복할 작업(함수)과 반복 주기를 파라미터로 전달해야함
		=> 이 때, 반복 주기는 밀리초(ms) 단위의 값 전달(1000밀리초 = 1초)
		< 기본 문법 >
		setInterval(함수, 반복주기);
		*/
		
		// 외부에서 정의한 별도의 함수를 파라미터로 전달하는 방법
// 		setInterval(reloadClock, 1000); // reloadClock() 함수를 1초마다 호출(반복)
		
		// setInterval() 함수의 파라미터에 작업을 수행할 함수를 직접 정의하여 전달
		// => 익명 함수의 형태로 정의(= 외부로부터 호출이 불필요한 함수이므로)
		setInterval(function() {
			// 주의! setInterval() 외부에서 서버 시각을 생성할 경우 같은 값을 가지므로
			// setInterval() 함수 내에서 서버 시각을 가져와야 함
			clock2.innerHTML = new Date().toString(); // 서버 시각을 가져와서 표시
		}, 1000);

	}
	
	// clock2에 해당하는 태그에 현재 시각 표시하는 함수 reloadClock() 정의
	
	function reloadClock() {
		var clock2 = document.getElementById("clock2")
		var now = new Date().toString();
		clock2.innerHTML = now;
	}
</script>
</head>
<body>
	<h1>test10-2.html</h1>
	<h2 id="clock1">시각 표시 위치1</h2>
	<h2 id="clock2">시각 표시 위치2</h2>
</body>
</html>
```
