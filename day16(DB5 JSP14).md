# [오전수업] DB 5차

> - DB 4차에 실시한 수업 복습 실시하였음.
> - 가상머신 구동 -> cmd에서  ssh root@ip로 로그인 -> mysql -u root -p로 로그인 -> show databses; 입력 -> 안에 있는 test 데이터베이스를 사용하기 위해 use test; 입력 -> 안에 있는 student 테이블의 구조를 확인하기 위해 DESC student; 까지 입력 실시하였음
> - DB 4차, UPDATE 항목 이어서 수업 실시. 

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
+--------+-----------+------------+
```

- WHERE절을 생략하는 경우 테이블의 모든 행에 대해서 SET절의 갱신의 영향을 받게 된다.
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

### 데이터 삭제
- 기존 테이블의 데이터를 삭제하는 문법
```
-문법
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




- birth_date 컬럼의 값이 NULL인 행을 삭제

DELETE FROM student 입력
WHERE birth_date IS NULL; 입력

+--------+-----------+------------+
| stu_no | last_name | birth_date |
+--------+-----------+------------+
|      6 | Name      | 2022-02-11 |
|      7 | Name      | 2022-02-14 |
+--------+-----------+------------+





- WHERE절을 생략하는 경우 테이블의 모든 데이터가 삭제

DELETE FROM student; 입력

Empty set (0.00 sec)
```

## 데이터 조회(SELECT)
> - 데이터 조회용 샘플 데이터 추가하기 (https://antop.tistory.com/attachment/cfile22.uf@120E12034B315C3B3FCAA1.zip)
> - cmd 창으로 돌아와서 exit 입력 -> mkdir Downloads 입력 -> cd Downloads/ 입력 -> pwd 입력 -> 
> - 압축 푼 폴더 안에서 Shift + 마우스 오른쪽 클릭 -> 여기에 PowerShell 창 열기 클릭 -> PowerShell 창에서 scp hr.sql root@아이피:/root/Downloads/. 입력 -> 비밀번호 입력
> - cmd 창으로 돌아와서 ls 입력 -> hr.sql이 있으면 다운로드 완료
> - cmd에서 mysql -u root -p로 로그인 -> CREATE DATABASE hr; 입력 -> use hr; 입력 -> source /root/Downloads/hr.sql 입력 -> show tables; 입력 -> 






