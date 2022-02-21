# [오전수업] DB 6차

> DB 5차 수업때 실시한 SELECT 내용을 보충해 놓았음

## WHERE절
- 조건을 통해서 출력을 원하는 행을 선택
- 행을 제한하는 절
```
- 문법
WHERE 조건컬럼 연산자 조건값
```

```
- HR_EMPLOYEES 테이블의 employee_id, last_name, department_id의 데이터를 출력하되, department_id는 데이터가 90인 것만을 출력

SELECT employee_id, last_name, department_id 입력
FROM HR_EMPLOYEES 입력
WHERE department_id = 90; 입력

+-------------+-----------+---------------+
| employee_id | last_name | department_id |
+-------------+-----------+---------------+
|         100 | King      |            90 |
|         101 | Kochhar   |            90 |
|         102 | De Haan   |            90 |
+-------------+-----------+---------------+


-------------------------------------------------------------------------------------------------------------------------


- HR_EMPLOYEES 테이블의 employee_id, last_name, department_id의 데이터를 출력하되, last_name은 데이터가 king인 것만을 출력

SELECT employee_id, last_name, department_id 입력
FROM HR_EMPLOYEES 입력
WHERE last_name = 'King'; 입력


+-------------+-----------+---------------+
| employee_id | last_name | department_id |
+-------------+-----------+---------------+
|         100 | King      |            90 |
|         156 | King      |            80 |
+-------------+-----------+---------------+
```

## 비교연산자
```
=   :  같은 값
>   :  보다 크다, 초과
<   :  보다 작다, 미만
>=  :  보다 크거나 같다, 이상
<=  :  보다 작거나 같다, 이하
!=  :  같지 않다
<>  :  같지 않다
^=  :  같지 않다
```

```
- HR_EMPLOYEES 테이블의 employee_id, last_name, department_id의 데이터를 출력하되, employee_id는 데이터가 110 이하인 것만을 출력

SELECT employee_id, last_name, department_id 입력
FROM HR_EMPLOYEES 입력
WHERE employee_id <= 110; 입력


+-------------+-----------+---------------+
| employee_id | last_name | department_id |
+-------------+-----------+---------------+
|         100 | King      |            90 |
|         101 | Kochhar   |            90 |
|         102 | De Haan   |            90 |
|         103 | Hunold    |            60 |
|         104 | Ernst     |            60 |
|         105 | Austin    |            60 |
|         106 | Pataballa |            60 |
|         107 | Lorentz   |            60 |
|         108 | Greenberg |           100 |
|         109 | Faviet    |           100 |
|         110 | Chen      |           100 |
+-------------+-----------+---------------+



-------------------------------------------------------------------------------------------------------------------------

- HR_EMPLOYEES테이블에서 salary의 값이 17000달러 이상인 사원을 조회해서 employee_id, last_name, salary, department_id의 컬럼을 출력

SELECT employee_id, last_name, salary, department_id 입력
FROM HR_EMPLOYEES 입력
WHERE salary >= 17000; 입력


+-------------+-----------+--------+---------------+
| employee_id | last_name | salary | department_id |
+-------------+-----------+--------+---------------+
|         100 | King      |  24000 |            90 |
|         101 | Kochhar   |  17000 |            90 |
|         102 | De Haan   |  17000 |            90 |
+-------------+-----------+--------+---------------+

```

# [오후수업] JAVA 12차



