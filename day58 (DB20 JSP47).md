# [오전수업] DB 20차
### 형식문자 ','
```
- 천자리 표시자 ','를 추가하여 숫자를 문자열로 변환하기

SELECT TO_CHAR(1234567, '9,999,999')
FROM dual;

TO_CHAR(1234567,'9,999,999')|
----------------------------+
 1,234,567                  |


-----------------------------------------------------------------------


- 천자리 표시자 기호는 값의 길이에 맞춰 출력된다.
SELECT TO_CHAR(12345, '9,999,999')
FROM dual;

TO_CHAR(12345,'9,999,999')|
--------------------------+
    12,345                |


-----------------------------------------------------------------------


- ',' 기호의 위치나 수는 제한이 없음.
SELECT TO_CHAR(12345, '9,999,9,9,9')
FROM dual;

TO_CHAR(12345,'9,999,9,9,9')|
----------------------------+
    12,3,4,5                |


-----------------------------------------------------------------------


SELECT TO_CHAR(salary, '$99,999.00') salary
FROM employees
WHERE last_name = 'Ernst';

SALARY     |
-----------+
  $6,000.00|
```

- 연습문제 
```
SELECT last_name || ' earns ' 
|| TO_CHAR(salary, '$99,999.00')
|| ' monthly but wants ' 
|| TO_CHAR(3*salary, '$99,999.00')
|| '.'AS "Dream"
FROM employees;

Dream                                                       |
------------------------------------------------------------+
King earns  $24,000.00 monthly but wants  $72,000.00.       |
Kochhar earns  $17,000.00 monthly but wants  $51,000.00.    |
De Haan earns  $17,000.00 monthly but wants  $51,000.00.    |
Hunold earns   $9,000.00 monthly but wants  $27,000.00.     |
Ernst earns   $6,000.00 monthly but wants  $18,000.00.      |
Austin earns   $4,800.00 monthly but wants  $14,400.00.     |
Pataballa earns   $4,800.00 monthly but wants  $14,400.00.  |
Lorentz earns   $4,200.00 monthly but wants  $12,600.00.    |
Greenberg earns  $12,008.00 monthly but wants  $36,024.00.  |
Faviet earns   $9,000.00 monthly but wants  $27,000.00.     |


-----------------------------------------------------------------------------------


SELECT LPAD(last_name, 11) || ' earns ' 
|| TO_CHAR(salary, '$99,999.00')
|| ' monthly but wants ' 
|| TO_CHAR(3*salary, '$99,999.00')
|| '.'AS "Dream"
FROM employees;

Dream                                                       |
------------------------------------------------------------+
       King earns  $24,000.00 monthly but wants  $72,000.00.|
    Kochhar earns  $17,000.00 monthly but wants  $51,000.00.|
    De Haan earns  $17,000.00 monthly but wants  $51,000.00.|
     Hunold earns   $9,000.00 monthly but wants  $27,000.00.|
      Ernst earns   $6,000.00 monthly but wants  $18,000.00.|
     Austin earns   $4,800.00 monthly but wants  $14,400.00.|
  Pataballa earns   $4,800.00 monthly but wants  $14,400.00.|
    Lorentz earns   $4,200.00 monthly but wants  $12,600.00.|
  Greenberg earns  $12,008.00 monthly but wants  $36,024.00.|
```

### TO_NUMBER (문자 → 숫자)
- 문자데이터를 입력 받아 숫자로 변환하는 함수
- 입력 받은 문자의 형태를 형식문자로 묘사하여 숫자부분을 추려서 변환
- 이때 사용되는 형식문자는 TO_CHAR의 숫자→문자 변환에 사용한 형식문자와 동일하다.

```
SELECT TO_NUMBER('$24,000.00', '$99,999.99')
FROM dual;

TO_NUMBER('$24,000.00','$99,999.99')|
------------------------------------+
                               24000|


------------------------------------------------------------------------------------------


문자열에 숫자 이외의 문자가 없는 경우 형식문자를 생략해도 숫자로 변환이 된다.
SELECT TO_NUMBER('12345')
FROM dual;

TO_NUMBER('12345')|
------------------+
             12345|
```

### TO_DATE(문자→날짜)
- 날짜의 요소가 포함된 문자열을 날짜데이터로 변환하는 함수
- DB나 접속도구에서 기본적으로 DB가 인식하는 날짜의 형식인 경우 형식문자 없이도 날짜데이터로 변환 가능

```
sql*plus 
SQL> SELECT TO_DATE('20-OCT-99')
  2  FROM dual;


TO_DATE('
---------
20-OCT-99

----------------------------------------------------------------------------------------


DBeaver
SELECT TO_DATE('99/10/20')
FROM dual;

TO_DATE('99/10/20')  |
---------------------+
1999-10-20 00:00:00.0|


-----------------------------------------------------------------------------------------
SQL> SELECT TO_DATE('99/10/20', 'YY/MM/DD')
  2  FROM dual;


TO_DATE('
---------
20-OCT-99


--------------------------------------------------------------------------------------------
SELECT TO_DATE('15 / 15 / 04 / 18', 'HH24 / DD / MM / MI')
FROM dual;

TO_DATE('15/15/04/18','HH24/DD/MM/MI')|
--------------------------------------+
                 2022-04-15 15:18:00.0|


----------------------------------------------------------------------------------------------
SELECT employee_id, last_name, salary, hire_date
FROM employees
WHERE hire_date < TO_DATE('2004/08/09', 'YYYY/MM/DD');


EMPLOYEE_ID LAST_NAME                     SALARY HIRE_DATE
----------- ------------------------- ---------- ---------
        100 King                           24000 17-JUN-03
        102 De Haan                        17000 13-JAN-01
        108 Greenberg                      12008 17-AUG-02
        109 Faviet                          9000 16-AUG-02
        114 Raphaely                       11000 07-DEC-02
        115 Khoo                            3100 18-MAY-03
        120 Weiss                           8000 18-JUL-04
        122 Kaufling                        7900 01-MAY-03
        133 Mallin                          3300 14-JUN-04
        137 Ladwig                          3600 14-JUL-03
        141 Rajs                            3500 17-OCT-03

EMPLOYEE_ID LAST_NAME                     SALARY HIRE_DATE
----------- ------------------------- ---------- ---------
        156 King                           10000 30-JAN-04
        157 Sully                           9500 04-MAR-04
        158 McEwen                          9000 01-AUG-04
        174 Abel                           11000 11-MAY-04
        184 Sarchand                        4200 27-JAN-04
        192 Bell                            4000 04-FEB-04
        200 Whalen                          4400 17-SEP-03
        201 Hartstein                      13000 17-FEB-04
        203 Mavris                          6500 07-JUN-02
        204 Baer                           10000 07-JUN-02
        205 Higgins                        12008 07-JUN-02

EMPLOYEE_ID LAST_NAME                     SALARY HIRE_DATE
----------- ------------------------- ---------- ---------
        206 Gietz                           8300 07-JUN-02

23 rows selected.
```

## 일반함수
- 데이터타입 구분 없이 처리할 수 있는 함수
- NULL값을 처리하는 함수들

### NVL 함수
- NULL이 아닌 값은 그대로 출력
- NULL값을 직접 대체값으로 출력해주는 함수
- 컬럼의 값과 NULL의 대체값이 같은 컬럼에 출력이 되므로 데이터타입을 통일시켜야 동작한다.

```

SELECT commission_pct, NVL(commission_pct, 0)
FROM employees;

COMMISSION_PCT|NVL(COMMISSION_PCT,0)|
--------------+---------------------+
              |                    0|
              |                    0|
           0.4|                  0.4|
           0.3|                  0.3|
           0.3|                  0.3|
           0.3|                  0.3|
…


--------------------------------------------------------------------------------------------------------------


SELECT commission_pct, NVL(commission_pct, 0),
NVL(hire_date, '97-01-01'), NVL(job_id, 'No Job Yet')
FROM employees;


- 표현식에 NULL값이 들어가게 되면 표현식의 경우 연산식의 내용과는 상관없이 연산 결과를 NULL이 출력된다.
SELECT last_name, salary, commission_pct,
(salary*12) + (salary*12*commission_pct) an_sal
FROM employees;

LAST_NAME  |SALARY|COMMISSION_PCT|AN_SAL|
-----------+------+--------------+------+
Patel      |  2500|              |      |
Rajs       |  3500|              |      |
Davies     |  3100|              |      |
Matos      |  2600|              |      |
Vargas     |  2500|              |      |
Russell    | 14000|           0.4|235200|
Partners   | 13500|           0.3|210600|
Errazuriz  | 12000|           0.3|187200|



------------------------------------------------------------------------------------------------------------------


SELECT last_name, salary, NVL(commission_pct, 0),
(salary*12) + (salary*12*NVL(commission_pct, 0)) an_sal
FROM employees;

LAST_NAME  |SALARY|NVL(COMMISSION_PCT,0)|AN_SAL|
-----------+------+---------------------+------+
King       | 24000|                    0|288000|
Kochhar    | 17000|                    0|204000|
De Haan    | 17000|                    0|204000|
Hunold     |  9000|                    0|108000|
Ernst      |  6000|                    0| 72000|
Austin     |  4800|                    0| 57600|
Pataballa  |  4800|                    0| 57600|
```

### NVL2 함수
- NULL값을 간접적으로 활용하는 함수
- NVL2(NULL컬럼, NULL이 아닐 때, NULL일 때)
- 2,3번째 인자가 실제로 하나의 컬럼에 출력되는 값들이기 때문에 데이터타입은 통일되어 있어야 한다. 

```
SELECT last_name, salary, commission_pct, 
NVL2(commission_pct, 'SAL+COMM', 'SAL') income
FROM employees
WHERE department_id IN (50, 80);

LAST_NAME  |SALARY|COMMISSION_PCT|INCOME  |
-----------+------+--------------+--------+
Rajs       |  3500|              |SAL     |
Davies     |  3100|              |SAL     |
Matos      |  2600|              |SAL     |
Vargas     |  2500|              |SAL     |
Russell    | 14000|           0.4|SAL+COMM|
Partners   | 13500|           0.3|SAL+COMM|
Errazuriz  | 12000|           0.3|SAL+COMM|
Cambrault  | 11000|           0.3|SAL+COMM|
…
```

### COALESCE 함수
- NULL에 대한 여러 대체값을 출력할 수 있는 함수
- 입력값의 목록에서 NULL이 아닌 첫번째 값을 출력한다.

```

SELECT last_name, employee_id,
COALESCE(TO_CHAR(commission_pct), TO_CHAR(manager_id),
'No commission and no manager')
FROM employees;

LAST_NAME  |EMPLOYEE_ID|COALESCE(TO_CHAR(COMMISSION_PCT),TO_CHAR(MANAGER_ID),'NOCOMMISSIONANDNOMANAGER')|
-----------+-----------+--------------------------------------------------------------------------------+
King       |        100|No commission and no manager                                                    |
Kochhar    |        101|100                                                                             |
De Haan    |        102|100                                                                             |
Hunold     |        103|102                                                                             |
Ernst      |        104|103                                                                             |
Austin     |        105|103                                                                             |
…
Matos      |        143|124                                                                             |
Vargas     |        144|124                                                                             |
Russell    |        145|.4                                                                              |
Partners   |        146|.3                                                                              |
Errazuriz  |        147|.3                                                                              |
Cambrault  |        148|.3                                                                              |
…
```

- 연습문제

``` 
SELECT last_name, 
NVL(TO_CHAR(commission_pct), 'No Commission') comm
FROM employees;
LAST_NAME  |COMM         |
-----------+-------------+
Davies     |No Commission|
Matos      |No Commission|
Vargas     |No Commission|
Russell    |.4           |
Partners   |.3           |
Errazuriz  |.3           |
Cambrault  |.3           |
```


---

# [오후수업] JSP 47차

* java 파일
```java

------------------------------------------------WriteProServlet------------------------------------------------



import java.io.IOException;
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.SQLException;

import javax.naming.Context;
import javax.naming.InitialContext;
import javax.naming.NamingException;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.sql.DataSource;

@WebServlet("/WritePro")
public class WriteProServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;
       
    public WriteProServlet() {
        super();
    }

	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		doProcess(request, response);
	}

	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		doProcess(request, response);
	}
	
	protected void doProcess(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// 작성자, 제목 가져와서 출력
		request.setCharacterEncoding("UTF-8");
		String name = request.getParameter("name");
		String pass = request.getParameter("pass");
		String subject = request.getParameter("subject");
		String content = request.getParameter("content");
		
//		System.out.println("작성자 : " + name);
//		System.out.println("패스워드 : " + pass);
//		System.out.println("제목 : " + subject);
//		System.out.println("내용 : " + content);
		
		try {
			// context.xml 에 설정된 DBCP(커넥션풀)로부터 Connection 객체를 가져오기
			// 1. 톰캣으로부터 context.xml 파일의 Context 태그 부분(객체) 가져오기
			// => InitialContext 객체 생성하여 Context 타입으로 업캐스팅하여 저장
			Context initCtx = new InitialContext();
			
			// 2. 생성된 Context 객체(initCtx)로부터 context.xml의 Resource 태그 부분(객체) 가져오기
			// => Context 객체의 lookup() 메서드를 호출하여 찾아올 리소스 지정
			// => 리턴되는 Object 타입 객체를 Context 타입으로 다운캐스팅하여 저장
			// => 파라미터로 "java:comp/env" 문자열 전달(Resource 태그 가리킴)
//			Context envCtx = (Context)initCtx.lookup("java:comp/env");
			
			// 3. Resource 태그 내의 name 속성(리소스 이름 "jdbc/MySQL") 을 가져오기(*)
			// => 생성된 Context 객체(envCtx)의 lookup() 메서드를 호출하여 찾아올 리소스 이름 지정
			// => 리턴되는 Object 타입 객체를 javax.sql.DataSource 타입으로 다운캐스팅하여 저장
			// => 주의! context.xml 파일에 지정된 이름에 따라 문자열 바뀔 수 있다!
//			DataSource ds = (DataSource)envCtx.lookup("jdbc/MySQL");

			// --- 참고! 2번과 3번 작업을 하나로 결합하는 경우 ---
			// 2번과 3번에서 지정한 문자열을 결합(2번문자열/3번문자열)
			DataSource ds = (DataSource)initCtx.lookup("java:comp/env/jdbc/MySQL");
			// -------------------------------------------------------------------
			
			// 4. DataSource 객체(= 커넥션풀)로부터 미리 생성되어 있는 Connection 객체 가져오기
			// => 리턴타입 : java.sql.Connection
			Connection con = ds.getConnection();
			// => 만약, context.xml 파일 내에서 계정명(username)과 패스워드(password) 미등록 시
			//	  getConnection() 메서드 파라미터로 계정명, 패스워드 전달도 가능함
			// 	  ex) Connection con = ds.getConnection("root", "1234");
			// => 또한, Properties 객체 활용하여 아이디와 패스워드를 외부 파일로부터 가져올 수도 있음
			// ========================================================================
			// Connection 객체를 가져왔으므로 3단계, 4단계를 통해 DB 작업 가능
			String sql = "INSERT INTO board VALUES(?,?,?,?)";
			PreparedStatement pstmt = con.prepareStatement(sql);
			pstmt.setString(1, name);
			pstmt.setString(2, pass);
			pstmt.setString(3, subject);
			pstmt.setString(4, content);
			
			pstmt.executeUpdate();

		} catch (NamingException e) {
			e.printStackTrace();
		} catch (SQLException e) {
			e.printStackTrace();
		}
		
		
	}

}

```

> 모델2 수업을 위한 기초 세팅 작업 실시

