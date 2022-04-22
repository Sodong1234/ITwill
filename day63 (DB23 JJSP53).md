# [오전수업] DB 23차

> 22차 수업 복습 실시

### INLINE VIEW
- 가상의 테이블을 구현할 수 있는 VIEW의 일종인 INLINE VIEW는 서브쿼리를 통해서 구현가능하다.
- 서브쿼리의 출력 결과가 가상의 테이블로 사용된다. 이렇게 생성된 VIEW의 경우 뷰의 이름을 활용하기 어렵기 때문에 필수적으로 table alias를 활용한다.
- 또한 서브쿼리의 출력 컬럼이 view의 컬럼으로 사용되는데 컬럼명으로 특수기호들은 사용이 안되므로 함수나 표현식의 연산 내용이 있는 경우 이를 column alias로 이름을 대체해야 한다.
- column alias를 사용한 경우 view의 컬럼명은 column alias로 적용된다.

```

SELECT a.last_name, a.salary, a.department_id, b.salavg
FROM employees a
JOIN (
    SELECT department_id, trunc(AVG(salary)) salavg
    FROM employees
    GROUP BY department_id
) b ON a.department_id = b.department_id
       AND a.salary > b.salavg;

LAST_NAME|SALARY|DEPARTMENT_ID|SALAVG|
---------+------+-------------+------+
King     | 24000|           90| 19333|
Hunold   |  9000|           60|  5760|
Ernst    |  6000|           60|  5760|
Greenberg| 12008|          100|  8601|
Faviet   |  9000|          100|  8601|
Raphaely | 11000|           30|  4150|
Weiss    |  8000|           50|  3475|
Fripp    |  8200|           50|  3475|
Kaufling |  7900|           50|  3475|
Vollman  |  6500|           50|  3475|
Mourgos  |  5800|           50|  3475|
Ladwig   |  3600|           50|  3475|
Rajs     |  3500|           50|  3475|
Russell  | 14000|           80|  8955|
Partners | 13500|           80|  8955|
…


-----------------------------------------------------------------------------------------------------------------------------------------------



TOP n 쿼리
rownum은 의사컬럼으로 테이블에 숨겨진 컬럼이다. rownum은 테이블의 기본 입력 순서값을 저장하고 있고, 데이터를 입력한 순으로 값이 매겨진다.
INLINE VIEW로 테이블을 원하는 순서로 정렬해서 출력하게 되면 이 inline view에서는 rownum의 값이 정렬된 순서로 적용된다.(테이블 원본은 바뀌지 않음)

SELECT employee_id, last_name, salary
FROM (
    SELECT *
    FROM employees
    ORDER BY salary DESC
)
WHERE rownum <= 10;


-----------------------------------------------------------------------------------------------------------------------------------------------



VIEW로 만들기
view는 구문 형태로만 만들어지는 오브젝트로 생성하더라도 자원을 거의 소모하지 않는다.
베이스 테이블의 데이터를 가져와서 출력하는 형태이므로 데이터의 중복 문제 없이 다양하게 원하는 형태로 구조를 가공하여 사용할 수 있는 장점이 있는 오브젝트 → 10단원에서 다룰 예정!!
CREATE VIEW salary_top_10 AS
    SELECT employee_id, last_name, salary
    FROM (
        SELECT *
        FROM employees
        ORDER BY salary DESC
    )
    WHERE ROWNUM <= 10;


SELECT *
FROM salary_top_10;


```


연습문제
```
p.28
SELECT employee_id, last_name, salary
FROM employees
WHERE salary >= (
    SELECT AVG(salary)
    FROM employees
)
ORDER BY salary ASC;

EMPLOYEE_ID|LAST_NAME |SALARY|
-----------+----------+------+
        203|Mavris    |  6500|
        123|Vollman   |  6500|
        165|Lee       |  6800|
        113|Popp      |  6900|
        155|Tuvault   |  7000|
        161|Sewall    |  7000|
        178|Grant     |  7000|
        164|Marvins   |  7200|
        172|Bates     |  7300|
        171|Smith     |  7400|
        154|Cambrault |  7500|
```


---

# [오후수업] JSP 53차
