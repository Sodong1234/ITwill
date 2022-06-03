# [오전수업] DB 35차
## 비쌍비교방식
- WHERE절에서 여러 서브쿼리로 값을 메인쿼리로 전달하는 비교방식

```sql

SELECT manager_id, department_id
FROM employees
WHERE employee_id IN ( 174, 141 );

MANAGER_ID|DEPARTMENT_ID|
----------+-------------+
       124|           50|
       149|           80|

- 174, 141 사원들의 매니저와 소속부서의 값들 중 각각 하나라도 일치한 값을 가진 행을 출력하되 171, 141의 사원들은 결과에서 제외한 연산
```
```sql
AND 연산을 통해서 하나의 WHERE절에서 여러 서브쿼리로 조건값을 메인쿼리로 전달하므로 비쌍비교방식이 된다.
SELECT employee_id,
       manager_id,
       department_id
FROM employees
WHERE manager_id IN (
    SELECT manager_id
    FROM employees
    WHERE employee_id IN ( 174, 141 )
)
      AND department_id IN (
    SELECT department_id
    FROM employees
    WHERE employee_id IN ( 174, 141 )
)
      AND employee_id NOT IN ( 174, 141 );



EMPLOYEE_ID|MANAGER_ID|DEPARTMENT_ID|
-----------+----------+-------------+
        142|       124|           50|
        143|       124|           50|
        144|       124|           50|
        196|       124|           50|
        197|       124|           50|
        198|       124|           50|
        199|       124|           50|
        175|       149|           80|
        176|       149|           80|
        177|       149|           80|
        179|       149|           80|
```

## 쌍비교방식
- 값 비교 시 컬럼과 조건값을 쌍으로 묶어서 비교하는 방식
- 예제 1 
```sql
SELECT manager_id,
       department_id
FROM employees
WHERE employee_id IN ( 174, 199 );

MANAGER_ID|DEPARTMENT_ID|
----------+-------------+
       149|           80|
       124|           50|



- manager_id, department_id컬럼을 쌍으로 묶어서 동일한 구성의 쌍의 조건값과 비교하여 조건을 만족하는 행을 출력

SELECT employee_id,
       manager_id,
       department_id
FROM employees
WHERE ( manager_id, department_id ) IN (
    SELECT manager_id, department_id
    FROM employees
    WHERE employee_id IN ( 174, 199 )
)
      AND employee_id NOT IN ( 174, 199 );



EMPLOYEE_ID|MANAGER_ID|DEPARTMENT_ID|
-----------+----------+-------------+
        141|       124|           50|
        142|       124|           50|
        143|       124|           50|
        144|       124|           50|
        196|       124|           50|
        197|       124|           50|
        198|       124|           50|
        175|       149|           80|
        176|       149|           80|
        177|       149|           80|
        179|       149|           80|
```

- 예제 2
```sql
- 각 부서별 최소급여값과 부서번호 출력
SELECT MIN(salary),
       department_id
FROM employees
GROUP BY department_id;


MIN(SALARY)|DEPARTMENT_ID|
-----------+-------------+
       6900|          100|
       2500|           30|
       7000|             |
      17000|           90|
       6000|           20|
      10000|           70|
       8300|          110|
       2100|           50|
       6100|           80|
       6500|           40|
       4200|           60|
       4400|           10|





- 사원들 중 각 부서의 최소급여, 부서번호의 쌍의 값과 일치한 값을 가지는 사원을 출력
SELECT first_name,
       department_id,
       salary
FROM employees
WHERE ( salary, department_id ) IN (
    SELECT MIN(salary), department_id
    FROM employees
    GROUP BY department_id
)
ORDER BY department_id;


FIRST_NAME|DEPARTMENT_ID|SALARY|
----------+-------------+------+
Jennifer  |           10|  4400|
Pat       |           20|  6000|
Karen     |           30|  2500|
Susan     |           40|  6500|
TJ        |           50|  2100|
Diana     |           60|  4200|
Hermann   |           70| 10000|
Sundita   |           80|  6100|
Neena     |           90| 17000|
Lex       |           90| 17000|
Luis      |          100|  6900|
William   |          110|  8300|

---
```

- 연습문제 풀이

```sql
p.71_1
SELECT employee_id, last_name
FROM employees
WHERE department_id IN (
	SELECT department_id
	FROM employees
	WHERE last_name LIKE '%u%'
);

EMPLOYEE_ID|LAST_NAME  |
-----------+-----------+
        103|Hunold     |
        104|Ernst      |
        105|Austin     |
        106|Pataballa  |
        107|Lorentz    |
        120|Weiss      |
        121|Fripp      |
        122|Kaufling   |
        123|Vollman    |
…


p.71_2
SELECT employee_id, last_name, salary
FROM employees
WHERE salary > (
	SELECT AVG(salary)
	FROM employees
)
AND department_id IN (
	SELECT department_id
	FROM employees
	WHERE last_name LIKE '%u%'
);

EMPLOYEE_ID|LAST_NAME |SALARY|
-----------+----------+------+
        103|Hunold    |  9000|
        120|Weiss     |  8000|
        121|Fripp     |  8200|
        122|Kaufling  |  7900|
        123|Vollman   |  6500|
        145|Russell   | 14000|
        146|Partners  | 13500|
        147|Errazuriz | 12000|
        148|Cambrault | 11000|
```
# [오후수업] JSP 77차

- controller
```java
-----------------------------------BoardController.java-----------------------------------
```

- service
```java
-----------------------------------BoardService.java-----------------------------------
```

- mapper
```java
-----------------------------------BoardMapper.java-----------------------------------
```
```xml
-----------------------------------BoardMapper.xml-----------------------------------
```

```jsp
-----------------------------------qna_board_list.jsp-----------------------------------
<%@page import="java.util.ArrayList"%>
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>MVC 게시판</title>
<style type="text/css">
	#listForm {
		width: 1024px;
		max-height: 610px;
		margin: auto;
	}
	
	h2 {
		text-align: center;
	}
	
	table {
		margin: auto;
		width: 1024px;
	}
	
	#tr_top {
		background: orange;
		text-align: center;
	}
	
	table td {
		text-align: center;
	}
	
	#subject {
		text-align: left;
		padding-left: 20px;
	}
	
	#pageList {
		margin: auto;
		width: 1024px;
		text-align: center;
	}
	
	#emptyArea {
		margin: auto;
		width: 1024px;
		text-align: center;
	}
	
	#buttonArea {
		margin: auto;
		width: 1024px;
		text-align: right;
	}
	
	a {
		text-decoration: none;
	}
</style>
</head>
<body>
	<!-- JSTL 의 c:set 태그를 사용하여 PageInfo 객체의 값들을 변수에 저장 -->
	<c:set var="pageNum" value="${pageInfo.getPageNum() }" />
	<c:set var="maxPage" value="${pageInfo.getMaxPage() }" />
	<c:set var="startPage" value="${pageInfo.getStartPage() }" />
	<c:set var="endPage" value="${pageInfo.getEndPage() }" />
	<c:set var="listCount" value="${pageInfo.getListCount() }" />
	

	<!-- 게시판 리스트 -->
	<section id="listForm">
	<h2>게시판 글 목록</h2>
	<table>
		<tr id="tr_top">
			<td width="100px">번호</td>
			<td>제목</td>
			<td width="150px">작성자</td>
			<td width="150px">날짜</td>
			<td width="100px">조회수</td>
		</tr>
		<!-- JSTL의 forEach 태그를 사용하여 articleList 에서 BoardDTO 객체를 꺼내서 내용 출력 -->
		<!-- 단, 게시물 목록이 하나라도 존재할 경우에만 출력 c:if 태그 사용 -->
		<c:if test="${not empty boardList && pageInfo.getListCount() > 0}">
			<c:forEach var='board' items="${boardList }">
				<tr>
					<td>${board.getBoard_num() }</td>
					<td id="subject">
						<a href="detail?board_num=${board.getBoard_num() }&pageNum=${pageNum }">
								<!-- 답글에 대한 들여쓰기(공백 추가) 작업 처리 -->
							<c:forEach var="i" begin="0" end="${board.getBoard_re_lev() }">
								&nbsp;
							</c:forEach>
							${board.getBoard_subject() }
						</a>
					</td>
					<td>${board.getBoard_name() }</td>
					<td>${board.getBoard_date() }</td>
					<td>${board.getBoard_readcount() }</td>
				</tr>
			</c:forEach>
		</c:if>
	</table>
	</section>
	<section id="buttonArea">
		<form action="list" method="post">
			<input type="hidden" name="pageNum" value="${pageNum}">
			<select name="searchType">
				<option value="subject">제목</option>
				<option value="content">내용</option>
				<option value="subject_content">제목&내용</option>
				<option value="name">작성자</option>			
			</select>
			<input type="text" name="keyword">
			<input type="submit" value="검색"/>
		</form>
		<input type="button" value="글쓰기" onclick="location.href='write'" />
	</section>
	<section id="pageList">
		<!-- 
		현재 페이지 번호(pageNum)가 1보다 클 경우에만 Prev 링크 동작
		=> 클릭 시 notice.jsp 로 이동하면서 
		   현재 페이지 번호(pageNum) - 1 값을 page 파라미터로 전달
		-->
		<c:choose>
			<c:when test="${pageNum > 1}">
				<input type="button" value="이전" onclick="location.href='list?pageNum=${pageNum - 1}'">
			</c:when>
			<c:otherwise>
				<input type="button" value="이전">
			</c:otherwise>
		</c:choose>

		<!-- 페이지 번호 목록은 시작 페이지(startPage)부터 끝 페이지(endPage) 까지 표시 -->
		<c:forEach var="i" begin="${startPage }" end="${endPage }">
			<!-- 단, 현재 페이지 번호는 링크 없이 표시 -->
			<c:choose>
				<c:when test="${pageNum eq i}">
					${i }
				</c:when>
				<c:otherwise>
					<a href="list.bo?pageNum=${i }">${i }</a>
				</c:otherwise>
			</c:choose>
		</c:forEach>

		<!-- 현재 페이지 번호(pageNum)가 총 페이지 수보다 작을 때만 [다음] 링크 동작 -->
		<c:choose>
			<c:when test="${pageNum < maxPage}">
				<input type="button" value="다음" onclick="location.href='list?pageNum=${pageNum + 1}'">
			</c:when>
			<c:otherwise>
				<input type="button" value="다음">
			</c:otherwise>
		</c:choose>

	</section>
</body>
</html>


```
