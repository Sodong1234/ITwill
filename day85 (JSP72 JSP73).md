# JSP 72, 73차

> - Spring_MVC_Board 프로젝트 생성
>   - 생성한 프로젝트 우클릭 -> Java Compiler -> version 1.8로 변경
>   - -> Project Facets -> Dynamic Web Module을 3.1, Java versio을 1.8로 변경
> - 과거에 이클립스에서 실습했던 MVC_Board의 board, member, upload 폴더 & index.jsp 파일을 복사해 현재 프로젝트의 view 폴더에 붙여넣기
> - com.itwillbs.mvc_board.dao /  com.itwillbs.mvc_board.svc /  com.itwillbs.mvc_board.vo 패키지 생성 후
>   - com.itwillbs.mvc_board.vo 에는 MVC_Board의 BoardDTO.java / MemberDTO.java / Pageinfo.java 파일 복사 후 붙여넣고, 이름을 BoardVO.java / MemberVO.java 로 변경
> - com.itwillbs.mvc_board 패키지에 BoardController.java / MemberController.java 파일 생성
> - 한글 깨짐 방지를 위하여 web.xml 수정
```xml
<!-- POST 방식 한글 처리를 위한 필터 설정 -->
	<filter>
		<filter-name>encodingFilter</filter-name>
		<filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
		<init-param>
			<param-name>encoding</param-name>
			<param-value>UTF-8</param-value>
		</init-param>
	</filter>
	<!-- POST 방식 한글 처리 필터와 서블릿 주소 패턴 매핑(모든 주소에 대해 UTF-8 필터링) -->
	<filter-mapping>
		<filter-name>encodingFilter</filter-name>
		<url-pattern>/*</url-pattern>
	</filter-mapping>	
  ```
> - mysql 활용을 위하여 pom.xml 수정
```xml
<!-- https://mvnrepository.com/artifact/mysql/mysql-connector-java -->
		<dependency>
			<groupId>mysql</groupId>
			<artifactId>mysql-connector-java</artifactId>
			<version>8.0.29</version>
		</dependency>
```

  
```java
* board 패키지
----------------------------------------BoardController.java----------------------------------------
package com.itwillbs.mvc_board;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;

@Controller
public class BoardController {
	
	// BoardWriteForm.bo 서블릿 요청에 대해 write() 메서드 정의
	// => board 디렉토리의 qna_board_write.jsp 페이지로 포워딩
	@RequestMapping(value = "/BoardWriteForm.bo", method = RequestMethod.GET)
	public String write() {
		return "member/qna_board_write";
	}

}

----------------------------------------MemberController.java----------------------------------------
package com.itwillbs.mvc_board;

import javax.servlet.http.HttpSession;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.ModelAttribute;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;

import com.itwillbs.mvc_board.svc.MemberServiceImpl;
import com.itwillbs.mvc_board.vo.MemberVO;

@Controller
public class MemberController {
	@RequestMapping(value = "/MemberJoinForm.me", method = RequestMethod.GET)
	public String join() {
		return "member/join_form";
	}
	
	@RequestMapping(value = "/MemberLoginForm.me", method = RequestMethod.GET)
	public String login() {
		return "member/login_form";
	}
	
	@RequestMapping(value = "/MemberJoinPro.me", method = RequestMethod.POST)
	public String joinPost(@ModelAttribute MemberVO member) {
//		System.out.println("이름 : " + member.getName());
//		System.out.println("아이디 : " + member.getId());
//		System.out.println("비밀번호 : " + member.getPasswd());
//		System.out.println("이메일 : " + member.getEmail());
//		System.out.println("성별 : " + member.getGender());
		
		// MemberServiceImple 객체 생성 및 joinMember() 메서드 호출
		MemberServiceImpl service = new MemberServiceImpl();
		service.joinMember(member);
		
		// 홈(index.jsp 페이지)으로 이동
		return "redirect:/";
	}
	
	@RequestMapping(value = "/MemberCheckId.me", method = RequestMethod.GET)
	public String checkId() {
		return "member/check_id";
	}
	
	@RequestMapping(value = "/MemberLoginPro.me", method = RequestMethod.POST)
	public String loginPost(@ModelAttribute MemberVO member, Model model, HttpSession session) {
//		System.out.println("이름 : " + member.getName());
		System.out.println("아이디 : " + member.getId());
		System.out.println("비밀번호 : " + member.getPasswd());
//		System.out.println("이메일 : " + member.getEmail());
//		System.out.println("성별 : " + member.getGender());
		
		// MemberServiceImple 객체 생성 및 joinMember() 메서드 호출
		MemberServiceImpl service = new MemberServiceImpl();
		MemberVO memberResult = service.loginMember(member);
		
		// 리턴받은 MemberVO 객체가 null 일 경우 로그인 실패, 아니면 성공
		// => 만약, 로그인 실패 시 Model 객체에 "msg" 라는 속성명으로 "로그인 실패!" 문자열 저장 후
		//	  member/fail_back.jsp 페이지로 포워딩
		//	  (Model 객체를 메서드 파라미터로 전달받아야 함)
		// => 아니면, 세션 객체에 "sId" 라는 속성명으로 로그인 성공한 ID 값을 저장 후 홈으로 이동
		//	  (HttpSession 객체를 메서드 파라미터로 전달받아야 함)

		if(memberResult == null) { // 로그인 실패
			model.addAttribute("msg", "로그인 실패!");
			// Dispatcher 방식으로 member/fail_back.jsp 페이지로 포워딩
			return "member/fail_back";
		} else { // 로그인 성공
			session.setAttribute("sId", memberResult.getId());
			// 홈(index.jsp 페이지)으로 이동
			return "redirect:/";
		}
		
		
	}
}


```

```java
* dao 패키지
----------------------------------------MemberDAO.java----------------------------------------
package com.itwillbs.mvc_board.dao;

import com.itwillbs.mvc_board.vo.MemberVO;

// DAO 클래스 역할의 서브클래스가 정의해야할 메서드를 추상메서드 형태로 제공해주는 인터페이스 정의
// => MemberDAOImpl 클래스에서 실제 작업(비즈니스 로직) 수행을 하기 위한 가이드라인 제공
public interface MemberDAO {

	// 회원 추가 작업을 수행할 insertMember() 메서드의 추상 메서드 정의
	// => 파라미터 : MemberVO(member)	리턴타입 : void
	public void insertMember(MemberVO member);
	
	// 로그인 작업을 수행할 checkMember() 메서드 정의
	// 파리미터 : MemberVO, 리턴타입 : MemberVO
	public MemberVO checkMember(MemberVO member);
}

----------------------------------------MemberDAOImpl.java----------------------------------------
package com.itwillbs.mvc_board.dao;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;

import com.itwillbs.mvc_board.vo.MemberVO;

public class MemberDAOImpl implements MemberDAO {

	public Connection getConnection() {
		Connection con = null;
		
		try {
			String driver = "com.mysql.cj.jdbc.Driver";
			String url = "jdbc:mysql://localhost:3306/mvc_board3";
			String user = "root";
			String password = "1234";

			// 1단계. 드라이버 로드
			Class.forName(driver);

			// 2단계. DB 연결
			con = DriverManager.getConnection(url, user, password);
			System.out.println("DB 연결 성공!");
			
		} catch (ClassNotFoundException e) {
			e.printStackTrace();
		} catch (SQLException e) {
			e.printStackTrace();
		}
		
		return con;
	}

	public void close(Connection con) {
		try {
			con.close();
		} catch (SQLException e) {
			e.printStackTrace();
		}
	}
	
	public void close(PreparedStatement pstmt) {
		try {
			pstmt.close();
		} catch (SQLException e) {
			e.printStackTrace();
		}
	}
	
	public void close(ResultSet rs) {
		try {
			rs.close();
		} catch (SQLException e) {
			e.printStackTrace();
		}
	}
	
	@Override
	public void insertMember(MemberVO member) {
		// getConnection() 메서드를 호출하여 Connection 객체 가져오기
		Connection con = getConnection();
		
		
		PreparedStatement pstmt = null;

		try {
			String sql = "INSERT INTO member VALUES(null,?,?,?,?,?,now())";
			pstmt = con.prepareStatement(sql);
			pstmt.setString(1, member.getName());
			pstmt.setString(2, member.getId());
			pstmt.setString(3, member.getPasswd());
			pstmt.setString(4, member.getEmail());
			pstmt.setString(5, member.getGender());

			pstmt.executeUpdate();
		} catch (SQLException e) {
			System.out.println("insertMember() 메서드 오류");
			e.printStackTrace();
		} finally {
			close(pstmt);
			close(con);
		}
	}

	// 로그인 작업 수행 후 성공 시 조회된 데이터를 MemberVO 객체에 저장하여 리턴
	@Override
	public MemberVO checkMember(MemberVO member) {
		MemberVO memberResult = null;
		
		Connection con = getConnection();
		PreparedStatement pstmt = null;
		ResultSet rs = null;

		try {
			String sql = "SELECT * FROM member WHERE id=? AND passwd=?";
			pstmt = con.prepareStatement(sql);
			pstmt.setString(1, member.getId());
			pstmt.setString(2, member.getPasswd());
			rs = pstmt.executeQuery();

			if (rs.next()) { // 로그인 성공 시
				// MemberVO 객체 생성 및 조회 결과 저장
				memberResult = new MemberVO();
				memberResult.setIdx(rs.getInt("idx"));
				memberResult.setName(rs.getString("name"));
				memberResult.setId(rs.getString("id"));
				memberResult.setPasswd(rs.getString("passwd"));
				memberResult.setEmail(rs.getString("email"));
				memberResult.setGender(rs.getString("gender"));
				memberResult.setDate(rs.getDate("date"));


			}
		} catch (SQLException e) {
			System.out.println("checkMember() 메서드 오류!");
			e.printStackTrace();
		} finally {
			close(rs);
			close(pstmt);
			close(con);
		}
		
		
		return memberResult;
	}

}


```

```java
* svc 패키지
----------------------------------------MemberService.java----------------------------------------
package com.itwillbs.mvc_board.svc;

import com.itwillbs.mvc_board.vo.MemberVO;

// 서비스 클래스 역할의 서브클래스가 정의해야할 메서드를 추상메서드 형태로 제공해주는 인터페이스 정의
// => MemberServiceImpl 클래스에서 실제 작업(비즈니스 로직) 수행을 하기 위한 가이드라인 제공
public interface MemberService {
	// 추상메서드 정의
	// 1. 회원 가입 작업을 수행할 joinMember() 메서드 정의
	// => 파라미터 : MemberVO(member)	리턴타입 : void
	public void joinMember(MemberVO member);
	
	// 1. 회원 로그인 작업을 수행할 loginMember() 메서드 정의
	// => 파라미터 : MemberVO(member)	리턴타입 : MemberVO
	public MemberVO loginMember(MemberVO member);
}

----------------------------------------MemberServiceImpl.java----------------------------------------
package com.itwillbs.mvc_board.dao;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;

import com.itwillbs.mvc_board.vo.MemberVO;

public class MemberDAOImpl implements MemberDAO {

	public Connection getConnection() {
		Connection con = null;
		
		try {
			String driver = "com.mysql.cj.jdbc.Driver";
			String url = "jdbc:mysql://localhost:3306/mvc_board3";
			String user = "root";
			String password = "1234";

			// 1단계. 드라이버 로드
			Class.forName(driver);

			// 2단계. DB 연결
			con = DriverManager.getConnection(url, user, password);
			System.out.println("DB 연결 성공!");
			
		} catch (ClassNotFoundException e) {
			e.printStackTrace();
		} catch (SQLException e) {
			e.printStackTrace();
		}
		
		return con;
	}

	public void close(Connection con) {
		try {
			con.close();
		} catch (SQLException e) {
			e.printStackTrace();
		}
	}
	
	public void close(PreparedStatement pstmt) {
		try {
			pstmt.close();
		} catch (SQLException e) {
			e.printStackTrace();
		}
	}
	
	public void close(ResultSet rs) {
		try {
			rs.close();
		} catch (SQLException e) {
			e.printStackTrace();
		}
	}
	
	@Override
	public void insertMember(MemberVO member) {
		// getConnection() 메서드를 호출하여 Connection 객체 가져오기
		Connection con = getConnection();
		
		
		PreparedStatement pstmt = null;

		try {
			String sql = "INSERT INTO member VALUES(null,?,?,?,?,?,now())";
			pstmt = con.prepareStatement(sql);
			pstmt.setString(1, member.getName());
			pstmt.setString(2, member.getId());
			pstmt.setString(3, member.getPasswd());
			pstmt.setString(4, member.getEmail());
			pstmt.setString(5, member.getGender());

			pstmt.executeUpdate();
		} catch (SQLException e) {
			System.out.println("insertMember() 메서드 오류");
			e.printStackTrace();
		} finally {
			close(pstmt);
			close(con);
		}
	}

	// 로그인 작업 수행 후 성공 시 조회된 데이터를 MemberVO 객체에 저장하여 리턴
	@Override
	public MemberVO checkMember(MemberVO member) {
		MemberVO memberResult = null;
		
		Connection con = getConnection();
		PreparedStatement pstmt = null;
		ResultSet rs = null;

		try {
			String sql = "SELECT * FROM member WHERE id=? AND passwd=?";
			pstmt = con.prepareStatement(sql);
			pstmt.setString(1, member.getId());
			pstmt.setString(2, member.getPasswd());
			rs = pstmt.executeQuery();

			if (rs.next()) { // 로그인 성공 시
				// MemberVO 객체 생성 및 조회 결과 저장
				memberResult = new MemberVO();
				memberResult.setIdx(rs.getInt("idx"));
				memberResult.setName(rs.getString("name"));
				memberResult.setId(rs.getString("id"));
				memberResult.setPasswd(rs.getString("passwd"));
				memberResult.setEmail(rs.getString("email"));
				memberResult.setGender(rs.getString("gender"));
				memberResult.setDate(rs.getDate("date"));


			}
		} catch (SQLException e) {
			System.out.println("checkMember() 메서드 오류!");
			e.printStackTrace();
		} finally {
			close(rs);
			close(pstmt);
			close(con);
		}
		
		
		return memberResult;
	}

}


```

```jsp
----------------------------------------fail_back.jsp----------------------------------------
<%@page import="org.springframework.ui.Model"%>
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<script type="text/javascript">
		// 전달받은 오류 메세지(msg) 출력 후 이전페이지로 돌아가기
		alert("${msg}");
		history.back();
	</script>
</body>
</html>
```

---

## MyBatis
[ Spring - MyBatis ]
- SQL 매핑 프레임워크라고도 함
- Spring 을 활용하여 JDBC 애플리케이션을 구현 시 코드의 복잡한 구조를 단순화하는데 활용하는 프레임워크
	- 장점1) 자동 Connection 객체 close
	- 장점2) 내부적으로 PreparedStatement 객체를 사용한 처리(자동)
	- 장점3) 리턴 타입 지정을 통해 자동으로 객체 생성 및 ResultSet 객체에 대한 처리
---
[ MyBatis 를 활용한 데이터베이스 연동 애플리케이션 개발 과정 ]
1. pom.xml 라이브러리 추가
2. root-context.xml 설정
3. servlet-context.xml 설정
---
< Mapper 정의 >
- SQL 구문 작성 및 처리를 수행
- 인터페이스와 어노테이션을 활용 또는 XML Mapper 활용 방식으로 나뉨
  => 주로 사용하는 방식인 XML Mapper 활용 방식으로 수업
---
> - Window - Preference - XML - XML Catalog - Add 에서 Location 란에 http://mybatis.org/dtd/mybatis-3-mapper.dtd 입력 / Key 란에 -//mybatis.org//DTD Mapper 3.0//EN 입력
> - Window - Preference - XML - XML Catalog - Add 에서 Location 란에 http://mybatis.org/dtd/mybatis-3-config.dtd 입력 / Key 란에 -//mybatis.org//DTD Config 3.0//EN 입력
> - Spring_MVC_Board2 프로젝트 생성
> - Spring_MVC_Board의 board, member 폴더 & index.jsp 파일을 복사해 현재 프로젝트의 view 폴더에 붙여넣기
> - com.itwillbs.mvc_board.dao /  com.itwillbs.mvc_board.svc / com.itwillbs.mvc_board.vo / com.itwillbs.mvc_board.contoller / com.itwillbs.mvc_board.mapper 패키지 생성 후
>   - com.itwillbs.mvc_board.vo 에는 Spring_MVC_Board 프로젝트의 BoardVO.java / MemberVO.java / Pageinfo.java 파일 복사 후 붙여넣기 실시
>   - com.itwillbs.mvc_board 에는 Spring_MVC_Board 프로젝트의 HomeController.java의 내용물과 같이 수정
>   - com.itwillbs.mvc_board.controller 에는 Spring_MVC_Board 프로젝트의 MemberController.java를 복사 후 붙여넣기 실시
>   - com.itwillbs.mvc_board.mapper 에는 boardMapper.java / memberMapper.java 파일 생성
>   - com.itwillbs.mvc_board.service 에는 memberService.java 파일 생성
> - /src/main/resources 에 com.itwillbs.mvc_board.mapper 패키지 생성 후 
>   - XML Files -> 제목에 MemberMapper.xml 입력 -> Next -> Create file using a DTD or XML Schema file 체크 후 Next -> Select XML Catalog entry 체크 후 위에서 생성한 mapper.dtd 선택 후 Finish
> - src/main/resources의 META-INF 폴더에 mybatis-config.xml 파일 생성
>   - XML Files -> 제목에 mybatis-config.xml 입력 -> Next -> Create file using a DTD or XML Schema file 체크 후 Next -> Select XML Catalog entry 체크 후 위에서 생성한 config.dtd 선택 후 Finish
> - 한글 깨짐 방지를 위하여 web.xml 수정
```xml
<!-- POST 방식 한글 처리를 위한 필터 설정 -->
	<filter>
		<filter-name>encodingFilter</filter-name>
		<filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
		<init-param>
			<param-name>encoding</param-name>
			<param-value>UTF-8</param-value>
		</init-param>
	</filter>
	<!-- POST 방식 한글 처리 필터와 서블릿 주소 패턴 매핑(모든 주소에 대해 UTF-8 필터링) -->
	<filter-mapping>
		<filter-name>encodingFilter</filter-name>
		<url-pattern>/*</url-pattern>
	</filter-mapping>	
  ```
> - mybatis 활용을 위하여 pom.xml 수정
```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/maven-v4_0_0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<groupId>com.itwillbs</groupId>
	<artifactId>mvc_board</artifactId>
	<name>Spring_MVC_Board2</name>
	<packaging>war</packaging>
	<version>1.0.0-BUILD-SNAPSHOT</version>
	<properties>
		<java-version>1.8</java-version>
		<org.springframework-version>5.3.18</org.springframework-version>
		<org.aspectj-version>1.6.10</org.aspectj-version>
		<org.slf4j-version>1.6.6</org.slf4j-version>
	</properties>
	<dependencies>
		<!-- Spring -->
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-context</artifactId>
			<version>${org.springframework-version}</version>
			<exclusions>
				<!-- Exclude Commons Logging in favor of SLF4j -->
				<exclusion>
					<groupId>commons-logging</groupId>
					<artifactId>commons-logging</artifactId>
				</exclusion>
			</exclusions>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-webmvc</artifactId>
			<version>${org.springframework-version}</version>
		</dependency>

		<!-- AspectJ -->
		<dependency>
			<groupId>org.aspectj</groupId>
			<artifactId>aspectjrt</artifactId>
			<version>${org.aspectj-version}</version>
		</dependency>

		<!-- Logging -->
		<dependency>
			<groupId>org.slf4j</groupId>
			<artifactId>slf4j-api</artifactId>
			<version>${org.slf4j-version}</version>
		</dependency>
		<dependency>
			<groupId>org.slf4j</groupId>
			<artifactId>jcl-over-slf4j</artifactId>
			<version>${org.slf4j-version}</version>
			<scope>runtime</scope>
		</dependency>
		<dependency>
			<groupId>org.slf4j</groupId>
			<artifactId>slf4j-log4j12</artifactId>
			<version>${org.slf4j-version}</version>
			<scope>runtime</scope>
		</dependency>
		<dependency>
			<groupId>log4j</groupId>
			<artifactId>log4j</artifactId>
			<version>1.2.15</version>
			<exclusions>
				<exclusion>
					<groupId>javax.mail</groupId>
					<artifactId>mail</artifactId>
				</exclusion>
				<exclusion>
					<groupId>javax.jms</groupId>
					<artifactId>jms</artifactId>
				</exclusion>
				<exclusion>
					<groupId>com.sun.jdmk</groupId>
					<artifactId>jmxtools</artifactId>
				</exclusion>
				<exclusion>
					<groupId>com.sun.jmx</groupId>
					<artifactId>jmxri</artifactId>
				</exclusion>
			</exclusions>
			<scope>runtime</scope>
		</dependency>

		<!-- @Inject -->
		<dependency>
			<groupId>javax.inject</groupId>
			<artifactId>javax.inject</artifactId>
			<version>1</version>
		</dependency>

		<!-- Servlet -->
		<dependency>
			<groupId>javax.servlet</groupId>
			<artifactId>servlet-api</artifactId>
			<version>2.5</version>
			<scope>provided</scope>
		</dependency>
		<dependency>
			<groupId>javax.servlet.jsp</groupId>
			<artifactId>jsp-api</artifactId>
			<version>2.1</version>
			<scope>provided</scope>
		</dependency>
		<dependency>
			<groupId>javax.servlet</groupId>
			<artifactId>jstl</artifactId>
			<version>1.2</version>
		</dependency>

		<!-- https://mvnrepository.com/artifact/mysql/mysql-connector-java -->
		<!-- MySQL 을 통해 JDBC 활용에 필요한 라이브러리 추가 -->
		<dependency>
			<groupId>mysql</groupId>
			<artifactId>mysql-connector-java</artifactId>
			<version>8.0.29</version>
		</dependency>

		<!-- Tomcat-DBCP 대용으로 사용 가능한 커넥션풀 -->
		<!-- HikariCP 라이브러리 사용(Tomcat DBCP 에 비해 가볍고 빠르고 안정적인 라이브러리) -->
		<!-- mvcrepository.com 대신 https://github.com/brettwooldridge/HikariCP 링크 
			접속 -->
		<!-- Java 8 버전에 적합한 4.0.3 버전 라이브러리 추가 -->
		<dependency>
			<groupId>com.zaxxer</groupId>
			<artifactId>HikariCP</artifactId>
			<version>4.0.3</version>
		</dependency>

		<!-- JDBC 연동 등에 필요한 기능을 제공하는 spring-jdbc 라이브러리(기존 스프링 버전 연동) -->
		<!-- https://mvnrepository.com/artifact/org.springframework/spring-jdbc -->
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-jdbc</artifactId>
			<version>${org.springframework-version}</version>
		</dependency>


		<!-- MyBatis 활용에 필요한 라이브러리 추가 -->
		<!-- https://mvnrepository.com/artifact/org.mybatis/mybatis -->
		<!-- 1. MyBatis 기본 라이브러리 -->
		<dependency>
			<groupId>org.mybatis</groupId>
			<artifactId>mybatis</artifactId>
			<version>3.5.10</version>
		</dependency>

		<!-- https://mvnrepository.com/artifact/org.mybatis/mybatis-spring -->
		<!-- 2. MyBatis - Spring 연동 라이브러리 추가 -->
		<dependency>
			<groupId>org.mybatis</groupId>
			<artifactId>mybatis-spring</artifactId>
			<version>2.0.7</version>
		</dependency>


		<!-- Test -->
		<dependency>
			<groupId>junit</groupId>
			<artifactId>junit</artifactId>
			<version>4.7</version>
			<scope>test</scope>
		</dependency>
	</dependencies>
	<build>
		<plugins>
			<plugin>
				<artifactId>maven-eclipse-plugin</artifactId>
				<version>2.9</version>
				<configuration>
					<additionalProjectnatures>
						<projectnature>org.springframework.ide.eclipse.core.springnature</projectnature>
					</additionalProjectnatures>
					<additionalBuildcommands>
						<buildcommand>org.springframework.ide.eclipse.core.springbuilder</buildcommand>
					</additionalBuildcommands>
					<downloadSources>true</downloadSources>
					<downloadJavadocs>true</downloadJavadocs>
				</configuration>
			</plugin>
			<plugin>
				<groupId>org.apache.maven.plugins</groupId>
				<artifactId>maven-compiler-plugin</artifactId>
				<version>2.5.1</version>
				<configuration>
					<source>1.6</source>
					<target>1.6</target>
					<compilerArgument>-Xlint:all</compilerArgument>
					<showWarnings>true</showWarnings>
					<showDeprecation>true</showDeprecation>
				</configuration>
			</plugin>
			<plugin>
				<groupId>org.codehaus.mojo</groupId>
				<artifactId>exec-maven-plugin</artifactId>
				<version>1.2.1</version>
				<configuration>
					<mainClass>org.test.int1.Main</mainClass>

				</configuration>


			</plugin>
		</plugins>
	</build>
</project>


```

> - root-context.xml 수정
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:mybatis-spring="http://mybatis.org/schema/mybatis-spring"
	xmlns:context="http://www.springframework.org/schema/context"
	xsi:schemaLocation="http://mybatis.org/schema/mybatis-spring http://mybatis.org/schema/mybatis-spring-1.2.xsd
		http://www.springframework.org/schema/beans https://www.springframework.org/schema/beans/spring-beans.xsd
		http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.3.xsd">
	<!-- Root Context: defines shared resources visible to all other web components -->
		
	<!-- HiKariCP 라이브러리 정보 설정 -->
	<!-- 1. HikariCP 를 활용한 DB 접속 정보 설정을 위한 HikariConfig 객체 설정(JDBC 기본 설정) -->
	<bean id="hikariConfig" class="com.zaxxer.hikari.HikariConfig">
		<property name="driverClassName" value="com.mysql.cj.jdbc.Driver"></property>
		<property name="jdbcUrl" value="jdbc:mysql://localhost:3306/mvc_board3"></property>
		<property name="username" value="root"></property>
		<property name="password" value="1234"></property>		
	</bean>
	
	<!-- 2. HikariCP 의 DataSource 객체 생성을 위해 HikariConfig 객체 전달 -->
	<bean id="dataSource" class="com.zaxxer.hikari.HikariDataSource" destroy-method="close">
		<constructor-arg ref="hikariConfig"></constructor-arg>
	</bean>	
	
	<!-- Connection 객체 생성 및 SQL 전달, 결과 리턴 등의 작업을 수행할 SQLSessionFactory 객체 설정 -->
	<!-- MyBatis - Spring 연결 담당하며, 내부적으로 SQLSession 객체를 통해 작업 수행 -->
	<!-- 주의! 이전에 DataSource 객체 설정이 완료되어 있어야 함 -->
	<bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
		<property name="dataSource" ref="dataSource"></property>
		<property name="configLocation" value="classpath:/mybatis-config.xml"></property>
		<property name="mapperLocations" value="classpath:/com/itwillbs/mvc_board/mapper/*Mapper.xml"></property>
	</bean>
	
	<!-- myBatis 연동에 사용될 객체들의 패키지 위치 지정(기본 루트 패키지 지정) -->
	<mybatis-spring:scan base-package="com.itwillbs.mvc_board"/>
	
</beans>

```
> - servlet-context.xml 파일에 다음과 같은 내용 추가
```xml
<!-- 컨트롤러(@Controller) 를 비롯한 어노테이션을 통해 자동 주입될 클래스가 포함된 패키지 지정 -->
	<context:component-scan base-package="com.itwillbs.mvc_board" />
	<context:component-scan base-package="com.itwillbs.mvc_board.controller" />
	<context:component-scan base-package="com.itwillbs.mvc_board.service" />
```

---

```java
* board 패키지
-----------------------------------------HomeController.java-----------------------------------------
package com.itwillbs.mvc_board;

import java.text.DateFormat;
import java.util.Date;
import java.util.Locale;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;

@Controller
public class HomeController {
	
	private static final Logger logger = LoggerFactory.getLogger(HomeController.class);
	
	@RequestMapping(value = "/", method = RequestMethod.GET)
	public String home() {
		return "index";
	}
	
}

```
```java
* service 패키지
-----------------------------------------memberService.java-----------------------------------------
package com.itwillbs.mvc_board.service;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import com.itwillbs.mvc_board.mapper.memberMapper;
import com.itwillbs.mvc_board.vo.MemberVO;

// 서비스 클래스 용도의 클래스 표시를 위한 @Service 어노테이션 지정 => 객체 자동 주입 기능에 활용됨
@Service
public class memberService {
	// SQL 구문 실행을 담당할 XXXMapper.xml 파일과 연동된 XXXMapper 객체 자동 주입 설정
	// MemberMapper 객체 자동 주입을 위한 어노테이션 설정
	@Autowired
	private memberMapper mapper;
	
	// 1. 회원가입
	public int joinMember(MemberVO member) {
//		System.out.println("joinMember");
		
		// Mapper 객체의 메서드를 호출하여 SQL 구문 실행 요청(DAO 객체 없이 실행)
		// => Mapper 객체의 메서드 실행 후 리턴되는 값을 직접 return 문에 사용하도록
		//	  메서드 호출 코드 자체를 return 문 뒤에 바로 작성
		//	  (리턴값이 없을 경우 메서드만 호출)
		// => 단, 추가작업이 필요한 경우에는 메서드 호출과 리턴값 리턴을 분리
		return mapper.insertMember(member);
		
	}
	
	
//	public MemberVO loginMember(MemberVO member) {
//		
//		return memberResult;
//	}
}
```

```
```java
* controller 패키지
-----------------------------------------MemberController.java-----------------------------------------
package com.itwillbs.mvc_board.controller;

import javax.servlet.http.HttpSession;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.ModelAttribute;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;

import com.itwillbs.mvc_board.service.memberService;
import com.itwillbs.mvc_board.vo.MemberVO;

@Controller
public class MemberController {
	// Service 객체를 직접 생성하지 않고 자동 주입 기능을 위한 어노테이션 사용
	// => @Inject(자바-플랫폼공용) 또는 @Autowired(스프링 전용) 어노테이션 사용 가능
	// => 어노테이션 지정 후 자동 주입으로 객체를 생성 후 저장할 클래스 타입 변수 선언
	@Autowired
	private memberService service;
	
	
	
	@RequestMapping(value = "/MemberJoinForm.me", method = RequestMethod.GET)
	public String join() {
		return "member/join_form";
	}
	
	@RequestMapping(value = "/MemberLoginForm.me", method = RequestMethod.GET)
	public String login() {
		return "member/login_form";
	}
	
	@RequestMapping(value = "/MemberJoinPro.me", method = RequestMethod.POST)
	public String joinPost(@ModelAttribute MemberVO member, Model model) {
		System.out.println("이름 : " + member.getName());
		System.out.println("아이디 : " + member.getId());
		System.out.println("비밀번호 : " + member.getPasswd());
		System.out.println("이메일 : " + member.getEmail());
		System.out.println("성별 : " + member.getGender());
		
		// MemberServiceImple 객체 생성 및 joinMember() 메서드 호출
//		MemberServiceImpl service = new MemberServiceImpl();
		// => @Autowired 어노테이션으로 객체 자동 주입되어 있으므로 객체 생성 없이 그대로 사용 가능
		int insertCount = service.joinMember(member);
		
		if(insertCount == 0) { // 가입 실패
			System.out.println("가입 실패!");
			model.addAttribute("msg", "로그인 실패!");
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
}


```

```java
* mapper 패키지
-----------------------------------------memberMapper.java-----------------------------------------
package com.itwillbs.mvc_board.mapper;

import com.itwillbs.mvc_board.vo.MemberVO;

// Service 객체에서 사용할(호출할) 메서드 형태를 추상메서드로 정의(DAO 클래스 대신)
// => 정의된 추상메서드는 XML(MemberMapper.xml 파일) 에서 활용됨
public interface memberMapper {
	// 1. 회원 가입에 필요한 insertMember() 메서드 정의
	// => 파리미터 : MemberVO(member),	 리턴타입 : int
	public int insertMember(MemberVO member);
	
	// 2. 로그인에 필요한 checkMember() 메서드 정의
	// => 파라미터 : MemberVO(member),	 리턴타입 : MemberVO
	public MemberVO checkMember(MemberVO member);
}


-----------------------------------------boardMapper.java-----------------------------------------
package com.itwillbs.mvc_board.mapper;

public interface boardMapper {

}

```

```xml
-----------------------------------------MemberMapper.xml-----------------------------------------
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd" >
<!-- mapper 태그 내에 namespace 속성 지정 후 Mapper 인터페이스 위치 지정 -->
<mapper namespace="com.itwillbs.mvc_board.mapper.MemberMapper">
	<!-- 실행할 SQL 구문을 태그 형식으로 작성(CRUD 작업에 해당하는 태그가 제공됨 -->
	<!-- 태그의 id 속성에 지정할 이름은 namespace 에서 지정한 인터페이스 내의 메서드명과 동일해야함 -->
	<!-- 태그 사이에 실제 SQL 구문을 작성 -->
	<!-- 만능문자 파라미터는 ? 대신 #{파라미터명} 형태로 지정(VO 객체의 변수명 활용) -->
	
	<!-- 1. 회원 가입 작업 수행을 위한 insert 태그 작성(id 는 MemberMapper 객체의 메서드명과 동일 -->
	<insert id="insertMember">
	INSERT INTO member
	VALUES (null, #{name}, #{id}, #{passwd}, #{email}, #{gender}, now())
	</insert>

</mapper>


```

  

  
