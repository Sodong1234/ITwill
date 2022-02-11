# [오전수업]

> 저번 시간에 만들어놓은 테이블에서 추가 진행
```
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


SELECT * FROM student;

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
```

### UPDATE 문법
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
+--------+-----------+------------+
```


  
