# [오전수업] DB 34차
## 고급 Subquery
### 단일행 subquery
- 하나의 행의 결과를 돌려주는 서브쿼리
- 서브쿼리가 사용된 경우 메인쿼리보다 우선 실행되어 그 결과를 메인쿼리에게 전달 후 메인쿼리가 실행된다.
- 주로 조건절에 사용되는 경우 비교연산자와 활용이 되는 편이다.
```sql

employee_id가 176인 사원의 job_id와 salary컬럼의 값이 고정적이라는 보장이 없으므로 이 값들을 직접 조회해서 메인 쿼리의 조건으로 사용하는 경우 구문이 실행되는 시점에 따라서 정확하지 않은 조건으로 구문이 실행될 수도 있다.
따라서 위의 경우에는 사용자가 직접 조건을 찾아서 값을 입력하기보다 구문이 실행되는 시점에서 필요한 조건값을 대신 출력해주는 메인 쿼리를 보조해주는 서브쿼리를 사용하는 것이 낫다.
SELECT last_name,
       job_id,
       salary
FROM employees
WHERE job_id = (
        SELECT job_id
        FROM employees
        WHERE employee_id = 176
    )
      AND salary > (
    SELECT salary
    FROM employees
    WHERE employee_id = 176
);


LAST_NAME|JOB_ID|SALARY|
---------+------+------+
Tucker   |SA_REP| 10000|
Bernstein|SA_REP|  9500|
Hall     |SA_REP|  9000|
King     |SA_REP| 10000|
Sully    |SA_REP|  9500|
McEwen   |SA_REP|  9000|
Vishney  |SA_REP| 10500|
Greene   |SA_REP|  9500|
Ozer     |SA_REP| 11500|
Bloom    |SA_REP| 10000|
Fox      |SA_REP|  9600|
Abel     |SA_REP| 11000|
Hutton   |SA_REP|  8800|

------------------------------------------------------------------------------------------------------

아래의 예문에서는 subquery에서 그룹을 사용한 그룹함수의 결과를 메인쿼리의 비교연산의 조건값으로 입력을 받았음.
이 경우 department_id로 그룹을 묶었을 때 1개의 그룹이 나오는 상황이 아니라면 서브쿼리가 돌려주는 값이 1개가 넘어가는 행을 돌려주게 되므로 비교연산자 '='가 조건값으로 받을 수 없게된다.

SELECT employee_id,
       last_name
FROM employees
WHERE salary = (
    SELECT MIN(salary)
    FROM employees
    GROUP BY department_id
);



SQL Error [1427] [21000]: ORA-01427: 단일 행 하위 질의에 2개 이상의 행이 리턴되었습니다.


-----------------------------------------------------------------------------------------------------


아래의 예문에서는 사원의 last_name이 'Haas'인 사원이 여럿인 경우를 제외하면 특별히 문제가 되지 않는 구문이지만 결과가 출력되지 않는다.
SELECT last_name,
       job_id
FROM employees
WHERE job_id = (
    SELECT job_id
    FROM employees
    WHERE last_name = 'Haas'
);

LAST_NAME|JOB_ID|
---------+------+

이유는 서브쿼리가 돌려주는 행이 없기 때문에 조건을 만족하는 행이 존재하지 않게된다.
SELECT job_id
FROM employees
WHERE last_name = 'Haas';


JOB_ID|
------+

```

### 다중행 비교연산자
- 여러 행의 결과를 받아서 비교연산을 할 수 있는 연산자

- IN	: 비교연산자 중 '='과 기능이 유사하나 여러 조건값을 받아 비교하여 조건을 만족하는 행을 출력해준다.
- ANY	: 비교연산자와 조합하여 사용하며 조건값들 중 하나라도 비교연산의 조건을 만족하는 경우 출력. SOME 연산자와 기능이 동일하다.
- ALL	: 비교연산자와 조합하여 사용하며 조건값들에 대해서 모두 비교연산의 조건을 만족하는 경우 출력

```sql
사원 중 job_id이 'IT_PROG'인 사원들의 급여값들에 대해서 더 작은 급여를 받으면서, job_id의 값이 'IT_PROG'가 아닌 조건을 만족하는 행을 출력.
SELECT employee_id,
       last_name,
       job_id,
       salary
FROM employees
WHERE salary < ANY (
    SELECT salary
    FROM employees
    WHERE job_id = 'IT_PROG'
)
      AND job_id <> 'IT_PROG';


EMPLOYEE_ID|LAST_NAME  |JOB_ID    |SALARY|
-----------+-----------+----------+------+
        175|Hutton     |SA_REP    |  8800|
        176|Taylor     |SA_REP    |  8600|
        177|Livingston |SA_REP    |  8400|
        206|Gietz      |AC_ACCOUNT|  8300|
        110|Chen       |FI_ACCOUNT|  8200|
        121|Fripp      |ST_MAN    |  8200|
        120|Weiss      |ST_MAN    |  8000|
        159|Smith      |SA_REP    |  8000|
        153|Olsen      |SA_REP    |  8000|
        122|Kaufling   |ST_MAN    |  7900|
        112|Urman      |FI_ACCOUNT|  7800|
        111|Sciarra    |FI_ACCOUNT|  7700|
        160|Doran      |SA_REP    |  7500|
        154|Cambrault  |SA_REP    |  7500|
        171|Smith      |SA_REP    |  7400|




```
![unnamed](https://user-images.githubusercontent.com/95197594/170614909-e2c314d8-f2d2-407d-ba6e-05c98e9108e2.png)

```sql
SELECT employee_id,
       last_name,
       job_id,
       salary
FROM employees
WHERE salary < ALL (
    SELECT salary
    FROM employees
    WHERE job_id = 'IT_PROG'
)
      AND job_id <> 'IT_PROG';

EMPLOYEE_ID|LAST_NAME  |JOB_ID  |SALARY|
-----------+-----------+--------+------+
        185|Bull       |SH_CLERK|  4100|
        192|Bell       |SH_CLERK|  4000|
        193|Everett    |SH_CLERK|  3900|
        188|Chung      |SH_CLERK|  3800|
        137|Ladwig     |ST_CLERK|  3600|
        189|Dilly      |SH_CLERK|  3600|
        141|Rajs       |ST_CLERK|  3500|
        186|Dellinger  |SH_CLERK|  3400|
        133|Mallin     |ST_CLERK|  3300|
        129|Bissot     |ST_CLERK|  3300|
        180|Taylor     |SH_CLERK|  3200|
        138|Stiles     |ST_CLERK|  3200|
        125|Nayer      |ST_CLERK|  3200|
        194|McCain     |SH_CLERK|  3200|
```

![unnamed (1)](https://user-images.githubusercontent.com/95197594/170614984-45a862ee-38af-4292-bf5b-cb438607b751.png)

---

# [오후수업] JSP 74차
