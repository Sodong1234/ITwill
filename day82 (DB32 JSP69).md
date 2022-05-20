# [오전수업] DB 32차
## 데이터 딕셔너리
- 데이터 딕셔너리는 데이터베이스 운영에 필요한 정보들을 저장하고 조회할 수 있는 테이블, 뷰를 말한다.
- 기본적으로 데이터베이스가 관리를 하므로 사용자는 조회만 할 수 있다.
- 베이스가 되는 테이블에는 수많은 컬럼들의 정보들이 보관되어 있어 직접 사용하기에는 어렵다.
- 뷰는 베이스 테이블의 정보를 사용자가 사용하기 편하도록 세분화하여 생성되어져 있으므로 관련 작업을 하는 경우 필요한 뷰를 활용하여 작업활용 할 수 있다.

- 사용자의 권한 수준에 따른 정보의 범위 별로 뷰가 구분되어 생성되어 있음.
- 아래의 뷰에 대한 접두어로 범위를 구분 할 수 있다.

![unnamed](https://user-images.githubusercontent.com/95197594/169433713-a5782cd7-0228-4e9a-8d11-e9cbb114f0c7.png)
```
- USER : 사용자가 소유한 오브젝트의 범위에서 정보 조회
- ALL : 사용자의 소유 + 다른 사용자의 오브젝트 중 사용자가 사용권한을 가지고 있는 오브젝트의 범위에서 정보 조회
- DBA : 데이터베이스 전 범위의 정보를 볼 수 있는 뷰
```

## 고급 JOIN
### NATURAL JOIN
- 자연 조인이라고도하며 각 테이블에서 동일한 테이블명, 데이터타입, 크기를 가진 컬럼들을 조인해주는 JOIN 문법
- 테이블의 설계 시 JOIN을 고려하여 만든 경우 NATURAL JOIN을 사용할 수 있음.

- 아래의 예문에서는 JOB_ID컬럼이 양 테이블에 존재하고 데이터타입 크기가 동일하므로 NATURAL JOIN이 가능하다.
```sql

SELECT employee_id, first_name, job_id, job_title
FROM employees NATURAL JOIN jobs;


EMPLOYEE_ID|FIRST_NAME |JOB_ID    |JOB_TITLE                      |
-----------+-----------+----------+-------------------------------+
        206|William    |AC_ACCOUNT|Public Accountant              |
        205|Shelley    |AC_MGR    |Accounting Manager             |
        200|Jennifer   |AD_ASST   |Administration Assistant       |
        100|Steven     |AD_PRES   |President                      |
        102|Lex        |AD_VP     |Administration Vice President  |
        101|Neena      |AD_VP     |Administration Vice President  |
        110|John       |FI_ACCOUNT|Accountant                     |
```

### USING절을 사용한 JOIN
- JOIN을 연산할 두 테이블에 조인 조건의 컬럼들이 이름이 같은 경우 활용할 수 있는 JOIN 문법
- 조인 조건에 해당하는 컬럼을 USING절에 작성한다.
- USING절에 사용된 컬럼은 컬럼명 앞에 테이블명이나 table alias를 붙일 수 없다.
```sql


SELECT employee_id,
       last_name,
       location_id,
       department_id
FROM employees
JOIN departments USING ( department_id );


EMPLOYEE_ID|LAST_NAME  |LOCATION_ID|DEPARTMENT_ID|
-----------+-----------+-----------+-------------+
        200|Whalen     |       1700|           10|
        201|Hartstein  |       1800|           20|
        202|Fay        |       1800|           20|
        114|Raphaely   |       1700|           30|
        115|Khoo       |       1700|           30|
        116|Baida      |       1700|           30|
…

```

- USING절의 조건인 location_id에는 테이블명, table alias를 컬럼명 앞에 붙일 수 없다.
```sql
SELECT l.city, d.department_name
FROM locations l JOIN departments d
USING (location_id)
WHERE d.location_id = 1400;

SQL Error [25154] [99999]: ORA-25154: USING 절의 열 부분은 식별자를 가질 수 없음
```
```sql
SELECT l.city, d.department_name
FROM locations l JOIN departments d
USING (location_id)
WHERE location_id = 1400;

CITY     |DEPARTMENT_NAME|
---------+---------------+
Southlake|IT             |
```

### self-join
```sql
SELECT worker.last_name emp, manager.last_name mgr
FROM employees worker JOIN employees manager
ON (worker.manager_id = manager.employee_id);

EMP        |MGR      |
-----------+---------+
Bates      |Cambrault|
Bloom      |Cambrault|
Fox        |Cambrault|
Kumar      |Cambrault|
Ozer       |Cambrault|
Smith      |Cambrault|
Hunold     |De Haan  |
…
```

### Non-equi JOIN
- 실습용 테이블 생성
```sql
CREATE TABLE job_grades (
    grade_level CHAR(5) PRIMARY KEY,
    lowest_sal  NUMBER NOT NULL,
    higest_sal  NUMBER NOT NULL
);



INSERT INTO job_grades
VALUES ('A', 1000, 2999);
INSERT INTO job_grades
VALUES ('B', 3000, 5999);
INSERT INTO job_grades
VALUES ('C', 6000, 9999);
INSERT INTO job_grades
VALUES ('D', 10000, 14999);
INSERT INTO job_grades
VALUES ('E', 15000, 24999);
INSERT INTO job_grades
VALUES ('F', 25000, 40000);
COMMIT;
```
```sql
SELECT e.last_name, e.salary, j.grade_level
FROM employees e JOIN job_grades j
ON e.salary BETWEEN j.lowest_sal AND j.highest_sal;


LAST_NAME  |SALARY|GRADE_LEVEL|
-----------+------+-----------+
Olson      |  2100|A          |
Markle     |  2200|A          |
Philtanker |  2200|A          |
Landry     |  2400|A          |
Gee        |  2400|A          |
Colmenares |  2500|A          |
Marlow     |  2500|A          |
…
Greenberg  | 12008|D          |
Hartstein  | 13000|D          |
Partners   | 13500|D          |
Russell    | 14000|D          |
De Haan    | 17000|E          |
Kochhar    | 17000|E          |
King       | 24000|E          |
```

---

# [오후수업] JSP 69차


## Spring
[ 스프링 MVC 기본 동작 ]
1. 웹서버로 요청이 들어오면 web.xml 의 서블릿 매핑 내용 확인 
- (기본 서블릿 매핑 주소는 / 로 되어 있음)
	- 일치할 경우 app-servlet 매핑 정보에 따라 "/WEB-INF/spring/appServlet/servlet-context.xml" 위치에서 정보 탐색

2. servlet-context.xml 의 다음 내용을 통해 포워딩 할 정보 결합 문자열 탐색
```xml
   <beans:bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
	<beans:property name="prefix" value="/WEB-INF/views/" />
	<beans:property name="suffix" value=".jsp" />
   </beans:bean>
```
- @Controller 어노테이션이 사용된 클래스에서 뷰페이지(*.jsp) 로 이동할 때 페이지 이름 앞뒤에 붙을 문자열을 지정
- 기본 설정값 : "/WEB-INF/views/" + 페이지명 + ".jsp" 형태로 결합됨

3. @Controller 어노테이션이 사용된 컨트롤러 클래스가 실행되고 @RequestMapping 어노테이션에 지정된 value 속성값에 해당하는 패턴이 일치할 경우 아래쪽 메서드를 자동으로 호출하게 되며 (단, value 속성만 있을 경우 value 속성 표시 생략 가능) 해당 메서드에서 return "XXX" 형식으로 되어 있는 리턴문의 "XXX" 문자열과 servlet-context.xml 에서 지정한 prefix, suffix 문자열을 앞뒤로 결합하여 Dispatcher 방식으로 해당 파일로 포워딩
- ex) return "home"; 일 경우 
	- "/WEB-INF/views/" + "home" + ".jsp" 형태로 결합되어 "/WEB-INF/views/home.jsp" 위치로 포워딩하게 됨
```   
   < 추가사항 >
   1) 속성 중에서 method 속성을 사용할 경우 HTTP method 타입(GET, POST 등)을 구분하게 됨
      ex) @RequestMapping(value = "매핑주소", method = RequestMethod.메서드명)
   2) @RequestMapping 어노테이션을 통해 자동 호출되는 메서드의 매개변수 선언 시 해당 서블릿 주소가 요청될 때 전달되는 파라미터가 있을 경우 파라미터명과 매개변수명이 일치하면 해당 파라미터의 데이터는 매개변수에 자동 저장됨  
      @RequestMapping(value = "test3", method = RequestMethod.POST)
      public String test3(String name, int age) { 
		// name 과 age 파라미터가 POST 방식으로 전달되면
		// 자동으로 String name, int age 변수에 저장됨
      }
```			

================================================================

[ 프로젝트 설정 관련 ]
1. 프로젝트 생성
2. web.xml 설정(서블릿 관련 설정)
```xml
<servlet>
	<servlet-name>appServlet</servlet-name>
	<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
		<init-param>
			<param-name>contextConfigLocation</param-name>
			<param-value>/WEB-INF/spring/appServlet/servlet-context.xml</param-value>
		</init-param>
		<load-on-startup>1</load-on-startup>
	</servlet>
		
	<servlet-mapping>
		<servlet-name>appServlet</servlet-name>
		<url-pattern>/</url-pattern>
	</servlet-mapping>
```	
- servlet-mapping 에서 루트(/) 요청을 받으면 appServlet 이름을 갖는  servlet 태그를 찾아 연결(매핑) 수행
	- /WEB-INF/spring/appServlet/servlet-context.xml 파일에 지정된 파라미터를 사용 (prefix, suffix 속성)

---

```xml
----------------------------------------------web.xml-------------------------------------------------------
<?xml version="1.0" encoding="UTF-8"?>
<web-app version="2.5" xmlns="http://java.sun.com/xml/ns/javaee"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://java.sun.com/xml/ns/javaee https://java.sun.com/xml/ns/javaee/web-app_2_5.xsd">

	<!-- The definition of the Root Spring Container shared by all Servlets and Filters -->
	<context-param>
		<param-name>contextConfigLocation</param-name>
		<param-value>/WEB-INF/spring/root-context.xml</param-value>
	</context-param>
	
	<!-- Creates the Spring Container shared by all Servlets and Filters -->
	<listener>
		<listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
	</listener>

	<!-- Processes application requests -->
	<servlet>
		<servlet-name>appServlet</servlet-name>
		<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
		<init-param>
			<param-name>contextConfigLocation</param-name>
			<param-value>/WEB-INF/spring/appServlet/servlet-context.xml</param-value>
		</init-param>
		<load-on-startup>1</load-on-startup>
	</servlet>
		
	<servlet-mapping>
		<servlet-name>appServlet</servlet-name>
		<url-pattern>/</url-pattern>
	</servlet-mapping>
	
	<!-- POSt 방식 한글 처리를 위한 필터 설정 -->
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

</web-app>

```





```xml
----------------------------------------------servlet-context.xml----------------------------------------------
<?xml version="1.0" encoding="UTF-8"?>
<beans:beans xmlns="http://www.springframework.org/schema/mvc"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:beans="http://www.springframework.org/schema/beans"
	xmlns:context="http://www.springframework.org/schema/context"
	xsi:schemaLocation="http://www.springframework.org/schema/mvc https://www.springframework.org/schema/mvc/spring-mvc.xsd
		http://www.springframework.org/schema/beans https://www.springframework.org/schema/beans/spring-beans.xsd
		http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">

	<!-- DispatcherServlet Context: defines this servlet's request-processing infrastructure -->
	
	<!-- Enables the Spring MVC @Controller programming model -->
	<annotation-driven />

	<!-- Handles HTTP GET requests for /resources/** by efficiently serving up static resources in the ${webappRoot}/resources directory -->
	<resources mapping="/resources/**" location="/resources/" />

	<!-- Resolves views selected for rendering by @Controllers to .jsp resources in the /WEB-INF/views directory -->
	<beans:bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
	<!-- "/WEB-INF/views/" 경로와 ".jsp" 확장자를 미리 정의해놓고 파일명만 결합하도록 해주는 역할 -->
		<beans:property name="prefix" value="/WEB-INF/views/" />
		<beans:property name="suffix" value=".jsp" />
	</beans:bean>
	
	<context:component-scan base-package="com.itwillbs.test" />
	
	
	
</beans:beans>
```
```java
----------------------------------------------HomeController.java--------------------------------------------------
package com.itwillbs.test;

import java.text.DateFormat;
import java.util.Date;
import java.util.Locale;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;

/**
 * Handles requests for the application home page.
 */
@Controller
public class HomeController {

	private static final Logger logger = LoggerFactory.getLogger(HomeController.class);

	/**
	 * Simply selects the home view to render by returning its name.
	 */
	@RequestMapping(value = "/", method = RequestMethod.GET)
	public String home(Locale locale, Model model) {
		logger.info("Welcome home! The client locale is {}.", locale);
		
		Date date = new Date();
		DateFormat dateFormat = DateFormat.getDateTimeInstance(DateFormat.LONG, DateFormat.LONG, locale);
		
		String formattedDate = dateFormat.format(date);
		
		model.addAttribute("serverTime", formattedDate );
		
		return "home"; // WEB-INF/vies/home.jsp 파일 포워딩
//		return "a"; // WEB-INF/vies/a.jsp 파일 포워딩
//		return "sub/test";
	}
	
}


```
```java
> TestController.java 파일 생성
package com.itwillbs.test;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;

@Controller
public class TestController {
	
	// @RequestMapping 어노테이션을 지정한 메서드 정의
	// 지정된 문자열과 일치하는 주소가 매핑될 경우 해당 메서드가 자동으로 호출됨
	@RequestMapping("test")
	public String test() { // "/test" 서블릿 주소 요청되면 자동으로 호출되는 메서드
		// 포워딩 할 뷰페이지 파일명("xxx.jsp")을 파일이름만 return 문 뒤에 표기
		System.out.println("TestController");
		return "test";
		// => prefix 값으로 설정되어 있는 "/WEB-INF/views" 와
		//	  suffix 값으로 설정되어 있는 ".jsp"를 앞 뒤로 결합하여 포워딩 할 위치를 생성함
		// => 즉, /WEB-INF/views/ 디렉토리의 test.jsp 페이지로 포워딩(Dispatcher 방식)
		
	}
	
	// 클라이언트로부터 POST 방식의 요청으로 "test2" 서블릿 주소가 요청될 때 처리하는 메서드 정의
	@RequestMapping(value = "test2", method = RequestMethod.POST)
	public String test2() { // "/test2" 서블릿 주소 요청되면 자동으로 호출되는 메서드
		// 주의! test2 서블릿 주소에 대한 test2() 메서드 호출은 반드시 POST 방식일 경우에만 가능
		// => method 속성에 RequestMethod.POST 값이 설정되어 있기 때문에
		
		System.out.println("TestController - test2(POST)");
		return "test2";
		// => prefix 값으로 설정되어 있는 "/WEB-INF/views" 와
		//	  suffix 값으로 설정되어 있는 ".jsp"를 앞 뒤로 결합하여 포워딩 할 위치를 생성함
		// => 즉, /WEB-INF/views/ 디렉토리의 test2.jsp 페이지로 포워딩(Dispatcher 방식)
		
	}
	
	// 서블릿 주소가 같고 요청 메서드 방식이 다를 경우 각각 별도로 처리 가능함
	@RequestMapping(value = "test2", method = RequestMethod.GET)
	public String test2_2() { // "/test2" 서블릿 주소 요청되면 자동으로 호출되는 메서드
		// 주의! test2 서블릿 주소에 대한 test2() 메서드 호출은 반드시 POST 방식일 경우에만 가능
		// => method 속성에 RequestMethod.POST 값이 설정되어 있기 때문에
		
		System.out.println("TestController - test2(GET)");
		return "test2";
		// => prefix 값으로 설정되어 있는 "/WEB-INF/views" 와
		//	  suffix 값으로 설정되어 있는 ".jsp"를 앞 뒤로 결합하여 포워딩 할 위치를 생성함
		// => 즉, /WEB-INF/views/ 디렉토리의 test2.jsp 페이지로 포워딩(Dispatcher 방식)
		
	}
	
	@RequestMapping(value = "test3")
	public String test3() { 
		// 주의! test2 서블릿 주소에 대한 test2() 메서드 호출은 반드시 POST 방식일 경우에만 가능
		// => method 속성에 RequestMethod.POST 값이 설정되어 있기 때문에
		
		System.out.println("TestController - test3(GET)");
		
		return "test3";
	}
	
	// 주의! 클라이언트로부터 요청 파라미터가 전달될 경우
	// 메서드 오버로딩을 통해 메서드가 구분되면 문법적 오류는 없으나
	// 위의 동일한 매핑이 된 메서드와 동시에 사용할 경우
	// 파라미터를 전달 과정에서 GET/POST 방식에 대한 구별이 불가능하므로 매핑 오류가 발생하게 됨
//	@RequestMapping(value = "test3")
//	public String test3(String name, int age) { // "/test3" 서블릿 주소 요청되면 자동으로 호출되는 메서드
//		// 주의! test2 서블릿 주소에 대한 test2() 메서드 호출은 반드시 POST 방식일 경우에만 가능
//		// => method 속성에 RequestMethod.POST 값이 설정되어 있기 때문에
//		
//		System.out.println("TestController - test3(GET)");
//		System.out.println("이름 : " + name);
//		System.out.println("나이 : " + age);
//		
//		return "test3";
//	}
	
	// value 속성과 method 속성을 명시하여 오버롣잉 수행될 경우에는
	// 동일한 요청 서블릿 주소라도 GEt/POST 방식을 통해 구별이 가능해진다
	// => test3 서블릿 주소가 동일하더라도 POST 방식일 경우 현재 메서드가 처리 가능함
	// => 이 때, 요청 파라머티로 name 과 age 라는 이름의 파라미터가 전달되면
	//	  현재 메서드가 호출될 때 매개변수와 이름이 일치하는 파라미터는 자동으로 값이 저장됨
	@RequestMapping(value = "test3", method = RequestMethod.POST)
	public String test3(String name, int age) { // "/test3" 서블릿 주소 요청되면 자동으로 호출되는 메서드
		// 주의! test2 서블릿 주소에 대한 test2() 메서드 호출은 반드시 POST 방식일 경우에만 가능
		// => method 속성에 RequestMethod.POST 값이 설정되어 있기 때문에
		
		System.out.println("TestController - test3(GET)");
		System.out.println("이름 : " + name);
		System.out.println("나이 : " + age);
		
		return "test3";
	}
}

```

```jsp
---------------------------------------------------home.jsp--------------------------------------------------
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>
<%@ page session="false" %>
<html>
<head>
	<title>Home</title>
</head>
<body>
<h1>
	Hello world!  
</h1>

<P>  The time on the server is ${serverTime}. </P>
<a href="test">test.jsp 로 이동</a>  <!-- GET 방식 요청 -->

<form action="test" method="post"> <!-- POST 방식 요청 -->
	<input type="submit" value="test.jsp 로 이동">
</form>
<hr>

<a href="test2">test2.jsp 로 이동</a>  <!-- GET 방식 요청 -->

<form action="test2" method="post"> <!-- POST 방식 요청 -->
	<input type="submit" value="test2.jsp 로 이동">
</form>

<hr>

<a href="test3">test3.jsp 로 이동</a>  <!-- GET 방식 요청 -->
<!--  <a href="test3?name=hong&age=20">test3.jsp 로 이동</a> GET 방식 요청 -->

<form action="test3" method="post"> <!-- POST 방식 요청 -->
	<input type="text" name="name">
	<input type="text" name="age">
	<input type="submit" value="test3.jsp 로 이동">
</form>

</body>
</html>

```
