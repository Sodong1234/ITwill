# [오전수업] DB 39차

## with admin option
- system 권한에서 사용되는 옵션 절로 해당 옵션절을 사용한 grant 구문을 실행하는 경우 단순한 실행권한 뿐 아니라 해당 권한을 부여, 회수 할 수 있는 관리 권한을 얻게 됨
- 권한을 부여한 계정이 권한을 회수 당하더라도 시스템 권한은 종속적으로 회수되지 않음

![image](https://user-images.githubusercontent.com/95197594/174510812-b6f36b9c-ac33-432a-9a6c-8508975c82ea.png)

* 실습 사용 계정 생성
```sql
SQL> conn sys/oracle as sysdba

Connected.
SQL> CREATE USER turner
  2  IDENTIFIED BY lover;


User created.

SQL> CREATE USER ford
  2  IDENTIFIED BY henry;


User created.

SQL> GRANT create session, unlimited tablespace
  2  TO turner, ford;


Grant succeeded.
```

* WITE ADMIN OPTION 실습
```sql
- SYS → turner으로 create table 관리 권한 부여
SQL> GRANT create table
  2  TO turner
  3  WITH ADMIN OPTION;


Grant succeeded.

SQL> conn turner/lover

Connected.
- 테이블 생성 권한 확인
SQL> CREATE TABLE turner_table (id NUMBER);


Table created.

- turner → ford create table 권한 부여
SQL> GRANT create table
  2  TO ford;


Grant succeeded.

SQL> conn ford/henry

Connected.
- ford 계정에서도 테이블 생성 확인
SQL> CREATE TABLE ford_table (id NUMBER);


Table created.

- sys ← turner 권한 회수
SQL> conn sys/oracle as sysdba

Connected.
SQL> REVOKE create table
  2  FROM turner;


Revoke succeeded.

- 기존 turner에게 create table 권한을 부여받은 ford로 create table 권한 유지 확인
SQL> conn ford/henry

Connected.
SQL> CREATE TABLE ford_table2 ( id NUMBER);


Table created.
```

## WITH GRANT OPTION
- 오브젝트 권한에 대한 관리 권한을 부여하는 옵션절
- 관리 권한을 부여하더라도 소유권이 바뀌는 것은 아니므로 스키마명이 필요한 경우 오브젝트앞에 작성

![image](https://user-images.githubusercontent.com/95197594/174510947-677ebb4e-a4d9-413a-ab7a-3d8ce1058ce6.png)

## WITH GRANT OPTION 실습
```sql
- hr → turner 에게 employees 테이블에 대한 select권한을 관리 권한과 함께 부여
SQL> conn hr/hr

Connected.
SQL> GRANT select
  2  ON employees
  3  TO turner
  4  WITH GRANT OPTION;


Grant succeeded.

- turner로 hr.employees 테이블에 대한 select 권한 확인
SQL> conn turner/lover

Connected.
SQL> SELECT last_name, job_id
  2  FROM hr.employees
  3  WHERE employee_id = 103;


LAST_NAME                 JOB_ID
------------------------- ----------
Hunold                    IT_PROG

- turner → ford 로 hr.employees 테이블의 select 권한을 부여
SQL> GRANT select
  2  ON hr.employees
  3  TO ford;


Grant succeeded.

- ford도 hr.employees에 대한 select 권한을 확인
SQL> conn ford/henry

Connected.
SQL> SELECT last_name, job_id
  2  FROM hr.employees
  3  WHERE employee_id = 103;


LAST_NAME                 JOB_ID
------------------------- ----------
Hunold                    IT_PROG

- hr ← turner로 권한 회수
SQL> conn hr/hr

Connected.
SQL> REVOKE select
  2  ON employees
  3  FROM turner;


Revoke succeeded.

- ford는 hr에게 직접 hr.employees에 대한 select권한을 회수당했지는 않지만 turner계정이 권한을 회수당하며 종속적으로 권한을 회수당하였음.
SQL> conn ford/henry

Connected.
SQL> SELECT last_name, job_id
  2  FROM hr.employees
  3  WHERE employee_id = 103;

FROM hr.employees
        *
ERROR at line 2:
ORA-00942: table or view does not exist
```



---

# [오후수업] JAVA 59차
