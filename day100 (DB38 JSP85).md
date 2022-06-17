# [오전수업] DB 38차
## 데이터제어어 - DCL(Data Control Language)
- 데이터베이스 사용자의 권한을 부여하거나 회수하는 문법

### 사용자 생성 문법(create user)
```sql
[oracle@itwillbs ~]$ sqlplus /nolog


SQL*Plus: Release 12.2.0.1.0 Production on Fri Jun 17 09:49:49 2022

Copyright (c) 1982, 2016, Oracle.  All rights reserved.

- DBA권한을 가지고 있는 sys계정으로 접속
SQL> conn sys/oracle as sysdba

Connected.

SQL> CREATE USER demo     // 계정명
  2  IDENTIFIED BY demo;  // 패스워드


User created.

- 권한 부족으로 demo 계정은 접속할 수 없음
SQL> conn demo/demo

ERROR:
ORA-01045: user DEMO lacks CREATE SESSION privilege; logon denied

- 사용자의 작업 내용은 접속 계정에 저장되는게 아니라 사용자별로 생성되는 세션에 저장된다.
- 세션을 만들 수 없으면 데이터베이스에 접근할 수 없다.
```
![캡처](https://user-images.githubusercontent.com/95197594/174204079-404310bb-d676-4297-ab11-743946688f25.PNG)

### SYSTEM권한
- 데이터베이스의 설정값이나 구조를 변경할 수 있는 권한
- DBA나 권한을 부여받은 계정이 다룰 수 있음

* system권한 부여
```sql
SQL> conn sys/oracle as sysdba

Connected.
SQL> GRANT create session, create table, create sequence, create view
  2  TO demo;


Grant succeeded.

- GRANT 절	: 부여 할 권한, ROLE(권한의 묶음)의 목록을 작성하는 절
- TO 절		: 권한을 부여받을 계정의 목록

demo 계정은 create session과 create table권한을 얻었으므로 접속과 테이블 생성작업을 할 수 있다.
SQL> conn demo/demo

Connected.

SQL> CREATE TABLE test
  2  (id NUMBER);


Table created.
```

* system권한 회수
```sql
최소 권한의 원칙으로 필요없어진 권한은 바로바로 회수하는 것이 보안상 안전하다.
권한의 회수는 권한을 부여할 수 있는 권한을 가진 계정이 수행할 수 있다.
SQL> conn sys/oracle as sysdba

Connected.
SQL> REVOKE create table
  2  FROM demo;


Revoke succeeded.
REVOKE 절	: 회수할 권한, role의 목록을 작성
FROM 절	: 권한이 회수 될 계정 작성

demo 계정은 create table 권한이 회수되어 더 이상 테이블을 생성할 수 없다.
SQL> conn demo/demo

Connected.
SQL> CREATE TABLE test2
  2  (id NUMBER);

CREATE TABLE test2
*
ERROR at line 1:
ORA-01031: insufficient privileges
```

### OBJECT 권한
- system권한을 통해서 만들어진 object들은 object 권한을 통해서 관리
- object별로 기능이나 사용하는 형태가 다르므로 권한 또한 다르게 적용

![캡처](https://user-images.githubusercontent.com/95197594/174216773-60d71b13-0c97-46f0-8477-fd532a655da6.PNG)

- 사용자가 system권한으로 만든 오브젝트들은 계정명과 동명의 schema에 오브젝트들이 위치하며 해당 오브젝트에 대한 모든 사용권한을 관리할 수 있음
```sql

SQL> conn demo/demo

Connected.
접속 중인 계정 소유의 오브젝트가 아닌 다른 사용자 소유의 오브젝트에 접근하는 경우 오브젝트의 이름 앞에 스키마명을 붙여야한다.
SQL> SELECT employee_id, last_name, salary
  2  FROM hr.employees;


FROM hr.employees
        *
ERROR at line 2:
ORA-00942: table or view does not exist
```

* object권한의 부여
- 오브젝트 권한의 관리는 오브젝트를 생성한 소유자가 관리

```sql

SQL> conn hr/hr

Connected.
SQL> GRANT select
  2  ON employees
  3  TO demo;


Grant succeeded.

GRANT 절	: 부여할 오브젝트 권한 목록
ON 절		: 권한이 적용 될 오브젝트
TO 절		: 권한을 부여받을 계정

SQL> conn demo/demo

Connected.
SQL> SELECT employee_id, last_name, salary
  2  FROM hr.employees;


EMPLOYEE_ID LAST_NAME                     SALARY
----------- ------------------------- ----------
        100 King                           24000
        101 Kochhar                        17000
        102 De Haan                        17000
        103 Hunold                          9000
        104 Ernst                           6000
        105 Austin                          4800
        106 Pataballa                       4800
        107 Lorentz                         4200
        108 Greenberg                      12008
        109 Faviet                          9000
        110 Chen                            8200
…
```

* object 권한 회수
```sql
오브젝트 권한의 회수는 오브젝트의 소유자 또는 DBA가 할 수 있다.
SQL> conn hr/hr

Connected.
SQL> REVOKE select
  2  ON employees
  3  FROM demo;


Revoke succeeded.
REVOKE 절	: 회수할 권한의 목록
ON 절		: 권한이 적용 될 오브젝트
FROM 절	: 권한을 회수할 계정

demo 계정은 hr 계정에 의해 hr.employees 테이블에 대한 select권한을 회수당했으므로 다시 테이블을 조회할 수 없는 상태가 되었음.
SQL> conn demo/demo

Connected.
SQL> SELECT employee_id, last_name, salary
  2  FROM hr.employees;

FROM hr.employees
        *
ERROR at line 2:
ORA-00942: table or view does not exist
```




---

# [오후수업] JSP 85차
