# [오전수업] DB 10차
> 8차에서 실시한 시험의 해설 및 9차 수업 복습 실시
> 9차 수업 이어서 실시

### 연결연산자(||)
- 연결연산자로 연결된 요소들을 하나로 연결한 결과를 연산하여 출력하는 문법
- oracle에서는 ||이 연결연산자로 동작하지만, mysql에서는 OR의 연산 형태로 동작

```

mysql에서는 CONCAT 함수로 이 기능을 활용
mysql> SELECT last_name, job_id, CONCAT(last_name, job_id) AS "Employees" FROM HR_EMPLOYEES;

+-------------+------------+---------------------+
| last_name   | job_id     | Employees           |
+-------------+------------+---------------------+
| King        | AD_PRES    | KingAD_PRES         |
| Kochhar     | AD_VP      | KochharAD_VP        |
| De Haan     | AD_VP      | De HaanAD_VP        |
| Hunold      | IT_PROG    | HunoldIT_PROG       |
… 

컬럼의 내용을 조합하여 출력 시 고정적으로 출력해야할 문자열 요소가 있는 경우 리터럴 문자를 활용
mysql> SELECT last_name, job_id, CONCAT(last_name, ' ', job_id) AS "Employees" FROM HR_EMPLOYEES;

+-------------+------------+----------------------+
| last_name   | job_id     | Employees            |
+-------------+------------+----------------------+
| King        | AD_PRES    | King AD_PRES         |
| Kochhar     | AD_VP      | Kochhar AD_VP        |
| De Haan     | AD_VP      | De Haan AD_VP        |
| Hunold      | IT_PROG    | Hunold IT_PROG       |
…

```

### DISTINCT
- 행의 데이터의 중복값을 제거하여 출력
- DISTINCT 키워드의 위치는 SELECT절의 첫번째 자리
- 여러 컬럼이 SELECT절에 오는 경우 개별 컬럼 별로 중복제거가 아닌 컬럼의 값들을 하나의 세트로 보고 모든 컬럼의 값이 일치한 경우 중복값으로 제거하여 출력

```


mysql> SELECT department_id
    -> FROM HR_EMPLOYEES;

+---------------+
| department_id |
+---------------+
|          NULL |
|            10 |
|            20 |
|            20 |
|            30 |
|            30 |
… 





mysql> SELECT DISTINCT department_id FROM HR_EMPLOYEES;

+---------------+
| department_id |
+---------------+
|          NULL |
|            10 |
|            20 |
|            30 |
|            40 |
|            50 |
|            60 |
|            70 |
|            80 |
|            90 |
|           100 |
|           110 |
+---------------+






아래의 두개의 컬럼에 대한 중복값 제거는 department_id, job_id의 값이 모두 일치한 경우에만 중복값으로 제거   
mysql> SELECT DISTINCT department_id, job_id FROM HR_EMPLOYEES;

+---------------+------------+
| department_id | job_id     |
+---------------+------------+
|            90 | AD_PRES    |
|            90 | AD_VP      |
|            60 | IT_PROG    |
|           100 | FI_MGR     |
|           100 | FI_ACCOUNT |
|            30 | PU_MAN     |
|            30 | PU_CLERK   |
|            50 | ST_MAN     |
|            50 | ST_CLERK   |
|            80 | SA_MAN     |
|            80 | SA_REP     |
|          NULL | SA_REP     |
|            50 | SH_CLERK   |
|            10 | AD_ASST    |
|            20 | MK_MAN     |
|            20 | MK_REP     |
|            40 | HR_REP     |
|            70 | PR_REP     |
|           110 | AC_MGR     |
|           110 | AC_ACCOUNT |
+---------------+------------+

```

# [오후수업] JSP 27차

## 모듈화
- 어떤 기능을 담당하는 작은 단위를 모듈(Module)이라고 함
- 하나의 프로그램에서 특정 기능에 따라 작은 단위의 클래스 또는 메서드 형태로 잘게 분리하는 것을 모듈화라고 함

> StudyJSP Project의 src/main/java 폴더에 
