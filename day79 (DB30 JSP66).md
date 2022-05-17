# [오전수업] DB 30차
## 테이블 모델링
아래의 내용을 예시로 DB 구조만들어보기.

![unnamed](https://user-images.githubusercontent.com/95197594/168720649-1331026b-3fb0-4ee5-909b-2ed55d2ac768.png)

- 개념적 모델링(Entity-Relation Diagram)

![unnamed (1)](https://user-images.githubusercontent.com/95197594/168720734-091a9a9d-a50d-4536-9120-3ad0adcb0007.png)


### 저장 될 값 분류하기
- 저장해서 관리 할 값들을 분류하여 정리한다.	

- 논리적 모델링

![캡처](https://user-images.githubusercontent.com/95197594/168721113-6b265956-27da-4bbd-a9eb-2b565a3dabb4.PNG)
- 데이터 필드에는 하나의 값만 있도록 테이블을 구성한다. 위의 정보에서 하이라이트 된 값들은 여러 값을 가질 수도 있는 속성들이다.
- ex) 배우 : 배역 컬럼의 경우 값이 하나가 아님. 관계형 데이터베이스에서는 각 필드에서 원자값을 가져야 하므로 분리해준다.

![캡처](https://user-images.githubusercontent.com/95197594/168721213-59792c92-9efa-4656-a1ec-3278d494319f.PNG)

- 배역과 같이 하나의 속성에 여러 값이 있는 경우 하나의 필드에 다 넣을 수 없으므로 행으로 나누어 값을 저장한다.
- 다만 이런 경우 나머지 컬럼의 값에 대해서 중복 데이터가 발생하게 된다.

### 테이블 속성 분리하기
- 위의 정보들에서 참여감독, 참여배우, 배역, 참여영화, ... 와 같은 속성의 값들은 여러 값이 생길 수 있는 속성들로 베이스 테이블로부터 분리하여 별도의 테이블로 나눠서 보관한다.
- ex) 테이블 속성 분리하기
  - 이런 정보들을 분리하기 위해서는 이 속성값을 결정지을 수 있는 정보들을 참조하여 같이 테이블에 보관한다.
  - 배우의 배역은 출연하는 영화에 따라 결정되므로 배우, 영화, 배역의 형태로 구성
  - 이 때 배우의 정보와 영화의 정보는 각 테이블의 데이터들을 참조하는 형태로 가정

![캡처](https://user-images.githubusercontent.com/95197594/168721640-3a898595-a069-4c82-932f-3d2e9eb23001.PNG)
- 모델링에서 위와 같은 형태로 다른 테이블의 참조값을 받는 외래키이자 기본키의 일부가 되어 외래키의 값이 필수가 되는 경우를 식별관계라고 한다. 또한 위의 외래키의 값을 바로 기본키로 활용하는 형태를 자연키라고 한다.
- 다만 위의 자연키 방식인 경우 드물게 부모테이블의 값이 바뀌게 되는 상황이 있는 경우 자식테이블에도 영향을 미치게 되므로 이보다는 인공키의 방식이 안전하다.

인공키의 방식을 적용하는 경우 아래와 같이 볼 수 있다.
![캡처](https://user-images.githubusercontent.com/95197594/168721705-6935afe8-cb54-4fa0-aac9-79bd621b6c42.PNG)
- 별도의 영화_배우_id 컬럼을 추가하여 PK로 만들어 사용하는 경우 부모컬럼의 값에 변동이 있더라도 일반컬럼으로 값을 받기 때문에 행에 대한 영향은 적다. 또한 각 외래키의 값은 필수가 아니므로 비식별의 관계가 된다.

### 이행적 종속 없애기
- 이행적 종속이란 위와 같이 PK가 아닌 일반 컬럼에 의해서 다른 컬럼의 값이 종속적으로 결정되는 관계를 말한다.

![캡처](https://user-images.githubusercontent.com/95197594/168721827-c216aaca-b425-4aff-8d7c-5857bd78f0c6.PNG)
- 위의 컬럼의 관계 중 출생국가의 값은 배우_ID가 아닌 출신지에 따라 결정되는 상황이 생김.
- 기본적으로 모든 컬럼의 값은 PK에 의해서 결정이 되어야 하므로 이런 부분은 분리가 되어야 한다.
![캡처](https://user-images.githubusercontent.com/95197594/168721890-876b5736-4eec-4dd0-b633-f6b89d2ee883.PNG)


### 외래키
- 외래키를 통해서 테이블들 간에 참조관계를 만들어 둔다면, 테이블에 정확한 값을 입력만 허용하는 규칙이 생기므로 데이터베이스의 일관성은 올라간다. 
- 다만 관련 테이블에서 작업 시 데이터 입력, 갱신, 삭제의 작업에 여러 불편한 점이 발생하므로 상황에 따라 유연하게 제약조건을 결정을 해준다.

- 외래키 제약조건 활성 / 비활성
```
외래키 제약조건 일시적으로 끄기
set foreign_key_checks=0;

외래키 제약조건 일시적으로 켜기
set foreign_key_checks=1;
```
- 제약조건 선언 시 사용가능한 옵션
```
ON UPDATE CASCADE : 기존테이블 데이터 변경 시, 제약조건 테이블 데이터도 자동 변경
ON DELETE CASCADE : 기존테이블 데이터 삭제 시, 제약조건 테이블 데이터도 자동 삭제
```

- 물리적 모델링

![캡처](https://user-images.githubusercontent.com/95197594/168722018-f6ae2134-7153-4369-8a92-7ebf9b1bb971.PNG)
```
CREATE TABLE 배우 (
    배우번호 INT PRIMARY KEY,
    이름 VARCHAR(50) NOT NULL,
    성별 VARCHAR(30),
    생년월일 TIMESTAMP,
    키 VARCHAR(20),
    몸무게 VARCHAR(20),
    혈액형 VARCHAR(10)
);


CREATE TABLE 영화 (
    영화코드 VARCHAR(50) PRIMARY KEY,
    제목 VARCHAR(50) NOT NULL,
    제작년도 INT,
    제작국가 VARCHAR(50),
    상영시간 VARCHAR(20),
    개봉일자 TIMESTAMP,
    제작사 VARCHAR(50)
);


CREATE TABLE 감독 (
    감독번호 INT PRIMARY KEY,
    이름 VARCHAR(50) NOT NULL,
    성별 VARCHAR(30),
    생년월일 VARCHAR(50),
    출생지 VARCHAR(50),
    최종학력 VARCHAR(50)
); 


CREATE TABLE 배우_영화 (
    영화코드 VARCHAR(50) REFERENCES 영화 (영화코드),
    배우번호 INT REFERENCES 배우 (배우번호),
    배역 VARCHAR(50),
    PRIMARY KEY (영화코드 , 배우번호)
);


CREATE TABLE 영화_감독 (
    연출코드 INT PRIMARY KEY,
    영화코드 VARCHAR(50) REFERENCES 영화 (영화코드),
    감독번호 INT REFERENCES 감독 (감독번호)
);
```

- 부모테이블의 값을 참조해서 받아올 때 일반컬럼으로 받는 경우 부모테이블의 값이 필수가 아니게 되므로 비식별관계라하고 각 개체들을 점선으로 연결한다.
- 부모테이블의 값을 참조해서 받아올 때 기본키로 받는 경우 부모테이블의 값이 필수이므로 식별관계라하고 각 개체들을 실선으로 연결한다.

---

### 서브쿼리로 테이블 생성하기
- 서브쿼리가 출력해주는 내용으로 새로운 테이블을 생성하는 문법
- 서브쿼리의 컬럼명이 새로운 테이블의 컬럼명이 된다. 따라서 테이블의 컬럼의 이름으로 사용할 수 없는 함수명, 표현식의 경우 column alias 를 활용하여 대체한다.
- AS 키워드는 기능은 없으나 구문의 필수 요소이다.
- NOT NULL제약조건은 복사 됨.
```
CREATE TABLE dept80
    AS
        SELECT employee_id,
               last_name,
               salary * 12 annsal,
               hire_date
        FROM employees
        WHERE department_id = 80;


desc dept80

이름          널?       유형           
----------- -------- ------------ 
EMPLOYEE_ID          NUMBER(6)    
LAST_NAME   NOT NULL VARCHAR2(25) 
ANNSAL               NUMBER       
HIRE_DATE   NOT NULL DATE         


SELECT *
FROM dept80;

EMPLOYEE_ID|LAST_NAME |ANNSAL|HIRE_DATE            |
-----------+----------+------+---------------------+
        145|Russell   |168000|2004-10-01 00:00:00.0|
        146|Partners  |162000|2005-01-05 00:00:00.0|
        147|Errazuriz |144000|2005-03-10 00:00:00.0|
        148|Cambrault |132000|2007-10-15 00:00:00.0|
        149|Zlotkey   |126000|2008-01-29 00:00:00.0|
…
```
- WHERE절의 조건식을 항상 거짓이 되는 조건으로 만드는 경우 생성되는 테이블에는 데이터 없이 테이블의 구조만 복사되어 생성되어진다.
```

CREATE TABLE dept80
    AS
        SELECT employee_id,
               last_name,
               salary * 12 annsal,
               hire_date
        FROM employees
        WHERE 1 = 2;
```

### alter table
- 기존 테이블의 구조를 변형하는 문법
- 테이블의 구성에 새로운 부분을 추가하거나 기존 내용을 변경, 일부 구조를 삭제할 수 있는 문법

### ADD절
- 테이블에 구조를 더 하는 문법으로 새로운 컬럼이나 제약조건 등을 추가할 수 있다.
- 추가할 요소의 온전한 내용을 다 작성해야 한다.
- 새롭게 추가되는 컬럼에는 값이 null로 비어 있음.
```
ALTER TABLE dept80 ADD (
    job_id VARCHAR2(9)
);


EMPLOYEE_ID|LAST_NAME |ANNSAL|HIRE_DATE            |JOB_ID|
-----------+----------+------+---------------------+------+
        145|Russell   |168000|2004-10-01 00:00:00.0|      |
        146|Partners  |162000|2005-01-05 00:00:00.0|      |
        147|Errazuriz |144000|2005-03-10 00:00:00.0|      |
        148|Cambrault |132000|2007-10-15 00:00:00.0|      |
        149|Zlotkey   |126000|2008-01-29 00:00:00.0|      |
…
```

### MODIFY절
- 테이블의 기존 컬럼에 대해서 설정을 바꾸는 명령어.
- 테이블의 데이터가 입력되어 있는 상태에서는 작업의 일부에 제한이 있을 수 있다.
```
desc dept80
이름          널?       유형           
----------- -------- ------------ 
EMPLOYEE_ID          NUMBER(6)    
LAST_NAME   NOT NULL VARCHAR2(25) 
ANNSAL               NUMBER       
HIRE_DATE   NOT NULL DATE         
JOB_ID               VARCHAR2(9)  


ALTER TABLE dept80 MODIFY (
    last_name VARCHAR2(30)
);


이름          널?       유형           
----------- -------- ------------ 
EMPLOYEE_ID          NUMBER(6)    
LAST_NAME   NOT NULL VARCHAR2(30) 
ANNSAL               NUMBER       
HIRE_DATE   NOT NULL DATE         
JOB_ID               VARCHAR2(9)  
```

### drop 절
- 테이블의 기존 구조에서 일부를 삭제할 때 사용하는 절

```

ALTER TABLE dept80 DROP COLUMN job_id;


desc dept80

이름          널?       유형           
----------- -------- ------------ 
EMPLOYEE_ID          NUMBER(6)    
LAST_NAME   NOT NULL VARCHAR2(30) 
ANNSAL               NUMBER       
HIRE_DATE   NOT NULL DATE         

SELECT constraint_name,
       constraint_type
FROM user_constraints
WHERE table_name = 'EMP2';

CONSTRAINT_NAME|CONSTRAINT_TYPE|
---------------+---------------+
EMP2_MGR_FK    |R              |
SYS_C007714    |C              |
SYS_C007715    |C              |
SYS_C007716    |C              |
SYS_C007717    |C              |
EMP2_EID_PK    |P              |

ALTER TABLE emp2 DROP CONSTRAINT emp2_mgr_fk;


```

---

# [오후수업] JSP 66차
