# [오전수업] DB 15차
> DB 14차에서 수업한 내용 복습 실시

### 테이블 생성 연습
```
테이블명 : member

컬럼명    / 데이터타입 / 크기 / 제약조건
member_id / NUMBER    / 5    / PRIMARY KEY
last_name / VARCHAR2  / 30   / NOT NULL
phone_no  / VARCHAR2  / 25   / UNIQUE



CREATE TABLE MEMBER
(member_id	NUMBER(5) PRIMARY KEY,
last_name	VARCHAR2(30) NOT NULL,
phone_no	VARCHAR2(25) UNIQUE);

```


연습문제
```
# employees 테이블로부터 커미션을 받지 않는 모든 사원의 last_name, salary, commission_pct를 출력하되
salary를 기준으로 내림차순 정렬기


SELECT last_name, salary, commission_pct
FROM employees 
WHERE commission_pct IS NULL
ORDER BY salary DESC;


LAST_NAME  |SALARY|COMMISSION_PCT|
-----------+------+--------------+
King       | 24000|              |
Kochhar    | 17000|              |
De Haan    | 17000|              |
Hartstein  | 13000|              |
Higgins    | 12008|              |
Greenberg  | 12008|              |
Raphaely   | 11000|              |
…
```


## 단일행 함수(Function)
- 연산을 보조해주는 오브젝트
- 단일행 함수는 행단위로 함수의 연산 결과를 출력

## 문자함수
### 대소문자 변환 함수
- higgins의 성을 가진 사원은 있으나 결과로 출력되지 않음
- 문자열 값 비교 시 대소문자까지 일치하지 않으면 결과로 출력되지 않음

```

SELECT employee_id, last_name, department_id
FROM employees
WHERE last_name = 'higgins';

EMPLOYEE_ID|LAST_NAME|DEPARTMENT_ID|
-----------+---------+-------------+



- last_name 컬럼의 값에 LOWER함수를 적용하여 연산된 내용과 조건값인 'higgins' 비교하여 결과를 출력

SELECT employee_id, last_name, department_id 
FROM employees
WHERE LOWER(last_name) = 'higgins';

EMPLOYEE_ID|LAST_NAME|DEPARTMENT_ID|
-----------+---------+-------------+
        205|Higgins  |          110|
        
        
        

SELECT 'The job id for ' || UPPER(last_name) || ' is ' || LOWER(job_id)
AS "EMPLOYEE DETAILS"
FROM employees;

EMPLOYEE DETAILS                      |
--------------------------------------+
The job id for ABEL is sa_rep         |
The job id for ANDE is sa_rep         |
The job id for ATKINSON is st_clerk   |
The job id for AUSTIN is it_prog      |
The job id for BAER is pr_rep         |
The job id for BAIDA is pu_clerk      |
The job id for BANDA is sa_rep        |
…
```

