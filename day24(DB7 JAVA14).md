# [오전수업] DB 7차

> DB 6차 복습 수업 실시


## 조건이 여러가지인 WHERE 
- 비교연산자인 '='은 한번에 하나의 조건값만 입력받을 수 있음

```
- department_id컬럼의 값이 90 또는 100의 값을 가지고 있는 행을 출력

SELECT employee_id, last_name, department_id 
FROM HR_EMPLOYEES 
WHERE department_id = 90;

+-------------+-----------+---------------+
| employee_id | last_name | department_id |
+-------------+-----------+---------------+
|         100 | King      |            90 |
|         101 | Kochhar   |            90 |
|         102 | De Haan   |            90 |
+-------------+-----------+---------------+

-------------------------------------------------------------------------------

SELECT employee_id, last_name, department_id 
FROM HR_EMPLOYEES 
WHERE department_id = 100;
```

## IN 연산자
- IN 연산자는 '='연산자와 비슷하게 조건값과 동일한 값을 가진 행을 찾는 연산자
  - 다만 입력받을 수 있는 조건값은 여러개를 동시에 받을 수 있음
```
SELECT employee_id, last_name, department_id 
FROM HR_EMPLOYEES 
WHERE department_id IN (90, 100);

+-------------+-----------+---------------+
| employee_id | last_name | department_id |
+-------------+-----------+---------------+
|         100 | King      |            90 |
|         101 | Kochhar   |            90 |
|         102 | De Haan   |            90 |
|         108 | Greenberg |           100 |
|         109 | Faviet    |           100 |
|         110 | Chen      |           100 |
|         111 | Sciarra   |           100 |
|         112 | Urman     |           100 |
|         113 | Popp      |           100 |
+-------------+-----------+---------------+
```

## AND / OR
```
- AND의 연산자를 양옆의 조건식을 동시에 만족하는 행을 결과로 출력

SELECT employee_id, last_name, salary, department_id
FROM HR_EMPLOYEES
WHERE department_id IN (90, 100) AND salary >= 9000;

+-------------+-----------+--------+---------------+
| employee_id | last_name | salary | department_id |
+-------------+-----------+--------+---------------+
|         100 | King      |  48000 |            90 |
|         101 | Kochhar   |  17000 |            90 |
|         102 | De Haan   |  17000 |            90 |
|         108 | Greenberg |  12000 |           100 |
|         109 | Faviet    |   9000 |           100 |
+-------------+-----------+--------+---------------+


-------------------------------------------------------------------------------


SELECT employee_id, last_name, salary, department_id
FROM HR_EMPLOYEES
WHERE department_id = 90 OR department_id = 100;

+-------------+-----------+--------+---------------+
| employee_id | last_name | salary | department_id |
+-------------+-----------+--------+---------------+
|         100 | King      |  48000 |            90 |
|         101 | Kochhar   |  17000 |            90 |
|         102 | De Haan   |  17000 |            90 |
|         108 | Greenberg |  12000 |           100 |
|         109 | Faviet    |   9000 |           100 |
|         110 | Chen      |   8200 |           100 |
|         111 | Sciarra   |   7700 |           100 |
|         112 | Urman     |   7800 |           100 |
|         113 | Popp      |   6900 |           100 |
+-------------+-----------+--------+---------------+


-------------------------------------------------------------------------------

SELECT employee_id, last_name, salary, department_id 
FROM HR_EMPLOYEES 
WHERE department_id = 90 OR salary >= 10000;

+-------------+-----------+--------+---------------+
| employee_id | last_name | salary | department_id |
+-------------+-----------+--------+---------------+
|         100 | King      |  48000 |            90 |
|         101 | Kochhar   |  17000 |            90 |
|         102 | De Haan   |  17000 |            90 |
|         108 | Greenberg |  12000 |           100 |
|         114 | Raphaely  |  11000 |            30 |
|         145 | Russell   |  14000 |            80 |
|         146 | Partners  |  13500 |            80 |
|         147 | Errazuriz |  12000 |            80 |
|         148 | Cambrault |  11000 |            80 |
|         149 | Zlotkey   |  10500 |            80 |
|         150 | Tucker    |  10000 |            80 |
|         156 | King      |  10000 |            80 |
|         162 | Vishney   |  10500 |            80 |
|         168 | Ozer      |  11500 |            80 |
|         169 | Bloom     |  10000 |            80 |
|         174 | Abel      |  11000 |            80 |
|         201 | Hartstein |  13000 |            20 |
|         204 | Baer      |  10000 |            70 |
|         205 | Higgins   |  12000 |           110 |
+-------------+-----------+--------+---------------+


-------------------------------------------------------------------------------


HR_EMPLOYEES테이블에서 급여가 10000이 넘으면서 소속부서가 80 또는 50인 사원을 출력

SELECT employee_id, last_name, salary, department_id 
FROM HR_EMPLOYEES 
WHERE department_id = 80 OR department_id = 50 AND salary >= 10000;

+-------------+------------+--------+---------------+
| employee_id | last_name  | salary | department_id |
+-------------+------------+--------+---------------+
|         145 | Russell    |  14000 |            80 |
|         146 | Partners   |  13500 |            80 |
|         147 | Errazuriz  |  12000 |            80 |
|         148 | Cambrault  |  11000 |            80 |
|         149 | Zlotkey    |  10500 |            80 |
|         150 | Tucker     |  10000 |            80 |
...
|         172 | Bates      |   7300 |            80 |
|         173 | Kumar      |   6100 |            80 |
|         174 | Abel       |  11000 |            80 |
|         175 | Hutton     |   8800 |            80 |
|         176 | Taylor     |   8600 |            80 |
|         177 | Livingston |   8400 |            80 |
|         179 | Johnson    |   6200 |            80 |
+-------------+------------+--------+---------------+



-------------------------------------------------------------------------------


SELECT employee_id, last_name, salary, department_id 
FROM HR_EMPLOYEES 
WHERE (department_id = 80 OR department_id = 50) AND salary >= 8000;

+-------------+------------+--------+---------------+
| employee_id | last_name  | salary | department_id |
+-------------+------------+--------+---------------+
|         120 | Weiss      |   8000 |            50 |
|         121 | Fripp      |   8200 |            50 |
|         145 | Russell    |  14000 |            80 |
|         146 | Partners   |  13500 |            80 |
...
|         174 | Abel       |  11000 |            80 |
|         175 | Hutton     |   8800 |            80 |
|         176 | Taylor     |   8600 |            80 |
|         177 | Livingston |   8400 |            80 |
+-------------+------------+--------+---------------+


-------------------------------------------------------------------------------


SELECT employee_id, last_name, salary, department_id 
FROM HR_EMPLOYEES 
WHERE salary >= 5000 AND salary <= 8000;

+-------------+-----------+--------+---------------+
| employee_id | last_name | salary | department_id |
+-------------+-----------+--------+---------------+
|         104 | Ernst     |   6000 |            60 |
|         111 | Sciarra   |   7700 |           100 |
|         112 | Urman     |   7800 |           100 |
...
|         179 | Johnson   |   6200 |            80 |
|         202 | Fay       |   6000 |            20 |
|         203 | Mavris    |   6500 |            40 |
+-------------+-----------+--------+---------------+
```

> 이후 다음주에 있을 테스트를 대비해 배운 내용들을 전체적으로 요약 복습 실시 
