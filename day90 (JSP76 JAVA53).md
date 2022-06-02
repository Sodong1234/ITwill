# [오전수업] JSP 76차

- controller
```java
------------------------------------BoardController.java------------------------------------
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
	public String writePost(@ModelAttribute BoardVO board, Model model) {
		// BoardService 객체의 writeBoard() 메서드 호출
		// => 파라미터 : BoardVO 객체, 리턴타입 : void
		int insertCount = service.writeBoard(board);

		// 글쓰기 실패(insertCount 가 0) 시 "글 등록 실패!" 메세지를 담아 fail_back.jsp 로 포워딩
		if (insertCount == 0) {
			model.addAttribute("msg", "글 등록 실패!");
			return "fail_back";
		}

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
		int maxPage = (int) Math.ceil((double) listCount / listLimit);
		int startPage = ((int) ((double) pageNum / pageLimit + 0.9) - 1) * pageLimit + 1;
		int endPage = startPage + pageLimit - 1;
		if (endPage > maxPage) {
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
//		List<BoardVO> boardList = service.getBoardList(pageInfo);
		List<BoardVO> boardList = service.getBoardList("", "", pageInfo);
//		System.out.println(boardList);

		// Model 객체에 게시물 목록과 페이징 처리 정보 저장
		model.addAttribute("boardList", boardList);
		model.addAttribute("pageInfo", pageInfo);

		return "board/qna_board_list";
	}

	// 3-2. 글목록 - POST(검색기능)
	// => 파라미터로 전달되는 pageNum 파라미터가 없을 경우를 대비하여 기본값 0 으로 지정
	@RequestMapping(value = "/board/list", method = RequestMethod.POST)
	public String listSearch(@RequestParam String searchType, @RequestParam String keyword,
			@RequestParam(defaultValue = "1") int pageNum, Model model) {

		// 페이징 처리에 필요한 전체 게시물 수 조회 - getBoardListCount()
		// => 파라미터 : 없음, 리턴타입 : int(listCount)
		// => 게시물이 없을 경우 null 이 아닌 0 이 리턴되므로 Integer 대신 int 사용 가능
		int listCount = service.getBoardListCount();
//		System.out.println("전체 게시물 수 : " + listCount);
		int listLimit = 10; // 한 페이지 당 표시할 게시물 목록 갯수
		int pageLimit = 10; // 한 페이지 당 표시할 페이지 목록 갯수

		// 페이징 처리를 위한 계산 작업
		int maxPage = (int) Math.ceil((double) listCount / listLimit);
		int startPage = ((int) ((double) pageNum / pageLimit + 0.9) - 1) * pageLimit + 1;
		int endPage = startPage + pageLimit - 1;
		if (endPage > maxPage) {
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

		List<BoardVO> boardList = service.getBoardList(searchType, "%" + keyword + "%", pageInfo);
		
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

	// 5. 글 삭제 폼 - GET
	@RequestMapping(value = "/board/delete", method = RequestMethod.GET)
	public String delete() {
		return "board/qna_board_delete";
	}

	// 6. 글 삭제 비즈니스 로직 - POST
	// => 게시물 정보를 전달받아 저장하기 위해 BoardVO 타입 파라미터 선언
	@RequestMapping(value = "/board/delete", method = RequestMethod.POST)
	public String deletepost(@ModelAttribute BoardVO board, @RequestParam int pageNum, Model model) {
		// service 객체의 removeBoard() 메서드 호출하여 삭제 작업 요청
		// => 파라미터 : BoardVO 객체, 리턴타입 : int(deleteCount)
		// (Mapper 객체의 deleteBoard() 메서드를 통해 삭제 작업 수행)
		int deleteCount = service.removeBoard(board);

		// 삭제 실패(deleteCount 가 0) 시 "글 삭제 실패!" 메세지 저장 후 fail_back.jsp 로 포워딩
		// 아니면, qna_board_list.jsp 페이지로 포워딩(페이지번호 전달)
		if (deleteCount == 0) {
			model.addAttribute("msg", "글 삭제 실패!");
			return "fail_back";
		}

		// Redirect 방식 또는 Dispatcher 방식에 상관없이 전달할 데이터(파라미터)는 Model 객체 사용
		// => Redirect 방식일 경우 URL 의 파라미터 형태로 전달
		model.addAttribute("pageNum", pageNum);

		// board/list 요청 - Redirect 방식
		return "redirect:/board/list";
	}

	// 7. 글 수정 폼 (modify - modfy()) - GET
	// => qna_board_modify.jsp
	@RequestMapping(value = "/board/modify", method = RequestMethod.GET)
	public String modify(@RequestParam int board_num, Model model) {
		// 글 상세정보 조회 작업을 재사용하여 수정할 내용 가져오기
		// Service 객체의 getBoardDetail() 메서드를 호출하여 게시물 가져오기
		// => 파라미터 : 글번호(board_num), 리턴타입 : BoardVO(board)
		BoardVO board = service.getBoardDetail(board_num);

		model.addAttribute("board", board);

		return "board/qna_board_modify";
	}

	@RequestMapping(value = "/board/modify", method = RequestMethod.POST)
	public String modifyPost(@ModelAttribute BoardVO board, @RequestParam int pageNum, Model model) {
		// 8. 글 수정 비즈니스 로직(modfy - modifyPost()) - POST
		// => service - modifyBoard(), mapper - deleteBoard()
		int updateCount = service.modifyBoard(board);

		// => 수정 결과 판별(씰패 시 "패스워드 틀림" fail_back.jsp, 성공 시 detail)
		if (updateCount == 0) {
			model.addAttribute("msg", "패스워드 틀림!");
			return "fail_back";
		}
		model.addAttribute("board_num", board.getBoard_num());
		model.addAttribute("pageNum", pageNum);

		return "redirect:/board/detail";
	}

	// 9. 답글 작성 폼 (reply - reply()) - GET
	// => qna_board_reply.jsp
	@RequestMapping(value = "/board/reply", method = RequestMethod.GET)
	public String reply(@RequestParam int board_num, Model model) {
		// 글 상세정보 조회 작업을 재사용하여 수정할 내용 가져오기
		// Service 객체의 getBoardDetail() 메서드를 호출하여 게시물 가져오기
		// => 파라미터 : 글번호(board_num), 리턴타입 : BoardVO(board)
		BoardVO board = service.getBoardDetail(board_num);
		model.addAttribute("board", board);

		return "board/qna_board_reply";
	}

	// 10. 답글 작성 비즈니스 로직 - POST
	// => 게시물 정보를 전달받아 저장하기 위해 BoardVO 타입 파라미터 선언
	@RequestMapping(value = "/board/reply", method = RequestMethod.POST)
	public String replyPost(@ModelAttribute BoardVO board, @RequestParam int pageNum, Model model) {
		// BoardService 객체의 writeReplyBoard() 메서드 호출
		// => 파라미터 : BoardVO 객체, 리턴타입 : void
		int insertCount = service.writeReplyBoard(board);

		// 글쓰기 실패(insertCount 가 0) 시 "답글 등록 실패!" 메세지를 담아 fail_back.jsp 로 포워딩
		if (insertCount == 0) {
			model.addAttribute("msg", "답글 등록 실패!");
			return "fail_back";
		}

		// board/list 요청 - Redirect 방식
		model.addAttribute("pageNum", pageNum);

		return "redirect:/board/list";
	}

}

```

- Service
```java
------------------------------------BoardService.java------------------------------------
package com.itwillbs.mvc_board.service;

import java.util.List;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
import org.springframework.web.bind.annotation.RequestParam;

import com.itwillbs.mvc_board.mapper.BoardMapper;
import com.itwillbs.mvc_board.vo.BoardVO;
import com.itwillbs.mvc_board.vo.PageInfo;

@Service
public class BoardService {
	@Autowired
	private BoardMapper mapper;

	// 1. 글쓰기
	public int writeBoard(BoardVO board) {
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
		return mapper.insertBoard(board);
		
	}

	// 2. 글목록
	public int getBoardListCount() {
		return mapper.selectBoardListCount();
	}

	// 3. 게시물 목록 조회
//	public List<BoardVO> getBoardList(PageInfo pageInfo) {
//		return mapper.selectBoardList(pageInfo);
//	}
	
	// 3-2. 게시물 목록 조회(검색 기능 추가)
	public List<BoardVO> getBoardList(String searchType, String keyword, PageInfo pageInfo) {
		return mapper.selectBoardList(searchType, keyword, pageInfo);
	}

	// 4. 게시물 조회수 증가
	public void increaseReadcount(int board_num) {
		mapper.updateReadcount(board_num);
		
	}

	// 5. 글 상세내용 조회
	public BoardVO getBoardDetail(int board_num) {
		return mapper.selectBoardDetail(board_num);
		
	}

	// 6. 글 삭제
	public int removeBoard(BoardVO board) {
		return mapper.deleteBoard(board);
		
	}

	// 7. 글 수정
	public int modifyBoard(BoardVO board) {
		return mapper.updateBoard(board);
		
	}

	// 8. 답글 등록
	public int writeReplyBoard(BoardVO board) {
		// 기존 원본글에 대한 답글 존재 시 순서번호 조정을 위해 updateBoardReSeq() 메서드 호출
		mapper.updateBoardReSeq(board);
		
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
		// -------------------------------------------------------------------------------------------
		board.setBoard_num(num); 
		
		// 주의해야할 부분(답글 관련 처리)
		// 답글에 사용될 참조글 번호(board_re_ref) 는 원본글의 참조글번호와 동일하게 지정
		board.setBoard_re_ref(board.getBoard_re_ref()); 
		// 들여쓰기레벨(board_re_ref), 순서번호(board_re_seq)는 원본글의 값 + 1 
		board.setBoard_re_lev(board.getBoard_re_lev() + 1);
		board.setBoard_re_seq(board.getBoard_re_seq() + 1);
		
		// 조회수(readcount)는 0으로 설정
		board.setBoard_readcount(0);
		
		// 실제 비즈니스 로직은 기존 글쓰기 로직을 재사용
		return mapper.insertBoard(board);
	}

}

```

- Mapper
```java
------------------------------------BoardMapper.java------------------------------------
package com.itwillbs.mvc_board.mapper;

import java.util.List;

import org.apache.ibatis.annotations.Param;
import org.springframework.ui.Model;

import com.itwillbs.mvc_board.vo.BoardVO;
import com.itwillbs.mvc_board.vo.PageInfo;

public interface BoardMapper {

	// 1. 최대 글번호 조회
	public Integer selectMaxNum();

	// 2. 글쓰기(작업 결과 int 타입 리턴 => Mapper.xml 에서 실행 후 자동 리턴됨)
	public int insertBoard(BoardVO board);

	// 3. 총 게시물 수 조회
	public int selectBoardListCount();

	// 4. 게시물 목록 조회
//	public List<BoardVO> selectBoardList(PageInfo pageInfo);
	// 4-2. 게시물 목록 조회(검색기능 추가)
	// => Mapper 를 통한 파라미터가 복수개일 경우 @Param 어노테이션을 통해 파라미터 이름을 지정
	public List<BoardVO> selectBoardList(@Param("searchType") String serachType, @Param("keyword") String keyword, @Param("pageInfo") PageInfo pageInfo);

	// 5. 게시물 조회수 증가
	public void updateReadcount(int board_num);
	
	// 6. 글 상세내용 조회
	public BoardVO selectBoardDetail(int board_num);

	// 7. 글 삭제
	public int deleteBoard(BoardVO board);

	// 8. 글 수정
	public int updateBoard(BoardVO board);

	// 9. 기존 답글의 순서번호 조정
	public void updateBoardReSeq(BoardVO board);


}

```

```xml
------------------------------------BoardMapper.xml------------------------------------
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
<!-- 	<select id="selectBoardList" resultType="com.itwillbs.mvc_board.vo.BoardVO"> -->
<!-- 		SELECT * FROM board ORDER BY board_re_ref DESC, board_re_seq ASC LIMIT #{startRow}, #{listLimit} -->
		
<!-- 	</select> -->

	<!-- 4-2. 게시물 목록 조회(검색기능 추가) -->
	<select id="selectBoardList" resultType="com.itwillbs.mvc_board.vo.BoardVO">
	<!-- 파라미터가 복수개일 때 @Param 어노테이션으로 지정된 이름을 사용하고, 객체는 객체명.변수명 사용 -->
		SELECT * FROM board 
		WHERE #{searchType} LIKE #{keyword}
		ORDER BY board_re_ref DESC, board_re_seq ASC
		LIMIT #{pageInfo.startRow}, #{pageInfo.listLimit} 
	</select>
	
	<!-- 5. 게시물 조회수 증가 -->
	<update id="updateReadcount">
		UPDATE board
		SET board_readcount = board_readcount + 1
		WHERE board_num = #{board_num}
	</update>
	
	<!-- 6. 글 상세내용 조회 -->
	<select id="selectBoardDetail" resultType="com.itwillbs.mvc_board.vo.BoardVO">
		SELECT * FROM board WHERE board_num=#{board_num}
	</select>
	
	<!-- 7. 글 삭제 -->
	<delete id="deleteBoard">
		DELETE FROM board
		WHERE board_num=#{board_num} AND board_pass=#{board_pass}
	</delete>
	
	<!-- 8. 글 수정 -->
	<update id="updateBoard">
		UPDATE board
		SET board_name = #{board_name}, board_subject = #{board_subject}, board_content = #{board_content}
		WHERE board_num = #{board_num} AND board_pass = #{board_pass}
	</update>
	
	<!-- 9. 기존 답글 순서번호 조정 -->
	<!-- 원본글과 참조글번호가 같고, 순서번호가 원본글의 순서번호보다 큰 게시물들의 순서번호 + 1 -->
	<update id="updateBoardReSeq">
		UPDATE board
		SET board_re_seq = board_re_seq + 1
		WHERE board_re_ref = #{board_re_ref} AND board_re_seq > #{board_re_seq}
	</update>
	
	
	
</mapper>

```


```jsp
------------------------------------qna_board_delete.jsp------------------------------------
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>MVC 게시판</title>
<style>
	#passForm {
		width: 300px;
		margin: auto;
		border: 1px solid orange;
		text-align: center;
	}
	
	h2 {
		text-align: center;
	}
	
	table {
		width: 300px;
		margin: auto;
		text-align: center;
	}
	
</style>
</head>
<body>
	<!-- 게시판 글 삭제 -->
	<h2>게시판 글 삭제</h2>
	<section id="passForm">
		<form action="delete" name="deleteForm" method="post">
			<!-- input type="hidden" 속성을 통해 board_num, page 파라미터를 다시 전달해야함 -->
			<input type="hidden" name="board_num" value="${param.board_num }">
			<input type="hidden" name="pageNum" value="${param.pageNum }">
			<table>
				<tr>
					<td><label>글 비밀번호</label></td>
					<td><input type="password" name="board_pass" required="required"></td>
				</tr>
				<tr>
					<td colspan="2">
						<input type="submit" value="삭제">&nbsp;&nbsp;
						<input type="button" value="돌아가기" onclick="javascript:history.back()">
					</td>
				</tr>
			</table>
		</form>
	</section>
</body>
</html>

------------------------------------qna_board_view.jsp------------------------------------
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
		<input type="button" value="답변" onclick="location.href='reply?board_num=${param.board_num}&pageNum=${param.pageNum}'">
		<input type="button" value="수정" onclick="location.href='modify?board_num=${param.board_num}&pageNum=${param.pageNum}'">
		<input type="button" value="삭제" onclick="location.href='delete?board_num=${param.board_num}&pageNum=${param.pageNum}'">
		<input type="button" value="목록" onclick="location.href='list?pageNum=${param.pageNum}'">
	</section>
</body>
</html>

------------------------------------qna_board_modify.jsp------------------------------------
<%@page import="com.itwillbs.mvc_board.vo.BoardVO"%>
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>MVC 게시판</title>
<style type="text/css">
	#modifyForm {
		width: 500px;
		height: 610px;
		border: 1p solid red;
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
	
	<!-- 게시판 글 수정 -->
	<section id="modify">
		<h1>게시판 글 수정</h1>
		<!-- 수정 대상에서 파일은 제외시킬 경우 enctype 속성 제거(일반 form) -->
		<form action="modify" name="boardForm" method="post">
			<!-- input type="hidden" 사용하여 글번호(board_num)와 페이지번호(page) 전달 -->
			<input type="hidden" name="board_num" value="${param.board_num }">
			<input type="hidden" name="pageNum" value="${param.pageNum }">
			<table>
				<tr>
					<td class="td_left"><label for="board_name">글쓴이</label></td>
					<td class="td_right">
						<input type="text" name="board_name" value="${board.board_name }" required="required" />
					</td>
				</tr>
				<tr>
					<td class="td_left"><label for="board_pass">비밀번호</label></td>
					<td class="td_right">
						<input type="password" name="board_pass" required="required" />
					</td>
				</tr>
				<tr>
					<td class="td_left"><label for="board_subject">제목</label></td>
					<td class="td_right"><input type="text" name="board_subject" value="${board.board_subject }" required="required" /></td>
				</tr>
				<tr>
					<td class="td_left"><label for="board_content">내용</label></td>
					<td class="td_right">
						<textarea id="board_content" name="board_content" cols="40" rows="15" required="required"> ${board.board_content }</textarea>
					</td>
				</tr>
				<tr>
					<td class="td_left"><label for="board_file">파일</label></td>
					<!-- 파일 수정 기능은 제외(파일명만 표시) -->
					<td class="td_right">${board.board_file } (수정불가)</td>
				</tr>	
			</table>
			<section id="commandCell">
				<input type="submit" value="수정">&nbsp;&nbsp;
				<input type="reset" value="다시쓰기">&nbsp;&nbsp;
				<input type="button" value="취소" onclick="history.back()">
			</section>
		</form>
	</section>
	
</body>
</html>
------------------------------------qna_board_reply.jsp------------------------------------
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>MVC 게시판</title>
<style type="text/css">
	#replyForm {
		width: 500px;
		height: 610px;
		border: 1p solid red;
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
	
	<!-- 게시판 답글 작성 -->
	<section id="replyForm">
		<h1>게시판 답글 작성</h1>
		<form action="reply" name="boardForm" method="post">
			<!-- input type="hidden" 사용하여 글번호(board_num)와 페이지번호(page) 전달 -->
			<input type="hidden" name="board_num" value="${param.board_num }">
			<input type="hidden" name="pageNum" value="${param.pageNum }">
			<!-- 답글 관련 원본글 정보를 담는 board_re_ref, board_re_lev, board_re_seq 도 전달 -->
			<input type="hidden" name="board_re_ref" value="${board.board_re_ref }">
			<input type="hidden" name="board_re_lev" value="${board.board_re_lev }">
			<input type="hidden" name="board_re_seq" value="${board.board_re_seq }">
			<table>
				<tr>
					<td class="td_left"><label for="board_name">글쓴이</label></td>
					<td class="td_right">
						<input type="text" name="board_name" required="required" />
					</td>
				</tr>
				<tr>
					<td class="td_left"><label for="board_pass">비밀번호</label></td>
					<td class="td_right">
						<input type="password" name="board_pass" required="required" />
					</td>
				</tr>
				<!-- 답글 작성 시 원본글의 제목, 내용은 표시 -->
				<tr>
					<td class="td_left"><label for="board_subject">제목</label></td>
					<td class="td_right">
						<input type="text" name="board_subject" value="Re:${board.board_subject }" required="required" />
					</td>
				</tr>
				<tr>
					<td class="td_left"><label for="board_content">내용</label></td>
					<td class="td_right">
						<textarea id="board_content" name="board_content" cols="40" rows="15" required="required">
							-------- 원본 글 내용 --------
							${board.board_content }
						</textarea>
					</td>
				</tr>
			</table>
			<section id="commandCell">
				<input type="submit" value="답글등록">&nbsp;&nbsp;
				<input type="reset" value="다시쓰기">&nbsp;&nbsp;
				<input type="button" value="취소" onclick="history.back()">
			</section>
		</form>
	</section>
	
</body>
</html>

------------------------------------qna_board_list.jsp------------------------------------
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
				<option value="subject&content">제목&내용</option>
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


---

# [오후수업] JAVA 53차

## Clock
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity"
    android:orientation="vertical">

    <TextView
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="AnalogClock"
        android:textSize="20sp"/>

    <AnalogClock
        android:layout_width="match_parent"
        android:layout_height="wrap_content"/>

    <TextView
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="DigitalClock"
        android:textSize="20sp"/>

    <DigitalClock
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:textSize="20sp"
        android:gravity="center"/>

    <TextView
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="TextClock"
        android:textSize="20sp"/>

    <TextClock
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:textSize="20sp"
        android:gravity="center"/>

    <TextView
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Chronometer"
        android:textSize="20sp"/>

    <Chronometer
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:textSize="20sp"
        android:format="측정 시간 %s"
        android:id="@+id/chronometer"/>

    <!-- 측정시작, 측정 종료 버튼 부착 -->
    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="horizontal">

        <Button
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:layout_weight="1"
            android:id="@+id/btnStart"
            android:text="측정시작"
            android:textSize="20sp"/>

        <Button
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:layout_weight="1"
            android:id="@+id/btnStop"
            android:text="측정종료"
            android:textSize="20sp"/>

        <Button
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:layout_weight="1"
            android:id="@+id/btnContinue"
            android:text="측정계속"
            android:textSize="20sp"/>
    </LinearLayout>





</LinearLayout>
```
```java
package com.example.and0602_adwidget_clock;

import androidx.appcompat.app.AppCompatActivity;

import android.os.Bundle;
import android.os.SystemClock;
import android.view.View;
import android.widget.Button;
import android.widget.Chronometer;

public class MainActivity extends AppCompatActivity {

    Chronometer chronometer;
    Button btnStart, btnStop, btnContinue;


    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        chronometer = findViewById(R.id.chronometer);
        btnStart = findViewById(R.id.btnStart);
        btnStop = findViewById(R.id.btnStop);
        btnContinue = findViewById(R.id.btnContinue);

        btnStart.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                // 크로노미터는 위젯이 표시될 때 카운트가 시작되므로
                // 타이머 기능을 사용하기 위해서는 타이머를 0으로 초기화 필요
                chronometer.setBase(SystemClock.elapsedRealtime());
                chronometer.start();
            }
        });

        btnStop.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                chronometer.stop();
            }
        });

        btnContinue.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                chronometer.start();
            }
        });



    }
}
```

## Calendar
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity"
    android:orientation="vertical">

    <CalendarView
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:id="@+id/calendarView"/>

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="horizontal">

        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:id="@+id/tvYear"
            android:text="0000년"
            android:textSize="30sp"/>

        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:id="@+id/tvMonth"
            android:text="00월"
            android:textSize="30sp"/>

        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:id="@+id/tvday"
            android:text="00일"
            android:textSize="30sp"/>

    </LinearLayout>

    <Button
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:id="@+id/btnOk"
        android:text="설정"
        android:textSize="20sp"/>


</LinearLayout>
```
```java
package com.example.and0602_adwidget_calendar;

import androidx.annotation.NonNull;
import androidx.appcompat.app.AppCompatActivity;

import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.CalendarView;
import android.widget.TextView;

import java.util.Calendar;
import java.util.Date;

public class MainActivity extends AppCompatActivity {

    CalendarView calendarView;
    TextView tvYear, tvMonth, tvDay;
    Button btnOk;

    int selectedYear, selectedMonth, selectedDay;


    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        calendarView = findViewById(R.id.calendarView);
        tvYear = findViewById(R.id.tvYear);
        tvMonth = findViewById(R.id.tvMonth);
        tvDay = findViewById(R.id.tvday);
        btnOk = findViewById(R.id.btnOk);

        calendarView.setOnDateChangeListener(new CalendarView.OnDateChangeListener() {
            @Override
            public void onSelectedDayChange(@NonNull CalendarView calendarView, int y, int m, int d) {

                // TextView 위젯에 각각의 연, 월, 일 정보를 출력 (xxxx년 xx월 xx일)
//                tvYear.setText(y + "년");
//                tvMonth.setText((m + 1) + "월");
//                tvDay.setText(d + "일");

                selectedYear = y;
                selectedMonth = m + 1;
                selectedDay = d;
            }
        });

        btnOk.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
//                long date = calendarView.getDate();
//                Date d = new Date(date);
//
//                // java.util.Calendar 클래스를 활용하여 Date -> Calendar 로 변환하여 사용
//                Calendar cal = Calendar.getInstance();
//                cal.setTime(d);
//                int year = cal.get(Calendar.YEAR);
//                int month = cal.get(Calendar.MONTH);
//                int dayOfMonth = cal.get(Calendar.DAY_OF_MONTH);
//
//                tvYear.setText(year + "년");
//                tvMonth.setText((month + 1) + "월");
//                tvDay.setText(dayOfMonth + "일");

                tvYear.setText(selectedYear + "년");
                tvMonth.setText(selectedMonth + "월");
                tvDay.setText(selectedDay + "일");


            }
        });


    }
}
```
