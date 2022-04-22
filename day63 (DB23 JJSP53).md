# [오전수업] DB 23차

> 22차 수업 복습 실시

### INLINE VIEW
- 가상의 테이블을 구현할 수 있는 VIEW의 일종인 INLINE VIEW는 서브쿼리를 통해서 구현가능하다.
- 서브쿼리의 출력 결과가 가상의 테이블로 사용된다. 이렇게 생성된 VIEW의 경우 뷰의 이름을 활용하기 어렵기 때문에 필수적으로 table alias를 활용한다.
- 또한 서브쿼리의 출력 컬럼이 view의 컬럼으로 사용되는데 컬럼명으로 특수기호들은 사용이 안되므로 함수나 표현식의 연산 내용이 있는 경우 이를 column alias로 이름을 대체해야 한다.
- column alias를 사용한 경우 view의 컬럼명은 column alias로 적용된다.

```

SELECT a.last_name, a.salary, a.department_id, b.salavg
FROM employees a
JOIN (
    SELECT department_id, trunc(AVG(salary)) salavg
    FROM employees
    GROUP BY department_id
) b ON a.department_id = b.department_id
       AND a.salary > b.salavg;

LAST_NAME|SALARY|DEPARTMENT_ID|SALAVG|
---------+------+-------------+------+
King     | 24000|           90| 19333|
Hunold   |  9000|           60|  5760|
Ernst    |  6000|           60|  5760|
Greenberg| 12008|          100|  8601|
Faviet   |  9000|          100|  8601|
Raphaely | 11000|           30|  4150|
Weiss    |  8000|           50|  3475|
Fripp    |  8200|           50|  3475|
Kaufling |  7900|           50|  3475|
Vollman  |  6500|           50|  3475|
Mourgos  |  5800|           50|  3475|
Ladwig   |  3600|           50|  3475|
Rajs     |  3500|           50|  3475|
Russell  | 14000|           80|  8955|
Partners | 13500|           80|  8955|
…


-----------------------------------------------------------------------------------------------------------------------------------------------



TOP n 쿼리
rownum은 의사컬럼으로 테이블에 숨겨진 컬럼이다. rownum은 테이블의 기본 입력 순서값을 저장하고 있고, 데이터를 입력한 순으로 값이 매겨진다.
INLINE VIEW로 테이블을 원하는 순서로 정렬해서 출력하게 되면 이 inline view에서는 rownum의 값이 정렬된 순서로 적용된다.(테이블 원본은 바뀌지 않음)

SELECT employee_id, last_name, salary
FROM (
    SELECT *
    FROM employees
    ORDER BY salary DESC
)
WHERE rownum <= 10;


-----------------------------------------------------------------------------------------------------------------------------------------------



VIEW로 만들기
view는 구문 형태로만 만들어지는 오브젝트로 생성하더라도 자원을 거의 소모하지 않는다.
베이스 테이블의 데이터를 가져와서 출력하는 형태이므로 데이터의 중복 문제 없이 다양하게 원하는 형태로 구조를 가공하여 사용할 수 있는 장점이 있는 오브젝트 → 10단원에서 다룰 예정!!
CREATE VIEW salary_top_10 AS
    SELECT employee_id, last_name, salary
    FROM (
        SELECT *
        FROM employees
        ORDER BY salary DESC
    )
    WHERE ROWNUM <= 10;


SELECT *
FROM salary_top_10;


```


연습문제
```
p.28
SELECT employee_id, last_name, salary
FROM employees
WHERE salary >= (
    SELECT AVG(salary)
    FROM employees
)
ORDER BY salary ASC;

EMPLOYEE_ID|LAST_NAME |SALARY|
-----------+----------+------+
        203|Mavris    |  6500|
        123|Vollman   |  6500|
        165|Lee       |  6800|
        113|Popp      |  6900|
        155|Tuvault   |  7000|
        161|Sewall    |  7000|
        178|Grant     |  7000|
        164|Marvins   |  7200|
        172|Bates     |  7300|
        171|Smith     |  7400|
        154|Cambrault |  7500|
```


---

# [오후수업] JSP 53차
- qna_board_view.jsp 파일 수정 & qna_board_delete.jsp 파일 수정

```jsp
---------------------------------------------qna_board_view.jsp---------------------------------------------

<%@page import="vo.BoardDTO"%>
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%
// BoardDTO article = (BoardDTO)request.getAttribute("article"); // request 객체에 저장된 article 객체 저장
// String pageNum = request.getParameter("page"); // URL 에 전달받은 page 파라미터 저장
%>
    
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>MVC 게시판</title>
<style type="text/css">
	#articleForm {
		width: 500px;
		height: 550px;
		border: 1px solid red;
		margin: auto;
	}
	
	h2 {
		text-align: center;
	}
	
	table {
		border: 1px solid black;
		border-collapse: collapse; 
	 	width: 500px;
	}
	
	th {
		text-align: center;
	}
	
	td {
		width: 150px;
		text-align: center;
	}
	
	#basicInfoArea {
		height: 70px;
		text-align: center;
	}
	
	#articleContentArea {
		background: orange;
		margin-top: 20px;
		height: 350px;
		text-align: center;
		overflow: auto;
		white-space: pre-line;
	}
	
	#commandList {
		margin: auto;
		width: 500px;
		text-align: center;
	}
</style>
</head>
<body>
	<!-- 게시판 상세내용 보기 -->
	<section id="articleForm">
		<h2>글 상세내용 보기</h2>
		<section id="basicInfoArea">
			<table border="1">
			<tr><th width="70">제 목</th><td colspan="3" >${article.getBoard_subject() }</td></tr>
			<tr>
				<th width="70">작성자</th><td>${article.getBoard_name() }</td>
				<th width="70">작성일</th><td>${article.getBoard_date() }</td>
			</tr>
			<tr>
				<th width="70">첨부파일</th>
				<td>
					<!-- 
					파일 다운로드 링크 사용법(중복 파일명 처리로 인해 실제 파일과 업로드 한 파일명이 다를 수 있음)
					<a href="다운로드 할 실제 파일명(위치포함)" download> 화면에 보여줄 파일명 
					-->
					<a href="./upload/${article.getBoard_real_file() }" download="${article.getBoard_file() }">
						${article.getBoard_file() }
					</a>
				</td>
				<th  width="70">조회수</th>
				<td>${article.getBoard_readcount()}</td>
			</tr>
			</table>
		</section>
		<section id="articleContentArea">
		</section>
	</section>
	<section id="commandList">
		<input type="button" value="답변" onclick="location.href='BoardReplyForm.bo?board_num=${param.board_num}&page=${param.page}'">
		<input type="button" value="수정" onclick="location.href='BoardModifyForm.bo?board_num=${param.board_num}&page=${param.page}'">
		<input type="button" value="삭제" onclick="location.href='BoardDeleteForm.bo?board_num=${param.board_num}&page=${param.page}'">
		<input type="button" value="목록" onclick="location.href='BoardList.bo?page=${param.page}'">
	</section>
</body>
</html>



---------------------------------------------qna_board_delete.jsp---------------------------------------------



```






