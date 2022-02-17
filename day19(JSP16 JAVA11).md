# [오전수업] JSP 16차
> 15차 수업때 실시한 form 파트 보충분을 추가하였음

## 체크박스 활용
```javascript
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<script type="text/javascript">
function printCheckbox() {
	// 체크박스에서 name 속성이 같은 요소들은 모두 하나의 배열로 관리됨
	// => 이 때, 배열의 이름은 name 속성의 이름을 사용
	// => 배열에 저장된 실제 데이터는 value 속성의 값을 사용
	// => 체크박스 체크 여부는 checked 속성에 접근(true : 체크, false : 미체크)
	
	// 체크박스(hobby) 중 첫 번째 요소(게임)에 접근하여 value 속성값과 checked 속성값 출력
// 	alert(document.fr.hobby[0].value + " : " + document.fr.hobby[0].checked);
	
	
	// -------------------------------------------------------------------------------------------
	// 취미(게임, 등산, 노래)가 아무것도 체크 되지 않았을 경우를 판별하여
	// "취미 하나 이상 선택 필수!" 메세지 출력하기
	
// 	if(document.fr.hobby[0].checked == false) {
		// 취미 첫번째 항목 체크 상태가 false 와 같은지 여부가 true인지 판별 
		// => 말이 이상하게 됨. if문 자체가 ture&false 를 판별하는 문법.
		
	if(!document.fr.hobby[0].checked && !document.fr.hobby[1].checked && !document.fr.hobby[2].checked) { 
// 		not 연산자 !를 붙임으로써 checked == false 와 동일한 코드가 만들어짐.
// 		취미 첫 번째 항목 체크 상태가 false 와 같은지 판별
		alert("취미 하나 이상 선택 필수!");
		return;
	}
	
	// -------------------------------------------------------------------------------------------
	// 배열 요소를 반복문으로 처리 가능
// 	for(var i = 0; i <= document.fr.hobby.length; i++) {
// 		alert(document.fr.hobby[i].value + " : " + document.fr.hobby[i].checked);
// 	}
	
	// 취미 체크박스 요소 배열 접근 코드가 반복 => 반복 제거를 위해 변수 활용
// 	var arrHobby = document.fr.hobby;
// 		for(var i = 0; i <= arrHobby.length; i++) {
// 		alert(arrHobby[i].value + " : " + arrHobby[i].checked);
// 	}

	// 일반 for문 대신 for...XXX 문 사용
	var arrHobby = document.fr.hobby;
	for(var hobby of arrHobby) { // 우변 arrHobby 배열 요소 하나씩 꺼내서 좌변의 hobby 에 저장
		alert(hobby.value + " : " + hobby.checked) 
	}
		
	
	
}


function printRadio() {
	var gender = document.fr.gender;
	// 라디오버튼도 체크박스와 동일하게 취급됨
	// 하나도 선택되지 않은 경우 alert("성별 선택 필수!"); 출력
	
// 	if(!gender[0].checked && !gender[1].checked) {
// 		alert("성별 선택 필수!")		
// 		return;
// 	}
	
	
	
		// 만약, 선택된 항목이 있을 경우 해당 항목값 출력(ex. 성별 : 여)

// 		if(gender[0].checked) { // "남" 항목 선택 시
// 			alert(gender[0].value + " : 선택됨");
// 		} else { //여 항목 선택 시
// 			alert(gender[1].value + " : 선택됨");
// 		}
		
		
		
		// 위의 두 가지(하나라도 체크 or 모두 미체크)를 결합할 경우
		if(gender[0].checked) { // "남" 항목 선택 시
			alert(gender[0].value + " : 선택됨");
		} else if(gender[1].checked) { //여 항목 선택 시
			alert(gender[1].value + " : 선택됨");
		} else {
			alert("성별 선택 필수!")		
			return;
			}

		}
	


		function checkAll() {
			// "전체선택" 체크 박스 체크 상태에 따라 취미(게임, 등산, 노래) 항목 체크 상태를 일괄 변경
			// => "전체선택" 체크 시 : 게임, 등산, 노래를 모두 체크
			//	  "전체선택" 체크 해제 시 : 게임, 등산, 노래를 모두 체크 해제
// 			alert(document.fr.hobbyAll.checked) // 단일 체크박스는 배열 미사용
			
			var hobbyAll = document.fr.hobbyAll;
			
			
			if(hobbyAll.checked) {
				alert("전체선택 체크됨!")
				// 취미 항목의 모든 체크 상태(checked 속성값)를 체크상태(true)로 변경
				document.fr.hobby[0].checked = true;
				document.fr.hobby[1].checked = true;
				document.fr.hobby[2].checked = true;
			} else {
				alert("전체선택 체크해제됨!")
				// 취미 항목의 모든 체크 상태(checked 속성값)를 체크해제상태(false)로 변경
				document.fr.hobby[0].checked = false;
				document.fr.hobby[1].checked = false;
				document.fr.hobby[2].checked = false;
			}
				}
			
			
</script>
</head>
<body>
	<form action="test14-2.html" name="fr">
		<!-- 
	체크박스, 라디오버튼 등 묶음으로 관리하는 요소들은 name 속성값을 같게 설정할 경우 자동으로 그룹화가 이루어짐
	=> 체크박스의 경우 복수개의 체크박스 항목 데이터가 배열로 관리되도록 변함
	=> 라디오버튼의 경우 복수개의 항목 중 하나만 선택(단일 선택) 되도록 변함 -->
		취미 : <input type="checkbox" name="hobby" value="게임" checked="checked">게임
		<input type="checkbox" name="hobby" value="등산">등산 <input
			type="checkbox" name="hobby" value="노래">노래 <input
			type="checkbox" name="hobbyAll" value="전체선택" onclick="checkAll()">전체선택
		<input type="button" value="취미 체크완료" onclick="printCheckbox()">
		<br> 성별 : <input type="radio" name="gender" value="남">남 <input
			type="radio" name="gender" value="여">여 <input type="button"
			value="성별 체크완료" onclick="printRadio()"> <br> <input
			type="submit" value="가입">
	</form>
</body>
</html>
```
## Select 활용
```javascript
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<script type="text/javascript">
	function check() {
		// 셀렉트박스 선택 항목에 접근하려면 셀렉트박스의 value 속성 사용
		alert(document.fr.fruits.value);
	}

	function check2() {
		// 현재 셀렉트박스 항목이 "선택하세요" 일 경우 다른 항목을 선택하도록 오류 메세지 출력
		// 1. 셀렉트박스 항목의 value 속성값이 "" 인지 판별"
		// 	if(document.fr.fruits4.value == "") {
		// 		alert("과일 선택 필수!");
		// 	}

		// 2. 셀렉트박스의 options 배열을 사용하여 접근(첫번째 항목 인덱스 0부터 시작)
		// => "선택하세요" 항목은 0번 인덱스의 selected 속성값이 true 이면 아무것도 선택 안 됨
		if (document.fr.fruits4.options[0].selected) { // 첫번째 항목("선택하세요") 선택된 경우
			alert("과일 선택 필수!");
		}

	}

	// function itemSelected() {
	// 		alert("선택항목 : " + document.fr.fruits5.value)
	// }

	function itemSelected(selectedValue) {
// 		alert("선택항목 : " + selectedValue);
		// "선택하세요" 항목일때는 아무 작업도 수행하지 않고, 나머지 항목만 출력할 경우
		if (selectedValue != "") {
// 			alert("선택항목 : " + selectedValue)
			// 텍스트박스("fruit")에 선택 항목 value 속성값 출력
			document.fr.fruit.value = selectedValue;
		} else {
			// "선택하세요 항목일 때 텍스트박스 텍스트 초기화(삭제)
		}
	}
	
	
</script>
</head>
<body>
	<h1>test15.html</h1>
	<form action="" name="fr">
		<select name="fruits">
			<option value="Apple">사과</option>
			<option value="Strawberry">딸기</option>
			<option value="Banana">바나나</option>
		</select> <input type="button" value="확인" onclick="check()">

		<hr>

		<select name="fruits2">
			<option value="Apple">사과</option>
			<option value="Strawberry" selected="selected">딸기</option>
			<!-- 기본값으로 선택 -->
			<option value="Banana" disabled="disabled">바나나</option>
			<!-- 비활성화(= 선택 불가) -->
		</select>

		<hr>

		<!-- 셀렉트박스 size 속성을 지정 시 리스트박스 형태로 변경됨 -->
		<select name="fruits3" size="2">
			<!-- 목록 2개까지 표시 후 나머지는 스크롤링 -->
			<option value="Apple">사과</option>
			<option value="Strawberry">딸기</option>
			<!-- 기본값으로 선택 -->
			<option value="Banana">바나나</option>
			<!-- 비활성화(= 선택 불가) -->
		</select>

		<hr>

		<select name="fruits4">
			<!-- 셀렉트박스 항목 선택이 필수가 아닐 경우 또는 항목을 반드시 확인하고 선택해야하는 경우 
			"" 값을 갖는 option 태그 추가 
			-->
			<option value="">선택하세요</option>
			<option value="Apple">사과</option>
			<option value="Strawberry">딸기</option>
			<option value="Banana">바나나</option>
		</select>
		<!-- 확인 버튼 클릭 시 check() 함수 호출 -->
		<input type="button" value="확인" onclick="check2()">

		<hr>

		<select name="fruits5" onchange="itemSelected(this.value)">
			<!-- 셀렉트박스 항목 변경을 감지하는 이벤트 : onchange -->
			<!-- 함수 호출 시 this.value 지정하면 선택된 항목의 value 속성값을 파라미터로 전달 -->
			<option value="">선택하세요</option>
			<option value="Apple">사과</option>
			<option value="Strawberry">딸기</option>
			<option value="Banana">바나나</option>
		</select>

	</form>
</body>
</html>
```

---
# [오후수업] JAVA 11차
> 저번 차수에 이어 복습문제 이어서 실시
8. 마방진 만들기
```java
import java.util.Scanner;

public class test1 {

	public static void main(String[] args) {
//		마방진(magic square) : 가로, 세로, 대각선의 합이 같은 사각형. 
//		홀수 n을 입력으로 받아 n*n 마방진 만들기
//		
//		구현 방법 :
//		1. 시작은 첫 행, 한 가운데 열에 1을 둔다
//		2. 행을 감소, 열을 증가하면서 순차적으로 수를 넣어감
//		3. 행은 감소하므로 첫행보다 작아지는 경우에는 마지막으로 넘어감
//		4. 열은 증가하므로 마지막 열보다 커지는 경우에는 첫 열로 넘아감
//		5. 넣은 수가 n의 배수이면 행만 증가. 열은 변화없음
//			
//		8 1 6
//		3 5 7
//		4 9 2
		Scanner sc = new Scanner(System.in);
		int n = sc.nextInt();
//		
//		int row = 0;
//		int col = n/2;
//		
//		int[][] arr = new int[n][n];
//		
//		for(int i = 1; i < n*n; i++) {
//			arr[row][col] = i;
//			if(i % n == 0) { // n의 배수이면 행만 증가
//				row++;
//			} else {
//				row--;
//				col++;
//			}
//			if(row < 0) {
//				row = n - 1;
//			}
//			
//			if(col > n - 1) {
//				col = 0;
//			}
//		}
//		


//		// 출력
//		for(int i = 0; i < arr.length; i++) {
//			for(int j = 0; j < arr[i].length; j++) {
//				System.out.print(arr[i][j] + " ");
//			}
//			System.out.println();
//		}

		int[][] arr = makeMagicSquare(n);
//		 출력
//		for (int i = 0; i < arr.length; i++) {
//			for (int j = 0; j < arr[i].length; j++) {
//				System.out.print(arr[i][j] + " ");
//			}
//			System.out.println();

			System.out.println("=========================================================");

			// 향상된 for문
			for (int[] arr2 : arr) {
				for (int num : arr2) {
					System.out.print(num + " ");
				}
				System.out.println();
			}
		
	} // main() 메서드 끝
		// 정수 n을 전달받아 2차원배열 마방진을 생성해서 리턴하는 makeMagicSquare 메서드 작성

	public static int[][] makeMagicSquare(int n) {
		int row = 0;
		int col = n / 2;

		int[][] arr = new int[n][n];

		for (int i = 1; i < n * n; i++) {
			arr[row][col] = i;
			if (i % n == 0) {
				row++;
			} else {
				row--;
				col++;
			}
			if (row < 0) {
				row = n - 1;
			}
			if (col > n - 1) {
				col = 0;
			}
		}
		return arr;
	}
} // test1 클래스 끝
```
9. 홀수 짝수 계산식 
```java
// 정수 2개가 입력되면 아래와 같이 출력
// 입력) 5 7
// 출력) "홀수 + 홀수 = 짝수"
//
// 입력) 2 7
// 출력) "짝수 + 홀수 = 홀수"

import java.util.Scanner;

public class Test2 {

	public static void main(String[] args) {
		
		// 1.
		String result = "";
//		if(num1 % 2 == 1 && num2 % 2 == 1) { 	// 홀홀짝
//			result = "홀수 + 홀수 = 짝수";
//		} else if(num1 % 2 == 1 && num2 % 2 == 0) {		// 홀짝홀 
//			result = "홀수 + 짝수 = 홀수";
//		} else if(num1 % 2 == 0 && num2 % 2 == 1) {		// 짝홀홀
//			result = "짝수 + 홀수 = 홀수";
//		} else if(num1 % 2 == 0 && num2 % 2 == 0) {		// 짝짝짝
//			result = "짝수 + 짝수 = 짝수";
//		}
		
		
		// 2. 
//		if(num1 % 2 == 1) { // 홀수
//			result += "홀수 + ";
//			
//			if(num2 % 2 == 1 ) { // 홀수
//				result += "홀수 = 짝수";
//			} else { // 짝수
//				result += "짝수 = 홀수";
//			}
//			
//		} else { // 짝수
//			result += "짝수 + ";
//			
//			if(num2 % 2 == 1 ) { // 홀수
//				result += "홀수 = 홀수";
//			} else { // 짝수
//				result += "짝수 = 짝수";
//			}
//		}
//		System.out.println(result);
		
		
		
		
		// 3.
//		result += (num1 % 2 == 0) ? "짝수 + " : "홀수 + ";
//		result += (num2 % 2 == 0) ? "짝수 + " : "홀수 = ";
//		result += ((num1+num2)%2 == 0) ? "짝수" : "홀수";
//		
//		System.out.println(result);
		
		
		
		
		// 4.
		System.out.println(method(num1) + " + " + method(num2) + " = " + method(num1 + num2));
		
		
		
	} //main() 메서드 끝
		// 4.
	public static String method(int n) {
		return (n % 2 == 0) ? "짝수" : "홀수";
		
	}

} // Test2 클래스 끝
```
10. 격자판 문제
```java
4
6	2
9	3	1
19	10	7	?
위와 같은 규칙을 좀 더 일반화하여 각 행의 제일 첫 번째 숫자들만 주어지면 N 크기의 모든 
격자판 정보를 출력하는 프로그램을 작성하시오.

입력예시)
4
4
6
9
19
출력예시)
4
6	2
9	3	1
19	10	7	?
