# [오전수업] DB 25차
> 24차 수업 복습 실시

![캡처](https://user-images.githubusercontent.com/95197594/165881455-b2d2e709-3205-47ec-9fe0-7394a30e2792.PNG)

```

SQL> INSERT INTO departments
  2  VALUES (&dept_id, '&dept_name', &mgr_id, &loc_id);

Enter value for dept_id: 310
Enter value for dept_name: Lunch
Enter value for mgr_id: 100
Enter value for loc_id: 1700
old   2: VALUES (&dept_id, '&dept_name', &mgr_id, &loc_id)
new   2: VALUES (310, 'Lunch', 100, 1700)

1 row created.
```




> 연습문제를 통하여 치환변수, 스크립트파일 생성 및 실행, INSERT & UPDATE & DELETE 구문, commit, SAVEPOINT 및 ROLLBACK 연습 실시


![1](https://user-images.githubusercontent.com/95197594/166170394-2eb0c56b-ac28-45c0-aeca-396f1ea1c28d.PNG)
```
1.
SQL> @/home/oracle/labs_12c/sql1/lab_08_01.sql
Table created.

2.
SQL> desc my_employee
 Name                                      Null?    Type
 ----------------------------------------- -------- ----------------------------
 ID                                        NOT NULL NUMBER(4)
 LAST_NAME                                          VARCHAR2(25)
 FIRST_NAME                                         VARCHAR2(25)
 USERID                                             VARCHAR2(8)
 SALARY                                             NUMBER(9,2)
```
---

![2](https://user-images.githubusercontent.com/95197594/166170396-c0510b59-6284-4df2-a159-eff8004e4298.PNG)
```
3.
SQL> INSERT INTO my_employee
  2  VALUES (1, 'Patel', 'Ralph', 'rpatel', 895);
1 row created.

4.
SQL> INSERT INTO my_employee
  2  VALUES (2, 'Dancs', 'Betty', 'bdancs', 860);
1 row created.

5.
SQL> SELECT * FROM my_employee;

        ID LAST_NAME                 FIRST_NAME                USERID       SALARY
---------- ------------------------- ------------------------- -------- ----------
         1 Patel                     Ralph                     rpatel          895
         2 Dancs                     Betty                     bdancs          860
```

---

![3](https://user-images.githubusercontent.com/95197594/166170398-0f47c7d7-eaf0-44b0-b8e2-5ef04bb3febf.PNG)
```
6.
SQL> INSERT INTO my_employee
  2  VALUES (&id, '&last_name', '&first_name', '&userid', &salary);
Enter value for id: 3
Enter value for last_name: Biri
Enter value for first_name: Ben
Enter value for userid: bbiri
Enter value for salary: 1100
old   2: VALUES (&id, '&last_name', '&first_name', '&userid', &salary)
new   2: VALUES (3, 'Biri', 'Ben', 'bbiri', 1100)
1 row created.

7.
SQL> save /home/oracle/load_emp.sql
Created file /home/oracle/load_emp.sql

SQL> @/home/oracle/load_emp.sql
Enter value for id: 4
Enter value for last_name: Newman
Enter value for first_name: Chad
Enter value for userid: cnewman
Enter value for salary: 750
old   2: VALUES (&id, '&last_name', '&first_name', '&userid', &salary)
new   2: VALUES (4, 'Newman', 'Chad', 'cnewman', 750)
1 row created.

8.
SQL> SELECT * FROM my_employee;

        ID LAST_NAME                 FIRST_NAME                USERID       SALARY
---------- ------------------------- ------------------------- -------- ----------
         1 Patel                     Ralph                     rpatel          895
         2 Dancs                     Betty                     bdancs          860
         3 Biri                      Ben                       bbiri          1100
         4 Newman                    Chad                      cnewman         750
```

---

![4](https://user-images.githubusercontent.com/95197594/166170399-dd34dd40-ab0b-462f-9646-039a079230b1.PNG)
```
9.
SQL> COMMIT;
Commit complete.

10.
SQL> UPDATE my_employee
  2  SET last_name = 'Drexler'
  3  WHERE id = 3;
1 row updated.

11.
SQL> UPDATE my_employee
  2  SET salary = 1000
  3  WHERE salary < 900;
3 rows updated.

12.
SQL> SELECT * FROM my_employee;

        ID LAST_NAME                 FIRST_NAME                USERID       SALARY
---------- ------------------------- ------------------------- -------- ----------
         1 Patel                     Ralph                     rpatel         1000
         2 Dancs                     Betty                     bdancs         1000
         3 Drexler                   Ben                       bbiri          1100
         4 Newman                    Chad                      cnewman        1000
```

---

![5](https://user-images.githubusercontent.com/95197594/166170401-cb28a799-f81b-4c37-8b91-a63898bdeda7.PNG)
```
13.
SQL> DELETE FROM my_employee
  2  WHERE id = 2;
1 row deleted.

14.
SQL> SELECT * FROM my_employee;

        ID LAST_NAME                 FIRST_NAME                USERID       SALARY
---------- ------------------------- ------------------------- -------- ----------
         1 Patel                     Ralph                     rpatel         1000
         3 Drexler                   Ben                       bbiri          1100
         4 Newman                    Chad                      cnewman        1000

15.
SQL> commit;
Commit complete.

16.
SQL> @/home/oracle/load_emp.sql
Enter value for id: 5
Enter value for last_name: Ropeburn
Enter value for first_name: Audrey
Enter value for userid: aropebur
Enter value for salary: 1550
old   2: VALUES (&id, '&last_name', '&first_name', '&userid', &salary)
new   2: VALUES (5, 'Ropeburn', 'Audrey', 'aropebur', 1550)
1 row created.
```

---

![6](https://user-images.githubusercontent.com/95197594/166170402-ee2669ec-ff86-46e8-b59d-5a082d5606f1.PNG)
```
17.
SQL> SELECT * FROM my_employee;

        ID LAST_NAME                 FIRST_NAME                USERID       SALARY
---------- ------------------------- ------------------------- -------- ----------
         1 Patel                     Ralph                     rpatel         1000
         3 Drexler                   Ben                       bbiri          1100
         4 Newman                    Chad                      cnewman        1000
         5 Ropeburn                  Audrey                    aropebur       1550

18.
SQL> SAVEPOINT a;
Savepoint created.

19.
SQL> DELETE FROM my_employee;
4 rows deleted.

20.
SQL> SELECT * FROM my_employee;
no rows selected
```

---

![7](https://user-images.githubusercontent.com/95197594/166170403-24c45c8c-6eac-41e9-880f-d5ab63cee26b.PNG)
```
21.
SQL> ROLLBACK TO SAVEPOINT a;
Rollback complete.

22.
SQL> SELECT * FROM my_employee;

        ID LAST_NAME                 FIRST_NAME                USERID       SALARY
---------- ------------------------- ------------------------- -------- ----------
         1 Patel                     Ralph                     rpatel         1000
         3 Drexler                   Ben                       bbiri          1100
         4 Newman                    Chad                      cnewman        1000
         5 Ropeburn                  Audrey                    aropebur       1550

23.
SQL> COMMIT;
Commit complete.
```

---

# [오후수업] JSP 57차

> 조별프로젝트 팀원 발표 및 팀원간 회의로 간단한 주제 구상

## AJAX(에이잭스)
- Asyncronous Javascript And XML(비동기식 자바스크립트)
- 웹페이지의 갱신 없이 화면 상의 요소를 다룰 수 있는 기술(구현 방식)

```jsp

--------------------------------------------test1.jsp--------------------------------------------

<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<!-- AJAX 사용을 위해서는 jQuery 라이브러리 필수! -->
<script src="../js/jquery-3.6.0.js"></script>
<script type="text/javascript">
	// document 객체에 대한 ready 이벤트
	$(function() {
		// btnOk 버튼 클릭 시 동작하는 이벤트에 익명 함수 실행
		$("#btnOk").on("click", function() {
			// 폼을 지정하여 serialize() 함수 호출 시 해당 폼의 데이터 직렬화(= 모두 내보내기)
// 			let sendData = $("form").serialize();
			
			$.ajax({
				// 속성명: "값" 형태로 지정
				type: "GET", // 요청 방식
				url: "test1_result.jsp", // 요청 URL
				data: {
					// 복수개의 파라미터 직접 전달 시
// 					id: "admin",
// 					passwd: "1234"
					id: $("#id").val(),
					passwd: $("#passwd").val()
				},
// 				data: sendData,	// $("form").serialize() 함수 사용 시 가져온 데이터 전달
				dataType: "text", // 전송하는 데이터의 타입
				success: function(msg) { // 요청 성공 시 수행할 작업 정의
					// => 요청 성공 시 응답 페이지의 응답 내용이 익명 함수의 파라미터로 전달되며
					// 	  해당 파라미터 내의 데이터를 사용하여 응답 페이지에 접근 가능
					$("#resultArea").html(msg); // 응답 내용을 #resultArea 에 출력
				}
			});
		});
	});
</script>

</head>
<body>
	<h1>AJAX - test.jsp</h1>
	<form>
		<table border="1">
			<tr>
				<td>아이디</td><td><input type="text" name="id" id="id"></td>
			</tr>
			<tr>	
				<td>패스워드</td><td><input type="password" name="passwd" id="passwd"></td>
			</tr>
			<tr>
				<td colspan="2"><input type="button" value="확인" id="btnOk">				
			</tr>
		</table>	
	</form>
	<div id="resultArea"></div>
</body>
</html>

--------------------------------------------test1_result.jsp--------------------------------------------

<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	${param.id } 님 반갑습니다. <br>
	입력하신 패스워드는 ${param.passwd } 입니다.
</body>
</html>

```

```jsp
--------------------------------------------test2.jsp--------------------------------------------

<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<script src="../js/jquery-3.6.0.js"></script>
<script type="text/javascript">
	$(function() {
		$("#btnOk").on("click", function() {
			// 폼 파라미터를 test2_result.jsp 로 전송
			let sendData = $("form").serialize();
			
			$.ajax({
				type: "GET",
				url: "test2_result.jsp",
// 				data:  {
// 					id: $("#id").val(),
// 					passwd: $("#passwd").val()
// 				},
				data: sendData,
				dataType: "text",
				success: function(msg) {
					$("#resultArea").html(msg);
				}
			});
		});
	});
</script>
</head>
<body>
	<h1>AJAX - test2.jsp</h1>
		<form>
			<table border="1">
				<tr>
					<td>아이디</td><td><input type="text" name="id" id="id"></td>
				</tr>
				<tr>	
					<td>패스워드</td><td><input type="password" name="passwd" id="passwd"></td>
				</tr>
				<tr>
					<td colspan="2"><input type="button" value="확인" id="btnOk">				
				</tr>
			</table>	
		</form>
		<div id="resultArea"></div>
</body>
</html>

--------------------------------------------test2_result.jsp--------------------------------------------

<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>    
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<c:if test="${param.id eq 'admin' }">
		<h3>관리자님, 반갑습니다!</h3>
	</c:if>
	<c:if test="${param.id != 'admin' }">
		<h3>${param.id }님, 반갑습니다!</h3>
		
	</c:if>
</body>
</html>

```
