# [오전수업] DB 37차

> 36차에서 실시한 연습문제 풀이 이어서 실시

```sql
p.75_3

SELECT job_id                                 "Job",
       SUM(decode(department_id, 20, salary)) "Dept 20",
       SUM(decode(department_id, 50, salary)) "Dept 50",
       SUM(decode(department_id, 80, salary)) "Dept 80",
       SUM(decode(department_id, 90, salary)) "Dept 90",
       SUM(salary)                            "Total"
FROM employees
GROUP BY job_id;
```

---

## 집합 연산자
### 합집합(UNION)
- 서로 다른 쿼리 구문의 결과를 합쳐서 출력하는 명령어
- 출력 시 출력하는 쿼리구문의 컬럼의 데이터 타입은 일치해야함
- 컬럼명은 첫번째 쿼리구문의 컬럼명을 출력
- UNION은 결과값을 정렬하고 중복값을 제거한 상태로 출력

```sql
SELECT employee_id,
       last_name,
       first_name,
       salary,
       department_id
FROM employees
WHERE salary > 10000
      AND department_id = 80;


EMPLOYEE_ID|LAST_NAME|FIRST_NAME|SALARY|DEPARTMENT_ID|
-----------+---------+----------+------+-------------+
        145|Russell  |John      | 14000|           80|
        146|Partners |Karen     | 13500|           80|
        147|Errazuriz|Alberto   | 12000|           80|
        148|Cambrault|Gerald    | 11000|           80|
        149|Zlotkey  |Eleni     | 10500|           80|
        162|Vishney  |Clara     | 10500|           80|
        168|Ozer     |Lisa      | 11500|           80|
        174|Abel     |Ellen     | 11000|           80|


-----------------------------------------------------------------------------

SELECT employee_id,
       last_name,
       first_name,
       salary,
       department_id
FROM employees
WHERE salary > 10000
      AND department_id = 90;

EMPLOYEE_ID|LAST_NAME|FIRST_NAME|SALARY|DEPARTMENT_ID|
-----------+---------+----------+------+-------------+
        100|King     |Steven    | 24000|           90|
        101|Kochhar  |Neena     | 17000|           90|
        102|De Haan  |Lex       | 17000|           90|

-----------------------------------------------------------------------------

SELECT employee_id,
       last_name,
       first_name,
       salary,
       department_id
FROM employees
WHERE salary > 10000
      AND department_id = 80
UNION
SELECT employee_id,
       last_name,
       first_name,
       salary,
       department_id
FROM employees
WHERE salary > 10000
      AND department_id = 90;


EMPLOYEE_ID|LAST_NAME|FIRST_NAME|SALARY|DEPARTMENT_ID|
-----------+---------+----------+------+-------------+
        100|King     |Steven    | 24000|           90|
        101|Kochhar  |Neena     | 17000|           90|
        102|De Haan  |Lex       | 17000|           90|
        145|Russell  |John      | 14000|           80|
        146|Partners |Karen     | 13500|           80|
        147|Errazuriz|Alberto   | 12000|           80|
        148|Cambrault|Gerald    | 11000|           80|
        149|Zlotkey  |Eleni     | 10500|           80|
        162|Vishney  |Clara     | 10500|           80|
        168|Ozer     |Lisa      | 11500|           80|
        174|Abel     |Ellen     | 11000|           80|



-----------------------------------------------------------------------------

컬럼 수가 맞지 않거나 출력할 내용이 없는 경우 NULL을 통해서 컬럼을 채울 수 있다.
SELECT manager_id,
       first_name,
       last_name,
       salary * 2,
       department_id
FROM employees
WHERE salary > 10000
      AND department_id = 80
UNION
SELECT department_id,
       department_name,
       NULL,
       manager_id,
       location_id
FROM departments;


MANAGER_ID|FIRST_NAME          |LAST_NAME|SALARY*2|DEPARTMENT_ID|
----------+--------------------+---------+--------+-------------+
        10|Administration      |         |     200|         1700|
        20|Marketing           |         |     201|         1800|
        30|Purchasing          |         |     114|         1700|
        40|Human Resources     |         |     203|         2400|
        50|Shipping            |         |     121|         1500|
        60|IT                  |         |     103|         1400|
        70|Public Relations    |         |     204|         2700|
        80|Sales               |         |     145|         2500|
        90|Executive           |         |     100|         1700|
       100|Alberto             |Errazuriz|   24000|           80|
       100|Eleni               |Zlotkey  |   21000|           80|
       100|Finance             |         |     108|         1700|
       100|Gerald              |Cambrault|   22000|           80|
       100|John                |Russell  |   28000|           80|
       100|Karen               |Partners |   27000|           80|
…

```

### UNION ALL
- UNION 과 동일하게 다른 쿼리 구문의 결과를 합쳐서 출력
  - 다만, 결과값에 대한 정렬이나 중복값의 제거는 하지 않음

```sql
SELECT employee_id,
       last_name,
       first_name,
       salary,
       department_id
FROM employees
WHERE salary > 10000
      AND department_id > 100
UNION ALL
SELECT employee_id,
       last_name,
       first_name,
       salary,
       department_id
FROM employees
WHERE salary > 10000
      AND department_id > 90;


EMPLOYEE_ID|LAST_NAME|FIRST_NAME|SALARY|DEPARTMENT_ID|
-----------+---------+----------+------+-------------+
        205|Higgins  |Shelley   | 12008|          110|
        108|Greenberg|Nancy     | 12008|          100|
        205|Higgins  |Shelley   | 12008|          110|

```

### 교집합(INTERSECT)
- 서로 다른 쿼리구문에서 동일한 결과의 행들만 선택하여 출력하는 명령어

```sql

SELECT employee_id,
       last_name,
       first_name,
       salary,
       department_id
FROM employees
WHERE salary > 10000
      AND department_id >= 100
INTERSECT
SELECT employee_id,
       last_name,
       first_name,
       salary,
       department_id
FROM employees
WHERE salary > 10000
      AND department_id >= 90;


EMPLOYEE_ID|LAST_NAME|FIRST_NAME|SALARY|DEPARTMENT_ID|
-----------+---------+----------+------+-------------+
        108|Greenberg|Nancy     | 12008|          100|
        205|Higgins  |Shelley   | 12008|          110|
```

### 차집합(MINUS)

```sql
SELECT employee_id,
       last_name,
       first_name,
       salary,
       department_id
FROM employees
WHERE salary > 10000
      AND department_id >= 90
MINUS
SELECT employee_id,
       last_name,
       first_name,
       salary,
       department_id
FROM employees
WHERE salary > 10000
      AND department_id >= 100;

EMPLOYEE_ID|LAST_NAME|FIRST_NAME|SALARY|DEPARTMENT_ID|
-----------+---------+----------+------+-------------+
        100|King     |Steven    | 24000|           90|
        101|Kochhar  |Neena     | 17000|           90|
        102|De Haan  |Lex       | 17000|           90|

```

## 계층형 쿼리
- start with	: 가장 상단에 위치할 행의 조건
- connect by	: 계층의 관계를 설정하는 조건
- prior		: connect by절에서 사용. 상위 계층에 해당하는 컬럼 옆에 작성

```sql
- 상위계층이 가지고 있는 manager_id컬럼의 값과 동일한 employee_id를 가진 행을 하위계층으로 연결하여 출력


SELECT employee_id, last_name, manager_id
FROM employees
START WITH employee_id = 107
CONNECT BY employee_id = PRIOR manager_id;


EMPLOYEE_ID|LAST_NAME|MANAGER_ID|
-----------+---------+----------+
        107|Lorentz  |       103|
        103|Hunold   |       102|
        102|De Haan  |       100|
        100|King     |          |

```

- 계층형 쿼리구문을 사용할 때 숨겨진 의사컬럼으로 LEVEL컬럼을 사용할 수 있음
- LEVEL 컬럼은 해당 행이 상위 몇 번째 계층인지를 나타내고 있는 값이며 숫자로 표현됨
- 최상위 계층은 LEVEL컬럼의 값이 1임

```sql

SELECT
	LEVEL,
	employee_id,
	lpad(' ', LEVEL, ' ')  --LPAD('출력문자', 출력을 원하는 길이(숫자), '여백문자')
       || first_name
       || ' '
       || last_name name,
	manager_id
FROM
	employees
START WITH
	manager_id IS NULL
CONNECT BY
	PRIOR employee_id = manager_id;


LEVEL|EMPLOYEE_ID|NAME                 |MANAGER_ID|
-----+-----------+---------------------+----------+
    1|        100| Steven King         |          |
    2|        101|  Neena Kochhar      |       100|
    3|        108|   Nancy Greenberg   |       101|
    4|        109|    Daniel Faviet    |       108|
    4|        110|    John Chen        |       108|
    4|        111|    Ismael Sciarra   |       108|
    4|        112|    Jose Manuel Urman|       108|
    4|        113|    Luis Popp        |       108|
    3|        200|   Jennifer Whalen   |       101|
    3|        203|   Susan Mavris      |       101|
    3|        204|   Hermann Baer      |       101|
    3|        205|   Shelley Higgins   |       101|
    4|        206|    William Gietz    |       205|
    2|        102|  Lex De Haan        |       100|
    3|        103|   Alexander Hunold  |       102|
    4|        104|    Bruce Ernst      |       103|
    4|        105|    David Austin     |       103|
    4|        106|    Valli Pataballa  |       103|
    4|        107|    Diana Lorentz    |       103|
    2|        114|  Den Raphaely       |       100|
    3|        115|   Alexander Khoo    |       114|
    3|        116|   Shelli Baida      |       114|
…
```
  



---

# [오후수업] JAVA 56차

