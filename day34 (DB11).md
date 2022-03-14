# [오전수업] DB 11차


> Oracle Database 접속

```
- 가상머신에 리눅스 ova 이미지 삽입 -> 계정란에서 비밀번호를 "oracle" 로 입력 -> 로딩된 창에서 마우스 오른쪽 -> Open Terminal 클릭 
-> lsnrctl start 입력(외부접근을 가능하게 하는 명령어 -> sqlplus /nolog 입력(접속 명령어) -> conn sys/oracle as sysdba 입력(sys = 계정명) -> startup 입력 -> conn hr/hr 입력(hr = 실습용 계정)
```

> rlwrap 설정

> [oracle@itwillbs ~]$ vi .bashrc -> 키보드 i 입력 -> fi 아래에 alias sqlplus='rlwrap sqlplus' 입력 -> esc 키 입력 후 :wq 입력 -> [oracle@itwillbs ~]$ source .bashrc 입력

```
# .bashrc

# Source global definitions
if [ -f /etc/bashrc ]; then
        . /etc/bashrc
fi
alias sqlplus='rlwrap sqlplus'
# Uncomment the following line if you don't like systemctl's auto-paging feature:
# export SYSTEMD_PAGER=

# User specific aliases and functions
```

## Oracle 명령어


### Oracle 테이블 목록 조회하기
- SQL> SELECT * FROM tab;
```
TNAME                          TABTYPE  CLUSTERID
------------------------------ ------- ----------
COUNTRIES                      TABLE
DEPARTMENTS                    TABLE
EMPLOYEES                      TABLE
EMP_DETAILS_VIEW               VIEW
JOBS                           TABLE
JOB_HISTORY                    TABLE
LOCATIONS                      TABLE
REGIONS                        TABLE
```

### 테이블 구조 조회하기
- SQL> DESC departments;
```
 Name                                      Null?    Type
 ----------------------------------------- -------- ----------------------------
 DEPARTMENT_ID                             NOT NULL NUMBER(4)
 DEPARTMENT_NAME                           NOT NULL VARCHAR2(30)
 MANAGER_ID                                         NUMBER(6)
 LOCATION_ID                                        NUMBER(4)
```

### NULL
- SQL> SELECT last_name, 12*salary + 12*salary*commission_pct, commission_pct FROM employees;
```
…
LAST_NAME                 12*SALARY+12*SALARY*COMMISSION_PCT COMMISSION_PCT
------------------------- ---------------------------------- --------------
Livingston                                            120960             .2
Grant                                                  96600            .15
Johnson                                                81840             .1
Taylor
Fleaur
Sullivan
Geoni
Sarchand
Bull
Dellinger
Cabrio
…
```

### column alias
- SQL> SELECT salary, 12*salary "Annual Salary"
  2  FROM employees;

```
    SALARY Annual Salary
---------- -------------
     24000        288000
     17000        204000
     17000        204000
      9000        108000
      6000         72000
      4800         57600
      4800         57600
      4200         50400
     12008        144096
      9000        108000
      8200         98400
```

### 연결연산자(||)
- ORACLE DB에서는 ||이 연결연산자로 잘 동작함.

- SQL> SELECT first_name, last_name, first_name||last_name AS name

```
FIRST_NAME           LAST_NAME                 NAME
-------------------- ------------------------- ---------------------------------------------
Ellen                Abel                      EllenAbel
Sundar               Ande                      SundarAnde
Mozhe                Atkinson                  MozheAtkinson
David                Austin                    DavidAustin
Hermann              Baer                      HermannBaer
Shelli               Baida                     ShelliBaida
Amit                 Banda                     AmitBanda
Elizabeth            Bates                     ElizabethBates
Sarah                Bell                      SarahBell
David                Bernstein                 DavidBernstein
Laura                Bissot                    LauraBissot

```

- SQL> SELECT first_name, last_name, first_name || ' ' || last_name AS name
```
FIRST_NAME           LAST_NAME                 NAME
-------------------- ------------------------- ----------------------------------------------
Ellen                Abel                      Ellen Abel
Sundar               Ande                      Sundar Ande
Mozhe                Atkinson                  Mozhe Atkinson
David                Austin                    David Austin
Hermann              Baer                      Hermann Baer
Shelli               Baida                     Shelli Baida
Amit                 Banda                     Amit Banda
Elizabeth            Bates                     Elizabeth Bates
Sarah                Bell                      Sarah Bell
David                Bernstein                 David Bernstein
Laura                Bissot                    Laura Bissot
```

### SELECT 문법
```
문장기호
|(vertical bar)	: OR, 또는	ex) A | B → A 또는 B
{}(brace)		: 묶음, 범위
[](bracket)		: 생략가능한 옵션
```







