# [오전수업] DB 25차
> 24차 수업 복습 실시

![캡처](https://user-images.githubusercontent.com/95197594/165881455-b2d2e709-3205-47ec-9fe0-7394a30e2792.PNG)

```

SQL> INSERT INTO departments
  2  VALUES (&dept_id, '&dept_name', &mgr_id, &loc_id);

Enter value for dept_id: 310
Enter value for dept_name: Lunch
Enter value for mgr_id: 100
Enter value for loc_id: 1700
old   2: VALUES (&dept_id, '&dept_name', &mgr_id, &loc_id)
new   2: VALUES (310, 'Lunch', 100, 1700)

1 row created.
```




> 연습문제를 통하여 치환변수, 스크립트파일 생성 및 실행, INSERT & UPDATE & DELETE 구문, commit, SAVEPOINT 및 ROLLBACK 연습 실시
```
SQL> @/home/oracle/labs_12c/sql1/lab_08_01.sql

Table created.

SQL> desc my_employee
 Name                                      Null?    Type
 ----------------------------------------- -------- ----------------------------
 ID                                        NOT NULL NUMBER(4)
 LAST_NAME                                          VARCHAR2(25)
 FIRST_NAME                                         VARCHAR2(25)
 USERID                                             VARCHAR2(8)
 SALARY                                             NUMBER(9,2)

SQL> INSERT INTO my_employee
  2  VALUES (1, 'Patel', 'Ralph', 'rpatel', 895);

1 row created.

SQL> INSERT INTO my_employee
  2  VALUES (2, 'Dancs', 'Betty', 'bdancs', 860);

1 row created.

SQL> SELECT * FROM my_employee;

        ID LAST_NAME                 FIRST_NAME                USERID       SALARY
---------- ------------------------- ------------------------- -------- ----------
         1 Patel                     Ralph                     rpatel          895
         2 Dancs                     Betty                     bdancs          860

SQL> INSERT INTO my_employee
  2  VALUES (&id, '&last_name', '&first_name', '&userid', &salary);
Enter value for id: 3
Enter value for last_name: Biri
Enter value for first_name: Ben
Enter value for userid: bbiri
Enter value for salary: 1100
old   2: VALUES (&id, '&last_name', '&first_name', '&userid', &salary)
new   2: VALUES (3, 'Biri', 'Ben', 'bbiri', 1100)

1 row created.


SQL> save /home/oracle/load_emp.sql
Created file /home/oracle/load_emp.sql

SQL> @/home/oracle/load_emp.sql
Enter value for id: 4
Enter value for last_name: Newman
Enter value for first_name: Chad
Enter value for userid: cnewman
Enter value for salary: 750
old   2: VALUES (&id, '&last_name', '&first_name', '&userid', &salary)
new   2: VALUES (4, 'Newman', 'Chad', 'cnewman', 750)

1 row created.

SQL> SELECT * FROM my_employee;

        ID LAST_NAME                 FIRST_NAME                USERID       SALARY
---------- ------------------------- ------------------------- -------- ----------
         1 Patel                     Ralph                     rpatel          895
         2 Dancs                     Betty                     bdancs          860
         3 Biri                      Ben                       bbiri          1100
         4 Newman                    Chad                      cnewman         750

SQL> COMMIT;

Commit complete.


SQL> UPDATE my_employee
  2  SET last_name = 'Drexler'
  3  WHERE id = 3;

1 row updated.

SQL> UPDATE my_employee
  2  SET salary = 1000
  3  WHERE salary < 900;

3 rows updated.

SQL> SELECT * FROM my_employee;

        ID LAST_NAME                 FIRST_NAME                USERID       SALARY
---------- ------------------------- ------------------------- -------- ----------
         1 Patel                     Ralph                     rpatel         1000
         2 Dancs                     Betty                     bdancs         1000
         3 Drexler                   Ben                       bbiri          1100
         4 Newman                    Chad                      cnewman        1000


SQL> DELETE FROM my_employee
  2  WHERE id = 2;

1 row deleted.

SQL> SELECT * FROM my_employee;

        ID LAST_NAME                 FIRST_NAME                USERID       SALARY
---------- ------------------------- ------------------------- -------- ----------
         1 Patel                     Ralph                     rpatel         1000
         3 Drexler                   Ben                       bbiri          1100
         4 Newman                    Chad                      cnewman        1000

SQL> commit;

Commit complete.

SQL> @/home/oracle/load_emp.sql
Enter value for id: 5
Enter value for last_name: Ropeburn
Enter value for first_name: Audrey
Enter value for userid: aropebur
Enter value for salary: 1550
old   2: VALUES (&id, '&last_name', '&first_name', '&userid', &salary)
new   2: VALUES (5, 'Ropeburn', 'Audrey', 'aropebur', 1550)

1 row created.

SQL> SELECT * FROM my_employee;

        ID LAST_NAME                 FIRST_NAME                USERID       SALARY
---------- ------------------------- ------------------------- -------- ----------
         1 Patel                     Ralph                     rpatel         1000
         3 Drexler                   Ben                       bbiri          1100
         4 Newman                    Chad                      cnewman        1000
         5 Ropeburn                  Audrey                    aropebur       1550

SQL> SAVEPOINT a;

Savepoint created.

SQL> DELETE FROM my_employee;

4 rows deleted.

SQL> SELECT * FROM my_employee;

no rows selected

SQL> ROLLBACK TO SAVEPOINT a;

Rollback complete.

SQL> SELECT * FROM my_employee;

        ID LAST_NAME                 FIRST_NAME                USERID       SALARY
---------- ------------------------- ------------------------- -------- ----------
         1 Patel                     Ralph                     rpatel         1000
         3 Drexler                   Ben                       bbiri          1100
         4 Newman                    Chad                      cnewman        1000
         5 Ropeburn                  Audrey                    aropebur       1550
```

---

# [오후수업] JSP 57차
