# [오전수업] DB 36차
## case 문법

### case 문법 사용법1
- 하나의 컬럼에 대해서 제시한 조건과 일치한 경우 출력값을 지정할 수 있다.

![unnamed](https://user-images.githubusercontent.com/95197594/172980208-d08a7d39-1701-4231-b7c4-5e03aa942672.png)

```sql
SELECT last_name,
       job_id,
       salary,
       CASE job_id
           WHEN 'IT_PROG'  THEN
               1.10 * salary
           WHEN 'ST_CLERK' THEN
               1.15 * salary
           WHEN 'SA_REP'   THEN
               1.20 * salary
           ELSE
               salary
       END "revised_salary"
FROM employees;



LAST_NAME  |JOB_ID    |SALARY|revised_salary|
-----------+----------+------+--------------+
King       |AD_PRES   | 24000|         24000|
Kochhar    |AD_VP     | 17000|         17000|
De Haan    |AD_VP     | 17000|         17000|
Hunold     |IT_PROG   |  9000|          9900|
Ernst      |IT_PROG   |  6000|          6600|
Austin     |IT_PROG   |  4800|          5280|
Pataballa  |IT_PROG   |  4800|          5280|
Lorentz    |IT_PROG   |  4200|          4620|
Greenberg  |FI_MGR    | 12008|         12008|
…
```

### case 구문 사용법 2
개별 조건식을 작성하여 조건과 일치한 경우의 출력값을 지정하여 결과를 만들 수 있다.

![unnamed (1)](https://user-images.githubusercontent.com/95197594/172980311-6b6ce9ee-e7ef-4f27-9541-ca4cdc97b759.png)

```sql
SELECT last_name,
       salary,
       (
           CASE
               WHEN salary < 5000  THEN
                   'Low'
               WHEN salary < 10000 THEN
                   'Medium'
               WHEN salary < 20000 THEN
                   'Good'
               ELSE
                   'Excellent'
           END
       ) qualified_salary
FROM employees;


LAST_NAME  |SALARY|QUALIFIED_SALARY|
-----------+------+----------------+
King       | 24000|Excellent       |
Kochhar    | 17000|Good            |
De Haan    | 17000|Good            |
Hunold     |  9000|Medium          |
Ernst      |  6000|Medium          |
Austin     |  4800|Low             |
Pataballa  |  4800|Low             |
Lorentz    |  4200|Low             |
Greenberg  | 12008|Good            |
…
```

## DECODE 함수
- case 구문과 유사하게 사용할 수 있는 함수로 함수의 사용법의 특성 상 조건값과 결과값들을 별도의 키워드 수식없이 함수의 인자값으로 나열하여 작성하여 사용하는 함수
- 각 인자값들간 순서의 구분이 표시되지 않기 때문에 함수의 구조를 모른다면 사용하기 어려울 수 있음
- ELSE에 해당하는 마지막 기본값은 필요없다면 사용하지 않아도 상관없음

![unnamed (2)](https://user-images.githubusercontent.com/95197594/172980418-e9f921b5-81ac-4c2e-a660-0885a6a523d2.png)

```sql
SELECT last_name,
       job_id,
       salary,
       decode(job_id, 'IT_PROG', 1.10 * salary, 
                      'ST_CLERK', 1.15 * salary,
                      'SA_REP', 1.20 * salary, 
                      salary) revised_salary
FROM employees;



LAST_NAME  |JOB_ID    |SALARY|REVISED_SALARY|
-----------+----------+------+--------------+
King       |AD_PRES   | 24000|         24000|
Kochhar    |AD_VP     | 17000|         17000|
De Haan    |AD_VP     | 17000|         17000|
Hunold     |IT_PROG   |  9000|          9900|
Ernst      |IT_PROG   |  6000|          6600|
Austin     |IT_PROG   |  4800|          5280|
Pataballa  |IT_PROG   |  4800|          5280|
Lorentz    |IT_PROG   |  4200|          4620|
Greenberg  |FI_MGR    | 12008|         12008|
Faviet     |FI_ACCOUNT|  9000|          9000|
Chen       |FI_ACCOUNT|  8200|          8200|
…
```

- salary의 값을 2000으로 나눈 몫을 정수범위로 버림연산한 결과를 기준으로 decode함수를 통한 연산 예제
```sql

SELECT last_name,
       salary,
       decode(trunc(salary / 2000, 0), 
                0, 0.00, 
                1, 0.09,
                2, 0.20, 
                3, 0.30, 
                4, 0.40, 
                5, 0.42, 
                6, 0.44,
                0.45) tax_rate
FROM employees
WHERE department_id = 80;


LAST_NAME |SALARY|TAX_RATE|
----------+------+--------+
Russell   | 14000|    0.45|
Partners  | 13500|    0.44|
Errazuriz | 12000|    0.44|
Cambrault | 11000|    0.42|
Zlotkey   | 10500|    0.42|
Tucker    | 10000|    0.42|
Bernstein |  9500|     0.4|
Hall      |  9000|     0.4|
Olsen     |  8000|     0.4|
…
```

* 연습문제 풀이
```sql
p.75_1
SELECT job_id,
       CASE job_id
           WHEN 'AD_PRES'  THEN
               'A'
           WHEN 'ST_MAN'   THEN
               'B'
           WHEN 'IT_PROG'  THEN
               'C'
           WHEN 'SA_REP'   THEN
               'D'
           WHEN 'ST_CLERK' THEN
               'E'
           ELSE
               '0'
       END grade
FROM employees;


SELECT job_id,
       decode(job_id, 
            'AD_PRES', 'A', 
            'ST_MAN', 'B',
            'IT_PROG', 'C', 
            'SA_REP', 'D', 
            'ST_CLERK','E', 
            '0') grade
FROM employees;


---------------------------------------------------------------------------------------------

p.75_2
SELECT COUNT(*)                                              total,
       SUM(decode(to_char(hire_date, 'YYYY'), '2002', 1, 0)) "2002",
       SUM(decode(to_char(hire_date, 'YYYY'), '2003', 1, 0)) "2003",
       SUM(decode(to_char(hire_date, 'YYYY'), '2004', 1, 0)) "2004",
       SUM(decode(to_char(hire_date, 'YYYY'), '2005', 1, 0)) "2005"
FROM employees;


TOTAL|2002|2003|2004|2005|
-----+----+----+----+----+
  107|   7|   6|  10|  29|

```

---

# [오후수업] JSP 81차

> 테스트 실시
