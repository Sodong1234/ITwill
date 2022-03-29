# [오전수업] DB 14차

## sql developer 접속하기
> sqldeveloper-21.4.3.063.0100-x64.zip 다운로드

- sqldevelpoer.exe 실행 -> 
- 이전에 사용한 sql developer가 없으면 아니오 선택

![unnamed](https://user-images.githubusercontent.com/95197594/160032712-ed4ce80d-d57e-4605-8956-d804c7acb6ff.png)

- 왼쪽 초록색 + 버튼 눌러서 새로운 접속 생성 -> 
- 설정 후 테스트 눌러보고 상태:성공 뜨면 저장 후 접속

![unnamed (1)](https://user-images.githubusercontent.com/95197594/160032828-c1201597-372f-43a5-8f62-76c6d2fae751.png)

## 테이블 생성
- 필수 요소 : 테이블명, 컬럼명, 데이터타입, 컬럼 크기
- 옵션 요소 : 기본값, 제약조건
```
CREATE TABLE dept (
    deptno      NUMBER(2), 
    dname       VARCHAR2(14), 
    loc         VARCHAR2(13), 
    create_date DATE DEFAULT sysdate
);

```

## 제약조건

### NOT NULL
- 컬럼에 값 입력 시 NOT NULL제약조건이 설정된 컬럼에는 NULL값은 입력 할 수 없게하는 제약조건
- test1의 id 컬럼에 NOT NULL 적용

```
CREATE TABLE test1 (
    id   NUMBER(10) NOT NULL, 
    name VARCHAR2(30)
);
```

- NOT NULL 제약조건이 적용된 id컬럼에는 NULL값을 입력할 수 없다.
```

INSERT INTO test1 (name) 
VALUES ('bye');

INSERT INTO test1 (name) 
VALUES ('bye')
오류 보고 -
ORA-01400: NULL을 ("HR"."TEST1"."ID") 안에 삽입할 수 없습니다
```

### UNIQUE
- 중복값을 허용하지 않는 제약조건

```
CREATE TABLE test2 (
    id   NUMBER(10) NOT NULL UNIQUE, 
    name VARCHAR2(30)
);




INSERT INTO test2(id)
values (50);
```

- 위의 데이터 입력 구문을 두번 실행때부터는 아래와 같은 에러가 발생한다.
> - 오류 보고 -
> ORA-00001: 무결성 제약 조건(HR.SYS_C007714)에 위배됩니다

### PRIMARY KEY
- 기본키 제약조건
- 테이블을 대표하는 컬럼에 주로 설정
- NOT NULL + UNIQUE 의 성질을 다 가지고 있음
- 테이블당 한번만 사용할 수 있음.

```
CREATE TABLE test3 (
    id   NUMBER(10) PRIMARY KEY, 
    name VARCHAR2(30)
);
```

- 동일한 값을 여러 번 입력시도하거나 컬럼에 NULL값을 입력하려고 하는 경우 제약조건 위배로 구문이 실행되지 않는다.
> - INSERT INTO test3 VALUES (20, 'HH');
> - insert into test3 (name) values ('KK');

### auto_increment
- 컬럼에 자동으로 값이 증가하는 숫자 입력 기능 추가
- mysql에서 사용하는 문법

```
CREATE TABLE test3 (
    id   NUMBER(10) PRIMARY KEY auto_increment, 
    name VARCHAR2(30)
);
```

### 기존 테이블 제약조건 추가하기
```
ALTER TABLE test3
MODIFY (name varchar2(40) NOT NULL);



SELECT *
FROM test1;


        ID NAME                          
---------- ------------------------------
        12 Hi                            
        12 Hi                            
        12                               


SELECT *
FROM test2;

        ID NAME                          
---------- ------------------------------
        50                               
        60        

SELECT *
FROM test3;

        ID NAME                                    
---------- ----------------------------------------
        10 HH                                      
        20 HH                                      
```


### 부록 : DBeaver 사용하기
- https://dbeaver.io/files/dbeaver-ce-latest-x86_64-setup.exe
- 마우스 오른쪽 -> Create -> Connection 클릭 -> Oracle 선택
- 세팅 설정 후 다운로드

![unnamed (2)](https://user-images.githubusercontent.com/95197594/160050514-3cff85ac-cf4c-443f-8746-4177c6031596.png)
![unnamed (3)](https://user-images.githubusercontent.com/95197594/160050517-a88763a3-71ee-41b6-b02f-fedc4ce3fa94.png)


---

# [오후수업] JAVA 25차
> 테스트 실시
