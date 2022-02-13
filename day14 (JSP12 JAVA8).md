# [오전수업] JSP 12차

## 함수를 변수에 저장
```javascript
<script type="text/javascript">
	function print(){
		alert("자바스크립트 함수입니다.")
		return "홍길동";
	}

	// 자바스크립트에서 함수는 객체처럼 취급되어 변수에 저장하거나 출력할 수도 있다
	var f = print; // print() 함수를 호출하여 리턴값을 전달받음
	//print() 함수가 리턴하는 데이터 "홍길동" 을 f 변수에 저장
// 	alert(f); // "홍길동" 출력됨

	//자바스크립트에서 함수는 객체처럼 취급되어 변수에 저장허거나 출력할 수도 있다.
	var f = print; // print() 함수 자체를 변수에 저장(=코드 자체 복사)(리턴값이 아님!)
	document.write(f + "<br>"); // 함수 코드 자체가 출력됨
	
	var result = print();
	document.write("print() 함수 호출 결과 : " + result + "<br>")
	
	var result = f();
	document.write(result + "<br>")
	// => print() 함수 호출과 f() 함수 호출 결과는 완벽하게 동일함(=함수 코드를 복사했기 때문)
	
</script>
```

## 이벤트
- 어떤 대상(버튼, 텍스트상자 등 = Component(컴포넌트))에 사용자가 액션을 취하는 것
  - ex) 버튼 클릭, 텍스트 입력, 마우스 오버

### 이벤트 처리
- 이벤트가 발생했을 때 지정된 특정 작업을 수행하는 것
	- ex) 버튼 클릭 시 메세지 출력, 패스워드 입력 시  규칙 판별, 마우스 오버 시 이미지를 교체
	- 이벤트가 발생하는 대상 컴포넌트에 onXXX 속성을 사용하여 이벤트를 지정하고, 해당 속성의 값으로 이벤트 발생 시 처리할 자바스크립트를 지정하여 이벤트 처리 수행
	- 이벤트 종류 클릭 이벤트(click), 마우스 이벤트(mouseXXX) 등
  - ex) 클릭 이벤트를 등록하는 속성 : onclick / 마우스 오버 이벤트를 등록하는 속성 : onmouseover
	- onXXX 속성의 값으로 자바스크립트 함수 등을 호출하거나 자바스크립트 속성을 지정하여 작업 수행
	- ex) onclick = "alert('메세지 출력')" 지정 시 해당 대상 클릭 시 '메세지 출력' 이라는 문자 출력됨
	> (주의! onXXX 속성을 큰따옴표("")로 지정하므로 자바스크립트 내에서 따옴표는 작은따옴표('') 사용)

```
[ HTML 태그에서 이벤트 등록 기본 문법 ]
	<XXX태그 onXXX="자바스크립트코드">

```

### HTML 태그를 사용하여 버튼 생성하기
```javascript
	 1) <input> 태그의 type="button" 속성 사용 => value 속성에 버튼 텍스트 지정
	 2) <button> 태그 사용 => 시작 태그와 끝 태그 사이에 버튼 텍스트 지정
	 -->

	<!-- 	 <input type="button" value="Button"><br> -->
	<!-- 	 <button>Button2</button> -->
```  
### 버튼에 다양한 이벤트 등록하여 처리
```javascript
<!-- 버튼 이벤트에 자바스크립트 내장함수를 직접 지정하지 않고, 사용자 정의함수도 지정하여 호출 가능-->
	<button onclick="btnClick()">버튼1</button>
	<br>
	<!-- 버튼2 클릭 시 btnClick2() 함수 호출 => 함수 파라미터에 "버튼2" 텍스트 전달 함수내에서 alert() 함수로 "버튼 2 클릭됨!" 출력) -->
	<button onclick="btnClick2('버튼2222')">버튼2</button>
	<br>
	<button onclick="alert(btnClick3())">버튼3</button>
	<br>
	<!-- 버튼에 이벤트를 등록하여 alert() 함수 호출(메세지 출력) 작업 수행 -->
	<input type="button" value="버튼4" onclick="alert('버튼4클릭됨')">
	<br>
	<input type="button" value="버튼5" ondblclick="alert('버튼6더블클릭됨')">
	<br>
	<input type="button" value="버튼6" onmouseover="alert('버튼6마우스오버')">
  
  
---------------------------btnClcik() 함수들---------------------------
<script type="text/javascript">
	// alert() 함수를 호출하여 "버튼 클릭됨" 메세지 출력하는 btnClick() 함수 정의
	function btnClick() {
		alert("버튼 클릭됨!");
	}
	
	// 전달받은 텍스트를 출력하는 btnClick2() 함수 정의
	function btnClick2(msg) { // 파라미터로 데이터 전달 
		alert(msg) // 전달받은 데이터 출력
	}
	
	// 문자열을 리턴하는 btnClick3() 함수 정의
	function btnClick3() {
		return "btnClick3 함수 리턴값";
	}
  </script>
---------------------------btnClcik() 함수들---------------------------


<hr>

<!-- "아이티윌 부산교육센터 홈페이지" 버튼 클릭 시 "http://www.itwillbs.co.kr"로 이동 -->
	<!-- 1. 하이퍼링크 태그(a)를 사용하여 이동 -->
	<a href="http://www.itwillbs.co.kr"><button>아이티윌 부산교육센터
			홈페이지</button></a>
	<br>
	<a href="http://www.itwillbs.co.kr"><input type="button"
		value="아이티윌 부산교육센터 홈페이지"></a>
	<br>
	<!-- 2. 버튼의 onclick 속성을 통해 자바스크립트 코드를 사용하여 이동 -->
	<!-- 	<input type="button" value="아이티윌 부산교육센터 홈페이지" onclick="goUrl()"><br> -->
	<!-- 2-2. 자바스크립트 코드를 직접 기술하여 이동하는 경우 -->
	<input type="button" value="아이티윌 부산교육센터 홈페이지"
		onclick="location.href='http://www.itwillbs.co.kr'">
	<br>

	<!-- 3. URL 입력 버튼 클릭 시 goInputUrl() 함수 호출 -->
	<input type="button" value="URL 입력" onclick="goInputUrl()">
  
  ----------------------------함수들-----------------------------
  //특정 웹사이트(URL)로 이동하는 goUrl() 함수 정의
	function goUrl() {
		// 자바스크립트 내장 객체인 location 객체의 href 속성을 사용하여 이동할 페이지를 지정(href 속성에 대입))
		// location.href = "이동할 url";
		location.href = "http://www.itwillbs.co.kr";
	}
	// 접속할 웹사이트 주소를 입력받아 해당 주소로 이동하는 goInputUrl() 함수 정의
	function goInputUrl() {
		// 1. 사용자로부터 주소를 입력받아 변수 url에 저장
		// => 주의! 주소 입력 시 http:// 필수로 입력해야함
		var url = prompt("URL 입력")

		// 2. 입력받은 주소(url)가 아무것도 없을 경우("") 또는 null 값이 입력됐을 경우 "취소합니다" 메세지 출력
		//	  아니면, 입력받은 주소(url) 로 이동 작업 수행
		if (url == "" || url == null) {
			alert("이동을 취소합니다")
		} else {
			loocation.href = url
		}

	}
 ----------------------------함수들-----------------------------

```  
## 객체
### 자바스크립트 구성 요소
1. 변수(var, let, const)
2. 함수(내장 함수, 사용자 정의 함수)
3. 객체(내장 객체, 사용자 정의 객체)

### 자바스크립트에서의 객체
- 자바와 달리 클래스 틀이 아닌 객체 자체를 정의
- 객체를 담기 위한 변수를 선언하고, 해당 변수에 중괄호를 사용하여 객체 내의 속성 정의
- 별도의 인스턴스 생성 과정이 없이 지정된 변수를 통해 객체를 즉시 사용 가능

```javascript
< 기본 문법 >
var 객체명 = {
		// 키 : 값(= 변수 : 데이터) 형태로 객체 내의 속성 정의
		키1 : 값1,
		키2 : 값2,
		...생략...
		키n : 값n
}

< 객체 사용 기본 문법 >
객체명.키
```
### 사용자 정의 객체 생성
```javascript
	// 사용자 정의 객체 생성
	// => 이름(name) 속성과 나이(age) 속성을 갖는 person 객체 정의
	var person = {
			// 객체가 갖는 키:값 을 명시(이 때 , 키:값의 한 쌍을 property(프로퍼티) 라고 함)
			name : "홍길동", // name 이라는 키에 "홍길동" 이라는 값을 저장 
			age : 20, // age 라는 키에 20 이라는 값을 저장
			hobby : "" // 저장할 데이터가 없을 경우 최소한의 데이터(널스트링"" 같은) 함께 지정
	}
	
	//객체를 저장하고 있는 person 변수의 타입은 객체라는 의미의 person 객체 정의
// 	alert(typeof(person))
	
	// 1. 객체 내의 프로퍼티 키(key)를 사용해서 값(value) 가져오기
	document.write("이름(name) : " + person.name + "<br>");
	document.write("나이(age) : " + person.age + "<br>");
	document.write("취미(hobby) : " + person.hobby + "<br>");
	
	// 2. 객체 내의 프로퍼티 키를 사용하여 값 변경
	// 이름(name) 키의 값을 "이순신", 나이(age) 키 값을 44 로 변경 후 출력
	person.name = "이순신";
	person.age = 44;
	person.hobby = "프로그래밍";
	
	document.write("변경 후 이름(name) : " + person.name + "<br>");
	document.write("변경 후 나이(age) : " + person.age + "<br>");
	document.write("변경 후 취미(hobby) : " + person.hobby + "<br>");
	
	// 3. 객체에 새로운 프로퍼티 추가(=새로운 키와 값을 저장)
	person.isHungry = true; // person 객체에 isHungry 라는 키를 생성하고 값으로 true 저장
	person.height = 180.5; // person 객체에 height 키의 180.5 값 저장
	
	document.write("추가된 isHungry : " + person.isHungry + "<br>");
	document.write("추가된 height : " + person.height + "<br>");
	
	// 4. 객체 내의 프로퍼티 삭제
	// => delete 연산자 사용(delete 객체명.키)
	// => 삭제 후 해당 프로퍼티는 존재하지 않게 되므로 값을 출력할 경우 undefined 가 출력됨
	delete person.height;
	document.write("추가된 height : " + person.height + "<br>");
	
	// 5. 객체 내에 특정 프로퍼티 존재 여부 확인
	// => "키" in 객체명
	document.write("name 이라는 키가 존재합니까? " + ("name" in person) + "<br>"); // true 출력됨
	document.write("height 이라는 키가 존재합니까? " + ("height" in person) + "<br>"); // false 출력됨
```

---

# [오후수업] JAVA 8차
## 배열
- 같은 타입 데이터 여러개를 하나의 변수명을 사용하여 연속된 공간으로 다루는 것
1. **같은 데이터타입만 하나의 배열로 저장 가능**
2. 기본 데이터타입과 참조 데이터타입 모두 배열로 저장 가능
3. 배열명(변수명)을 사용하여 여러 데이터 관리 가능
4. **배열 내에 자동으로 부여되는 번호(인덱스 index)를 사용하여 배열의 각 요소 접근 가능**
	- (인덱스 번호는 0부터 시작하여 배열크기 -1 번까지 자동으로 부여됨)
	- (ex. 배열크기가 5일 경우 5개의 데이터 저장 가능하며, 인덱스는 0~4번 까지)
5. 배열 크기는 배열명.length 속성을 통해 얻을 수 있음
6. 배열은 배열 선언 -> 생성 -> 초기화 과정을 거쳐서 사용함
7. **한 번 생성된 배열의 크기 변경이 불가능**

```java
// 배열 선언 : 데이터타입 [] 변수명;
		int[] score;
		// 스택(Stack) 공간에 배열 주소를 저장할 참조데티이터타입 변수 score가 생성됨
		// => 이 때, int형의 의미는 해당 배열에 저장될 데이터타입이 정수(INT) 라는 의미
		
//		score = 10;
		
		// 배열 생성 : 변수명 = new 데이터타입[배열크기]
		score = new int[4];
		
//		System.out.println(score.length);
//		System.out.println("0번 인덱스 요소 : " + score[0]);
//		System.out.println("1번 인덱스 요소 : " + score[1]);
//		System.out.println("2번 인덱스 요소 : " + score[2]);
//		System.out.println("3번 인덱스 요소 : " + score[3]);
//		System.out.println("4번 인덱스 요소 : " + score[4]);
		
		// 5개 크기를 갖는 배열의 인덱스는 0 ~ 4 까지 존재함
		// 이 때, 4보다 큰 인덱스를 사용할 경우 아래와 같이 오류 발생 (런타임 에러)
//		System.out.println("5번 인덱스 요소 : " + score[5]);
		
//		score[0] = 80;
//		score[1] = 100;
//		score[2] = 50;
//		score[3] = 90;
//		score[4] = 77;
//		System.out.println("0번 인덱스 요소 : " + score[0]);
//		System.out.println("1번 인덱스 요소 : " + score[1]);
//		System.out.println("2번 인덱스 요소 : " + score[2]);
//		System.out.println("3번 인덱스 요소 : " + score[3]);
//		System.out.println("4번 인덱스 요소 : " + score[4]);
		
		
		System.out.println("================================================");
		
		// 반복문(for문)을 사용하여 배열의 모든 인덱스 접근
//		for(int i = 0; i < 5; i++) {
//			System.out.println(i + "번 인덱스 요소 : " + score[i]);
//		}
		
		// 위의 반복문처럼 사용해도 되지만 배열 크기가 변하면 코드도 변경되어야함.
		// 따라서, 배열의 크기를 동적으로 대응할 수 있도록 작성할 필요가 있음.
		// 즉, 조건식 부분에 배열 크기를 직접 입력하지 않고 배열명.length를 지정
//		for(int i = 0; i < score.length; i++) {
//			System.out.println(i + "번 인덱스 요소 : " + score[i]);
//		}
		
		System.out.println("-------------------------------------------------");
		
		// 배열 선언 및 생성을 동시에 수행
		// 데이터타입[] 변수명 = new 데이터타입[배열크기]
		int[] arr = new int[10];
		
		// 배열 선언, 생성, 초기화를 동시에 수행
		// 데이터타입[] 변수명 = {데이터1, 데이터2, ..., 데이터n}
		// 위 표현방식은 무조건 1줄로 표현할때만 가능
		int[] arr2 = {10, 20, 30};
		System.out.println(arr2[0]);
		System.out.println(arr2[1]);
		System.out.println(arr2[2]);
		
		int[] arr3;
//		arr3 = {10, 20, 30}; // 변수명을 먼저 선언 후에 초기화를 할 때에는 해당 표현방식 X
//		arr3 = new int[3] {10, 20, 30}; // 배열 크기는 데이터의 갯수로 자동 지정되므로 배열 크기 지정은 생략 해야한다! 
		arr3 = new int[] {10, 20, 30};
```

## 배열 사용시 주의사항
1. 배열 선언시 [] 기호를 데이터타입 뒤 또는 변수명 뒤에 붙일 수 있으나 가급적 데이터타입 뒤에 붙여서 표기하도록 해야함
	- 데이터타입[] 변수명; 또는 데이터타입 변수명[]
```java
int[] a;
int b[];
```
- 만약, 동일한 데이터타입 변수를 한꺼번에 선언하는 경우
```java
int[] c, d; // 변수 c와 d 모두 배열 변수로 선언됨
int e, f[]; // 변수 e는 기본타입변수, 변수 f만 배열 변수로 선언됨
```

2. 배열 크기는 고정이므로 크기를 확장하려면 새로운 배열을 생성하고, 기존 데이터를 새 배열에 복사 해야함
```java
int[] arr = {10, 20, 30};	// 3개의 데이터가 저장된 배열
		// 만약, 점수 40을 추가할 경우 추가(확장) 불가능
		// 따라서, 4개의 정수가 저장되는 배열을 새로 생성한 후 갖는 배열 데이터를 복사해야함
//		int[] arr2 = {10, 20, 30, 40};
		
		int[] arr2 = new int[4];
		for(int i = 0; i < arr.length; i++) {
			arr2[i] = arr[i];
		}
		arr2[3] = 40;
		
		for(int i = 0; i < arr2.length; i++) {
			System.out.println(arr2[i]);
		}
		
		System.out.println("------------------------------------------------");
		
		int aNum = 10;
		int bNum = 20;
		bNum = aNum;		//aNum의 데이터를 bNum에 저장(기존에 있던 20은 제거됨)
		
		int[] aArr = {1, 2, 3};
		int[] bArr = {4, 5, 6};
		int[] cArr = {7, 8, 9};
		
		aArr = bArr; // bArr이 가리키는 4, 5, 6 공간의 주소값을 aArr 변수에 저장
		aArr[0] = 10;
		System.out.println(bArr[0]);
		// 이 때, aArr이 가리키는 1, 2, 3 공간은 더 이상 아무도 참조하지 않는 상태가 됨!
		// 따라서, 더 이상 참조되지 않는 힙 공간의 영역은 가비지 컬렉터(G.C)에 의한 정리 대상이 됨!
		// 즉, 쓸모없는 공간은 메모리 확보를 위해 자동으로 제거됨
		
		bArr = cArr;
```		

연습문제 1.
학생 5명의 점수를 배열 score에 저장하고 각 학생 점수의 총점과 평균을 계산하여 출력
```java
 < 출력 예시 >
 1번 학생: 80점
 2번 학생: 100점
 3번 학생: 50점
 4번 학생: 90점
 5번 학생: 77점
 --------------
 총점: 397점
 평균: 79.4점
 ========================
 < 추가 항목 >
 1. 학생 이름을 저장하는 배열 names를 생성하여
    이순신, 홍길동, 강감찬, 김태희, 전지현 문자열 5개를 저장한 후
    학생 번호 대신 이름을 출력
 2. 학생 점수 중 최고점수와 최저점수를 찾아 출력
 
 < 출력 예시 >
 이순신: 80점
 홍길동: 100점
 강감찬: 50점
 김태희: 90점
 전지현: 77점
 ------------
 총점: 397점
 평균: 79.4점
 최고 점수: 100점
 최저 점수: 50점

-----------------------------------------------------------------------------------------------------
//		int sum = 0;
//		int[] arr = {80, 100, 50, 90 ,77};
//		for(int i = 0; i < arr.length; i++) {
//			sum += arr[i];
//		}
//		System.out.println(arr[4]);
//		System.out.println(sum);
//		System.out.println(sum / arr.length);

		int[] score = { 80, 100, 50, 90, 77 };
		String[] names = { "이순신", "홍길동", "강감간", "김태희", "전지현" };

		int sum = 0;

		for (int i = 0; i < score.length; i++) {
			System.out.println(names[i] + " 학생 : " + score[i] + "점");
			sum += score[i];
		}
		double avg = sum / 5.0;
		System.out.println("총점 : " + sum + "점");
		System.out.println("평균 : " + avg + "점");

		// 최고점수, 최저점수
		int maxScore = score[0], minScore = score[0];
//		for (int i = 1; i < score.length; i++) {
//			// 최고점수
//			if (maxScore < score[i]) {
//				maxScore = score[i];
//			}
//
//			// 최저점수
//			if (minScore > score[i]) {
//				minScore = score[i];
//			}
//		}
		
		for(int i = 1; i < score.length; i++) {
			maxScore = maxScore < score[i] ? score[i] : maxScore;
			minScore = minScore > score[i] ? score[i] : minScore;
		}
		System.out.println("최고 점수 : " + maxScore);
		System.out.println("최저 점수 : " + minScore);
```
연습문제 2.
```java
n개의 숫자가 입력되면, 다음과 같이 크기를 비교한 후 양식에 맞춰 출력하시오.
예를 들어, 1 2 3 2 1 이라는 숫자가 입력되면,
첫 번째 1과 나머지 2, 3, 2, 1을 비교하면 1 < 2, 1 < 3, 1 < 2, 1 = 1 이므로 < < < = 를 출력한다
두 번째 2와 나머지 1, 3, 2, 1을 비교하면 2 > 1, 2 < 3, 2 = 2, 2 > 1 이므로 > < = > 를 출력한다.
세 번째 3과 나머지 1, 2, 2, 1을 비교하면 3 > 1, 3 > 2, 3 > 2, 3 > 1 이므로 > > > > 를 출력한다.
같은 방법으로 네 번째는 > = < >, 다섯번째는 = < < < 를 출력한다.
이와 같은 방식으로 대소 비교 결과를 출력하시오.

1: < < < =
2: > < = >
3: > > > > 
4: > = < >
5: = < < < 



------------------------------------------------------------------------
		int arr[] = {1, 2, 3, 2, 1};
		
		for (int i = 0; i < arr.length; i++) {
			int target = arr[i];
			
			String result = (i+1) + ": ";

			for (int j = 0; j < arr.length; j++) {
				
				if(i == j) continue;
				
				int num = arr[j];
				
				if(target > num) { 
					result += "> ";
				} else if(target < num) {
					result += "< ";
				} else {
					result += "= ";
				}
			}
			System.out.println(result);
		}
```		
