# [오전수업] DB 6차

> DB 5차 수업때 실시한 SELECT 내용을 보충해 놓았음
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

