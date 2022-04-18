# [오전수업] DB 21차

---

# [오후수업] DB 22차
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
- 





