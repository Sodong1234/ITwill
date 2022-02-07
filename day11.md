# [오전수업] DB 4차

> 가상머신 내보내기 & 불러오기
> - 내보내기 : Oracle VM VirtualBox 실행 -> 파일 -> 가상시스템 내보내기 -> 내보내기 할 가상머신 선택 -> MAC 주소정책을 '모든 네트워크 어댑터 MAC 주소 제외' 선택 -> 내보내기 끝
> - 불러오기 : Oracle VM VirtualBox 실행 -> 파일 -> 가상시스템 가져오기 -> 불러오기 할 가상머신 선택 -> MAC 주소정책을 '모든 네트워크 어댑터 MAC 주소 생성' 선택 -> 불러오기 끝
>   - 본인이 설정해놓은 가상머신을 어느 컴퓨터에서든 자유롭게 사용 가능


## 가상머신 터미널 원격접속하기
- 가상머신 실행 후 root 입력 -> 비밀번호 1234 입력-> 로그인 성공했다면 ip add show 입력 -> 본인의 장치명과 아이피 확인 -> ssh root@아이피주소 입력 -> 비밀번호 입력
![캡처](https://user-images.githubusercontent.com/95197594/152708890-ee36e657-6bf3-479f-bf06-999056c93b54.PNG)

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
  - 날짜 : DATE, TIMESTAMPE 
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
- DESC test_table 입력 -> 생성한 테이블의 구조가 확인됨.
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
CHANGE 기존컬럼명 변경할컬럼명 데이터타입(크기)
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
MODIFY 컬럼명 변경할데이터타입(크기)
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

## DDL (Data Definition Language) / 데이터 정의어
- 데이터 저장 구조를 다루는 문법
- CREATE : 오브젝트 생성
- ALTER : 오브젝트 변형
- DROP : 오브젝트 삭제



   
