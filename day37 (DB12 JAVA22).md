# [오전수업] DB 12차
> 11차 수업 복습 실시

### 비교연산자
- 컬럼의 값과 조건값을 비교하는 연산을 하는 연산자
```
=	같다
>	크다(초과)
<	작다(미만)
>=	크거나 같다(이상)
<=	작거나 같다(이하)
!=	같지 않다
<>	같지 않다
^=	같지 않다
```

- 사원의 급여(salary)가 3000 이하인 사원을 출력
```

SQL> SELECT last_name, salary
  2  FROM employees
  3  WHERE salary <= 3000;



LAST_NAME                     SALARY
------------------------- ----------
Baida                           2900
Tobias                          2800
Himuro                          2600
Colmenares                      2500
Mikkilineni                     2700
Landry                          2400
Markle                          2200
Atkinson                        2800
Marlow                          2500
Olson                           2100
Rogers                          2900
Gee                             2400
Philtanker                      2200
Seo                             2700
Patel                           2500
Matos                           2600
Vargas                          2500
Sullivan                        2500
Geoni                           2800
Cabrio                          3000
Gates                           2900
Perkins                         2500
Jones                           2800
Feeney                          3000
OConnell                        2600
Grant                           2600
```


- 문자열의 대소관계는 사전순으로 결정
- A에 가까울수록 작은 값, Z로 갈수록 큰 값 
- 문자열도 대소관계가 존재하므로 비교연산에 활용 가능

```

SQL> SELECT employee_id, last_name
  2  FROM employees
  3  WHERE last_name <= 'Baida';


EMPLOYEE_ID LAST_NAME
----------- -------------------------
        174 Abel
        166 Ande
        130 Atkinson
        105 Austin
        204 Baer
        116 Baida
        
        
----------------------------------------------------------

SQL> SELECT employee_id, last_name
  2  FROM employees
  3  WHERE last_name <= 'C';


EMPLOYEE_ID LAST_NAME
----------- -------------------------
        105 Austin
        116 Baida
        129 Bissot
        130 Atkinson
        151 Bernstein
        166 Ande
        167 Banda
        169 Bloom
        172 Bates
        174 Abel
        185 Bull
        192 Bell
        204 Baer
```

- 날짜의 경우 이전 < 이후의 대소관계를 가짐
- 아래의 예문은 03년도 8월 1일 이전 입사자를 조회하는 조건으로 작성
```

SQL> SELECT employee_id, last_name, hire_date
  2  FROM employees
  3  WHERE hire_date < '01-AUG-03';


EMPLOYEE_ID LAST_NAME                 HIRE_DATE
----------- ------------------------- ---------
        100 King                      17-JUN-03
        102 De Haan                   13-JAN-01
        108 Greenberg                 17-AUG-02
        109 Faviet                    16-AUG-02
        114 Raphaely                  07-DEC-02
        115 Khoo                      18-MAY-03
        122 Kaufling                  01-MAY-03
        137 Ladwig                    14-JUL-03
        203 Mavris                    07-JUN-02
        204 Baer                      07-JUN-02
        205 Higgins                   07-JUN-02
        206 Gietz                     07-JUN-02
```

### BETWEEN A AND B
- 행의 값이 특정 구간에 속하는 행을 선택하는 조건
```
- BETWEEN 최소값 AND 최대값
경계값인 최소, 최대값을 포함한 결과를 출력한다.
```

```
SQL> SELECT last_name, salary
  2  FROM employees
  3  WHERE salary BETWEEN 2500 AND 3500;


LAST_NAME                     SALARY
------------------------- ----------
Khoo                            3100
Baida                           2900
Tobias                          2800
Himuro                          2600
Colmenares                      2500
Nayer                           3200
Mikkilineni                     2700
Bissot                          3300
Atkinson                        2800
Marlow                          2500
Mallin                          3300
Rogers                          2900
Stiles                          3200
Seo                             2700
Patel                           2500
Rajs                            3500
Davies                          3100
Matos                           2600
Vargas                          2500
Taylor                          3200
Fleaur                          3100
Sullivan                        2500
Geoni                           2800
Dellinger                       3400
Cabrio                          3000
Gates                           2900
Perkins                         2500
McCain                          3200
Jones                           2800
Walsh                           3100
Feeney                          3000
OConnell                        2600
Grant                           2600
```
- last_name의 값이 'B' 이상 'D' 이하인 행을 출력
```

SQL> SELECT employee_id, last_name
  2  FROM employees
  3  WHERE last_name BETWEEN 'B' AND 'D';


EMPLOYEE_ID LAST_NAME
----------- -------------------------
        110 Chen
        116 Baida
        119 Colmenares
        129 Bissot
        148 Cambrault
        151 Bernstein
        154 Cambrault
        167 Banda
        169 Bloom
        172 Bates
        185 Bull
        187 Cabrio
        188 Chung
        192 Bell
        204 Baer
```
- 고용일(hire_date)이 07년 8월 1일 이상 07년 8월 31일 이하인 값을 가진 행을 출력
```

SQL> SELECT employee_id, hire_date
  2  FROM employees
  3  WHERE hire_date BETWEEN '01-AUG-07' AND '31-AUG-07';


EMPLOYEE_ID HIRE_DATE
----------- ---------
        119 10-AUG-07
```

### IN 연산자(다중행 연산자)
- 다중행 연산자는 여러 조건값을 연산에 사용할 수 있는 문법으로 IN연산자는 조건값들 중 일치한 값을 가진 행을 출력
- '=' 연산자과 유사하지만 '='연산자는 조건값을 하나만 사용 가능
- 데이터타입은 컬럼과 동일하게 조건값을 작성

```

SQL> SELECT employee_id, last_name, salary, manager_id
  2  FROM employees
  3  WHERE manager_id IN (100, 101, 201);


EMPLOYEE_ID LAST_NAME                     SALARY MANAGER_ID
----------- ------------------------- ---------- ----------
        101 Kochhar                        17000        100
        102 De Haan                        17000        100
        114 Raphaely                       11000        100
        120 Weiss                           8000        100
        121 Fripp                           8200        100
        122 Kaufling                        7900        100
        123 Vollman                         6500        100
        124 Mourgos                         5800        100
        145 Russell                        14000        100
        146 Partners                       13500        100
        147 Errazuriz                      12000        100
        148 Cambrault                      11000        100
        149 Zlotkey                        10500        100
        201 Hartstein                      13000        100
        108 Greenberg                      12008        101
        200 Whalen                          4400        101
        203 Mavris                          6500        101
        204 Baer                           10000        101
        205 Higgins                        12008        101
        202 Fay                             6000        201

-------------------------------------------------------------------------------------------

SQL> SELECT employee_id, last_name, salary
  2  FROM employees
  3  WHERE last_name IN ('Greenberg', 'Hartstein', 'Mavris');


EMPLOYEE_ID LAST_NAME                     SALARY
----------- ------------------------- ----------
        108 Greenberg                      12008
        201 Hartstein                      13000
        203 Mavris                          6500

```

### LIKE 연산자
- 패턴문자를 활용하여 패턴과 일치한 값을 가지는 행을 출력하는 연산자
- 패턴문자
  - _	: 한자리의 임의의 문자
  - %	: 0~n자리의 임의의문자

- '_o%' 는 임의의 첫글자, 두번째자리엔 'ㅇ', 이후의 문자는 없어도 되고 여러 임의의 문자가 오는 패턴과 일치한 last_name컬럼의 값을 가진 행 출력

```
SQL> SELECT last_name
  2  FROM employees
  3  WHERE last_name LIKE '_o%';


LAST_NAME
-------------------------
Colmenares
Doran
Fox
Johnson
Jones
Kochhar
Lorentz
Mourgos
Popp
Rogers
Tobias
Vollman



-------------------------------------------------------------------------------------

SQL> SELECT last_name
  2  FROM employees
  3  WHERE last_name LIKE '__p_';


LAST_NAME
-------------------------
Popp
```

- last_name컬럼의 값이 소문자 'r'로 끝나는 행 출력
```

SQL> SELECT last_name
  2  FROM employees
  3  WHERE last_name LIKE '%r';


LAST_NAME
-------------------------
Baer
Dellinger
Fleaur
Kochhar
Kumar
Nayer
Ozer
Philtanker
Taylor
Taylor
Tucker
```

- 숫자값에도 패턴을 적용해서 연산결과를 출력 가능
```

SQL> SELECT employee_id, last_name
  2  FROM employees
  3  WHERE employee_id LIKE '10_';


EMPLOYEE_ID LAST_NAME
----------- -------------------------
        105 Austin
        102 De Haan
        104 Ernst
        109 Faviet
        108 Greenberg
        103 Hunold
        100 King
        101 Kochhar
        107 Lorentz
        106 Pataballa



--------------------------------------------------------------------------------------


SQL> SELECT employee_id, last_name, hire_date
  2  FROM employees
  3  WHERE hire_date LIKE '___AUG___';


EMPLOYEE_ID LAST_NAME                 HIRE_DATE
----------- ------------------------- ---------
        108 Greenberg                 17-AUG-02
        109 Faviet                    16-AUG-02
        119 Colmenares                10-AUG-07
        129 Bissot                    20-AUG-05
        134 Rogers                    26-AUG-06
        152 Hall                      20-AUG-05
        158 McEwen                    01-AUG-04
        189 Dilly                     13-AUG-05
        202 Fay                       17-AUG-05


-------------------------------------------------------------------------------------------

SQL> SELECT employee_id, last_name, hire_date
  2  FROM employees
  3  WHERE hire_date LIKE '%AUG%';


EMPLOYEE_ID LAST_NAME                 HIRE_DATE
----------- ------------------------- ---------
        108 Greenberg                 17-AUG-02
        109 Faviet                    16-AUG-02
        119 Colmenares                10-AUG-07
        129 Bissot                    20-AUG-05
        134 Rogers                    26-AUG-06
        152 Hall                      20-AUG-05
        158 McEwen                    01-AUG-04
        189 Dilly                     13-AUG-05
        202 Fay                       17-AUG-05
```


# [오후수업] JAVA 22차

## 예외 처리에 대한 위임 (= 예외던지기 =throws)

- 예외 발생 시 예외가 발생한 메서드(자신)에서 직접 처리(try ~ catch) 하지 않고 자신의 메서드를 호출한 곳으로 예외 처리를 위임할 수 있음
- 메서드 선언부 마지막에 throws 키워드를 사용하여 발생 가능성이 있는 예외 클래스 이름을 명시하여 해당 예외가 발생했을 때 예외를 던짐(throws)
- 주로, 메서드 내의 특정 코드에서 예외가 발생할 것으로 예상되는 경우 외부에서 해당 메서드를 호출하는 시점에서 미리 알 수 있도록 내부 예외를 외부로 던진 후 외부에서 해당 예외를 탐지하고 처리하도록 할 때 사용
- throws 키워드를 통해 예외를 위임하는 경우 예외가 발생한 위치에서는 예외 처리를 수행하지 않아도 컴파일 에러가 발생하지 않으며, 예외 처리를 위임받은 곳에서 반드시 예외 처리를 하는 코드(try ~ catch)를 기술해야함
  - 만약, 위임받은 예외를 직접 처리하지 않으려면 자신의 메서드에서도 throws 키워드를 통해 호출한 곳으로 위임할 수 있음
- throws 키워드 뒤에 기술되는 예외클래스는 catch 블록과 마찬가지로 각각의 예외클래스를 따로 기술하거나 상위타입(업캐스팅) 예외클래스 하나만으로 대체 가능

> - ex) throws ArithmeticException, NullPointerException 기술 시 두 개의 예외만 외부로 던지고, 나머지 예외는 직접 처리해야한다!
> - ex) throws RuntimeException 기술 시 RuntimeException 계열 예외는 외부로 던지고, 나머지 예외는 직접 처리
> - ex) throws Exception 기술 시 모든 예외를 외부로 던짐
``` 
 < 기본 문법 >
[제한자] 리턴타입 메서드명([매개변수...]) throws 예외클래스1, 예외클래스2 .. {}
```












### 사용자 정의 예외 발생
- 미리 지정된 예외 상황이 아닌 개발자가 지정한 상황에서 직접 예외를 발생시키는 것
- throw 키워드를 사용하여 특정 지점에서 예외를 발생 시킬 수 있다
 - throws 키워드가 사용된 위치의 코드는 예외 발생으로 인한 컴파일 에러가 발생하며 예외 처리를 위해 try ~ catch 문을 사용하거나 throws 키워드로 위임해야함
- 주로, 문법 오류는 아니나 개발자에 의해 논리적인 오류로 취급해야하는 코드에서 해당 코드를 호출하는 곳으로 예외 발생 사실을 알려주기 위해(위임) 사용

```
< 기본 문법 >
예외가 발생해야하는 위치에서 다음 코드를 기술

throw new 예외클래스명("예외 메세지");
=> 예외클래스명에 해당하는 예외 객체가 생성되며 예외가 발생함
   해당 객체의 메세지를 e.getMessage()로 리턴받아 확인하거나 e.printStackTrace() 메서드를 호출하여 확인 가능
```

```java
package try_catch;

public class Ex2 {

	public static void main(String[] args) { 
  
		int score = 185;
		try {
			grade(score);
		} catch (Exception e) {
			// 정상범위가 아닌 점수가 입력되면 Exception 타입 예외가 발생하고
			// 해당 예외가 발생 했을 때 수행할 작업을 catch 블록에서 기술
			System.out.println(e.getMessage() + ": " + score);
			e.printStackTrace();
		}
	}
	
	public static void grade(int score) throws Exception {
		// 실제 점수의 범위가 0 ~ 100 사이이므로 그 외의 점수는 정상적인 입력이 아님
		// 따라서, 별도의 if문 등의 조건식을 통해 정상범위와 비정상범위를 판별해야함
		// (int형 파라미터의 범위가 -21억 ~ +21억 이므로 0 ~ 100 사이가 아니어도 문법 오류 X)
		// => 만약, 외부에서 grade() 메서드를 호출하는 시점에서
		//	  0 ~ 100 사이 범위의 점수가 아닐 때를 대비해서 예외를 처리할 수 있도록
		//	  개발자가 직접 예외를 발생 시킬 수 있다!
	
		if(score < 0 || score > 100) {
//			System.out.println("점수 입력 오류!");
			// 개발자에 의해 직접 예외를 발생 시키려면
			// 예외 발생 코드를 조건문을 통해 판별 후 해당 조건문 내에서
			// throw 키워드를 사용하여 예외클래스 타입 객체를 생성(ne)하여 던져야함(throw)
			// => 예외 클래스 객체의 파라미터에 예외 메세지를 전달 가능
			
			// 예외를 직접 처리하는 경우 (별로 사용하지 않음)
//			try {
//				throw new ArithmeticException("점수 입력 오류!");
//			} catch (ArithmeticException e) {
//				e.printStackTrace();
//			}
//			throw new ArithmeticException("점수 입력 오류!");
			throw new Exception("점수 입력 오류!");
		}
	}

}
```

