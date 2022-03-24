# [오전수업] JSP 35차

> 개인 프로젝트용 홈페이지 구현작업 실시. repositories에 파일을 업로드해놓음

## static import
- 특정 클래스의 static 메서드를 클래스명 없이 접근하기 위한 import

```
< 기본 문법 > 
import static 패키지명.클래스명.메서드명; 
또는 import static 패키지명.클래스명.*

// import static db.JdbcUtil.getConnection;
// import static db.JdbcUtil.close;

=> db.jdbcUtil 클래스의 모든 static 메서드를 import 하는 경우
import static db.JdbcUtil.*;
```

---

# [오후수업] DB 13차


- 연습문제
```

SQL> SELECT last_name, hire_date
  2  FROM employees
  3  WHERE hire_date LIKE '%04';


SQL> SELECT last_name, hire_date
  2  FROM employees
  3  WHERE hire_date LIKE '__-___-04';


SQL> SELECT last_name, hire_date
  2  FROM employees
  3  WHERE hire_date LIKE '_______04';




LAST_NAME                 HIRE_DATE
------------------------- ---------
Weiss                     18-JUL-04
Mallin                    14-JUN-04
Russell                   01-OCT-04
King                      30-JAN-04
Sully                     04-MAR-04
McEwen                    01-AUG-04
Abel                      11-MAY-04
Sarchand                  27-JAN-04
Bell                      04-FEB-04
Hartstein                 17-FEB-04
```

## IS NULL 연산자
- 특정 컬럼의 값이 NULL인 행을 선택하는 연산자
```
SQL> SELECT last_name, manager_id
  2  FROM employees
  3  WHERE manager_id IS NULL;


LAST_NAME                 MANAGER_ID
------------------------- ----------
King
```

