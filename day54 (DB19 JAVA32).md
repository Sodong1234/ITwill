# [오전수업] DB 19차
> 18차 수업 복습 실시

```
SELECT TO_CHAR(SYSDATE, 'MON-YYYY-DY-DAY-D')
FROM dual;


TO_CHAR(SYSDATE,'MON-YYYY-DY-DAY-D')
---------------------------------------------------------------------
APR-2022-MON-MONDAY   -2
```

### 시간 관련 형식문자
- Hour	: (12시간)HH/HH12 (24시간) HH24
- Minute	: MI
- Second	: SS
- AM/PM

```
SELECT TO_CHAR(SYSDATE + 5/24, 'HH:MI:SS AM')
FROM dual;

TO_CHAR(SYS
-----------
03:32:04 PM



-----------------------------------------------------------------------



SELECT TO_CHAR(SYSDATE + 5/24, 'HH24:MI:SS AM')
FROM dual;

TO_CHAR(SYS
-----------
15:34:48 PM
```

- 날짜데이터 관련 형식문자가 아닌 일반적인 리터럴 문자열을 출력 결과에 포함하는 경우 형식문자를 묶는 ''와 구분하여 ""로 리터럴 문자를 표현해준다.
```

SELECT TO_CHAR(SYSDATE, 'DD " of " MONTH')
FROM dual;


TO_CHAR(SYSDATE,'DD"OF"MONTH')
--------------------------------------------
11  of  APRIL
```

### 접미어
- 숫자 형식문자의 끝부분에 조합하여 사용하는 형식문자
- sp : 숫자를 영문자로 변환하여 출력
- th : 기수표현 방법에서 서수의 표현방법으로 변환하여 출력
```
SELECT TO_CHAR(hire_date, 'ddspth') 서수_스펠링, TO_CHAR(hire_date, 'dd') 기수, 
TO_CHAR(hire_date, 'ddsp') 기수_스펠링, TO_CHAR(hire_date, 'ddth') 서수
FROM employees;

서수_스펠링   |기수|기수_스펠링|서수  |
--------------+--+------------+----+
seventeenth   |17|seventeen   |17th|
twenty-first  |21|twenty-one  |21st|
thirteenth    |13|thirteen    |13th|
third         |03|three       |03rd|
twenty-first  |21|twenty-one  |21st|
twenty-fifth  |25|twenty-five |25th|
fifth         |05|five        |05th|
seventh       |07|seven       |07th|
seventeenth   |17|seventeen   |17th|
sixteenth     |16|sixteen     |16th|
twenty-eighth |28|twenty-eight|28th|
thirtieth     |30|thirty      |30th|
seventh       |07|seven       |07th|
seventh       |07|seven       |07th|
seventh       |07|seven       |07th|



-----------------------------------------------------------------------------------------------



SELECT TO_CHAR(sysdate, 'YEAR'),
TO_CHAR(sysdate, 'YYYYsp')
FROM dual;

TO_CHAR(SYSDATE,'YEAR')|TO_CHAR(SYSDATE,'YYYYSP')|
-----------------------+-------------------------+
TWENTY TWENTY-TWO      |TWO THOUSAND TWENTY-TWO  |
```

### 접두어 fm
= 숫자 앞에 fm을 붙이게 되면 해당 숫자에 필요없는 자리채우기 0의 값을 제거하여 출력해준다.

```
SELECT last_name, 
TO_CHAR(hire_date, 'fmDD MONTH YYYY') AS HIREDATE
FROM employees;

LAST_NAME                 HIREDATE
------------------------- --------------------------------------------
King                      17 JUNE 2003
Kochhar                   21 SEPTEMBER 2005
De Haan                   13 JANUARY 2001
Hunold                    3 JANUARY 2006
Ernst                     21 MAY 2007
Austin                    25 JUNE 2005
Pataballa                 5 FEBRUARY 2006
Lorentz                   7 FEBRUARY 2007
Greenberg                 17 AUGUST 2002
…



-------------------------------------------------------------------------------------------



SELECT last_name, 
TO_CHAR(hire_date, 'DD MONTH YYYY') AS HIREDATE
FROM employees;

LAST_NAME                 HIREDATE
------------------------- --------------------------------------------
King                      17 JUNE      2003
Kochhar                   21 SEPTEMBER 2005
De Haan                   13 JANUARY   2001
Hunold                    03 JANUARY   2006
Ernst                     21 MAY       2007
Austin                    25 JUNE      2005
Pataballa                 05 FEBRUARY  2006
Lorentz                   07 FEBRUARY  2007
Greenberg                 17 AUGUST    2002
Faviet                    16 AUGUST    2002
Chen                      28 SEPTEMBER 2005
…



------------------------------------------------------------------------------------------------------------



SELECT last_name, to_char(hire_date, 'fmDdspth "of" Month YYYY fmHH:MI:SS AM') AS hiredate
FROM employees;

LAST_NAME                 HIREDATE
------------------------- -----------------------------------------------------------------------
King                      Seventeenth of June 2003 12:00:00 AM
Kochhar                   Twenty-First of September 2005 12:00:00 AM
De Haan                   Thirteenth of January 2001 12:00:00 AM
Hunold                    Third of January 2006 12:00:00 AM
Ernst                     Twenty-First of May 2007 12:00:00 AM
Austin                    Twenty-Fifth of June 2005 12:00:00 AM
Pataballa                 Fifth of February 2006 12:00:00 AM
Lorentz                   Seventh of February 2007 12:00:00 AM
Greenberg                 Seventeenth of August 2002 12:00:00 AM
Faviet                    Sixteenth of August 2002 12:00:00 AM
Chen                      Twenty-Eighth of September 2005 12:00:00 AM
…


SELECT TO_CHAR(SYSDATE, 'MONTH / Month / month') FROM dual;

APRIL     / April     / april    

```

## TO_CHAR(숫자 → 문자)
### 형식문자 '9'
- 임의의 한자리 숫자를 의미
```
SELECT TO_CHAR(12345, '99999')
FROM dual;

TO_CHAR(12345,'99999')|
----------------------+
 12345                |



------------------------------------------------------------------------------------------------



- 출력할 숫자보다 형식문자의 길이가 더 길더라도 특별히 문제는 없음
SELECT TO_CHAR(12345, '99999999')
FROM dual;

TO_CHAR(12345,'99999999')|
-------------------------+
    12345                |



------------------------------------------------------------------------------------------------



- 출력할 숫자의 자리 값보다 형식문자의 길이가 더 짧은 경우 결과는 정상적으로 출력되지 않는다.
SELECT TO_CHAR(12345, '9999')
FROM dual;

TO_CHAR(12345,'9999')|
---------------------+
#####                |
```

### 형식문자 '0'
- 임의의 한자리 숫자를 표현할 수있으나 형식문자의 길이가 출력할 숫자보다 긴 경우 남는 길이를 문자 '0'으로 채워서 출력한다.
- 형식문자 '0'은 하나만 포함되어 있어도 기능을 한다.
```

SELECT TO_CHAR(12345, '000000000')
FROM dual;

TO_CHAR(12345,'000000000')|
--------------------------+
 000012345                |



-----------------------------------------------------------------------------------------------------------



SELECT TO_CHAR(12345, '099999999')
FROM dual;

TO_CHAR(12345,'099999999')|
--------------------------+
 000012345                |

```

### 형식문자 '$'
- '$' 통화기호를 붙인 형태로 값을 변환하고 싶은 경우
- 해당 '$' 기호는 고정적인 자리에 붙지 않고 형식문자 상 순서에 위치
```

SELECT LPAD(CONCAT('$', salary), 8, ' ')
FROM employees;

LPAD(CONCAT('$',SALARY),8,'')|
-----------------------------+
  $24000                     |
  $17000                     |
  $17000                     |
   $9000                     |
   $6000                     |
   $4800                     |
   $4800                     |
   $4200                     |
  $12008                     |
…
SELECT TO_CHAR(salary, '$99999')
FROM employees;



-----------------------------------------------------------------------------------------------



TO_CHAR(SALARY,'$99999')|
------------------------+
 $24000                 |
 $17000                 |
 $17000                 |
  $9000                 |
  $6000                 |
  $4800                 |
  $4800                 |
  $4200                 |
 $12008                 |
  $9000                 |
…
```

### 형식문자 'L'
- 접속 중인 도구의 언어설정 지역 설정에 따라서 출력되는 지역통화기호를 조합하여 출력
```

SELECT TO_CHAR(salary, 'L99999999')
FROM employees;

TO_CHAR(SALARY,'L99999999')|
---------------------------+
           ￦24000          |
           ￦17000          |
           ￦17000          |
            ￦9000          |
            ￦6000          |
            ￦4800          |
            ￦4800          |
            ￦4200          |
           ￦12008          |
            ￦9000          |
            ￦8200          |
…
```

### 형식문자 '.' 
- 형식문자 '.'없이 소수점이 없는 숫자를 문자열로 변환하는 경우 정수 아래 숫자들은 반올림되어 처리 됨.
```

SELECT TO_CHAR(123.46, '99999')
FROM dual;

TO_CHAR(123.46,'99999')|
-----------------------+
   123                 |



------------------------------------------------------------------------------------------------



SELECT TO_CHAR(123.46, '999.99')
FROM dual;

TO_CHAR(123.46,'999.99')|
------------------------+
 123.46                 |



------------------------------------------------------------------------------------------------



SELECT TO_CHAR(123.46, '9999.999')
FROM dual;

TO_CHAR(123.46,'9999.999')|
--------------------------+
  123.460                 |

```

---

# [오후수업] JAVA 32차
