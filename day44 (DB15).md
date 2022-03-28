# [오전수업] DB 15차

## 테이블 생성 연습
```
테이블명 : member

컬럼명    / 데이터타입 / 크기 / 제약조건
member_id / NUMBER    / 5    / PRIMARY KEY
last_name / VARCHAR2  / 30   / NOT NULL
phone_no  / VARCHAR2  / 25   / UNIQUE



CREATE TABLE MEMBER
(member_id	NUMBER(5) PRIMARY KEY,
last_name	VARCHAR2(30) NOT NULL,
phone_no	VARCHAR2(25) UNIQUE);

```

