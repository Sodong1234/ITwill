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


### Thread 상태 제어(sleep() 메서드를 통한 쓰레드 일시정지)
- Thread.sleep() 메서드를 호출하여 일시 정지 가능
  - 현재 수행중인 쓰레드를 지정된 시간동안 재우기
- 지정 가능한 시간은 밀리초(ms) 단위(1ms = 1/1000초, 1000ms = 1초) 
- 지정한 시간만큼만 정확히 소요되는 것은 아니고 자원 회수 및 할당 등으로 인해 약간의 시간차가 발생할 수 있다!
- 주로 특정 쓰레드에 대한 타이머 기능을 부여하기 위한 용도로 사용하거나 쓰레드간의 우선순위로 인한 기아(Starvation) 상태를 방지하는 용도로 사용됨

```java
package thread;

import java.time.LocalTime;

public class Ex1 {

	public static void main(String[] args) {
		
		Thread timer = new Thread(new Runnable() {
			
			@Override
			public void run() {
				for(int i = 1; i <= 60; i++) {
					try {
						Thread.sleep(1000);
					} catch (InterruptedException e) {
						e.printStackTrace();
					}
					System.out.println(LocalTime.now());
				}
				
			}
		}); 
		
		timer.start();

	}

}
```

### 쓰레드 우선순위(Priority)
- 실행중인 쓰레드가 우선적으로 실행되도록 고정 가능한 값
- 1 ~ 10 까지의 값을 부여 가능하며, 기본값은 5이다.
- 쓰레드 우선순위가 높을수록 실행될 수 있는 확률이 높으나 절대적으로 적용하지는 않음
  - 어디까지나 CPU 스케쥴에 따라 실행되며, 자주 실행될 수 있는 "확률"을 높임
- 현재 우선순위 확인 : int getPriority()
- 현재 우선순위 변경 : void setPriority(int priority)

```java
package thread;

public class Ex2 {

	public static void main(String[] args) {

		Thread t1 = new Thread(new Runnable() {
			
			@Override
			public void run() {
				for(int i = 1; i <= 100000; i++) {
					System.out.println("*************");
				}
			}
		});
		
		// 현재 우선순위 확인 : getPriority()
		System.out.println("t1 쓰레드 우선순위 : " + t1.getPriority());
		
		// 현재 쓰레드 우선순위 변경 : setPriority()
		t1.setPriority(8);	// 현재 우선순위를 8로 변경
		System.out.println("t1 쓰레드 우선순위 : " + t1.getPriority());

		t1.setPriority(Thread.MAX_PRIORITY);
		System.out.println("t1 쓰레드 우선순위 : " + t1.getPriority());


		Thread t2 = new Thread(new Runnable() {
			
			@Override
			public void run() {
				for(int i = 1; i <= 100000; i++) {
					System.out.println("=============");
				}
			}
		});
		

		Thread t3 = new Thread(new Runnable() {
			
			@Override
			public void run() {
				for(int i = 1; i <= 100000; i++) {
					System.out.println("○○○○○○○○○○○○○");
				}
			}
		});
	
		
		// t1, t2, t3 쓰데르의 우선순위를 서로 다르게 변경
		t1.setPriority(Thread.NORM_PRIORITY);	// *************
		t2.setPriority(Thread.MAX_PRIORITY);	// =============
		t3.setPriority(Thread.MIN_PRIORITY);	// ○○○○○○○○○○○○○

		
		
		t1.start();
		t2.start();
		t3.start();
	}

}
```

### 멀티쓰레딩 환경에서의 문제점(주의할 점!)
- 복수개의 쓰레드에서 동일한 객체의 데이터에 접근할 경우 각 쓰레드에서 사용되는 데이터의 일관성이 깨질 수 있다!
  - A라는 쓰레드에서 접근해서 사용하는 데이터를 B라는 쓰레드에서 동시에 접근해서 변경할 경우 올바른 데이터가 아니게 될 수 있음(= 데이터 일관성이 깨졌다!)
- 공유 데이터에 대한 일관성 문제를 해결하기 위해서 Lock 개념과 동기화(Synchronize) 기능을 사용
  - 메서드 또는 특정 코드 블럭에 synchronized 키워드를 사용하여 동기화 적용

```java
package thread;

public class Ex3 {

	public static void main(String[] args) {
		
		// 은행계좌 객체 1개 생성
		Account account = new Account(100000);
		
		// 출금 쓰레드를 수행할 객체 2개 생성
		WithdrawThread iBanking = new WithdrawThread("인터넷뱅킹", account, 2000);
		WithdrawThread mBanking = new WithdrawThread("모바일뱅킹", account, 2000);
		
		iBanking.start();
		mBanking.start();
		

		
	}

}

class Account {
	int balance;

	public Account(int balance) {
		this.balance = balance;
	}
	
	// 입금 : deposit()
	// => 입력받은 금액(amount)을 현재잔고(balance)에 누적 후 현재잔고 리턴
	public int deposit(int amount) {
		balance += amount;
		return balance;
	}
	
	// 출금 : withdraw()
	// => 출금할 금액(amount)을 전달받아 현재잔고(balance)에서 차감 후 출금금액 리턴
	//	  단, 현재잔고가 출금할 금액보다 크거나 같을 경우에만 출금을 수행하고 현재잔고 출력
	//	  아니면 "잔액이 부족하여 출금할 수 없습니다!" 출력
	
	public synchronized int withdraw(int amount) {
		if(balance >= amount) { // 출금 O
			balance -= amount;
		} else { // 출금 X
			System.out.println("잔액이 부족하여 출금할 수 없습니다!");
			amount = 0;
		}
		System.out.println("출금된 금액 : " + amount + ", 출금 후 잔액 : " + balance);
		return amount;
	}
}

class WithdrawThread extends Thread {
	Account account;
	int amount;
	
	public WithdrawThread(String threadName, Account account, int amount) {
		super(threadName);
		this.account = account;
		this.amount = amount;
	}
	
	@Override
	public void run() {
		for(int i = 1; i <= 6; i++) {
			// 현재 쓰레드를 0.5초 일시정지
			try {
				Thread.sleep(500);
			} catch (InterruptedException e) {
				e.printStackTrace();
			}
			int money = account.withdraw(amount);
		}
	}
	
	
}
```
## 람다식(Lambda Expressions)
- 자바 8에 추가된 가장 큰 특징
- "함수형" 프로그래밍 형태를 받아들인 결과

함수형 프로그래밍이란? (함수형 vs 객체지향)
- 함수형 : 1950년대, 객체지향 : 1990년대 (역사가 더 오래되긴 함)
- 기능 위주의 프로그래밍 기법
- 객체지향 : 클래스에 속성과 기능을 정의 / 함수형 : 기능. 즉, 함수가 따로 존재
```
< 기본 문법 >
(데이터타입 매개변수, ...) -> { 실행문... }

1. 기본형
(int x) -> { System.out.println(x); }

2. 매개변수의 타입을 추론 할 수 있는 경우에는 타입 생략 가능
(x) -> { System.out.println(x); }

3. 매개변수나 실행문이 하나라면 소괄호()와 중괄호{}를 생략 할 수 있다.
	  (이 때, 세미콜론(;)은 생략!)
x -> System.out.println(x)

4. 매개변수가 없을 경우에는 소괄호()를 사용한다. (생략불가)
() -> System.out.println(x)

5. 리턴이 필요한 경우 return 키워드를 사용
(x, y) -> { return x + y; }

6. 실행문이 단순히 return 문 하나로 표현되는 경우
	  표현식만 사용 할 수 있으며, 이 때 리턴값은 표현식의 결과값이 된다
	  (주의! 이 때, 세미콜론은 붙이지 않는다!)
(x, y) -> x + y
```

```java
package Lambda;

import java.util.Arrays;
import java.util.Comparator;

public class Ex1 {

	public static void main(String[] args) {
  
		String[] str = {"this", "is", "java", "world"};
		Integer[] arr = {10, 20, 22, 78, 12, 55};
		
		System.out.println(Arrays.toString(str));
		Arrays.sort(str); // 오름차순으로 정렬 후 : [is, java, this, world]
		System.out.println(Arrays.toString(str));
		
		/*
		 * 새로운 정렬기능은 만들려면?
		 * => 내부적으로 Comparator의 compare 메서드를 사용하기 때문에
		 * 	  새로운 기능을 만들고 Arrays.sort() 에 전달
		 * 	  => 자바에서는 함수만 전달할 방법 X
		 * 	     따라서, 해당 기능을 가지는 객체를 전달해야한다.
		 * 		 일회용으로 정렬 기능을 작성하려면? (익명의 내부 클래스)
		 */
		
//		Arrays.sort(str, new Comparator<String>() {
//			
//			@Override
//			public int compare(String o1, String o2) {
//				// -1을 곱해서 내림차순으로 정렬
//				return o1.compareTo(o2) * -1;
//			}
//		});
		
		Arrays.sort(str, (o1, o2) -> o1.compareTo(o2) * -1);
		Arrays.sort(arr, (o1, o2) -> o1.compareTo(o2) * -1);

		
		
		// 내림차순 정렬 후 :
		System.out.println(Arrays.toString(str));
		System.out.println(Arrays.toString(arr));
		
		/*
		 * - 결국 정렬을 위해 필요했던 '기능' 은 Comparator가 아니라
		 * 	 사실 compare() 라는 메서드!
		 * - compare() 메서드만 있으면 되지만, 자바언어의 특성으로 인해
		 * 	 익명의 내부클래스를 만들고 객체화해서 전달하고 있다!
		 * - 이러한 번거로움을 없애기 위해 람다식이 등장!
		 * 
		 */

		System.out.println("--------------------------------------------");
		Arrays.sort(str, (String o1, String o2) -> { return o1.compareTo(o2) * -1; });
		Arrays.sort(str, (o1, o2) -> { return o1.compareTo(o2) * -1; });
		Arrays.sort(str, (o1, o2) -> o1.compareTo(o2) * -1);
		
	}

}
```
```java
package Lambda;

// 1. 파라미터와 리턴타입 없는 경우 (파라미터 : X, 리턴타입 : X)
@FunctionalInterface
interface MyFunc1 {
	// 함수형 인터페이스 어노테이션(@FunctionalInterface) 선언 시 에러 발생!
	// => 함수형 인터페이스는 반드시 하나의 추상메서드(abstract method)를 가져야함
	public void methodA();
//	public void methodB();
//	public void methodC();
}

// 2. 파라미터가 있는 람다식 (파라미터 : O)
@FunctionalInterface
interface MyFunc2 {
	void methodB(String msg);
}

// 3. 리턴타입이 있는 람다식 (파라미터 : O, 리턴타입 : O)
@FunctionalInterface
interface MyFunc3 {
	String methodC(String msg);
}

public class Ex2 {

	// 1. 파라미터와 리턴타입 없는 경우 (파라미터 : X, 리턴타입 : X)
	public static void useFIMethodA(MyFunc1 fi) {
		fi.methodA();
	}
	
	// 2. 파라미터가 있는 람다식 (파라미터 : O)
	public static void useFIMethodB(MyFunc2 fi) {
		fi.methodB("홍길동");
	}

	// 3. 리턴타입이 있는 람다식 (파라미터 : O, 리턴타입 : O)
	public static String useFIMethodC(MyFunc3 fi) {
		return fi.methodC("홍길동");
	}

	
	public static void main(String[] args) {
		
		/*
		 * 함수형 인터페이스(functional interface) 또는 타겟타입(target type)
		 * - 람다식은 결과적으로 "인터페이스 타입의 클래스를 손쉽게 구현하는 방법"
		 * - 반드시 하나의 abstract 메서드만 존재
		 * - 만약, abstract 메서드가 없거나 두 개 이상 존재한다면 람다식으로 대체 할 수 없다!
		 * - 함수형 인터페이스 @FunctionalInterface 어노테이션 선언 
		 */
		
		useFIMethodA(new MyFunc1() {
			
			@Override
			public void methodA() {
				System.out.println("익명 내부클래스 형태");
			}
		});
		
		System.out.println("--------------------------------");
		
		
		// 1. 파라미터와 리턴타입 없는 경우 (파라미터 : X, 리턴타입 : X)
		System.out.println("(파라미터 : X, 리턴타입 : X)");
		
		// 표헌방식1
		useFIMethodA(() -> {
			System.out.println("람다식1");
		});
		
		// 표현방식2
		useFIMethodA(() -> System.out.println("람다식2"));

		System.out.println("------------------------------------");
		
		// 2. 파라미터가 있는 람다식 (파라미터 : O)
		System.out.println("(파라미터 : O)");
		
		// 표현방식1
		useFIMethodB((String msg) -> {
			System.out.println("람다식1 : " + msg);
		});
		
		// 표현방식2
		useFIMethodB(msg -> System.out.println("람다식2 : " + msg));
		
		System.out.println("------------------------------------");

		// 3. 리턴타입이 있는 람다식 (파라미터 : O, 리턴타입 : O)
		System.out.println("(파라미터 : O , 리턴타입 : O");

		// 표현방식1
		String result = useFIMethodC((String msg) -> {
			return "람다식1 : " + msg;
 		});
		System.out.println(result);
		
		// 표현방식2
		System.out.println(useFIMethodC(msg -> "람다식2 : " + msg));
		
	}

}
```

### 람다의 실행블록에서 변수 참조 (this)
- 람다식은 컴파일러에 의해 익명의 내부클래스로 처리되기 때문에 변수에 대한 참조 규칙이 동일하다
- 외부클래스에서 멤버를 자유롭게 사용 할 수 있다.
- 자바8부터는 일반 로컬 변수도 사용 할 수 있는데 final 키워드가 적용된 것처럼 새로운 값을 할당 할 수는 없다!
 
- 하지만 this를 사용하는 방법이 약간 다르다
  - 람다식에서 this는 타겟 인터페이스가 아닌 외부클래스를 나타낸다.
	- 따라서, 람다식 내에서 this와 외부클래스.this 는 동일한 객체를 나타낸다.

```java
package Lambda;

@FunctionalInterface
interface MyFuncInterface {
	int interfaceMember = 20;
	String method(String msg);
}

class VariableUseTest {
	private int memberVar = 10;
	
	public void useFiMethod(MyFuncInterface mi) {
		System.out.println(mi.method("홍길동"));
	}
	
	public void lambdaTest() {
		int localVar = 20;
		
		/*
		 * 출력결과 비교
		 * - 람다식 내부에서 this와 외부클래스.this는 같음을 알 수 있다.
		 * - 람다식에서의 this는 마치 해당되는 클래스(여기서는 VariableUseTest) 처럼 접근됨
		 * - 익명의 내부클래스에서의 this는 해당되는 인터페이스(여기서는 MyFuncInterface) 처럼 접근됨
		 * 
		 */
		useFiMethod((String msg) -> {
//			this.memberVar // 접근가능(O)
//			memberVar	// 접근가능(O)
			
//			interfaceMember	// 접근불가(X)
//			this.interfaceMember // 접근불가(X)
			
			System.out.println("this : " + this);
			System.out.println("외부클래스.this : " + VariableUseTest.this);
			System.out.println("변수 참조 : " + localVar + " : " + memberVar);
//			localVar++; // 로컬변수 수정은 불가!
			
			return "람다식 : " + msg + ", " + (++memberVar);
 		});
		
		System.out.println("------------------------------------");
		
		useFiMethod(new MyFuncInterface() {

			@Override
			public String method(String msg) {
//				this.memberVar // 접근불가(X)
//				memberVar	// 접근가능(O)
				
//				interfaceMember // 접근가능(O)
//				this.interfaceMember // 접근가능(O)
				
				System.out.println("this : " + this);
				System.out.println("외부클래스.this : " + VariableUseTest.this);
				System.out.println("변수 참조 : " + localVar + " : " + memberVar);
//				localVar++; // 로컬 변수 수정은 불가!
				
				return "익명의 내부클래스 : " + msg  + ", " + (++memberVar);
			}
			
		});
	}
}


public class Ex3 {

	public static void main(String[] args) {
		VariableUseTest vut = new VariableUseTest();
		vut.lambdaTest();
	}

}
```

## 자바에서 제공되는 함수형 인터페이스 (java.util.function 패키지)
- 자바에서 제공되는 함수형 인터페이스

```
- java.util.function 패키지

               파라미터 리턴
Consumer 계열     O      X
Supplier 계열     X      O
Function 계열     O      O
Operation계열     O      O
Predicate계열     O   boolean
```

### 1. Consumer 계열 (소비자)
- 파라미터를 받아서 소비
- 리턴 하지 않는다

```java
package Lambda;

import java.util.function.Consumer;

class Student {
	String name;
	double score;
	
	// Alt + Shift + S -> O
	public Student(String name, double score) {
		this.name = name;
		this.score = score;
	}
}

public class Ex4 {
	
	// 학생을 출력할 것이다. (아직 방식은 미정)
	// 출력방식을 전달 받는다. (Consumer<Student> printer) {
	public static void printStudent(Student s, Consumer<Student> printer) {
		printer.accept(s);
	}

	public static void main(String[] args) {

		
		// 익명의 내부클래스 방식
		Consumer<String> consumer = new Consumer<String>() {
			
			@Override
			public void accept(String t) {
				System.out.println(t);
			}
		};
		consumer.accept("Hello");
		System.out.println("-----------------------------");
		// 람다식 형태 consumer2 
//		Consumer<String> consumer2 = (String t) -> {System.out.println(t);};
		Consumer<String> consumer2 = t -> System.out.println(t);
		consumer2.accept("Hello2");
		
		System.out.println("---------------------------");
		
		Student s1 = new Student("홍길동", 80);
		
		// 한줄씩 표현
		printStudent(s1, s -> System.out.println(s.name));
		printStudent(s1, s -> s.score *= 1.5);
		printStudent(s1, s -> System.out.println(s.name + ": " + s.score));
		
		// 여러줄 표현
		printStudent(s1, s -> {
			System.out.println(s.name);
			s.score *= 1.5;
			System.out.println(s.name + ", " + s.score);
		});
		
		
	}

}
```
