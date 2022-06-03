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
		if(insertCount == 0) {
			model.addAttribute("msg", "글 등록 실패!");
			return "fail_back";
		}
		
		// board/list 요청 - Redirect 방식
		return "redirect:/board/list";
	}
	
	// 3. 글목록 - GET
	// => 파라미터로 전달되는 pageNum 파라미터가 없을 경우를 대비하여 기본값 0 으로 지정
	@RequestMapping(value = "/board/list", method = RequestMethod.GET)
	   public String list(@RequestParam(defaultValue = "") String searchType, @RequestParam(defaultValue = "") String keyword, @RequestParam(defaultValue = "1") int pageNum, Model model) {
		// 페이징 처리에 필요한 전체 게시물 수 조회 - getBoardListCount()
		// => 파라미터 : 없음, 리턴타입 : int(listCount)
		// => 게시물이 없을 경우 null 이 아닌 0 이 리턴되므로 Integer 대신 int 사용 가능
		// 검색 기능을 포함하는 경우 총 게시물 수 조회 시 널스트링 전달
		int listCount = service.getBoardListCount(searchType, "%" + keyword + "%");
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
		
		// 조회 시작 게시물 번호(행 번호) 계산
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
		List<BoardVO> boardList = service.getBoardList(searchType, "%" + keyword + "%", pageInfo);
//		System.out.println(boardList);
		
		// Model 객체에 게시물 목록과 페이징 처리 정보 저장
		model.addAttribute("boardList", boardList);
		model.addAttribute("pageInfo", pageInfo);
		
		return "board/qna_board_list";
	}
	
	// 3-2. 글목록 - POST(검색기능)
	// => 파라미터로 전달되는 pageNum 파라미터가 없을 경우를 대비하여 기본값 0 으로 지정
	@RequestMapping(value = "/board/list", method = RequestMethod.POST)
	public String listSearch(@RequestParam String searchType, @RequestParam String keyword, @RequestParam(defaultValue = "1") int pageNum, Model model) {
//		System.out.println("타입 : " + searchType);
//		System.out.println("검색어 : " + keyword);
		
		// ------------------------------------
		// 페이징 처리에 필요한 전체 게시물 수 조회 - getBoardListCount()
		// => 파라미터 : 없음, 리턴타입 : int(listCount)
		// => 게시물이 없을 경우 null 이 아닌 0 이 리턴되므로 Integer 대신 int 사용 가능
//		int listCount = service.getBoardListCount();
		
		// 검색어 기능을 포함할 경우 총 게시물 수를 조회 - getBoardListCount()
		// => 파라미터 : 검색타입, 검색어
		int listCount = service.getBoardListCount(searchType, "%" + keyword + "%");
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
		
		// 조회 시작 게시물 번호(행 번호) 계산
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
		// -------------------------
		// 검색 타입과 검색어를 활용하여 게시물 목록 조회하도록 파라미터 전달
		// => 단, 검색어(keyword)의 경우 앞뒤로 다른 문자를 포함 가능하도록 "%검색어%" 형태로 변환
		List<BoardVO> boardList = service.getBoardList(searchType, "%" + keyword + "%", pageInfo);
		
		// Model 객체에 게시물 목록과 페이징 처리 정보 저장
		model.addAttribute("boardList", boardList);
		model.addAttribute("pageInfo", pageInfo);
		// 검색타입과 검색어도 함께 전달
		model.addAttribute("searchType", searchType);
		model.addAttribute("keyword", keyword);
		
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
	// => 페이지 번호를 전달받아 저장하기 위해 int 타입 파라미터 선언
	@RequestMapping(value = "/board/delete", method = RequestMethod.POST)
	public String deletePost(@ModelAttribute BoardVO board, @RequestParam int pageNum, Model model) {
		// service 객체의 removeBoard() 메서드 호출하여 삭제 작업 요청
		// => 파라미터 : BoardVO 객체, 리턴타입 : int(deleteCount)
		// (Mapper 객체의 deleteBoard() 메서드를 통해 삭제 작업 수행)
		int deleteCount = service.removeBoard(board);
		
		// 삭제 실패(deleteCount 가 0) 시 "패스워드 틀림!" 메세지 저장 후 fail_back.jsp 로 포워딩
		// 아니면, qna_board_list.jsp 페이지로 포워딩(페이지번호 전달)
		if(deleteCount == 0) {
			model.addAttribute("msg", "패스워드 틀림!");
			return "fail_back";
		}
		
		// Redirect 방식 또는 Dispatcher 방식에 상관없이 전달할 데이터(파라미터)는 Model 객체 사용
		// => Redirect 방식일 경우 URL 의 파라미터 형태로 전달
		model.addAttribute("pageNum", pageNum);
		
		// board/list 요청 - Redirect 방식
		return "redirect:/board/list";
	}
	
	// 7. 글 수정 폼(modify - modify()) - GET
	// => qna_board_modify.jsp 
	@RequestMapping(value = "/board/modify", method = RequestMethod.GET)
	public String modify(@RequestParam int board_num, Model model) {
		// 글 상세정보 조회 작업을 재사용하여 수정할 내용 가져오기
		// Service 객체의 getBoardDetail() 메서드를 호출하여 게시물 상세 정보 조회
		// => 파라미터 : 글번호(board_num), 리턴타입 : BoardVO(board)
		BoardVO board = service.getBoardDetail(board_num);
		
		model.addAttribute("board", board);
		
		return "board/qna_board_modify";
	}
	
	// 8. 글 수정 비즈니스 로직(modify - modifyPost()) - POST
	// => service - modifyBoard(), mapper - deleteBoard()
	// => 수정 결과 판별(실패 시 "글 수정 실패" fail_back.jsp, 성공 시 detail)
	@RequestMapping(value = "/board/modify", method = RequestMethod.POST)
	public String modifyPost(@ModelAttribute BoardVO board, @RequestParam int pageNum, Model model) {
		// service 객체의 modifyBoard() 메서드 호출하여 수정 작업 요청
		// => 파라미터 : BoardVO 객체, 리턴타입 : int(updateCount)
		// (Mapper 객체의 updateBoard() 메서드를 통해 수정 작업 수행)
		int updateCount = service.modifyBoard(board);
		
		// 수정 실패(updateCount 가 0) 시 "패스워드 틀림!" 메세지 저장 후 fail_back.jsp 로 포워딩
		// 아니면, qna_board_list.jsp 페이지로 포워딩(페이지번호 전달)
		if(updateCount == 0) {
			model.addAttribute("msg", "패스워드 틀림!");
			return "fail_back";
		}
		
		// Redirect 방식 또는 Dispatcher 방식에 상관없이 전달할 데이터(파라미터)는 Model 객체 사용
		// => Redirect 방식일 경우 URL 의 파라미터 형태로 전달
		model.addAttribute("board_num", board.getBoard_num());
		model.addAttribute("pageNum", pageNum);
		
		// board/list 요청 - Redirect 방식
		return "redirect:/board/detail";
	}

	// 9. 답글 작성 폼(reply - reply()) - GET
	// => qna_board_reply.jsp 
	@RequestMapping(value = "/board/reply", method = RequestMethod.GET)
	public String reply(@RequestParam int board_num, Model model) {
		// 글 상세정보 조회 작업을 재사용하여 수정할 내용 가져오기
		// Service 객체의 getBoardDetail() 메서드를 호출하여 게시물 상세 정보 조회
		// => 파라미터 : 글번호(board_num), 리턴타입 : BoardVO(board)
		BoardVO board = service.getBoardDetail(board_num);
		System.out.println(board);
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
		if(insertCount == 0) {
			model.addAttribute("msg", "답글 등록 실패!");
			return "fail_back";
		}
		
		// board/list 요청 - Redirect 방식
		model.addAttribute("pageNum", pageNum);
		return "redirect:/board/list";
	}
	
}


```

- service
```java
-----------------------------------BoardService.java-----------------------------------
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

	// 2. 총 게시물 수 조회
//	public int getBoardListCount() {
//		return mapper.selectBoardListCount();
//	}
	
	public int getBoardListCount(String searchType, String keyword) {
		return mapper.selectBoardListCount(searchType, keyword);
	}

	// 3. 게시물 목록 조회
//	public List<BoardVO> getBoardList(PageInfo pageInfo) {
//		return mapper.selectBoardList(pageInfo);
//	}
	
	// 3-2. 게시물 목록 조회(검색 기능 추가)
	public List<BoardVO> getBoardList(String searchType, String keyword, PageInfo pageInfo) {
//		System.out.println(searchType + ", " + keyword);
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

- mapper
```java
-----------------------------------BoardMapper.java-----------------------------------
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
//	public int selectBoardListCount();
	// 3-2. 총 게시물 수 조회(검색기능 추가)
	public int selectBoardListCount(@Param("searchType") String searchType, @Param("keyword") String keyword);

	// 4. 게시물 목록 조회
//	public List<BoardVO> selectBoardList(PageInfo pageInfo);
	// 4-2. 게시물 목록 조회(검색기능 추가)
	// => Mapper 를 통한 파라미터가 복수개일 경우 @Param 어노테이션을 통해 파라미터 이름을 지정
	public List<BoardVO> selectBoardList(
			@Param("searchType") String searchType, @Param("keyword") String keyword, @Param("pageInfo") PageInfo pageInfo);
	
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
-----------------------------------BoardMapper.xml-----------------------------------
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
	
	<!-- 3. 총 게시물 수 조회 -->
<!-- 	<select id="selectBoardListCount" resultType="java.lang.Integer"> -->
<!-- 		SELECT COUNT(*) FROM board -->
<!-- 	</select> -->
	<!-- 3-2. 총 게시물 수 조회(검색 기능 추가) -->
	<select id="selectBoardListCount" resultType="java.lang.Integer">
		SELECT COUNT(*) FROM board
		<!-- choose 태그를 사용하여 if~else 또는 if~else if 문 구조 구현 -->
		<!-- 1. 키워드가 널스트링("")이 아니고, "subject" 일 경우 -->
		<!-- 2. 아니면, 키워드가 널스트링("")이 아니고, "content" 일 경우 -->
		<choose>
			<when test="!keyword.equals('') and searchType.equals('subject')">
				WHERE board_subject LIKE #{keyword}
			</when>
			<when test="!keyword.equals('') and searchType.equals('content')">
				WHERE board_content LIKE #{keyword}
			</when>
			<when test="!keyword.equals('') and searchType.equals('subject_content')">
				WHERE board_subject LIKE #{keyword} OR board_content LIKE #{keyword}
			</when>
			<when test="!keyword.equals('') and searchType.equals('name')">
				WHERE board_name LIKE #{keyword}
			</when>
		</choose>
	</select>
	
	<!-- 4. 게시물 목록 조회 -->
<!-- 	<select id="selectBoardList" resultType="com.itwillbs.mvc_board.vo.BoardVO"> -->
<!-- 		SELECT * FROM board -->
<!-- 		ORDER BY board_re_ref DESC, board_re_seq ASC -->
<!-- 		LIMIT #{startRow}, #{listLimit} -->
<!-- 	</select> -->

	<!-- 4-2. 게시물 목록 조회(검색기능 추가) -->
	<!-- 파라미터가 복수개일 때 @Param 어노테이션으로 지정된 이름을 사용하고, 객체는 객체명.변수명 사용 -->
	<select id="selectBoardList" resultType="com.itwillbs.mvc_board.vo.BoardVO">
		SELECT * FROM board
		<!-- if 태그를 사용하여 조건에 따른 구문 변화 -->
		<!-- 기본문법 : <if test="조건식">조건식 판별 결과가 true 일 때 사용할 문장</if> -->
		<!-- 1. 키워드가 널스트링("")이 아니고, "subject" 일 경우 WHERE 절 포함시키기 -->
		<!-- 단, if 태그는 단일 if 문과 동일하므로 else 또는 else if 판별 불가능 -->
<!-- 		<if test="!keyword.equals('') and searchType.equals('subject')"> -->
<!-- 			WHERE board_subject LIKE #{keyword} -->
<!-- 		</if> -->
		
		<!-- choose 태그를 사용하여 if~else 또는 if~else if 문 구조 구현 -->
		<!-- 1. 키워드가 널스트링("")이 아니고, "subject" 일 경우 -->
		<!-- 2. 아니면, 키워드가 널스트링("")이 아니고, "content" 일 경우 -->
		<choose>
			<when test="!keyword.equals('') and searchType.equals('subject')">
				WHERE board_subject LIKE #{keyword}
			</when>
			<when test="!keyword.equals('') and searchType.equals('content')">
				WHERE board_content LIKE #{keyword}
			</when>
			<when test="!keyword.equals('') and searchType.equals('subject_content')">
				WHERE board_subject LIKE #{keyword} OR board_content LIKE #{keyword}
			</when>
			<when test="!keyword.equals('') and searchType.equals('name')">
				WHERE board_name LIKE #{keyword}
			</when>
		</choose>
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
		SELECT * FROM board
		WHERE board_num = #{board_num}
	</select>
	
	<!-- 7. 글 삭제 -->
	<delete id="deleteBoard">
		DELETE FROM board
		WHERE board_num = #{board_num} AND board_pass = #{board_pass}
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
-----------------------------------qna_board_list.jsp-----------------------------------
<%@page import="java.util.ArrayList"%>
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%-- GET 방식으로 검색어가 전달될 경우 EL 의 ${param.파라미터명} 으로 접근하므로
Model 객체와 동일한 방법 ${파라미터명} 만으로 처리하기 위해
JSTL 의 c:set 태그를 사용하여 파라미터명으로 된 변수를 생성하기
--%>
<c:if test="${param.keyword ne null }">
	<c:set var="searchType" value="${param.searchType }"/>
	<c:set var="keyword" value="${param.keyword }"/>	
</c:if>
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
				<option value="subject" <c:if test="${searchType ne null and searchType eq 'subject'}">selected</c:if>>제목</option>
	            <option value="content"<c:if test="${searchType ne null and searchType eq 'content'}">selected</c:if>>내용</option>
	            <option value="subject_content"<c:if test="${searchType ne null and searchType eq 'subject_content'}">selected</c:if>>제목&내용</option>
	            <option value="name"<c:if test="${searchType ne null and searchType eq 'name'}">selected</c:if>>작성자</option>
			</select>
			<input type="text" name="keyword" value="<c:if test="${keyword ne null}">${keyword }</c:if>">
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
				<input type="button" value="이전" onclick="location.href='list?pageNum=${pageNum - 1}<c:if test="${keyword ne null }">&searchType=${searchType }&keyword=${keyword }</c:if>'">
			</c:when>
			<c:otherwise>
				<input type="button" value="이전" disabled="disabled">
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
					<a href="list.bo?pageNum=${i }<c:if test="${keyword ne null }">&searchType=${searchType }&keyword=${keyword }</c:if>">${i }</a>
				</c:otherwise>
			</c:choose>
		</c:forEach>

		<!-- 현재 페이지 번호(pageNum)가 총 페이지 수보다 작을 때만 [다음] 링크 동작 -->
		<c:choose>
			<c:when test="${pageNum < maxPage}">
				<input type="button" value="다음" onclick="location.href='list?pageNum=${pageNum + 1}<c:if test="${keyword ne null }">&searchType=${searchType }&keyword=${keyword }</c:if>'">
			</c:when>
			<c:otherwise>
				<input type="button" value="다음" disabled="disabled">
			</c:otherwise>
		</c:choose>

	</section>
</body>
</html>


```
