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
> 12차 수업 복습 실시

- 연습문제

```
# employees 테이블로부터 04년도에 입사한 모든 사원의 last_name과 hire_date를 출력하기


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

![캡처](https://user-images.githubusercontent.com/95197594/159871868-6fad9412-b669-4d72-bf23-baf86e79b796.PNG)


- hire_date컬럼을 기준으로 내림차순 정렬한 결과를 출력 (이후 → 이전)
```

SQL> SELECT last_name, job_id, department_id, hire_date
  2  FROM employees
  3  ORDER BY hire_date DESC;


LAST_NAME                 JOB_ID     DEPARTMENT_ID HIRE_DATE
------------------------- ---------- ------------- ---------
Kumar                     SA_REP                80 21-APR-08
Banda                     SA_REP                80 21-APR-08
Ande                      SA_REP                80 24-MAR-08
Markle                    ST_CLERK              50 08-MAR-08
Lee                       SA_REP                80 23-FEB-08
Philtanker                ST_CLERK              50 06-FEB-08
Geoni                     SH_CLERK              50 03-FEB-08
Zlotkey                   SA_MAN                80 29-JAN-08
Marvins                   SA_REP                80 24-JAN-08
Grant                     SH_CLERK              50 13-JAN-08
Johnson                   SA_REP                80 04-JAN-08
Perkins                   SH_CLERK              50 19-DEC-07
Gee                       ST_CLERK              50 12-DEC-07
Popp                      FI_ACCOUNT           100 07-DEC-07
…
```

- salary*12 표현식의 column alias인 annsal로 정렬된 결과를 출력
- 정렬 방식을 생략하는 경우 기본값인 ASC가 적용

```
SQL> SELECT employee_id, last_name, salary*12 annsal
  2  FROM employees
  3  ORDER BY annsal;
```
- 무조건 alias를 사용하지 않아도 된다.
```

SQL> SELECT employee_id, last_name, salary*12 annsal
  2  FROM employees
  3  ORDER BY salary*12;


EMPLOYEE_ID LAST_NAME                     ANNSAL
----------- ------------------------- ----------
        132 Olson                          25200
        128 Markle                         26400
        136 Philtanker                     26400
        135 Gee                            28800
        127 Landry                         28800
        119 Colmenares                     30000
        131 Marlow                         30000
…
```

- 3번째 컬럼을 기준으로 오름차순 정렬

```
SQL> SELECT last_name, job_id, department_id, hire_date
  2  FROM employees
  3  ORDER BY 3;


LAST_NAME                 JOB_ID     DEPARTMENT_ID HIRE_DATE
------------------------- ---------- ------------- ---------
Whalen                    AD_ASST               10 17-SEP-03
Hartstein                 MK_MAN                20 17-FEB-04
Fay                       MK_REP                20 17-AUG-05
Raphaely                  PU_MAN                30 07-DEC-02
Khoo                      PU_CLERK              30 18-MAY-03
Baida                     PU_CLERK              30 24-DEC-05
Tobias                    PU_CLERK              30 24-JUL-05
Himuro                    PU_CLERK              30 15-NOV-06
Colmenares                PU_CLERK              30 10-AUG-07
Mavris                    HR_REP                40 07-JUN-02
Weiss                     ST_MAN                50 18-JUL-04
Fripp                     ST_MAN                50 10-APR-05
Kaufling                  ST_MAN                50 01-MAY-03
Vollman                   ST_MAN                50 10-OCT-05
Mourgos                   ST_MAN                50 16-NOV-07
…

```
- 정렬 기준이 여러개인 경우 왼쪽의 정렬기준부터 순서대로 적용이 된다.
- 정렬 기준마다 개별 정렬방식을 지정해서 정렬한다.
```

SQL> SELECT last_name, department_id, salary
  2  FROM employees
  3  ORDER BY department_id, salary DESC;


LAST_NAME                 DEPARTMENT_ID     SALARY
------------------------- ------------- ----------
Whalen                               10       4400
Hartstein                            20      13000
Fay                                  20       6000
Raphaely                             30      11000
Khoo                                 30       3100
Baida                                30       2900
Tobias                               30       2800
Himuro                               30       2600
Colmenares                           30       2500
Mavris                               40       6500
Fripp                                50       8200
Weiss                                50       8000
Kaufling                             50       7900
```




