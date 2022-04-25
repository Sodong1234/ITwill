# [오전수업] DB 24차
## DML(데이터 조작어 / Data Manipulation Language)
- 테이블의 데이터를 조작하는데 사용하는 문법
- 새로운 데이터 입력 (INSERT)
- 기존 데이터 갱신(UPDATE)
- 기존 데이터 삭제(DELETE)

### INSERT 구문
1번사진
- insert into 절 : 데이터를 입력할 테이블명과 입력 값의 컬럼의 목록
- values 절 : 입력할 값들의 목록 (작성 시 컬럼에 대한 데이터타입 크기에 맞춰 작성)
```
- 컬럼의 목록을 작성한경우 VALUES절의 값은 목록의 컬럼 순서에 맞춰 작성해야한다.

INSERT INTO departments (department_id, department_name, manager_id, location_id)
VALUES (280, 'Prog', 100, 1700);


DEPARTMENT_ID|DEPARTMENT_NAME     |MANAGER_ID|LOCATION_ID|
-------------+--------------------+----------+-----------+
…
          240|Government Sales    |          |       1700|
          250|Retail Sales        |          |       1700|
          260|Recruiting          |          |       1700|
          270|Payroll             |          |       1700|
          280|Prog                |       100|       1700|
```

NULL값이 입력되는 경우
```
- 아래와 같이 일부 컬럼에만 입력값을 작성한 경우 입력값이 없는 나머지 컬럼에는 기본적으로 NULL값이 입력된다.
manager_id, location_id 컬럼에는 NULL이 입력되었음.
INSERT INTO departments (department_id, department_name)
VALUES (290, 'shopping');


DEPARTMENT_ID|DEPARTMENT_NAME     |MANAGER_ID|LOCATION_ID|
-------------+--------------------+----------+-----------+
…
          260|Recruiting          |          |       1700|
          270|Payroll             |          |       1700|
          280|Prog                |       100|       1700|
          290|shopping            |          |           |



------------------------------------------------------------------------------------------------------------------------



- 컬럼의 목록을 생략하는 경우 테이블의 모든 컬럼에 대한 입력값을 VALUES절에 작성해야 한다.
- 특정 컬럼에 대해서 입력할 값이 없어 NULL을 입력을 원하는 경우 값을 생략할 수 없으므로 명시적으로 NULL값을 입력해줘야 한다.
- 명시적으로 NULL값을 입력하는 경우 컬럼에 설정된 기본값을 무시하고 NULL값이 입력된다.

INSERT INTO departments
VALUES (300, 'Tetris', NULL, NULL);


DEPARTMENT_ID|DEPARTMENT_NAME     |MANAGER_ID|LOCATION_ID|
-------------+--------------------+----------+-----------+
…
          260|Recruiting          |          |       1700|
          270|Payroll             |          |       1700|
          280|Prog                |       100|       1700|
          290|shopping            |          |           |
          300|Tetris              |          |           |
```

서브쿼리로 데이터 입력하기
```
실습 테이블 생성
CREATE TABLE sales_reps
    AS
        ( SELECT employee_id id, last_name   name, salary, commission_pct
        FROM employees
        WHERE 1 = 2
        );


desc sales_reps

이름             널?       유형           
-------------- -------- ------------ 
ID                      NUMBER(6)    
NAME           NOT NULL VARCHAR2(25) 
SALARY                  NUMBER(8,2)  
COMMISSION_PCT          NUMBER(2,2)  



SELECT * FROM sales_reps;

ID|NAME|SALARY|COMMISSION_PCT|
--+----+------+--------------+



------------------------------------------------------------------------------------------------------------------



- VALUES절 대신 서브쿼리 작성
- 이 경우 서브쿼리의 출력 컬럼의 순서와 insert into절의 컬럼의 순서, 데이터타입에 주의!


INSERT INTO sales_reps (
    id, name, salary, commission_pct
)
    SELECT employee_id, last_name, salary, commission_pct
    FROM employees
    WHERE job_id LIKE '%REP%';


SELECT * FROM sales_reps;

ID |NAME      |SALARY|COMMISSION_PCT|
---+----------+------+--------------+
150|Tucker    | 10000|           0.3|
151|Bernstein |  9500|          0.25|
152|Hall      |  9000|          0.25|
153|Olsen     |  8000|           0.2|
154|Cambrault |  7500|           0.2|
155|Tuvault   |  7000|          0.15|
156|King      | 10000|          0.35|
157|Sully     |  9500|          0.35|
158|McEwen    |  9000|          0.35|
…
```

### UPDATE 구문
- 기존 행의 데이터를 다른 값으로 갱신하는 구문
2번사진

```
- 실습용 emp2 테이블 생성 / 기존 employees테이블을 통으로 복사

CREATE TABLE emp2
    AS
        ( SELECT *
        FROM employees
        );


SELECT employee_id, last_name, department_id
FROM emp2
WHERE employee_id = 113;

EMPLOYEE_ID|LAST_NAME|DEPARTMENT_ID|
-----------+---------+-------------+
        113|Popp     |          100|



---------------------------------------------------------------------------------------------------------



113 사원의 department_id컬럼의 값을 50으로 갱신
UPDATE emp2
SET department_id = 50
WHERE employee_id = 113;

EMPLOYEE_ID|LAST_NAME|DEPARTMENT_ID|
-----------+---------+-------------+
        113|Popp     |           50|
```

WHERE절 없이 갱신
```
- 현재 emp2 테이블의 department_id컬럼의 값은 12가지이다.

SELECT DISTINCT department_id
FROM emp2;

DEPARTMENT_ID|
-------------+
          100|
           30|
             |
           90|
           20|
           70|
          110|
           50|
           80|
           40|
           60|
           10|



------------------------------------------------------------------------------------------------------------------------



emp2테이블의 모든 행에 대해서 department_id 컬럼의 값을 110으로 갱신
UPDATE emp2
SET department_id = 110;


SELECT DISTINCT department_id
FROM emp2;

DEPARTMENT_ID|
-------------+
          110|
```

서브쿼리를 활용한 UPDATE구문 작성
```
SELECT employee_id, salary, job_id
FROM emp2
WHERE employee_id IN (113, 205);


EMPLOYEE_ID|SALARY|JOB_ID    |
-----------+------+----------+
        113|  6900|FI_ACCOUNT|
        205| 12008|AC_MGR    |



------------------------------------------------------------------------------------------------------------------------



- 113번 사원에 대해서 205번 사원의 job_id, salary컬럼의 값을 갱신하는 구문

UPDATE emp2
SET job_id = (
    SELECT job_id
    FROM employees
    WHERE employee_id = 205
), salary = (
    SELECT salary
    FROM employees
    WHERE employee_id = 205
)
WHERE employee_id = 113;


SELECT employee_id, salary, job_id
FROM emp2
WHERE employee_id IN (113, 205);


EMPLOYEE_ID|SALARY|JOB_ID|
-----------+------+------+
        113| 12008|AC_MGR|
        205| 12008|AC_MGR|
```

### DELETE구문
- 기존 테이블의 데이터를 삭제하는 구문
3번사진

```

INSERT INTO departments (department_id, department_name)
VALUES (290, 'shopping');


DEPARTMENT_ID|DEPARTMENT_NAME     |MANAGER_ID|LOCATION_ID|
-------------+--------------------+----------+-----------+
…
          260|Recruiting          |          |       1700|
          270|Payroll             |          |       1700|
          280|Prog                |       100|       1700|
          290|shopping            |          |           |



--------------------------------------------------------------------------------------------------------------------------



DELETE FROM departments
WHERE department_name = 'shopping';

DEPARTMENT_ID|DEPARTMENT_NAME     |MANAGER_ID|LOCATION_ID|
-------------+--------------------+----------+-----------+
…
          260|Recruiting          |          |       1700|
          270|Payroll             |          |       1700|
          280|Prog                |       100|       1700|
          300|Tetris              |          |           |
```

where 절 생략한 DELETE구문
```
DELETE FROM emp2;


SELECT * FROM emp2;

EMPLOYEE_ID|FIRST_NAME|LAST_NAME|EMAIL|PHONE_NUMBER|HIRE_DATE|JOB_ID|SALARY|COMMISSION_PCT|MANAGER_ID|DEPARTMENT_ID|
-----------+----------+---------+-----+------------+---------+------+------+--------------+----------+-------------+
```

## 트랜잭션
- 한번에 처리되어야 할 논리적인 작업단위
- 여러 개의 DML구문이 모여서 트랜잭션을 구성할 수 있다.
- 하나의 DDL, DCL구문이 트랜잭션을 구성한다.
4번사진

### TCL(Transaction Control Language)
- DML구문은 사용자가 TCL구문을 통해서 직접 트랜잭션을 종료할 수 있다.
- COMMIT : 트랜잭션의 작업을 DB에 반영
- ROLLBACK : 트랜잭션의 작업을 취소
- SAVEPOINT : 트랜잭션 진행 중 돌아갈 수 있는 지점을 생성
5번사진

```
SELECT * FROM sales_reps;
ID |NAME      |SALARY|COMMISSION_PCT|
---+----------+------+--------------+
…
177|Livingston|  8400|           0.2|
178|Grant     |  7000|          0.15|
179|Johnson   |  6200|           0.1|
202|Fay       |  6000|              |
203|Mavris    |  6500|              |
204|Baer      | 10000|              |



--------------------------------------------------------------------------------------------------------------------------



DELETE FROM sales_reps
WHERE id > 200;

SAVEPOINT after_del_200;

SELECT * FROM sales_reps;
ID |NAME      |SALARY|COMMISSION_PCT|
---+----------+------+--------------+
157|Sully     |  9500|          0.35|
158|McEwen    |  9000|          0.35|
159|Smith     |  8000|           0.3|
160|Doran     |  7500|           0.3|
161|Sewall    |  7000|          0.25|
162|Vishney   | 10500|          0.25|



--------------------------------------------------------------------------------------------------------------------------



DELETE FROM sales_reps
WHERE id > 160;

SELECT * FROM sales_reps;
ID |NAME      |SALARY|COMMISSION_PCT|
---+----------+------+--------------+
157|Sully    |  9500|          0.35|
158|McEwen   |  9000|          0.35|
159|Smith    |  8000|           0.3|
160|Doran    |  7500|           0.3|



--------------------------------------------------------------------------------------------------------------------------



ROLLBACK TO SAVEPOINT after_del_200;

SELECT * FROM sales_reps;
ID |NAME      |SALARY|COMMISSION_PCT|
---+----------+------+--------------+
159|Smith     |  8000|           0.3|
160|Doran     |  7500|           0.3|
161|Sewall    |  7000|          0.25|
162|Vishney   | 10500|          0.25|
163|Greene    |  9500|          0.15|
164|Marvins   |  7200|           0.1|
165|Lee       |  6800|           0.1|
```
---

# [오후수업] JAVA 37차
