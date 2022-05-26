# [오전수업] DB 33차
## INNER JOIN
- 기본적인 형태의 조인으로 조인의 조건을 만족하는 행들만 출력하는 분류의 조인의 문법이다.
  - ex) equi join, non-equi join, …
- 조인의 조건을 만족하는 행이 없는 경우 해당 행은 결과로 출력되지 않는다.

```sql
ex) 178번 사원은 현재 소속된 부서가 없으므로 NULL값이 입력되어 있음.

SELECT employee_id, last_name, department_id
FROM employees
WHERE department_id IS NULL;

EMPLOYEE_ID|LAST_NAME|DEPARTMENT_ID|
-----------+---------+-------------+
        178|Grant    |             |

---


이 때 사원과 부서 테이블간 동일한 department_id값을 가진 조건으로 JOIN구문을 작성한 경우
department_id컬럼의 값이 NULL인 부서는 존재하지 않으므로 사원번호가 178인 행은 JOIN조건을 만족하는 행이 존재하지 않아 결과로 출력되지 않는다.

SELECT employee_id, last_name, d.department_id, department_name
FROM employees e JOIN departments d
ON e.department_id = d.department_id
WHERE employee_id = 178;


EMPLOYEE_ID|LAST_NAME|DEPARTMENT_ID|DEPARTMENT_NAME|
-----------+---------+-------------+---------------+

```

## OUTER JOIN
- JOIN의 조건을 만족하는 행이 없는 행도 강제로 JOIN결과로 출력하는 JOIN방법
- 이 때 해당 행은 다른 행과 연결되는 대신 NULL값과 연결되어 출력된다.

### LEFT OUTER JOIN
- JOIN키워드 기준 왼쪽의 테이블에서 JOIN조건을 만족하지 않는 행을 강제로 출력

```sql
ANSI SQL 표준 JOIN 문법
SELECT employee_id, last_name, d.department_id, department_name
FROM employees e LEFT OUTER JOIN departments d
ON e.department_id = d.department_id;



ORACLE SQL 문법
SELECT employee_id, last_name, d.department_id, department_name
FROM employees e JOIN departments d
ON e.department_id = d.department_id(+);


EMPLOYEE_ID|LAST_NAME  |DEPARTMENT_ID|DEPARTMENT_NAME |
-----------+-----------+-------------+----------------+
        200|Whalen     |           10|Administration  |
        201|Hartstein  |           20|Marketing       |
        202|Fay        |           20|Marketing       |
        114|Raphaely   |           30|Purchasing      |
        115|Khoo       |           30|Purchasing      |
        116|Baida      |           30|Purchasing      |
…
        205|Higgins    |          110|Accounting      |
        206|Gietz      |          110|Accounting      |
        178|Grant      |             |                |
```


### RIGHT OUTER JOIN
- JOIN 키워드 기준 오른쪽의 테이블에서 JOIN조건을 만족하지 않는 행을 강제로 출력하는 JOIN문법
- INNER JOIN에서는 소속된 사원이 없는 부서들은 결과에서 출력되지 않았음.

```sql
ANSI SQL의 표준 문법 JOIN
SELECT employee_id, last_name, d.department_id, department_name
FROM employees e RIGHT OUTER JOIN departments d
ON e.department_id = d.department_id;




ORACLE SQL의 문법
SELECT employee_id, last_name, d.department_id, department_name
FROM employees e JOIN departments d
ON e.department_id(+) = d.department_id;


EMPLOYEE_ID|LAST_NAME  |DEPARTMENT_ID|DEPARTMENT_NAME     |
-----------+-----------+-------------+--------------------+
        200|Whalen     |           10|Administration      |
        201|Hartstein  |           20|Marketing           |
        202|Fay        |           20|Marketing           |
        114|Raphaely   |           30|Purchasing          |
…
        113|Popp       |          100|Finance             |
        205|Higgins    |          110|Accounting          |
        206|Gietz      |          110|Accounting          |
           |           |          120|Treasury            |
           |           |          130|Corporate Tax       |
           |           |          140|Control And Credit  |
           |           |          150|Shareholder Services|
```

### FULL OUTER JOIN
- JOIN 하는 양쪽테이블에서 JOIN조건을 만족하지 못하는 행들을 강제로 출력해주는 JOIN 문법
```sql
SELECT employee_id, last_name, d.department_id, department_name
FROM employees e FULL OUTER JOIN departments d
ON e.department_id = d.department_id;

EMPLOYEE_ID|LAST_NAME  |DEPARTMENT_ID|DEPARTMENT_NAME     |
-----------+-----------+-------------+--------------------+
…
        176|Taylor     |           80|Sales               |
        177|Livingston |           80|Sales               |
        178|Grant      |             |                    |
        179|Johnson    |           80|Sales               |
        180|Taylor     |           50|Shipping            |
…
        205|Higgins    |          110|Accounting          |
        206|Gietz      |          110|Accounting          |
           |           |          210|IT Support          |
           |           |          200|Operations          |
           |           |          270|Payroll             |
           |           |          180|Construction        |
           |           |          240|Government Sales    |
           |           |          250|Retail Sales        |
```

### CROSS JOIN
- 각 테이블의 행들을 논리곱의 형태로 결과를 만들어 출력하는 JOIN


![unnamed](https://user-images.githubusercontent.com/95197594/170404917-fb66f4ea-e897-4011-a36b-915ca0040f51.png)

```sql
ANSI SQL의 CROSS JOIN
SELECT last_name, department_name
FROM employees CROSS JOIN departments;



ORACLE SQL의 CATESIAN PRODUCT
SELECT last_name, department_name
FROM employees e , departments d;


LAST_NAME  |DEPARTMENT_NAME|
-----------+---------------+
Abel       |Administration |
Ande       |Administration |
Atkinson   |Administration |
…
Abel       |Marketing      |
Ande       |Marketing      |
Atkinson   |Marketing      |
Austin     |Marketing      |
…
Abel       |Purchasing     |
Ande       |Purchasing     |
Atkinson   |Purchasing     |
…



JOIN할 테이블이 가진 행들간 모든 조합을 만들어내는 JOIN으로 여기에 행에 대한 조건을 추가하면 일반적인 JOIN의 구문이 만들어진다.
SELECT last_name, department_name
FROM employees e , departments d
WHERE e.department_id = d.department_id;


LAST_NAME  |DEPARTMENT_NAME |
-----------+----------------+
Whalen     |Administration  |
Fay        |Marketing       |
Hartstein  |Marketing       |
Tobias     |Purchasing      |
Colmenares |Purchasing      |
Baida      |Purchasing      |
Raphaely   |Purchasing      |
Khoo       |Purchasing      |
Himuro     |Purchasing      |
Mavris     |Human Resources |
…
```

- 연습문제

![unnamed (1)](https://user-images.githubusercontent.com/95197594/170405334-a099ceef-5c79-461f-8294-c0ff16ec8ee0.png)
```sql
SELECT worker.last_name "Employee", worker.employee_id "EMP#",
manager.last_name "Manager", manager.employee_id "Mgr#"
FROM employees worker LEFT OUTER JOIN employees manager
ON worker.manager_id = manager.employee_id
ORDER BY worker.employee_id ASC;
```

![unnamed (2)](https://user-images.githubusercontent.com/95197594/170405386-037ae500-82df-470d-ba72-708dc3cae1d9.png)
```sql
SELECT worker.last_name, worker.hire_date,
manager.last_name, manager.hire_date
FROM employees worker JOIN employees manager
ON worker.manager_id = manager.employee_id
AND worker.hire_date < manager.hire_date;
```
---
# [오후수업] JAVA 50차
