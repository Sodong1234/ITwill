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

## AND/OR 연산자
- 여러 조건식을 한번에 적용할 때 조건들을 연결할 수 있는 연산자
- AND는 연결된 조건의 결과가 모두 참인 경우 결과로 출력
- OR은 연결된 조건의 결과가 하나라도 참인 경우 결과로 출력

```

SQL> SELECT employee_id, last_name, job_id, salary
  2  FROM employees
  3  WHERE salary >= 10000 AND job_id LIKE '%MAN%';


EMPLOYEE_ID LAST_NAME                 JOB_ID         SALARY
----------- ------------------------- ---------- ----------
        114 Raphaely                  PU_MAN          11000
        145 Russell                   SA_MAN          14000
        146 Partners                  SA_MAN          13500
        147 Errazuriz                 SA_MAN          12000
        148 Cambrault                 SA_MAN          11000
        149 Zlotkey                   SA_MAN          10500
        201 Hartstein                 MK_MAN          13000



-----------------------------------------------------------------------------------------------------



SQL> SELECT employee_id, last_name, job_id, salary
  2  FROM employees
  3  WHERE salary >= 10000 OR job_id LIKE '%MAN%';


EMPLOYEE_ID LAST_NAME                 JOB_ID         SALARY
----------- ------------------------- ---------- ----------
        100 King                      AD_PRES         24000
        101 Kochhar                   AD_VP           17000
        102 De Haan                   AD_VP           17000
        108 Greenberg                 FI_MGR          12008
        114 Raphaely                  PU_MAN          11000
        120 Weiss                     ST_MAN           8000
        121 Fripp                     ST_MAN           8200
        122 Kaufling                  ST_MAN           7900
        123 Vollman                   ST_MAN           6500
        124 Mourgos                   ST_MAN           5800
        145 Russell                   SA_MAN          14000
        146 Partners                  SA_MAN          13500
        147 Errazuriz                 SA_MAN          12000
        148 Cambrault                 SA_MAN          11000
        149 Zlotkey                   SA_MAN          10500
        150 Tucker                    SA_REP          10000
        156 King                      SA_REP          10000
        162 Vishney                   SA_REP          10500
        168 Ozer                      SA_REP          11500
        169 Bloom                     SA_REP          10000
        174 Abel                      SA_REP          11000
        201 Hartstein                 MK_MAN          13000
        204 Baer                      PR_REP          10000
        205 Higgins                   AC_MGR          12008
```

### 급여가 10000 이상 job_id에 'MAN' 또는 'REP'를 포함하는 행을 출력
- 잘못 작성된 예
  - 연산자 우선순위가 AND가 높으므로 salary의 조건과 job_id가 'MAN'을 포함하는 조건이 우선 연산된다. 이후 OR로 연결된 조건식이 연산이 된다.

```
SQL> SELECT employee_id, last_name, job_id, salary
  2  FROM employees
  3  WHERE salary >= 10000 AND job_id LIKE '%MAN%' OR job_id LIKE '%REP%';


EMPLOYEE_ID LAST_NAME                 JOB_ID         SALARY
----------- ------------------------- ---------- ----------
        114 Raphaely                  PU_MAN          11000
        145 Russell                   SA_MAN          14000
        146 Partners                  SA_MAN          13500
        147 Errazuriz                 SA_MAN          12000
        148 Cambrault                 SA_MAN          11000
        149 Zlotkey                   SA_MAN          10500
        150 Tucker                    SA_REP          10000
        151 Bernstein                 SA_REP           9500
        152 Hall                      SA_REP           9000
        153 Olsen                     SA_REP           8000
        154 Cambrault                 SA_REP           7500
        155 Tuvault                   SA_REP           7000
        156 King                      SA_REP          10000
        157 Sully                     SA_REP           9500
        158 McEwen                    SA_REP           9000
        159 Smith                     SA_REP           8000
        160 Doran                     SA_REP           7500
        161 Sewall                    SA_REP           7000
        162 Vishney                   SA_REP          10500
        163 Greene                    SA_REP           9500
        164 Marvins                   SA_REP           7200
        165 Lee                       SA_REP           6800
        166 Ande                      SA_REP           6400
        167 Banda                     SA_REP           6200
        168 Ozer                      SA_REP          11500
        169 Bloom                     SA_REP          10000
        170 Fox                       SA_REP           9600
        171 Smith                     SA_REP           7400
        172 Bates                     SA_REP           7300
        173 Kumar                     SA_REP           6100
        174 Abel                      SA_REP          11000
        175 Hutton                    SA_REP           8800
        176 Taylor                    SA_REP           8600
        177 Livingston                SA_REP           8400
        178 Grant                     SA_REP           7000
        179 Johnson                   SA_REP           6200
        201 Hartstein                 MK_MAN          13000
        202 Fay                       MK_REP           6000
        203 Mavris                    HR_REP           6500
        204 Baer                      PR_REP          10000
```

- 의도한 답안
  - 괄호()가 AND보다 우선 순위가 높으므로 괄호 안의 연산식이 먼저 수행이 된다.
```
EMPLOYEE_ID LAST_NAME                 JOB_ID         SALARY
----------- ------------------------- ---------- ----------
        114 Raphaely                  PU_MAN          11000
        145 Russell                   SA_MAN          14000
        146 Partners                  SA_MAN          13500
        147 Errazuriz                 SA_MAN          12000
        148 Cambrault                 SA_MAN          11000
        149 Zlotkey                   SA_MAN          10500
        150 Tucker                    SA_REP          10000
        156 King                      SA_REP          10000
        162 Vishney                   SA_REP          10500
        168 Ozer                      SA_REP          11500
        169 Bloom                     SA_REP          10000
        174 Abel                      SA_REP          11000
        201 Hartstein                 MK_MAN          13000
        204 Baer                      PR_REP          10000
```

## NOT 연산자
- 조건식의 연산 결과의 참, 거짓을 반대로 바꿔버리는 연산자
- NOT의 위치를 조건식의 앞으로 위치해도 동일한 연산으로 동작한다.
```

SQL> SELECT last_name, salary, job_id, commission_pct
  2  FROM employees
  3  WHERE NOT (job_id IN ('AC_ACCOUNT', 'AD_VP'));


LAST_NAME                     SALARY JOB_ID     COMMISSION_PCT
------------------------- ---------- ---------- --------------
King                           24000 AD_PRES
Hunold                          9000 IT_PROG
Ernst                           6000 IT_PROG
Austin                          4800 IT_PROG
Pataballa                       4800 IT_PROG
Lorentz                         4200 IT_PROG
Greenberg                      12008 FI_MGR
Faviet                          9000 FI_ACCOUNT
Chen                            8200 FI_ACCOUNT
Sciarra                         7700 FI_ACCOUNT
Urman                           7800 FI_ACCOUNT
Popp                            6900 FI_ACCOUNT
Raphaely                       11000 PU_MAN
…
```

## ORDER BY 절
- 지정한 기준으로 출력 결과의 순서를 정렬하여 출력할 수 있는 옵션절
- ORDER BY 정렬기준 [정렬방식]




