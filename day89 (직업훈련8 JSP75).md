# [오전수업] 직업훈련 8차

---

# [오후수업] JSP 75차

- vo
```java
---------------------------------------------PageInfo.java---------------------------------------------
package com.itwillbs.mvc_board.vo;

// 페이징 처리 관련 정보를 저장하는 클래스
public class PageInfo {
	private int pageNum; // 현재 페이지 번호
	private int maxPage; // 최대 페이지 수
	private int startPage; // 시작 페이지 번호
	private int endPage; // 끝 페이지 번호
	private int listCount; // 총 게시물 수
	private int startRow; // 조회 시작 행 번호
	private int listLimit; // 페이지 당 게시물 수
	


	public int getPageNum() {
		return pageNum;
	}

	public void setPageNum(int pageNum) {
		this.pageNum = pageNum;
	}

	public int getMaxPage() {
		return maxPage;
	}

	public void setMaxPage(int maxPage) {
		this.maxPage = maxPage;
	}

	public int getStartPage() {
		return startPage;
	}

	public void setStartPage(int startPage) {
		this.startPage = startPage;
	}

	public int getEndPage() {
		return endPage;
	}

	public void setEndPage(int endPage) {
		this.endPage = endPage;
	}

	public int getListCount() {
		return listCount;
	}

	public void setListCount(int listCount) {
		this.listCount = listCount;
	}

	public int getStartRow() {
		return startRow;
	}

	public void setStartRow(int startRow) {
		this.startRow = startRow;
	}

	public int getListLimit() {
		return listLimit;
	}

	public void setListLimit(int listLimit) {
		this.listLimit = listLimit;
	}
	
}

```
- controller 
```java
---------------------------------------------BoardController.java---------------------------------------------
package com.itwillbs.mvc_board.controller;

import java.util.List;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.ModelAttribute;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;
import org.springframework.web.bind.annotation.RequestParam;

import com.itwillbs.mvc_board.service.BoardService;
import com.itwillbs.mvc_board.vo.BoardVO;
import com.itwillbs.mvc_board.vo.PageInfo;

@Controller
public class BoardController {
	@Autowired
	private BoardService service;

	// 1. 글쓰기 폼 - GET
	@RequestMapping(value = "/board/write", method = RequestMethod.GET)
	public String write() {
		return "board/qna_board_write";
	}

	// 2. 글쓰기 비즈니스 로직 - POST
	// => 게시물 정보를 전달받아 저장하기 위해 BoardVO 타입 파라미터 선언
	@RequestMapping(value = "/board/write", method = RequestMethod.POST)
	public String writePost(@ModelAttribute BoardVO board) {
		// BoardService 객체의 writeBoard() 메서드 호출
		// => 파라미터 : BoardVO 객체, 리턴타입 : void
		service.writeBoard(board);

		// board/list 요청 - Redirect 방식
		return "redirect:/board/list";
	}

	// 3. 글목록 - GET
	// => 파라미터로 전달되는 pageNum 파라미터가 없을 경우를 대비하여 기본값 0 으로 지정
	@RequestMapping(value = "/board/list", method = RequestMethod.GET)
	public String list(@RequestParam(defaultValue = "1") int pageNum, Model model) {
		// 페이징 처리에 필요한 전체 게시물 수 조회 - getBoardListCount()
		// => 파라미터 : 없음, 리턴타입 : int(listCount)
		// => 게시물이 없을 경우 null 이 아닌 0 이 리턴되므로 Integer 대신 int 사용 가능
		int listCount = service.getBoardListCount();
//		System.out.println("전체 게시물 수 : " + listCount);
		int listLimit = 10; // 한 페이지 당 표시할 게시물 목록 갯수
		int pageLimit = 10; // 한 페이지 당 표시할 페이지 목록 갯수
		
		// 페이징 처리를 위한 계산 작업
		int maxPage = (int)Math.ceil((double)listCount / listLimit);
		int startPage = ((int)((double)pageNum / pageLimit + 0.9) - 1) * pageLimit + 1;
		int endPage = startPage + pageLimit - 1;
		if(endPage > maxPage) {
			endPage = maxPage;
		}
		
		// 조회 시작 게시물 번호(행 번호 계산)
		int startRow = (pageNum - 1) * listLimit;
		
		// 페이징 처리 정보를 PageInfo 객체에 저장
		PageInfo pageInfo = new PageInfo();
		pageInfo.setPageNum(pageNum);
		pageInfo.setMaxPage(maxPage);
		pageInfo.setStartPage(startPage);
		pageInfo.setEndPage(endPage);
		pageInfo.setListCount(listCount);
		pageInfo.setStartRow(startRow);
		pageInfo.setListLimit(listLimit);

				
		// Service 객체의 getBoardList() 메서드를 호출하여 게시물 목록 조회
		// => 파라미터 : 페이지 정보(PageInfo) 객체
		// => 리턴타입 : List<BoardVO>(boardList)
		List<BoardVO> boardList = service.getBoardList(pageInfo);
//		System.out.println(boardList);
		
		// Model 객체에 게시물 목록과 페이징 처리 정보 저장
		model.addAttribute("boardList", boardList);
		model.addAttribute("pageInfo", pageInfo);
		
		return "board/qna_board_list";
	}
	
	// 4. 글 상세내용 조회 - GET
	@RequestMapping(value = "/board/detail", method = RequestMethod.GET)
	public String detail(@RequestParam int board_num, Model model) {
		// Service 객체의 increaseReadcount() 메서드 호출하여 게시물 조회수 증가
		service.increaseReadcount(board_num);

		
		// Service 객체의 getBoardDetail() 메서드를 호출하여 게시물 상세 정보 조회
		// => 파라미터 : 글번호(board_num), 리턴타입 : BoardVO(board)
		BoardVO board = service.getBoardDetail(board_num);

		model.addAttribute("board", board);
		
		return "board/qna_board_view";
	}
}

```

- service
```java
---------------------------------------------BoardService.java---------------------------------------------
package com.itwillbs.mvc_board.service;

import java.sql.Date;
import java.sql.Timestamp;
import java.util.List;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
import org.springframework.ui.Model;

import com.itwillbs.mvc_board.mapper.BoardMapper;
import com.itwillbs.mvc_board.vo.BoardVO;
import com.itwillbs.mvc_board.vo.PageInfo;

@Service
public class BoardService {
	@Autowired
	private BoardMapper mapper;

	// 1. 글쓰기
	public void writeBoard(BoardVO board) {
		// BoardMapper 객체의 selectMaxNum() 메서드를 호출하여 게시물 번호 중 최대값 조회
		// => 게시물이 하나도 없을 경우 null 값이 리턴되므로 int 대신 Integer 타입 변수 필요
		//	  (int 타입 지정 시 오토언박싱이 일어나며, null 값의 경우 NullPointerException 발생)
		Integer num = mapper.selectMaxNum();
		
		// 리턴받은 글번호가 null 일 경우 BoardVO 객체의 board_num 값을 1로 설정하고
		// 아니면, 리턴받은 번호 + 1 값을 board_num 값으로 설정
		if(num == null) {
			num = 1;
		} else {
			num += 1;
		}
		
		board.setBoard_num(num); // Integer + int = int + int
		// 참조글번호(board_re_ref) 는 새 글이므로 새 글 번호와 동일하게 설정
		board.setBoard_re_ref(num);
		
//		System.out.println(board.getBoard_num());
		
		// 들여쓰기레벨(board_re_lev), 순서번호(board_re_seq), 조회수(readcount)는 0으로 설정
		board.setBoard_re_lev(0);
		board.setBoard_re_seq(0);
		board.setBoard_readcount(0);
		// 날짜는 시스템의 현재 시각 사용 => 웹서버의 시각 대신 DB 서버의 시각은 now() 함수 활용
//		board.setBoard_date(new Date(System.currentTimeMillis()));
		
		// BoardMapper 객체의 insertBoard() 메서드 호출
		// => 파라미터 : BoardVO 객체, 리턴타입 : void
		mapper.insertBoard(board);
		
	}

	// 2. 글목록
	public int getBoardListCount() {
		return mapper.selectBoardListCount();
	}

	// 3. 게시물 목록 조회
	public List<BoardVO> getBoardList(PageInfo pageInfo) {
		return mapper.selectBoardList(pageInfo);
	}

	// 4. 게시물 조회수 증가
	public void increaseReadcount(int board_num) {
		return mapper.updateReadcount(board_num);
		
	}

	// 5. 글 상세내용 조회
	public BoardVO getBoardDetail(int board_num) {
		return mapper.selectBoardDetail(board_num);
		
	}

}


```

- mapper
```java
---------------------------------------------BoardMapper.java---------------------------------------------
package com.itwillbs.mvc_board.mapper;

import java.util.List;

import org.springframework.ui.Model;

import com.itwillbs.mvc_board.vo.BoardVO;
import com.itwillbs.mvc_board.vo.PageInfo;

public interface BoardMapper {

	// 1. 최대 글번호 조회
	public Integer selectMaxNum();

	// 2. 글쓰기
	public void insertBoard(BoardVO board);

	// 3. 총 게시물 수 조회
	public int selectBoardListCount();

	// 4. 게시물 목록 조회
	public List<BoardVO> selectBoardList(PageInfo pageInfo);

	// 5. 글 상세내용 조회
	public BoardVO selectBoardDetail(int board_num);



}

```
```xml
---------------------------------------------BoardMapper.xml---------------------------------------------
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd" >
<mapper namespace="com.itwillbs.mvc_board.mapper.BoardMapper">
	<!-- 1. 최대 글번호 조회 -->
	<select id="selectMaxNum" resultType="java.lang.Integer">
		SELECT MAX(board_num) FROM board
	</select>
	
	<!-- 2. 글쓰기 -->
	<insert id="insertBoard">
		INSERT INTO board 
		VALUES (#{board_num}, #{board_name}, #{board_pass}, #{board_subject}, #{board_content}, 
				#{board_file}, #{board_real_file}, #{board_re_ref}, #{board_re_lev}, #{board_re_seq},
				#{board_readcount}, now())
		
	</insert>
	
	<!-- 3. 총 게시물 수 조회-->
	<select id="selectBoardListCount" resultType="java.lang.Integer">
		SELECT COUNT(*) FROM board
	</select>
	
	<!-- 4. 게시물 목록 조회 -->
	<select id="selectBoardList" resultType="com.itwillbs.mvc_board.vo.BoardVO">
		SELECT * FROM board ORDER BY board_re_ref DESC, board_re_seq ASC LIMIT #{startRow}, #{listLimit}
		
	</select>
	
	<!-- 5. 글 상세내용 조회 -->
	<select id="selectBoardDetail" resultType="com.itwillbs.mvc_board.vo.BoardVO">
		SELECT * FROM board WHERE board_num=#{board_num}
	</select>
	
	
</mapper>

```

```jsp
---------------------------------------------index.jsp---------------------------------------------
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%
// 세션 객체에 저장된 세션 아이디("sId") 가져와서 변수에 저장
String sId = (String)session.getAttribute("sId");
%>    
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<script type="text/javascript">
	function confirmLogout() {
		if(confirm("로그아웃 하시겠습니까?")) {
			// MemberLogout.me 서블릿 주소 요청
			location.href = "./MemberLogout.me";
		}
	}
</script>
</head>
<body>
	<header>
		<div align="right">
			<!-- 세션 아이디가 null 또는 "" 일 경우 로그인, 회원가입 링크 표시 -->
			<%if(sId == null || sId.equals("")) { %>
				<h5><a href="./MemberLogin.me">로그인</a> | <a href="./MemberJoinForm.me">회원가입</a></h5>
			<%} else { %>
				<h5>
					<a href="./MemberInfo.me"><%=sId %></a> 님 |
					<a href="javascript:void(0)" onclick="confirmLogout()">로그아웃</a> | 
<!-- 					<a href="#" onclick="confirmLogout()">로그아웃</a>  -->
					<!-- 
					하이퍼링크 클릭 시 자바스크립트를 실행해야할 경우
					href 속성에 # 또는 javscript:void(0) 를 지정하면 페이지 갱신 없이 실행됨
					-->
					<%if(sId.equals("admin")) { %>
					| <a href="./AdminPage.me">관리자페이지</a>
					<%} %>
				</h5>
			<%} %>
		</div>
	</header>
	<h1>MVC_Board 메인페이지</h1>
	<h3><a href="./BoardWrite.bo">글쓰기</a></h3> => board/write_form.jsp
	<h3><a href="./BoardList.bo">글목록</a></h3>
</body>
</html>


---------------------------------------------qna_board_write.jsp---------------------------------------------
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>MVC 게시판</title>
<style type="text/css">
	#writeForm {
		width: 500px;
		height: 450px;
		border: 1px solid red;
		margin: auto;
	}
	
	h1 {
		text-align: center;
	}
	
	table {
		margin: auto;
		width: 450px;
	}
	
	.td_left {
		width: 150px;
		background: orange;
		text-align: center;
	}
	
	.td_right {
		width: 300px;
		background: skyblue;
	}
	
	#commandCell {
		text-align: center;
	}
</style>
</head>
<body>
	<!-- 게시판 등록 -->
	<section id="writeForm">
		<h1>게시판 글 등록</h1>
		<!-- 
		form 데이터 중 파일 정보가 포함될 경우
		<form> 태그 속성에 enctype="multipart/form-data" 명시 필수!
		(생략 시 enctype="application/x-www-form-urlencoded" 속성이 기본값으로 설정됨)
		-->
		<form action="write" name="boardForm" method="post">
			<table>
				<tr>
					<td class="td_left"><label for="board_name">글쓴이</label></td>
					<td class="td_right"><input type="text" name="board_name" required="required" /></td>
				</tr>
				<tr>
					<td class="td_left"><label for="board_pass">비밀번호</label></td>
					<td class="td_right"><input type="password" name="board_pass" required="required" /></td>
				</tr>
				<tr>
					<td class="td_left"><label for="board_subject">제목</label></td>
					<td class="td_right"><input type="text" name="board_subject" required="required" /></td>
				</tr>
				<tr>
					<td class="td_left"><label for="board_content">내용</label></td>
					<td class="td_right">
						<textarea id="board_content" name="board_content" cols="40" rows="15" required="required"></textarea>
					</td>
				</tr>
			</table>
			<section id="commandCell">
				<input type="submit" value="등록">&nbsp;&nbsp;
				<input type="reset" value="다시쓰기">&nbsp;&nbsp;
				<input type="button" value="취소" onclick="history.back()">
			</section>
		</form>
	</section>
</body>
</html>

---------------------------------------------qna_board_list.jsp---------------------------------------------
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

---------------------------------------------qna_board_view.jsp---------------------------------------------
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
			<tr><th width="70">제 목</th><td colspan="3" >${board.board_subject }</td></tr>
			<tr>
				<th width="70">작성자</th><td>${board.board_name }</td>
				<th width="70">작성일</th><td>${board.board_date }</td>
			</tr>
			<tr>
				<th width="70">첨부파일</th>
				<td>
					<!-- 
					파일 다운로드 링크 사용법(중복 파일명 처리로 인해 실제 파일과 업로드 한 파일명이 다를 수 있음)
					<a href="다운로드 할 실제 파일명(위치포함)" download> 화면에 보여줄 파일명 
					-->
					<c:choose>
						<c:when test="${empty board.board_file }"> <!-- 첨부파일이 없을 경우(null) -->
						없음
						</c:when>
						<c:otherwise> <!-- 첨부파일이 있을 경우 -->
							<a href="./upload/${board.board_real_file }" download="${board.board_file }">
								${board.board_file }
							</a>
						</c:otherwise>
					</c:choose>
					
				</td>
				<th  width="70">조회수</th>
				<td>${board.board_readcount}</td>
			</tr>
			</table>
		</section>
		<section id="articleContentArea">
			${board.board_content }
		</section>
	</section>
	<section id="commandList">
		<input type="button" value="답변" onclick="location.href='reply?board_num=${param.board_num}&page=${param.pageNum}'">
		<input type="button" value="수정" onclick="location.href='modify?board_num=${param.board_num}&page=${param.pageNum}'">
		<input type="button" value="삭제" onclick="location.href='delete?board_num=${param.board_num}&page=${param.pageNum}'">
		<input type="button" value="목록" onclick="location.href='list?page=${param.pageNum}'">
	</section>
</body>
</html>

```







