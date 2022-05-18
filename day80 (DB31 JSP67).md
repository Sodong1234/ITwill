# [오전수업] DB 31차
## 제약조건 일시적으로 해제하기
- 제약조건의 상태가 enabled인 경우 제약조건이 정상적으로 동작하는 상태
```sql

SELECT constraint_name,
       constraint_type,
       status
FROM user_constraints
WHERE table_name = 'EMP2';


CONSTRAINT_NAME|CONSTRAINT_TYPE|STATUS |
---------------+---------------+-------+
SYS_C007714    |C              |ENABLED|
SYS_C007715    |C              |ENABLED|
SYS_C007716    |C              |ENABLED|
SYS_C007717    |C              |ENABLED|
EMP2_EID_PK    |P              |ENABLED|
```

- 제약조건을 일시적 해제가 필요한 경우 DISABLE 시키는 것도 가능하다.
```sql
ALTER TABLE emp2
DISABLE CONSTRAINT EMP2_EID_PK;


SELECT constraint_name,
       constraint_type,
       status
FROM user_constraints
WHERE table_name = 'EMP2';


CONSTRAINT_NAME|CONSTRAINT_TYPE|STATUS  |
---------------+---------------+--------+
SYS_C007714    |C              |ENABLED |
SYS_C007715    |C              |ENABLED |
SYS_C007716    |C              |ENABLED |
SYS_C007717    |C              |ENABLED |
EMP2_EID_PK    |P              |DISABLED|
```

- 206번 사원의 사번을 100으로 갱신
```sql

UPDATE emp2
SET employee_id = 100
WHERE employee_id = 206;


SELECT employee_id, last_name
FROM emp2
WHERE employee_id = 100;

EMPLOYEE_ID|LAST_NAME|
-----------+---------+
        100|King     |
        100|Gietz    |
```

- 기존 employee_id 컬럼은 PK제약조건이 적용되어 NULL값과 중복값을 허용하지 않으나 일시적으로 제약조건을 끄는 경우 설정 된 제약조건의 영향을 받지 않게 된다.
```sql
ALTER TABLE emp2
ENABLE CONSTRAINT EMP2_EID_PK;

```

- 제약조건 재활성 시 입력 값이 제약조건을 만족하지 않는 경우 아래와 같이 제약조건을 활성할 수 없다.
* SQL Error [2437] [72000]: ORA-02437: (HR.EMP2_EID_PK)을 검증할 수 없습니다 - 잘못된 기본 키입니다

- 다시 제약조건에 맞는 값으로 수정한 뒤에는 제약조건을 재활성에 문제없이 동작함.
```sql

UPDATE emp2
SET employee_id = 206
WHERE last_name = 'Gietz';


SELECT constraint_name,
       constraint_type,
       status
FROM user_constraints
WHERE table_name = 'EMP2';

CONSTRAINT_NAME|CONSTRAINT_TYPE|STATUS |
---------------+---------------+-------+
SYS_C007714    |C              |ENABLED|
SYS_C007715    |C              |ENABLED|
SYS_C007716    |C              |ENABLED|
SYS_C007717    |C              |ENABLED|
EMP2_EID_PK    |P              |ENABLED|
```

## DROP TABLE
- 기존 테이블을 삭제하는 문법
- 삭제하려는 테이블이 부모테이블인 경우 제약조건 삭제전에는 테이블을 삭제할 수 없다.
```sql
DROP TABLE emp2;
```

- 외래키 제약조건으로 엮어져 있는 자식테이블의 데이터를 포함하여 삭제하는 경우 CASCADE CONSTRAINTS 옵션을 사용하면 된다.
```sql
DROP TABLE 테이블명 [CASCADE CONSTRAINTS];
```

## FLASHBACK TABLE 기술
- ORACLEDB에서는 테이블을 삭제하는 경우 해당 테이블을 삭제할 수 없게된다.
- 다만, 테이블을 실수로 삭제했을 경우에 대비하여 바로 물리적으로 삭제하지 않고, 사용 할 수 없게 가려두는 방식으로 테이블을 일시적으로 보관해둔다.

```sql
DROP TABLE emp2;


SELECT original_name, operation, droptime
FROM recyclebin;

ORIGINAL_NAME|OPERATION|DROPTIME           |
-------------+---------+-------------------+
EMP2_EID_PK  |DROP     |2022-05-18:10:20:30|
EMP2         |DROP     |2022-05-18:10:20:30|
```

- recyclebin에서 조회가 되고 있는 경우 drop을 취소하고 다시 테이블을 원상복구 시킬 수 있다.
```sql

FLASHBACK TABLE emp2 TO BEFORE DROP;


SELECT * FROM tab;

TNAME           |TABTYPE|CLUSTERID|
----------------+-------+---------+
COUNTRIES       |TABLE  |         |
DEPARTMENTS     |TABLE  |         |
DEPT80          |TABLE  |         |
EMP2            |TABLE  |         |
EMPLOYEES       |TABLE  |         |
EMP_DETAILS_VIEW|VIEW   |         |
JOBS            |TABLE  |         |
JOB_HISTORY     |TABLE  |         |
LOCATIONS       |TABLE  |         |
REGIONS         |TABLE  |         |
```

## PURGE옵션
- 테이블삭제 시 PURGE옵션을 추가하여 삭제하는 경우 recyclebin을 거치지 않고 바로 물리적으로 테이블을 삭제하게 된다.
```sql

DROP TABLE emp2 PURGE;


SELECT original_name, operation, droptime
FROM recyclebin;

ORIGINAL_NAME|OPERATION|DROPTIME|
-------------+---------+--------+
```

## TRUNCATE 문법
- 테이블의 데이터 저장공간을 잘라서 자원을 데이터베이스 돌려주는 문법


![unnamed](https://user-images.githubusercontent.com/95197594/168948238-eee6c736-e66a-4a07-8a91-aad83a6cf017.png)

- DELETE 문법은 저장공간은 유지하기 때문에 다시 데이터 입력이 가능하다
- TRUNCATE문법은 저장공간을 잘라서 데이터베이스 반환하는 문법으로 재사용 시 공간의 재할당이 필요.

```sql
TRUNCATE TABLE dept80;


SELECT * FROM dept80;

EMPLOYEE_ID|LAST_NAME|ANNSAL|HIRE_DATE|
-----------+---------+------+---------+
```




---

# [오후수업] JSP 67차
