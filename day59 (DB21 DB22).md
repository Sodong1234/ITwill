# [오전수업] DB 21차
## 그룹함수(여러행 함수)
- 그룹 단위의 연산을 하는 함수
- 주로 통계 관련 연산을 하는 함수들이다.
- 출력값은 그룹의 수만큼 출력된다.
- 그룹을 묶지 않은 경우 테이블의 모든 행은 하나의 그룹으로 취급된다.
  - 숫자값 연산 그룹함수
```
AVG(n)			평균
STDDEV(n)		표준편차
VARIANCE(n)		분산
SUM(n)			합계
```
  - 일반 연산 그룹함수
```
COUNT(*|expr)		값의 수 세는 함수
MAX(expr)		최대값
MIN(expr)		최소값
```
```
SELECT AVG(salary), SUM(salary), STDDEV(salary), VARIANCE(salary)
FROM employees;

AVG(SALARY)                              |SUM(SALARY)|STDDEV(SALARY)                           |VARIANCE(SALARY)                         |
-----------------------------------------+-----------+-----------------------------------------+-----------------------------------------+
6461.831775700934579439252336448598130841|     691416|3909.579730552481921059198878167256201202|15284813.66954681713983424440134015164874|



----------------------------------------------------------------------------------------------------------------------------------------------



SELECT COUNT(department_id), MAX(salary), MIN(salary)
FROM employees;

COUNT(DEPARTMENT_ID)|MAX(SALARY)|MIN(SALARY)|
--------------------+-----------+-----------+
                 106|      24000|       2100|
                 
                 
----------------------------------------------------------------------------------------------------------------------------------------------




SELECT COUNT(last_name), MAX(first_name), MIN(first_name)
FROM employees;

COUNT(LAST_NAME)|MAX(FIRST_NAME)|MIN(FIRST_NAME)|
----------------+---------------+---------------+
             107|Winston        |Adam           |



----------------------------------------------------------------------------------------------------------------------------------------------



- 그룹함수는 연산 시 NULL값은 제외하고 연산한 결과를 출력한다.
- department_id 컬럼에는 값이 NULL인 사원이 있어 값이 다르게 출력되었음.

- 특정 컬럼의 NULL값 여부와는 상관없이 행의 수를 셀 때는 *값을 COUNT함수에 사용할 수 있음.
SELECT COUNT(commission_pct), COUNT(salary), COUNT(*)
FROM employees;

COUNT(COMMISSION_PCT)|COUNT(SALARY)|COUNT(*)|
---------------------+-------------+--------+
                   35|          107|     107|

```

### 그룹함수와 WHERE절 사용 시
- 그룹 연산 전에 WHERE절이 먼저 동작해서 그룹연산의 대상 행이 제한된다.
```

SELECT AVG(salary), MAX(salary), MIN(salary), SUM(salary)
FROM employees
WHERE job_id LIKE '%REP%';

AVG(SALARY)                              |MAX(SALARY)|MIN(SALARY)|SUM(SALARY)|
-----------------------------------------+-----------+-----------+-----------+
8272.727272727272727272727272727272727273|      11500|       6000|     273000|


------------------------------------------------------------------------------------------------------------


SELECT COUNT(commission_pct)
FROM employees
WHERE department_id = 80;

COUNT(COMMISSION_PCT)|
---------------------+
                   34|
```

- 연습문제 : employess 테이블로부터 전 직원의 커미션 평균을 출력하는 구문을 작성하시오
- commission_pct 컬럼은 값이 없는 사원은 NULL값이 채워져 있으므로 그룹 함수가 모든 값에 대한 연산을 할 수 없다.
- 이 때 NVL함수로 NULL을 대신 할 대체값을 지정해주는 경우 값이 없는 행도 그룹함수의 연산에 포함시켜 연산에 사용할 수 있게 된다.

```
SELECT AVG(NVL(commission_pct, 0)), COUNT(NVL(commission_pct, 0))
FROM employees;

AVG(NVL(COMMISSION_PCT,0))               |COUNT(NVL(COMMISSION_PCT,0))|
-----------------------------------------+----------------------------+
0.072897196261682242990654205607476635514|                         107|
```

## GROUP BY절

![unnamed](https://user-images.githubusercontent.com/95197594/163750377-688c21ec-2ceb-4f39-a7d6-935049bee6a9.png)

- GROUP BY절의 기준값에 대해서 동일한 값을 가지고 있는 행들끼리 그룹으로 묶게 된다.
- 그룹 함수들은 이 그룹들 단위로 연산 결과를 출력하게 된다.

```
- 그룹 함수를 사용한 경우 SELECT절에는 그룹 함수와 GROUP BY절에 사용했던 컬럼만 입력이 가능하다. 
- 이 때 SELECT절의 컬럼은 그룹의 이름을 출력하는 목적으로 사용된다.(필수는 아님)

SELECT department_id, AVG(salary)
FROM employees
GROUP BY department_id;

DEPARTMENT_ID|AVG(SALARY)                              |
-------------+-----------------------------------------+
          100|8601.333333333333333333333333333333333333|
           30|                                     4150|
             |                                     7000|
           90| 19333.3333333333333333333333333333333333|
           20|                                     9500|
           70|                                    10000|
          110|                                    10154|
           50|3475.555555555555555555555555555555555556|
           80|8955.882352941176470588235294117647058824|
           40|                                     6500|
           60|                                     5760|
           10|                                     4400|
           
```
```
- 그룹 관련 연산 시 WHERE절이 있으면 GROUP 연산보다 WHERE절이 먼저 연산된 이후 GROUP 연산이 실행
- GROUP BY절에 여러 컬럼이 있는 경우 해당 컬럼들의 값이 모두 일치하는 경우 동일한 그룹으로 묶이게 된다. 하나의 값이라도 다른 경우 다른 그룹으로 분리된다.

SELECT department_id, job_id, SUM(salary)
FROM employees
WHERE department_id > 40
GROUP BY department_id, job_id
ORDER BY department_id;

DEPARTMENT_ID|JOB_ID    |SUM(SALARY)|
-------------+----------+-----------+
           50|SH_CLERK  |      64300|
           50|ST_CLERK  |      55700|
           50|ST_MAN    |      36400|
           60|IT_PROG   |      28800|
           70|PR_REP    |      10000|
           80|SA_MAN    |      61000|
           80|SA_REP    |     243500|
           90|AD_PRES   |      24000|
           90|AD_VP     |      34000|
          100|FI_ACCOUNT|      39600|
          100|FI_MGR    |      12008|
          110|AC_ACCOUNT|       8300|
          110|AC_MGR    |      12008|
```

### GROUP BY 관련 오류 찾기
```
SQL> SELECT department_id, COUNT(last_name)
  2  FROM employees;

SELECT department_id, COUNT(last_name)
       *
ERROR at line 1:
ORA-00937: not a single-group group function
- 그룹 함수를 사용한 경우 GROUP BY절에서 사용한 컬럼이외에는 SELECT절에 컬럼명은 작성이 불가능하다.
- GROUP BY절을 사용하지 않은 경우 SELECT절에는 일반 컬럼은 입력할 수 없다.
```
         
```
SQL> SELECT department_id, job_id, COUNT(last_name)
  2  FROM employees
  3  GROUP BY department_id;

SELECT department_id, job_id, COUNT(last_name)
                      *
ERROR at line 1:
ORA-00979: not a GROUP BY expression

- job_id컬럼은 GROUP BY절에 사용하지 않은 컬럼으로 SELECT절에 사용할 수 없다.
- 따라서 GROUP BY절에 job_id를 추가하거나, SELECT절에서 job_id를 제거해야한다.
```

## HAVING절
- 그룹에 대한 조건절
- WHERE절은 테이블의 행에 대한 조건, HAVING절과는 조건이 적용되는 위치가 다르다.
- 그룹에 대한 조건은 그룹함수를 기준으로 조건식을 만들 수 있으며, 연산자나 조건값의 작성은 WHERE절과 동일하다.

![unnamed (1)](https://user-images.githubusercontent.com/95197594/163756820-75e30171-8f82-4288-aa72-ca986e2f0b97.png)

```
SELECT job_id, SUM(salary) payroll
FROM employees
WHERE job_id NOT LIKE '%REP%'
GROUP BY job_id
HAVING SUM(salary) > 13000
ORDER BY SUM(salary);

JOB_ID    |PAYROLL|
----------+-------+
PU_CLERK  |  13900|
AD_PRES   |  24000|
IT_PROG   |  28800|
AD_VP     |  34000|
ST_MAN    |  36400|
FI_ACCOUNT|  39600|
ST_CLERK  |  55700|
SA_MAN    |  61000|
SH_CLERK  |  64300|
```

연습문제
```

SELECT job_id, MAX(salary) "Maximum", MIN(salary) "Minimum",
SUM(salary) "Sum", AVG(salary) "Average"
FROM employees
GROUP BY job_id;

JOB_ID    |Maximum|Minimum|Sum   |Average|
----------+-------+-------+------+-------+
IT_PROG   |   9000|   4200| 28800|   5760|
AC_MGR    |  12008|  12008| 12008|  12008|
AC_ACCOUNT|   8300|   8300|  8300|   8300|
ST_MAN    |   8200|   5800| 36400|   7280|
PU_MAN    |  11000|  11000| 11000|  11000|
AD_ASST   |   4400|   4400|  4400|   4400|
AD_VP     |  17000|  17000| 34000|  17000|
SH_CLERK  |   4200|   2500| 64300|   3215|
FI_ACCOUNT|   9000|   6900| 39600|   7920|
FI_MGR    |  12008|  12008| 12008|  12008|
PU_CLERK  |   3100|   2500| 13900|   2780|
SA_MAN    |  14000|  10500| 61000|  12200|
MK_MAN    |  13000|  13000| 13000|  13000|
PR_REP    |  10000|  10000| 10000|  10000|
AD_PRES   |  24000|  24000| 24000|  24000|
SA_REP    |  11500|   6100|250500|   8350|
MK_REP    |   6000|   6000|  6000|   6000|
ST_CLERK  |   3600|   2100| 55700|   2785|
HR_REP    |   6500|   6500|  6500|   6500|



--------------------------------------------------------------------------------------------------



SELECT manager_id, MIN(salary)
FROM employees
WHERE manager_id IS NOT NULL
GROUP BY manager_id
HAVING MIN(salary) >= 6000
ORDER BY MIN(salary) DESC;

MANAGER_ID|MIN(SALARY)|
----------+-----------+
       102|       9000|
       205|       8300|
       146|       7000|
       145|       7000|
       108|       6900|
       149|       6200|
       147|       6200|
       148|       6100|
       201|       6000|
```

---

# [오후수업] DB 22차

## JOIN
- 여러 테이블의 데이터를 연결해서 출력하는 문법

![unnamed (2)](https://user-images.githubusercontent.com/95197594/163762524-06843940-e82a-4157-86a4-bd3f9cdac613.png)


### ON절을 사용한 JOIN
- FROM, JOIN절에 연결할 테이블명을 작성
- ON절에는 테이블 간 연결할 조건을 작성
```

SELECT employees.employee_id, employees.last_name, 
departments.department_id, departments.department_name
FROM employees JOIN departments
ON employees.department_id = departments.department_id;

EMPLOYEE_ID|LAST_NAME  |DEPARTMENT_ID|DEPARTMENT_NAME |
-----------+-----------+-------------+----------------+
        200|Whalen     |           10|Administration  |
        201|Hartstein  |           20|Marketing       |
        202|Fay        |           20|Marketing       |
        114|Raphaely   |           30|Purchasing      |
        115|Khoo       |           30|Purchasing      |
        116|Baida      |           30|Purchasing      |
        117|Tobias     |           30|Purchasing      |
        118|Himuro     |           30|Purchasing      |
        119|Colmenares |           30|Purchasing      |
        203|Mavris     |           40|Human Resources |
        120|Weiss      |           50|Shipping        |
        121|Fripp      |           50|Shipping        |
        122|Kaufling   |           50|Shipping        |
        123|Vollman    |           50|Shipping        |
        124|Mourgos    |           50|Shipping        |
…
```

### table alias 적용
- 테이블의 별명으로 구문이 실행되는 동안 유지된다.
```
SELECT emp.employee_id, emp.last_name, 
dept.department_id, dept.department_name
FROM employees emp JOIN departments dept
ON emp.department_id = dept.department_id;
```

### 테이블명 생략
```
- 테이블간 컬럼명이 중복되지 않는다면 컬럼이 속한 테이블명을 생략할 수 있다.

SELECT employee_id, last_name, 
dept.department_id, department_name
FROM employees emp JOIN departments dept
ON emp.department_id = dept.department_id;

- JOIN의 문법을 통해서 연결된 테이블의 정보에서도 WHERE절을 사용할 수 있다.
SELECT employee_id, last_name, 
dept.department_id, department_name
FROM employees emp JOIN departments dept
ON emp.department_id = dept.department_id
WHERE emp.manager_id = 149;

EMPLOYEE_ID|LAST_NAME |DEPARTMENT_ID|DEPARTMENT_NAME|
-----------+----------+-------------+---------------+
        174|Abel      |           80|Sales          |
        175|Hutton    |           80|Sales          |
        179|Johnson   |           80|Sales          |
        177|Livingston|           80|Sales          |
        176|Taylor    |           80|Sales          |

```

연습문제
```

SELECT employee_id, last_name, salary, 
dept.department_id, department_name
FROM employees emp JOIN departments dept
ON emp.department_id = dept.department_id;

EMPLOYEE_ID|LAST_NAME  |SALARY|DEPARTMENT_ID|DEPARTMENT_NAME |
-----------+-----------+------+-------------+----------------+
        200|Whalen     |  4400|           10|Administration  |
        201|Hartstein  | 13000|           20|Marketing       |
        202|Fay        |  6000|           20|Marketing       |
        114|Raphaely   | 11000|           30|Purchasing      |
        115|Khoo       |  3100|           30|Purchasing      |
        116|Baida      |  2900|           30|Purchasing      |
        117|Tobias     |  2800|           30|Purchasing      |
        118|Himuro     |  2600|           30|Purchasing      |
        119|Colmenares |  2500|           30|Purchasing      |
        203|Mavris     |  6500|           40|Human Resources |
        120|Weiss      |  8000|           50|Shipping        |
        121|Fripp      |  8200|           50|Shipping        |
        122|Kaufling   |  7900|           50|Shipping        |
…
```

### ON절을 사용한 self-join
- 동일한 테이블 간의 데이터를 연결하여 출력하는 JOIN 문법

![unnamed](https://user-images.githubusercontent.com/95197594/163773607-f2749bd8-e47f-4a1c-80de-876fa61bb5bf.png)
```
- 사원과 매니저직원의 정보를 같이 출력
SELECT worker.employee_id, worker.last_name, worker.manager_id,
manager.employee_id, manager.last_name, manager.manager_id
FROM employees worker JOIN employees manager
ON worker.manager_id = manager.employee_id;

EMPLOYEE_ID|LAST_NAME  |MANAGER_ID|EMPLOYEE_ID|LAST_NAME|MANAGER_ID|
-----------+-----------+----------+-----------+---------+----------+
        105|Austin     |       103|        103|Hunold   |       102|
…
        103|Hunold     |       102|        102|De Haan  |       100|
…
        102|De Haan    |       100|        100|King     |          |
…



------------------------------------------------------------------------------------------------



- 매니저의 사원번호가 103인 사원들을 출력 (103 사원의 부하직원 출력)
SELECT worker.employee_id, worker.last_name, worker.manager_id,
manager.employee_id, manager.last_name, manager.manager_id
FROM employees worker JOIN employees manager
ON worker.manager_id = manager.employee_id
WHERE worker.manager_id = 103;

EMPLOYEE_ID|LAST_NAME|MANAGER_ID|EMPLOYEE_ID|LAST_NAME|MANAGER_ID|
-----------+---------+----------+-----------+---------+----------+
        104|Ernst    |       103|        103|Hunold   |       102|
        105|Austin   |       103|        103|Hunold   |       102|
        106|Pataballa|       103|        103|Hunold   |       102|
        107|Lorentz  |       103|        103|Hunold   |       102|

```


연습문제
```

SELECT w.last_name "Employee", w.employee_id "Emp#",
m.last_name "Manager", m.employee_id "Mgr#"
FROM employees w JOIN employees m
ON w.manager_id = m.employee_id;

Employee   |Emp#|Manager  |Mgr#|
-----------+----+---------+----+
Ozer       | 168|Cambrault| 148|
Bloom      | 169|Cambrault| 148|
Fox        | 170|Cambrault| 148|
Smith      | 171|Cambrault| 148|
Bates      | 172|Cambrault| 148|
Kumar      | 173|Cambrault| 148|
Hunold     | 103|De Haan  | 102|
Vishney    | 162|Errazuriz| 147|
Greene     | 163|Errazuriz| 147|
Marvins    | 164|Errazuriz| 147|
Lee        | 165|Errazuriz| 147|
Ande       | 166|Errazuriz| 147|
Banda      | 167|Errazuriz| 147|
Bissot     | 129|Fripp    | 121|
…
```

### 3 이상의 테이블 JOIN
- JOIN과 ON절의 기능은 동일하며 반복해서 연결할 테이블의 정보를 작성해서 JOIN을 할 수 있다.
```

SELECT employee_id, last_name, d.department_id, department_name, 
l.location_id, city, c.country_id, country_name, r.region_id, region_name
FROM employees e
JOIN departments d
ON e.department_id = d.department_id
JOIN locations l
ON d.location_id = l.location_id
JOIN countries c
ON l.country_id = c.country_id
JOIN regions r
ON c.region_id = r.region_id;

EMPLOYEE_ID|LAST_NAME  |DEPARTMENT_ID|DEPARTMENT_NAME |LOCATION_ID|CITY               |COUNTRY_ID|COUNTRY_NAME            |REGION_ID|REGION_NAME|
-----------+-----------+-------------+----------------+-----------+-------------------+----------+------------------------+---------+-----------+
        100|King       |           90|Executive       |       1700|Seattle            |US        |United States of America|        2|Americas   |
        101|Kochhar    |           90|Executive       |       1700|Seattle            |US        |United States of America|        2|Americas   |
        102|De Haan    |           90|Executive       |       1700|Seattle            |US        |United States of America|        2|Americas   |
        103|Hunold     |           60|IT              |       1400|Southlake          |US        |United States of America|        2|Americas   |
        104|Ernst      |           60|IT              |       1400|Southlake          |US        |United States of America|        2|Americas   |
        105|Austin     |           60|IT              |       1400|Southlake          |US        |United States of America|        2|Americas   |
…
```

### equi join
- ON절과 JOIN절을 사용하지 않는 문법
```

SELECT employee_id, last_name, e.department_id, department_name
FROM employees e, departments d
WHERE e.department_id = d.department_id;

EMPLOYEE_ID|LAST_NAME  |DEPARTMENT_ID|DEPARTMENT_NAME |
-----------+-----------+-------------+----------------+
        200|Whalen     |           10|Administration  |
        201|Hartstein  |           20|Marketing       |
        202|Fay        |           20|Marketing       |
        114|Raphaely   |           30|Purchasing      |
        115|Khoo       |           30|Purchasing      |
        116|Baida      |           30|Purchasing      |
…
```

### 자연 조인
```
SELECT employee_id, last_name, department_id, department_name
FROM employees NATURAL JOIN departments;

EMPLOYEE_ID|LAST_NAME |DEPARTMENT_ID|DEPARTMENT_NAME|
-----------+----------+-------------+---------------+
        101|Kochhar   |           90|Executive      |
        102|De Haan   |           90|Executive      |
        104|Ernst     |           60|IT             |
        105|Austin    |           60|IT             |
        106|Pataballa |           60|IT             |
        107|Lorentz   |           60|IT             |
…
```

### subquery
- 쿼리구문의 실행을 보조해주는 쿼리구문
- 메인쿼리구문보다 먼저 실행되며 실행결과를 메인쿼리에 돌려주고 사라짐.

![unnamed](https://user-images.githubusercontent.com/95197594/163781778-f1e77826-9475-43b4-8c1a-84cacc4b54cc.png)

