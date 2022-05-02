# [오전수업] DB 26차

> 25차 수업 복습 실시

## 데이터 딕셔너리
```
SELECT *
FROM dictionary
WHERE table_name = 'USER_OBJECTS';

TABLE_NAME  |COMMENTS                 |
------------+-------------------------+
USER_OBJECTS|Objects owned by the user|
```

## 데이터정의어(DDL)
- 오브젝트의 구조를 정의하는 문법
- CREATE(생성), ALTER(변형), DROP(삭제), TRUNCATE(절단)
```
- 테이블
행으로 구성되어 있는, 데이터 저장의 기본 단위
- 뷰
하나 이상의 테이블에 대한 논리적으로 표현된 부분집합
- 시퀀스
숫자값 생성 오브젝트
- 인덱스
일부 쿼리 구문의 성능을 높여줌. 제약조건(PK, UK)
- 시노님
동의어. 기존 오브젝트에 반영구적인 별명을 부여
```

## FOREIGN KEY
![1](https://user-images.githubusercontent.com/95197594/166177495-ed3706f5-fb98-482a-bc46-8fa25d347332.PNG)

``` 
CREATE TABLE test_pk
(id NUMBER PRIMARY KEY, 
name VARCHAR2(30));

CREATE TABLE test_fk
(emp_id NUMBER REFERENCES test_pk(id),
emp_name varchar2(30));
```
![2](https://user-images.githubusercontent.com/95197594/166177557-53db965f-f9a7-4ca4-a3df-7e6d6abe76d5.PNG)
```
INSERT INTO test_pk
VALUES (10, 'ten');
INSERT INTO test_pk
VALUES (20, 'tenten');


COMMIT;


SELECT * FROM test_pk;
ID|NAME  |
--+------+
10|ten   |
20|tenten|



--------------------------------------------------------------------------------------------------------------------



INSERT INTO test_fk
VALUES (10, 't');
INSERT INTO test_fk
VALUES (5, 'f');

- 두번째 구문에서 5의 값은 부모컬럼인 test_pk의 id컬럼에는 없는 값으로 입력할수 없는 값을 입력해서 에러가 발생

INSERT INTO test_fk
VALUES (10, 'f');


SELECT * FROM test_fk;

EMP_ID|EMP_NAME|
------+--------+
    10|t       |
    10|f       |
```

### check 제약조건
- id 컬럼에 0이상의 값만 입력 허용하는 check제약조건 설정
```
CREATE TABLE test_ck
(id NUMBER CHECK (id >= 0),
name VARCHAR2(30));

INSERT INTO test_ck
VALUES (5, null);
INSERT INTO test_ck
VALUES (-5, null);


SQL Error [2290] [23000]: ORA-02290: 체크 제약조건(HR.SYS_C007728)이 위배되었습니다
```

## VIEW
- 쿼리구문의 출력결과를 가상의 테이블로 활용할 수 있게 해주는 문법
- 특정 연산을 하는 구문을 반복적으로 사용해야 할 일이 있는 경우 활용한다면 유용
- 뷰의 결과는 저장되지 않으며 뷰를 구성하는 SELECT구문만 데이터딕셔너리에 저장되어 있다.
- 뷰의 내용을 구성하는 데이터들의 테이블을 base table이라고 한다.
- 뷰를 통한 DML작업을 하는 경우 뷰의 데이터는 base table의 데이터를 사용하는 것이므로 결국 base table의 데이터에 작업이 이루어진다.
- 뷰의 DML 작업은 권한이나 제약조건에 의해서 일부 제한 될 수 있다.
```
SELECT employee_id, last_name, salary, salary * 12 an_sal
FROM employees;


- 뷰 생성 시 표현식, 함수를 사용한 경우 column alias를 필수로 사용해야 한다.
CREATE VIEW ann_salary AS 
SELECT employee_id, last_name, salary, salary * 12 an_sal
FROM employees;


SELECT * FROM ann_salary;


UPDATE ann_salary
SET salary = salary * 2
WHERE employee_id = 100;

SELECT employee_id, salary
FROM employees;

CREATE VIEW emp_dept AS 
SELECT employee_id, last_name, 
d.department_id, department_name
FROM employees e, departments d
WHERE e.department_id = d.department_id;


SELECT * FROM emp_dept;


- 뷰의 내용을 재정의해야 하는 경우 OR REPLACE 키워드를 추가하여 뷰를 재정의할 수 있다.
CREATE OR REPLACE VIEW emp_dept AS 
SELECT employee_id, last_name, salary, 
d.department_id, department_name
FROM employees e, departments d
WHERE e.department_id = d.department_id;


SELECT * FROM emp_dept;


```

## 시퀀스
- 고유한 숫자값 생성 오브젝트

> CREATE SEQEUNCE 시퀀스명

### 옵션
```
INCREMENT BY n  : 생성 숫자의 증감간격. 기본값 1
START WITH n    : 생성 시작값 지정. 기본값 1
MAXVALUE n      : 생성 최대값 지정
NOMAXVALUE      : 최대값을 지정하지 않음. 
MINVALUE n      : 생성 최소값 지정
NOMINVALUE      : 최소값을 지정하지 않음.
CYCLE           : 한계값 도달 시 초기값으로 돌아감
NOCYCLE         : 한계값 도달 시 값의 생성을 멈춤
CACHE n         : 값을 미리 생성해두는 옵션. 기본값 6
NOCACHE         : 값을 미리 생성하지 않음.
```

---

# [오후수업] JAVA 40차
