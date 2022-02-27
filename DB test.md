 > 28일에 있을 DB 테스트를 대비한 요약본. 테스트 종료 후 삭제 예정 
 
# 데이터베이스란?
- 대량의 데이터를 보관하는 소프트웨어 → 프로그램 / 웹 app에서 사용
- 데이터의 백업, 관리, 연산, 보안의 기능
- 저장하는 데이터의 종류(정형, 비정형), 규모에 따라 사용 데이터베이스를 결정
  - 정형 (글자, 문자, 숫자 등 눈에 보이는 것) 
  - 비정형 (영상, 이미지, 음향, 음성정보 등 완벽히 정형화 되어 있지 않은 것)
- 정형(Text)의 데이터를 다루는 DB(관계형 데이터베이스(RDBMS)) → SQL 언어를 통해서 사용
  - 주로 사용하는 RDBMS의 종류 : Oracle(빠름, 대규모) / MySQL(중규모)

- 주로 사용하는 서버용 운영체제(OS) - Wiodnws(비쌈) / Linus(오픈소스, 무료/상용)
  - Linus 오픈소스는 GPL이라는 라이센스가 존재, 개발자가 오픈된 소스로 무엇인가를 개발하였을 경우 그것을 오픈시켜 공개해야함. 

---

## DB 접속하기
### TUI(CUI)
- 터미널 환경에서 데이터베이스에 접속하는 방법
- 명령어로만 작업을 수행한다.
- cmd -> mysql -u root -p 입력 -> 비밀번호 입력

### 로그인
- 로그인명 root / 비밀번호 1234(입력해도 아무것도 안 보이는게 정상)
- 로그인에 성공하면 ip addr show 입력 -> 본인의 아이피 확인
- (윈도우와 원격 연결) 윈도우즈로 돌아와서 cmd 실행 -> ssh root@아이피 입력 -> 메세지 뜨면 yes -> 비밀번호 1234 입력 -> 로그인 완료 

## 가상머신 터미널 원격접속하기
- 가상머신 실행 후 root 입력 -> 비밀번호 1234 입력-> 로그인 성공했다면 ip add show 입력 -> 본인의 장치명과 아이피 확인 -> ssh root@아이피주소 입력 -> 비밀번호 입력
![캡처](https://user-images.githubusercontent.com/95197594/152708890-ee36e657-6bf3-479f-bf06-999056c93b54.PNG)

## SQL 작성하기
- 관계형 데이터베이스(RDBMS)의 환경에서 데이터베이스를 다루는 문법
- 대부분의 RDBMS간에는 문법이 호환이 되므로 다른 데이터베이스를 사용하게 되더라도 적응하기 쉬움
- 일반적인 문법에서는 줄바꿈이나 대소문자에 대한 제한이 없는 편이지만 특정 문법 작성시에는 주의
- 문장의 끝을 알려주는 탈출문자로 세미콜론(;) 기호를 사용하며, 줄바꿈으로는 입력 중인 구문이 끝나지 않음

### 키워드
- SQL 문법에서는 기능이 부여된 단어들
- 키워드는 단독으로 사용하는 경우도 있으나, 보통은 키워드에 맞는 요소를 더하여 절의 형태로 만들어 작성
- SQL 구문은 원하는 연산을 할 수 있도록 적절한 절들을 조합하여 작성

> - 키워드 : 기능 할단 된 단어
> - 키워드 + 요소 : 절
> - 절 + 절 : 구문


## DDL (Data Definition Language) / 데이터 정의어
- 데이터 저장 구조를 다루는 문법
	- CREATE : 오브젝트 생성
	- ALTER : 오브젝트 변형
	- DROP : 오브젝트 삭제

## MySQL (CLI 환경에서 접속)
- mysql -u root -p 입력 -> 비밀번호 입력

### 데이터베이스 목록 조회 & 생성 & 선택 & 삭제 명령어
- show databases; : 목록 조회
- CREATE DATABASE test; : test 데이터베이스 생성
- use test; : test 데이터베이스 선택
- DROP DATABASE test; : test 데이터베이스 삭제

## 테이블 생성 및 수정
```
-문법

CREATE TABLE 테이블명 (
컬럼명1      데이터타입1(크기) [DEFAULT] [제약조건] [,
컬럼명2 …]
);
```
- 데이터타입
  - 숫자 : INT(정수) / float(실수) 
  - 문자 : CHAR(고정크기) / VARCHAR(가변크기)
    - 고정크기 : 10바이트를 설정해주었으면 한 글자만 입력하고 끝내도 10바이트로 인식됨
    - 가변크기 : 10바이트를 설정해주었으면 한 글자당 1바이트로 인식됨. VARCHAR이 CHAR보다 좀 더 효율적 
  - 날짜 : DATE, TIMESTAMP 
    - 크기 지정 하지 않음. 

### 생성
```
CREATE TABLE test_table ( 입력
idx INT(10), 입력
name  VARCHAR(10) 입력
); 입력
```
### 테이블 목록 및 구조 조회
- show tables; 입력 -> 생성한 테이블이 확인됨.
```
+----------------+
| Tables_in_test |
+----------------+
| test_table     |
+----------------+
```
- DESC test_table; 입력 -> 생성한 테이블의 구조가 확인됨.
```
+-------+-------------+------+-----+---------+-------+
| Field | Type        | Null | Key | Default | Extra |
+-------+-------------+------+-----+---------+-------+
| idx   | int         | YES  |     | NULL    |       |
| name  | varchar(10) | YES  |     | NULL    |       |
+-------+-------------+------+-----+---------+-------+
```
- Null : 아직 입력되지 않은 값. (현재 비어있음) YES는 입력을 허용한다는 뜻!
  - 숫자 0, 문자 공백과 전혀 다른 값
- Default : 기본값. 컬럼(열)에 데이터 입력 값을 주지 않은 경우 해당 컬럼에 NULL값이 입력.

### 테이블 수정 (ALTER TABLE)
- 기존 존재하는 테이블의 구조를 변경할 수 있는 문법
- 데이터가 저장된 테이블이거나 설정에 따라서 수정이 어려운 경우도 있음

---
- 컬럼 추가
```
-문법

ALTER TABLE 테이블명
ADD   컬럼명 데이터타입(크기);
```
```
ALTER TABLE text_table 입력
ADD Salary INT(20); 입력


+--------+-------------+------+-----+---------+-------+
| Field  | Type        | Null | Key | Default | Extra |
+--------+-------------+------+-----+---------+-------+
| idx    | int         | YES  |     | NULL    |       |
| name   | varchar(10) | YES  |     | NULL    |       |
| salary | int         | YES  |     | NULL    |       |
+--------+-------------+------+-----+---------+-------+
```
- 컬럼 이름 변경
```
-문법

ALTER TABLE 테이블명
CHANGE 기존컬럼명 변경할컬럼명 데이터타입(크기);
```
```
ALTER TABLE test_table 입력
CHANGE idx test_number INT(10); 입력


+-------------+-------------+------+-----+---------+-------+
| Field       | Type        | Null | Key | Default | Extra |
+-------------+-------------+------+-----+---------+-------+
| test_number | int         | YES  |     | NULL    |       |
| name        | varchar(10) | YES  |     | NULL    |       |
| salary      | int         | YES  |     | NULL    |       |
+-------------+-------------+------+-----+---------+-------+
```
- 컬럼 데이터타입 변경
```
-문법

ALTER TABLE 테이블명
MODIFY 컬럼명 변경할데이터타입(크기);
```
```
ALTER TABLE test_table 입력
MODIFY name CHAR(10); 입력


+-------------+----------+------+-----+---------+-------+
| Field       | Type     | Null | Key | Default | Extra |
+-------------+----------+------+-----+---------+-------+
| test_number | int      | YES  |     | NULL    |       |
| name        | char(10) | YES  |     | NULL    |       |
| salary      | int      | YES  |     | NULL    |       |
+-------------+----------+------+-----+---------+-------+
```
- 컬럼 삭제
```
-문법

ALTER TABLE 테이블명
DROP 컬럼명;
```
```
ALTER TABLE test_table 입력
DROP salary; 입력


+-------------+----------+------+-----+---------+-------+
| Field       | Type     | Null | Key | Default | Extra |
+-------------+----------+------+-----+---------+-------+
| test_number | int      | YES  |     | NULL    |       |
| name        | char(10) | YES  |     | NULL    |       |
+-------------+----------+------+-----+---------+-------+
```
- 테이블명 변경
```
-문법

ALTER TABLE 기존테이블명
RENAME 변경할테이블명;
```
```
ALTER TABLE test_table 입력
RENAME test_itwill; 입력


+-------------+----------+------+-----+---------+-------+
| Field       | Type     | Null | Key | Default | Extra |
+-------------+----------+------+-----+---------+-------+
| test_number | int      | YES  |     | NULL    |       |
| name        | char(10) | YES  |     | NULL    |       |
+-------------+----------+------+-----+---------+-------+
```
- 테이블 삭제
```
-문법

DROP TABLE 테이블명;
```
```
DROP TABLE test_itwill;


mysql> show tables;
Empty set (0.00 sec)
```

### 테이블 생성 연습
```
테이블 명 : student
컬럼명1 : stu_no
데이터타입1 : INT
컬럼명2 : last_name
데이터타입2 : VARCHAR(20)
컬럼명3 : birth_date
데이터타입3 : DATE



CREATE TABLE student (
stu_no INT,
last_name VARCHAR(20),
birth_date DATE
);




mysql> DESC student;

+------------+-------------+------+-----+---------+-------+
| Field      | Type        | Null | Key | Default | Extra |
+------------+-------------+------+-----+---------+-------+
| stu_no     | int         | YES  |     | NULL    |       |
| last_name  | varchar(20) | YES  |     | NULL    |       |
| birth_date | date        | YES  |     | NULL    |       |
+------------+-------------+------+-----+---------+-------+
```

## DML (Data Manipulation Language) / 데이터 조작어
- 데이터를 입력(INSERT), 갱신(UPDATE), 삭제(DELETE) 할 수 있는 문법


### INSERT 문법
- 테이블에 데이터를 입력하는 문법



```
- INSERT INTO 테이블명 [(칼럼명1[, …])]
  VALUES (데이터1 [, …]);
  
- values절에서 숫자 데이터는 숫자로만 작성, 문자열 데이터는 ''로 묶어서 작성
- 컬럼에는 컬럼이 가지는 데이터타입의 데이터들로만 값을 채울 수 있다.
```
```
INSERT INTO student (stu_no, last_name) 입력
VALUES (1, 'hi'); 입력



- 입력된 데이터 확인
SELECT * FROM student; 입력

+--------+-----------+------------+
| stu_no | last_name | birth_date |
+--------+-----------+------------+
|      1 | hi        | NULL       |
+--------+-----------+------------+

---------------------------------------------------------------------------------------------------------------

INSERT INTO student (stu_no, last_name) 입력
VALUES (2, '6'); 입력


SELECT * FROM student; 입력

+--------+-----------+------------+
| stu_no | last_name | birth_date |
+--------+-----------+------------+
|      1 | hi        | NULL       |
|      2 | 6         | NULL       |
+--------+-----------+------------+


---------------------------------------------------------------------------------------------------------------


INSERT INTO student (last_name, stu_no) 입력
VALUES ('Kim', 'k'); 입력

ERROR 1366 (HY000): Incorrect integer value: 'k' for column 'stu_no' at row 1 INSERT INTO student (last_name, stu_no) 
// 타입에 맞지 않는 데이터를 입력시 에러 발생!


VALUES ('Kim', 3); 입력


SELECT * FROM student; 입력

+--------+-----------+------------+
| stu_no | last_name | birth_date |
+--------+-----------+------------+
|      1 | hi        | NULL       |
|      2 | 6         | NULL       |
|      3 | Kim       | NULL       |
+--------+-----------+------------+

---------------------------------------------------------------------------------------------------------------

- 컬럼의 목록을 생략하는 경우 테이블이 가진 컬럼의 순서와 갯수에 맞춰서 values절의 내용을 작성한다.
- 입력할 값이 없는 경우 NULL을 입력할 수 있다.

INSERT INTO student 입력
VALUES (4, 'bye', NULL); 입력

SELECT * FROM student; 입력

+--------+-----------+------------+
| stu_no | last_name | birth_date |
+--------+-----------+------------+
|      1 | hi        | NULL       |
|      2 | 6         | NULL       |
|      3 | Kim       | NULL       |
|      4 | bye       | NULL       |
+--------+-----------+------------+

---------------------------------------------------------------------------------------------------------------

- 함수와 같은 연산구조를 통한 값의 입력도 가능하다.

INSERT INTO student 입력
VALUES (5, 'Buy', now()); 입력

SELECT * FROM student; 입력

+--------+-----------+------------+
| stu_no | last_name | birth_date |
+--------+-----------+------------+
|      1 | hi        | NULL       |
|      2 | 6         | NULL       |
|      3 | Kim       | NULL       |
|      4 | bye       | NULL       |
|      5 | Buy       | 2022-02-11 |
+--------+-----------+------------+

---------------------------------------------------------------------------------------------------------------

INSERT INTO student 입력
VALUES (6, 'LEE', '2022-02-14'); 입력

SELECT * FROM student; 입력

+--------+-----------+------------+
| stu_no | last_name | birth_date |
+--------+-----------+------------+
|      1 | hi        | NULL       |
|      2 | 6         | NULL       |
|      3 | Kim       | NULL       |
|      4 | bye       | NULL       |
|      5 | Buy       | 2022-02-11 |
|      6 | LEE       | 2022-02-14 |
+--------+-----------+------------+


```

### UPDATE 문법
- 기존 데이터의 값을 갱신할 때 사용하는 문법

```
- UPDATE 테이블명
  SET 갱신컬럼 = 갱신할 값
  [WHERE 조건컬럼 = 조건값];
	
* WHERE절에 대한 내용은 SELECT 구문에서 다룰 예정

```
```
+--------+-----------+------------+
| stu_no | last_name | birth_date |
+--------+-----------+------------+
|      1 | hi        | NULL       |
|      2 | 6         | NULL       |
|      3 | Kim       | NULL       |
|      4 | bye       | NULL       |
|      5 | Buy       | 2022-02-11 |
|      6 | LEE       | 2022-02-14 |
+--------+-----------+------------+

student 테이블에서 stu_no 컬럼의 값이 3인 행에 대해서 last_name컬럼의 값을 'Gim'으로 갱신


UPDATE student 입력
SET last_name = 'GIM' 입력
WHERE stu_no = 3; 입력

SELECT * FROM student; 입력

+--------+-----------+------------+
| stu_no | last_name | birth_date |
+--------+-----------+------------+
|      1 | hi        | NULL       |
|      2 | 6         | NULL       |
|      3 | Gim       | NULL       |
|      4 | bye       | NULL       |
|      5 | Buy       | 2022-02-10 |
|      6 | LEE       | 2022-02-14 |
+--------+-----------+------------+
```

## UPDATE 문법
- 기존 데이터의 값을 갱신할 때 사용하는 문법

```
- UPDATE 테이블명
  SET 갱신컬럼 = 갱신할 값
  [WHERE 조건컬럼 = 조건값];
```
```
UPDATE student 입력
SET last_name = 'GIM' 입력
WHERE stu_no = 3; 입력

SELECT * FROM student; 입력

+--------+-----------+------------+
| stu_no | last_name | birth_date |
+--------+-----------+------------+
|      1 | hi        | NULL       |
|      2 | 6         | NULL       |
|      3 | Gim       | NULL       |
|      4 | bye       | NULL       |
|      5 | Buy       | 2022-02-10 |
|      6 | LEE       | 2022-02-14 |
+--------+-----------+------------+

```

- WHERE절을 생략하는 경우 테이블의 모든 행에 대해서 SET절의 갱신의 영향 받음
```
UPDATE student 입력
SET last_name = 'Name'; 입력

+--------+-----------+------------+
| stu_no | last_name | birth_date |
+--------+-----------+------------+
|      1 | Name      | NULL       |
|      2 | Name      | NULL       |
|      3 | Name      | NULL       |
|      4 | Name      | NULL       |
|      5 | Name      | 2022-02-11 |
|      6 | Name      | 2022-02-14 |
+--------+-----------+------------+
```
- stu_no의 순서를 1씩 더하기
```
UPDATE student 입력
SET stu_no = stu_no + 1; 입력

+--------+-----------+------------+
| stu_no | last_name | birth_date |
+--------+-----------+------------+
|      2 | Name      | NULL       |
|      3 | Name      | NULL       |
|      4 | Name      | NULL       |
|      5 | Name      | NULL       |
|      6 | Name      | 2022-02-11 |
|      7 | Name      | 2022-02-14 |
+--------+-----------+------------+

```

## DELETE 문법
- 기존 테이블의 데이터를 삭제하는 문법
```
- 문법
DELETE FROM 테이블명;
[WHERE 조건컬럼 연산자 조건절];
```

```
- student 테이블에서 stu_no컬럼의 값이 3의 값을 가진 행을 삭제하기

DELETE FROM student 입력
WHERE stu_no = 3; 입력

+--------+-----------+------------+
| stu_no | last_name | birth_date |
+--------+-----------+------------+
|      2 | Name      | NULL       |
|      4 | Name      | NULL       |
|      5 | Name      | NULL       |
|      6 | Name      | 2022-02-11 |
|      7 | Name      | 2022-02-14 |
+--------+-----------+------------+


---------------------------------------------------------------------------------------------


- birth_date 컬럼의 값이 NULL인 행을 삭제

DELETE FROM student 입력
WHERE birth_date IS NULL; 입력

+--------+-----------+------------+
| stu_no | last_name | birth_date |
+--------+-----------+------------+
|      6 | Name      | 2022-02-11 |
|      7 | Name      | 2022-02-14 |
+--------+-----------+------------+


---------------------------------------------------------------------------------------------


- WHERE절을 생략하는 경우 테이블의 모든 데이터가 삭제

DELETE FROM student; 입력

Empty set (0.00 sec)
```

## 데이터 조회(SELECT)
- 테이블에 저장된 데이터를 조회할 때 사용하는 문법
> - 데이터 조회용 샘플 데이터 추가하기 (https://antop.tistory.com/attachment/cfile22.uf@120E12034B315C3B3FCAA1.zip)
> - cmd 창으로 돌아와서 exit 입력 -> mkdir Downloads 입력 -> cd Downloads/ 입력 -> pwd 입력 -> 
> - 압축 푼 폴더 안에서 Shift + 마우스 오른쪽 클릭 -> 여기에 PowerShell 창 열기 클릭 -> PowerShell 창에서 scp hr.sql root@아이피:/root/Downloads/. 입력 -> 비밀번호 입력
> - cmd 창으로 돌아와서 ls 입력 -> hr.sql이 있으면 다운로드 완료
> - cmd에서 mysql -u root -p로 로그인 -> CREATE DATABASE hr; 입력 -> use hr; 입력 -> source /root/Downloads/hr.sql 입력 -> show tables; 입력으로 생성된 테이블 확인
```
+----------------+
| Tables_in_hr   |
+----------------+
| HR_COUNTRIES   |
| HR_DEPARTMENTS |
| HR_EMPLOYEES   |
| HR_JOBS        |
| HR_JOB_HISTORY |
| HR_LOCATIONS   |
| HR_REGIONS     |
+----------------+
```
```
DESC HR_DEPARTMENTS; 입력

+-----------------+-------------+------+-----+---------+-------+
| Field           | Type        | Null | Key | Default | Extra |
+-----------------+-------------+------+-----+---------+-------+
| DEPARTMENT_ID   | int         | NO   | PRI | NULL    |       |
| DEPARTMENT_NAME | varchar(30) | NO   |     |         |       |
| MANAGER_ID      | int         | YES  | MUL | NULL    |       |
| LOCATION_ID     | int         | YES  | MUL | NULL    |       |
+-----------------+-------------+------+-----+---------+-------+
DEPARTMENT_ID     : 부서번호
DEPARTMENT_NAME   : 부서명
MANAGER_ID        : 부서장 사원번호
LOCATION_ID       : 부서의 위치코드
```

```
-문법
SELECT 컬럼1 [, 컬럼2, ...]
FROM 테이블명
[WHERE 조건컬럼 연산자 조건값];
```
```
HR_DEPARTMENTS 테이블에서  department_id, department_name, manager_id, location_id 컬럼 조회해보기


SELECT department_id, department_name, manager_id, location_id 입력
FROM HR_DEPARTMENTS; 입력

+---------------+----------------------+------------+-------------+
| department_id | department_name      | manager_id | location_id |
+---------------+----------------------+------------+-------------+
|            10 | Administration       |        200 |        1700 |
|            20 | Marketing            |        201 |        1800 |
|            30 | Purchasing           |        114 |        1700 |
|            40 | Human Resources      |        203 |        2400 |
|            50 | Shipping             |        121 |        1500 |
|            60 | IT                   |        103 |        1400 |
|            70 | Public Relations     |        204 |        2700 |
|            80 | Sales                |        145 |        2500 |
|            90 | Executive            |        100 |        1700 |
|           100 | Finance              |        108 |        1700 |
|           110 | Accounting           |        205 |        1700 |
|           120 | Treasury             |       NULL |        1700 |
|           130 | Corporate Tax        |       NULL |        1700 |
|           140 | Control And Credit   |       NULL |        1700 |
|           150 | Shareholder Services |       NULL |        1700 |
|           160 | Benefits             |       NULL |        1700 |
|           170 | Manufacturing        |       NULL |        1700 |
|           180 | Construction         |       NULL |        1700 |
|           190 | Contracting          |       NULL |        1700 |
|           200 | Operations           |       NULL |        1700 |
|           210 | IT Support           |       NULL |        1700 |
|           220 | NOC                  |       NULL |        1700 |
|           230 | IT Helpdesk          |       NULL |        1700 |
|           240 | Government Sales     |       NULL |        1700 |
|           250 | Retail Sales         |       NULL |        1700 |
|           260 | Recruiting           |       NULL |        1700 |
|           270 | Payroll              |       NULL |        1700 |
+---------------+----------------------+------------+-------------+


---------------------------------------------------------------------------------------------
HR_DEPARTMENTS 테이블에서 department_id, department_name 컬럼 조회해보기

SELECT department_id, department_name 입력
FROM HR_DEPARTMENTS; 입력

+---------------+----------------------+
| department_id | department_name      |
+---------------+----------------------+
|            10 | Administration       |
|            20 | Marketing            |
|            30 | Purchasing           |
|            40 | Human Resources      |
|            50 | Shipping             |
|            60 | IT                   |
|            70 | Public Relations     |
|            80 | Sales                |
|            90 | Executive            |
|           100 | Finance              |
|           110 | Accounting           |
|           120 | Treasury             |
|           130 | Corporate Tax        |
|           140 | Control And Credit   |
|           150 | Shareholder Services |
|           160 | Benefits             |
|           170 | Manufacturing        |
|           180 | Construction         |
|           190 | Contracting          |
|           200 | Operations           |
|           210 | IT Support           |
|           220 | NOC                  |
|           230 | IT Helpdesk          |
|           240 | Government Sales     |
|           250 | Retail Sales         |
|           260 | Recruiting           |
|           270 | Payroll              |
+---------------+----------------------+

```

- 컬럼 목록을 하나하나 입력하는 것이 아닌, '*' 기호만 작성하는 경우 테이블의 모든 컬럼을 출력
```
DESC HR_EMPLOYEES; 입력

+----------------+--------------+------+-----+---------+-------+
| Field          | Type         | Null | Key | Default | Extra |
+----------------+--------------+------+-----+---------+-------+
| EMPLOYEE_ID    | int          | NO   | PRI | NULL    |       |
| FIRST_NAME     | varchar(20)  | YES  |     | NULL    |       |
| LAST_NAME      | varchar(25)  | NO   |     |         |       |
| EMAIL          | varchar(25)  | NO   | UNI |         |       |
| PHONE_NUMBER   | varchar(20)  | YES  |     | NULL    |       |
| HIRE_DATE      | date         | NO   |     | NULL    |       |
| JOB_ID         | varchar(10)  | NO   | MUL |         |       |
| SALARY         | double(22,0) | YES  |     | NULL    |       |
| COMMISSION_PCT | double(22,0) | YES  |     | NULL    |       |
| MANAGER_ID     | int          | YES  | MUL | NULL    |       |
| DEPARTMENT_ID  | int          | YES  | MUL | NULL    |       |
+----------------+--------------+------+-----+---------+-------+

SELECT * 입력 
FROM HR_EMPLOYEES; 입력

+-------------+-------------+-------------+----------+--------------------+------------+------------+--------+----------------+------------+---------------+
| EMPLOYEE_ID | FIRST_NAME  | LAST_NAME   | EMAIL    | PHONE_NUMBER       | HIRE_DATE  | JOB_ID     | SALARY | COMMISSION_PCT | MANAGER_ID | DEPARTMENT_ID |
+-------------+-------------+-------------+----------+--------------------+------------+------------+--------+----------------+------------+---------------+
|         100 | Steven      | King        | SKING    | 515.123.4567       | 1987-06-17 | AD_PRES    |  24000 |           NULL |       NULL |            90 |
|         101 | Neena       | Kochhar     | NKOCHHAR | 515.123.4568       | 1989-09-21 | AD_VP      |  17000 |           NULL |        100 |            90 |
|         102 | Lex         | De Haan     | LDEHAAN  | 515.123.4569       | 1993-01-13 | AD_VP      |  17000 |           NULL |        100 |            90 |
|         103 | Alexander   | Hunold      | AHUNOLD  | 590.423.4567       | 1990-01-03 | IT_PROG    |   9000 |           NULL |        102 |            60 |

...

|         204 | Hermann     | Baer        | HBAER    | 515.123.8888       | 1994-06-07 | PR_REP     |  10000 |           NULL |        101 |            70 |
|         205 | Shelley     | Higgins     | SHIGGINS | 515.123.8080       | 1994-06-07 | AC_MGR     |  12000 |           NULL |        101 |           110 |
|         206 | William     | Gietz       | WGIETZ   | 515.123.8181       | 1994-06-07 | AC_ACCOUNT |   8300 |           NULL |        205 |           110 |
+-------------+-------------+-------------+----------+--------------------+------------+------------+--------+----------------+------------+---------------+

```

## 표현식(expression)
- 수식을 통해서 데이터에 대한 연산을 한 결과를 출력할 수 있는 식
- 사칙연산
  - 덧셈 : +
  - 뺄셈 : -
  - 곱셈 : *
  - 나눗셈 : /

```
- salary(월급)을 이용하여 연봉 계산하기

SELECT salary, salary * 12 입력
FROM HR_EMPLOYEES; 입력

+--------+-------------+
| salary | salary * 12 |
+--------+-------------+
|  24000 |      288000 |
|  17000 |      204000 |
|  17000 |      204000 |

...

|  10000 |      120000 |
|  12000 |      144000 |
|   8300 |       99600 |
+--------+-------------+
```

## column alias (컬럼의 별명)
- 결과 출력 시 기존의 이름이 아닌 별명으로 임시의 이름을 붙여 결과를 출력
- 실제 컬럼의 이름은 바뀌지 않음

```
SELECT salary, salary * 12 "annual salary" 입력
FROM HR_EMPLOYEES; 입력

+--------+---------------+
| salary | annual salary |
+--------+---------------+
|  24000 |        288000 |
|  17000 |        204000 |
|  17000 |        204000 |

...

|  10000 |        120000 |
|  12000 |        144000 |
|   8300 |         99600 |
+--------+---------------+
```

- HR_EMPLOYEES 테이블의 데이터를 아래의 형식으로 출력하는 구문 작성

```
employee_id -> 사원번호
first_name -> 이름
last_name -> 성
hire_date -> 고용일
department_id -> 소속부서




SELECT employee_id "사원번호", first_name "이름", last_name "성", hire_date "고용일", department_id "소속부서" 입력
FROM HR_EMPLOYEES; 입력

+--------------+-------------+-------------+------------+--------------+
| 사원번호      | 이름        | 성          | 고용일      | 소속부서     |
+--------------+-------------+-------------+------------+--------------+
|          100 | Steven      | King        | 1987-06-17 |           90 |
|          101 | Neena       | Kochhar     | 1989-09-21 |           90 |
|          102 | Lex         | De Haan     | 1993-01-13 |           90 |
|          103 | Alexander   | Hunold      | 1990-01-03 |           60 |
|          104 | Bruce       | Ernst       | 1991-05-21 |           60 |
|          105 | David       | Austin      | 1997-06-25 |           60 |
...
|          201 | Michael     | Hartstein   | 1996-02-17 |           20 |
|          202 | Pat         | Fay         | 1997-08-17 |           20 |
|          203 | Susan       | Mavris      | 1994-06-07 |           40 |
|          204 | Hermann     | Baer        | 1994-06-07 |           70 |
|          205 | Shelley     | Higgins     | 1994-06-07 |          110 |
|          206 | William     | Gietz       | 1994-06-07 |          110 |
+--------------+-------------+-------------+------------+--------------+
```

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
