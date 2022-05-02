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

---

# [오후수업] JAVA 40차
