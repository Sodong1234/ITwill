# [오전수업] DB 5차

> - DB 4차에 실시한 수업 복습 실시하였음.
> - 가상머신 구동 -> cmd에서  ssh root@ip로 로그인 -> mysql -u root -p로 로그인 -> show databses; 입력 -> 안에 있는 test 데이터베이스를 사용하기 위해 use test; 입력 -> 안에 있는 student 테이블의 구조를 확인하기 위해 DESC student; 까지 입력 실시하였음
> - DB 4차, UPDATE 항목 이어서 수업 실시. 

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

