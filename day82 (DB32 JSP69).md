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

@Controller
public class TestController {
	
	// @RequestMapping 어노테이션을 지정한 메서드 정의
	// 지정된 문자열과 일치하는 주소가 매핑될 경우 해당 메서드가 자동으로 호출됨
	@RequestMapping("test")
	public String test() { // "/test" 서블릿 주소 요청되면 자동으로 호출되는 메서드
		// 포워딩 할 뷰페이지 파일명("xxx.jsp")을 파일이름만 return 문 뒤에 표기
		return "test";
		// => prefix 값으로 설정되어 있는 "/WEB-INF/views" 와
		//	  suffix 값으로 설정되어 있는 ".jsp"를 앞 뒤로 결합하여 포워딩 할 위치를 생성함
		// => 즉, /WEB-INF/views/ 디렉토리의 test.jsp 페이지로 포워딩(Dispatcher 방식)
		
	}

}
```

```jsp
---------------------------------------------------home.jsp--------------------------------------------------

```
