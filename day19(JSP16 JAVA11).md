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
