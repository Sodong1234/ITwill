# [오전수업] JSP

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
