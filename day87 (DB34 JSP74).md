# [오전수업] DB 34차
## 고급 Subquery
### 단일행 subquery
- 하나의 행의 결과를 돌려주는 서브쿼리
- 서브쿼리가 사용된 경우 메인쿼리보다 우선 실행되어 그 결과를 메인쿼리에게 전달 후 메인쿼리가 실행된다.
- 주로 조건절에 사용되는 경우 비교연산자와 활용이 되는 편이다.
```sql

employee_id가 176인 사원의 job_id와 salary컬럼의 값이 고정적이라는 보장이 없으므로 이 값들을 직접 조회해서 메인 쿼리의 조건으로 사용하는 경우 구문이 실행되는 시점에 따라서 정확하지 않은 조건으로 구문이 실행될 수도 있다.
따라서 위의 경우에는 사용자가 직접 조건을 찾아서 값을 입력하기보다 구문이 실행되는 시점에서 필요한 조건값을 대신 출력해주는 메인 쿼리를 보조해주는 서브쿼리를 사용하는 것이 낫다.
SELECT last_name,
       job_id,
       salary
FROM employees
WHERE job_id = (
        SELECT job_id
        FROM employees
        WHERE employee_id = 176
    )
      AND salary > (
    SELECT salary
    FROM employees
    WHERE employee_id = 176
);


LAST_NAME|JOB_ID|SALARY|
---------+------+------+
Tucker   |SA_REP| 10000|
Bernstein|SA_REP|  9500|
Hall     |SA_REP|  9000|
King     |SA_REP| 10000|
Sully    |SA_REP|  9500|
McEwen   |SA_REP|  9000|
Vishney  |SA_REP| 10500|
Greene   |SA_REP|  9500|
Ozer     |SA_REP| 11500|
Bloom    |SA_REP| 10000|
Fox      |SA_REP|  9600|
Abel     |SA_REP| 11000|
Hutton   |SA_REP|  8800|

------------------------------------------------------------------------------------------------------

아래의 예문에서는 subquery에서 그룹을 사용한 그룹함수의 결과를 메인쿼리의 비교연산의 조건값으로 입력을 받았음.
이 경우 department_id로 그룹을 묶었을 때 1개의 그룹이 나오는 상황이 아니라면 서브쿼리가 돌려주는 값이 1개가 넘어가는 행을 돌려주게 되므로 비교연산자 '='가 조건값으로 받을 수 없게된다.

SELECT employee_id,
       last_name
FROM employees
WHERE salary = (
    SELECT MIN(salary)
    FROM employees
    GROUP BY department_id
);



SQL Error [1427] [21000]: ORA-01427: 단일 행 하위 질의에 2개 이상의 행이 리턴되었습니다.


-----------------------------------------------------------------------------------------------------


아래의 예문에서는 사원의 last_name이 'Haas'인 사원이 여럿인 경우를 제외하면 특별히 문제가 되지 않는 구문이지만 결과가 출력되지 않는다.
SELECT last_name,
       job_id
FROM employees
WHERE job_id = (
    SELECT job_id
    FROM employees
    WHERE last_name = 'Haas'
);

LAST_NAME|JOB_ID|
---------+------+

이유는 서브쿼리가 돌려주는 행이 없기 때문에 조건을 만족하는 행이 존재하지 않게된다.
SELECT job_id
FROM employees
WHERE last_name = 'Haas';


JOB_ID|
------+

```

### 다중행 비교연산자
- 여러 행의 결과를 받아서 비교연산을 할 수 있는 연산자

- IN	: 비교연산자 중 '='과 기능이 유사하나 여러 조건값을 받아 비교하여 조건을 만족하는 행을 출력해준다.
- ANY	: 비교연산자와 조합하여 사용하며 조건값들 중 하나라도 비교연산의 조건을 만족하는 경우 출력. SOME 연산자와 기능이 동일하다.
- ALL	: 비교연산자와 조합하여 사용하며 조건값들에 대해서 모두 비교연산의 조건을 만족하는 경우 출력

```sql
사원 중 job_id이 'IT_PROG'인 사원들의 급여값들에 대해서 더 작은 급여를 받으면서, job_id의 값이 'IT_PROG'가 아닌 조건을 만족하는 행을 출력.
SELECT employee_id,
       last_name,
       job_id,
       salary
FROM employees
WHERE salary < ANY (
    SELECT salary
    FROM employees
    WHERE job_id = 'IT_PROG'
)
      AND job_id <> 'IT_PROG';


EMPLOYEE_ID|LAST_NAME  |JOB_ID    |SALARY|
-----------+-----------+----------+------+
        175|Hutton     |SA_REP    |  8800|
        176|Taylor     |SA_REP    |  8600|
        177|Livingston |SA_REP    |  8400|
        206|Gietz      |AC_ACCOUNT|  8300|
        110|Chen       |FI_ACCOUNT|  8200|
        121|Fripp      |ST_MAN    |  8200|
        120|Weiss      |ST_MAN    |  8000|
        159|Smith      |SA_REP    |  8000|
        153|Olsen      |SA_REP    |  8000|
        122|Kaufling   |ST_MAN    |  7900|
        112|Urman      |FI_ACCOUNT|  7800|
        111|Sciarra    |FI_ACCOUNT|  7700|
        160|Doran      |SA_REP    |  7500|
        154|Cambrault  |SA_REP    |  7500|
        171|Smith      |SA_REP    |  7400|




```
![unnamed](https://user-images.githubusercontent.com/95197594/170614909-e2c314d8-f2d2-407d-ba6e-05c98e9108e2.png)

```sql
SELECT employee_id,
       last_name,
       job_id,
       salary
FROM employees
WHERE salary < ALL (
    SELECT salary
    FROM employees
    WHERE job_id = 'IT_PROG'
)
      AND job_id <> 'IT_PROG';

EMPLOYEE_ID|LAST_NAME  |JOB_ID  |SALARY|
-----------+-----------+--------+------+
        185|Bull       |SH_CLERK|  4100|
        192|Bell       |SH_CLERK|  4000|
        193|Everett    |SH_CLERK|  3900|
        188|Chung      |SH_CLERK|  3800|
        137|Ladwig     |ST_CLERK|  3600|
        189|Dilly      |SH_CLERK|  3600|
        141|Rajs       |ST_CLERK|  3500|
        186|Dellinger  |SH_CLERK|  3400|
        133|Mallin     |ST_CLERK|  3300|
        129|Bissot     |ST_CLERK|  3300|
        180|Taylor     |SH_CLERK|  3200|
        138|Stiles     |ST_CLERK|  3200|
        125|Nayer      |ST_CLERK|  3200|
        194|McCain     |SH_CLERK|  3200|
```

![unnamed (1)](https://user-images.githubusercontent.com/95197594/170614984-45a862ee-38af-4292-bf5b-cb438607b751.png)

---

# [오후수업] JSP 74차
- java 패키지
```java
----------------------------------------MemberController.java---------------------------------------
package com.itwillbs.mvc_board.controller;

import java.util.List;

import javax.servlet.http.HttpSession;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.stereotype.Service;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.ModelAttribute;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;

import com.itwillbs.mvc_board.service.MemberService;
import com.itwillbs.mvc_board.vo.MemberVO;

@Controller
public class MemberController {
	// Service 객체를 직접 생성하지 않고 자동 주입 기능을 위한 어노테이션 사용
	// => @Inject(자바-플랫폼공용) 또는 @Autowired(스프링 전용) 어노테이션 사용 가능
	// => 어노테이션 지정 후 자동 주입으로 객체를 생성 후 저장할 클래스 타입 변수 선언
	@Autowired
	private MemberService service;
	
	@RequestMapping(value = "/MemberJoinForm.me", method = RequestMethod.GET)
	public String join() {
		return "member/join_form";
	}
	
	@RequestMapping(value = "/MemberJoinPro.me", method = RequestMethod.POST)
	public String joinPost(@ModelAttribute MemberVO member, Model model) {
		// join_form.jsp 페이지에서 서블릿 요청 시 게시물 정보를 전달받아
		// 파라미터에 지정된 memberVO 타입 객체를 자동으로 생성하고 자동으로 저장
		// => 파라미터를 각각의 변수로 선언해도 되지만, VO 객체를 활용하면 훨씬 코드가 간결해짐
		
//		System.out.println("이름 : " + member.getName());
//		System.out.println("아이디 : " + member.getId());
//		System.out.println("비밀번호 : " + member.getPasswd());
//		System.out.println("이메일 : " + member.getEmail());
//		System.out.println("성별 : " + member.getGender());
		
		// MemberServiceImple 객체 생성 및 joinMember() 메서드 호출
//		MemberServiceImpl service = new MemberServiceImpl();
		// => @Autowired 어노테이션으로 객체 자동 주입되어 있으므로 객체 직접 생성 없이 사용 가능
		int insertCount = service.joinMember(member);
		
		if(insertCount == 0) { // 가입 실패
			System.out.println("가입 실패!");
			model.addAttribute("msg", "가입 실패!");
			// Dispatcher 방식으로 member/fail_back.jsp 페이지로 포워딩
			return "member/fail_back";
		} else { // 가입 성공
			System.out.println("가입 성공!");
			
			// 홈(index.jsp 페이지)으로 이동
			return "redirect:/";
		}
		
	}
	
	@RequestMapping(value = "/MemberCheckId.me", method = RequestMethod.GET)
	public String checkId() {
		return "member/check_id";
	}
	
	
	// 요청 서블릿 주소가 동일하더라도 메서드(요청 방식)가 다르면 구분 가능함
	@RequestMapping(value = "/MemberLogin.me", method = RequestMethod.GET)
	public String login() {
		return "member/login_form";	
	}
	
	@RequestMapping(value = "/MemberLogin.me", method = RequestMethod.POST)
	public String loginPost(@ModelAttribute MemberVO member, Model model, HttpSession session) {
		// login_form.jsp 페이지에서 서블릿 요청 시 폼파라미터 데이터(id, passwd)를 전달받아 
		// 자동으로 저장하기 위해 String id, String passwd 또는 MemberVO member 타입 변수 선언 필요
		
		// MemberService 객체의 loginMember() 메서드 호출
		// => 파라미터 : MemberVO 객체, 리턴타입 : MemberVO(memberResult)
		MemberVO memberResult = service.loginMember(member);
		
		// SELECT 구문을 통해 조회 결과를 리턴받았을 때
		// 리턴받은 객체(MemberVO)가 null 이면 로그인 실패, 아니면 성공
		// => 로그인 실패 시 Model 객체에 "로그인 실패!" 메세지 저장 후 fail_back.jsp 로 포워딩
		//	  아니면, 세션 객체에 id 값을 "sId" 라는 속성명으로 저장 후 메인페이지로 포워딩
		if(memberResult == null) { // 로그인 실패
			model.addAttribute("msg", "로그인 실패!");
			return "member/fail_back";
		} else { // 로그인 성공
			session.setAttribute("sId", memberResult.getId());
			return "redirect:/";
		}
		
	}
	
	// 요청 서블릿 주소("MemberLogout.me" - GET) 일 때 로그아웃 작업을 처리할 logout() 메서드 정의
	// => 메서드 파라미터로 세션 객체 전달받기
	// => 세션 객체 초기화 후 메인페이지로 이동
	@RequestMapping(value = "/MemberLogout.me", method = RequestMethod.GET)
	public String logout(HttpSession session) {
		// 세션 초기화
		session.invalidate();
		return "redirect:/";
	}
	
	// 요청 서블릿 주소("MemberInfo.me" - GET) 요청 처리할 getMemberInfo() 메서드 정의
	// => 세션 아이디 값을 가져오기 위해 HttpSession 타입 파라미터 선언
	@RequestMapping(value = "/MemberInfo.me", method = RequestMethod.GET)
		public String getMemberInfo(HttpSession session, Model model) {
		// 세션 아이디 가져와서 id 변수에 저장 후
		// 세션 아이디가 없을 경우 fail_back.jsp 이동 "잘못된 접근입니다" 를 출력 후 이전페이지로 돌아가기
		// 세션 아이디가 있을 경우 Service 객체의 getMemberInfo() 메서드 호출
		// => 파라미터 : 아이디, 리턴타입 : MemberVO(member)
		String id = (String)session.getAttribute("sId");
		if(id == null) {
			model.addAttribute("msg", "잘못된 접근입니다!");
			return "member/fail_back";
		} else {
			MemberVO member = service.getMemberInfo(id);
			// 회원 정보 출력을 위해 필요한 데이터(MemberVO 객체)를 Model 객체에 저장 후 
			// member 디렉토리의 member_info.jsp 페이지로 포워딩
			System.out.println(member.getName());
			model.addAttribute("member", member);
			return "member/member_info";
		}
		
	}
	
	@RequestMapping(value = "/AdminPage.me", method = RequestMethod.GET)
	public String admin(HttpSession session, Model model) {
		// 세션 아이디가 없거나 또는 세션 아이디가 "admin" 이 아닐 경우
		// => fail_back.jsp 페이지를 통해 "잘못된 접근입니다!" 출력 후 이전페이지로 돌아가기
		// 아니면, Service 객체의 getMemberList() 메서드를 통해 전체 회원 목록 조회 후
		// 조회된 객체 저장한 뒤 member)list.jsp 페이지로 포워딩
		String id = (String)session.getAttribute("sId");
		if(id == null || !id.equals("admin")) {
			model.addAttribute("msg", "잘못된 접근입니다!");
			return "member/fail_back";
		} else {
			// Service 객체의 getMemberList() 메서드를 호출하여 전체 회원 목록 조회
			// => 파라미터 : 없음, 리턴타입 : List<MemberVO>(memberList)
			List<MemberVO> memberList = service.getMemberList();
			model.addAttribute("memberList", memberList);
			return "member/member_list";
		}
		
	
	}
}
----------------------------------------MemberService.java---------------------------------------
package com.itwillbs.mvc_board.service;

import java.util.List;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import com.itwillbs.mvc_board.mapper.MemberMapper;
import com.itwillbs.mvc_board.vo.MemberVO;

// 서비스 클래스 용도의 클래스 표시를 위한 @Service 어노테이션 지정 => 객체 자동 주입 기능에 활용됨
@Service
public class MemberService {
	// SQL 구문 실행을 담당할 XXXMapper.xml 파일과 연동된 XXXMapper 객체 자동 주입 설정
	// MemberMapper 객체 자동 주입을 위한 어노테이션 설정
	@Autowired
	private MemberMapper mapper;
	
	// 1. 회원 가입
	public int joinMember(MemberVO member) {
//		System.out.println("joinMember");
		
		// Mapper 객체의 메서드를 호출하여 SQL 구문 실행 요청(DAO 객체 없이 실행)
		// => Mapper 객체의 메서드 실행 후 리턴되는 값을 직접 return 문제 사용하도록
		//    메서드 호출 코드 자체를 return 문 뒤에 바로 작성
		//    (리턴값이 없을 경우 메서드만 호출)
		// => 단, 추가작업이 필요한 경우에는 메서드 호출과 리턴값 리턴을 분리
		return mapper.insertMember(member);
	}

	// 2. 로그인
	public MemberVO loginMember(MemberVO member) {
		// Mapper 객체의 checkMember() 메서드 호출하여 로그인 작업 수행
//		MemberVO memberResult = mapper.checkMember(member);
//		return memberResult;
		
		return mapper.checkMember(member);
	}

	// 3. 회원 정보 조회
	public MemberVO getMemberInfo(String id) {
		// Mapper 객체의 selectMemberInfo() 메서드 호출하여 로그인 작업 수행
		return mapper.selectMemberInfo(id);
	}

	// 4.전체 회원 목록 조회
	public List<MemberVO> getMemberList() {
		return mapper.selectMemberList();
	}

	
	
}

----------------------------------------MemberMapper.java---------------------------------------
package com.itwillbs.mvc_board.mapper;

import java.util.List;

import com.itwillbs.mvc_board.vo.MemberVO;

// Service 객체에서 사용할(호출할) 메서드 형태를 추상메서드로 정의(DAO 클래스 대신)
// => 정의된 추상메서드는 XML(MemberMapper.xml 파일) 에서 활용됨
public interface MemberMapper {
	// 1. 회원 가입에 필요한 insertMember() 메서드 정의 
	// => 파라미터 : MemberVO(member), 리턴타입 : int
	public int insertMember(MemberVO member);
	
	// 2. 로그인에 필요한 checkMember() 메서드 정의
	// => 파라미터 : MemberVO(member), 리턴타입 : MemberVO
	public MemberVO checkMember(MemberVO member);

	// 3. 회원 정보 조회에 필요한 selectMemberInfo() 메서드 정의
	public MemberVO selectMemberInfo(String id);

	// 4. 전체 회원 목록 조회
	public List<MemberVO> selectMemberList();
}

```

- xml 
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd" >
<!-- mapper 태그 내에 namespace 속성 지정 후 Mapper 인터페이스 위치 지정 -->
<mapper namespace="com.itwillbs.mvc_board.mapper.MemberMapper">
	<!-- 실행할 SQL 구문을 태그 형식으로 작성(CRUD 작업에 해당하는 태그가 제공됨) -->
	<!-- 태그의 id 속성에 지정할 이름은 namespace 에서 지정한 인터페이스 내의 메서드명과 동일해야함 -->
	<!-- 태그 사이에 실제 SQL 구문을 작성 -->
	<!-- 만능문자 파라미터는 ? 대신 #{파라미터명} 형태로 지정(VO 객체의 변수명 활용) -->
	
	<!-- 1. 회원 가입 작업 수행을 위한 insert 태그 작성(id 는 MemberMapper 객체의 메서드명과 동일) -->
	<!-- 주의! 구문 내에 만능문자 사용 위치에 #{속성명} 지정 시 속성명은 VO 객체의 변수명과 동일 -->
	<insert id="insertMember">
		INSERT INTO member
		VALUES (null, #{name}, #{id}, #{passwd}, #{email}, #{gender}, now())
	</insert>
	
	<!-- 2. 로그인 작업 수행을 위한 select 태그 작성
	=> (MemberMapper 객체의 checkMember() 메서드명을 id 속성값으로 지정
	=> 사용할 파라미터는 MemberVO 객체의 멤버변수명과 동일한 이름 지정
	=> 단, SELECT 태그는 조회 결과를 저장할 객체의 타입을 resultType 속성을 통해 지정
	   주로, VO 클래스명 지정(패키지명 포함)
	   (조회된 레코드가 복수개일 경우 자동으로 List<MemberVO> 타입에 해당하는 객체 생성 후 저장)
	-->
	<select id="checkMember" resultType="com.itwillbs.mvc_board.vo.MemberVO">
		SELECT * FROM member
		WHERE id=#{id} AND passwd=#{passwd}
	</select>
	
	<!-- 3. 회원 정보 조회 작업 수행을 위한 select 태그 작성 - selectMemberInfo -->
	<select id="selectMemberInfo" resultType="com.itwillbs.mvc_board.vo.MemberVO">
		SELECT * FROM member
		WHERE id=#{id}
	</select>
	
	<!-- 4. 전체 회원 목록 조회 작업 수행을 위한 select 태그 - selectMemberList -->
	<!-- 전체 데이터에 대한 resultType 이 아닌 각각의 레코드에 대한 타입을 resultType 으로 설정 -->
	<!-- (조회된 레코드가 복수개일 경우 자동으로 List<MemberVO> 타입에 해당하는 객체 생성 후 저장 -->
	<select id="selectMemberList" resultType="com.itwillbs.mvc_board.vo.MemberVO">
		SELECT * FROM member;
	</select>
</mapper>

```

- jsp 
```jsp
----------------------------------------login_form.jsp----------------------------------------
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<h1>로그인</h1>
	<form action="MemberLogin.me" method="post">
		<table>
			<tr>
				<td>아이디</td>
				<td><input type="text" name="id" required="required" size="20"></td>
			</tr>
			<tr>
				<td>패스워드</td>
				<td><input type="text" name="passwd" required="required" size="20"></td>
			</tr>
			<tr>
				<td colspan="2" align="center">
					<input type="submit" value="로그인">
					<input type="button" value="회원가입" onclick="location.href='MemberJoinForm.me'">
				</td>
			</tr>
		</table>
	</form>
</body>
</html>
----------------------------------------member_info.jsp----------------------------------------
<%@page import="com.itwillbs.mvc_board.vo.MemberVO"%>
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
<body>
	<h1>회원 상세정보</h1>
		<table border="1">
			<!-- Model 객체로 전달받은 MemberVO 객체(member)에 접근하여 데이터 출력 -->
			<tr>
				<td>이름</td>
				<td>${member.name }</td>
			</tr>
			<tr>
				<td>ID</td>
				<td>${member.id }</td>
			</tr>
			<tr>
				<td>E-Mail</td>
				<td>${member.email }</td>
			</tr>
			<tr>
				<td>성별</td>
				<td>${member.gender }</td>
			</tr>
		</table>
</body>
</html>
----------------------------------------member_list.jsp----------------------------------------
<%@page import="com.itwillbs.mvc_board.vo.MemberVO"%>
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>    
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
<body>
	<h1>전체 회원 목록</h1>
		<table border="1">
			<tr>
				<th>번호</th><th>이름</th><th>아이디</th><th>E-mail</th><th>성별</th><th>가입일</th>
			</tr>
	<%--JSTL 의 forEach 문을 사용하여 Model 객체에 저장된 List<MEmberVO> 객체(memberList) 반복 --%>
	<%-- c:forEach var="변수명" items="${데이터저장객체명} --%>
	<c:forEach var="member" items="${memberList }">
			<tr>
				<td>${member.idx }</td>
				<td>${member.name }</td>
				<td>${member.id }</td>
				<td>${member.email }</td>
				<td>${member.gender }</td>
				<td>${member.date }</td>
			</tr>
		</c:forEach>
	</table>
</body>
</html>
----------------------------------------index.jsp----------------------------------------
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
	<h3><a href="./BoardWriteForm.bo">글쓰기</a></h3>
	<h3><a href="./BoardList.bo">글목록</a></h3>
</body>
</html>



```
