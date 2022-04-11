# [오전수업] DB 18차
## 날짜 연산 함수
- 날짜를 활용한 단순 연산으로는 월 관련 연산이 어려운 부분이 있어 이를 보조해주는 함수들이 제공

### MONTHS_BETWEEN 함수
- 입력한 두 날짜간 차이를 월단위로 계산한 결과를 출력
- 첫번째 인수의 날짜에서 두번재 인수의 값을 빼는 연산으로 결과를 계산하므로 대소관계를 고려하여 값을 입력해준다.

```
* sql*plus, sql Developer에서 사용
SELECT MONTHS_BETWEEN('01-SEP-95', '11-JAN-94')
FROM dual;


* DBeaver의 날짜 양식
SELECT MONTHS_BETWEEN('95-09-01', '94-01-11')
FROM dual;

MONTHS_BETWEEN('95-09-01','94-01-11')    |
-----------------------------------------+
19.67741935483870967741935483870967741935|


--------------------------------------------------------------------------


작은 날짜값이 앞에 있으므로 결과는 음수가 출력된다.
SELECT MONTHS_BETWEEN('94-01-11', '95-09-01')
FROM dual;

MONTHS_BETWEEN('94-01-11','95-09-01')     |
------------------------------------------+
-19.67741935483870967741935483870967741935|
```

### ADD_MONTHS 함수
- 날짜 데이터에 월단위로 이전/이후 날짜를 계산하는 함수
- TO_DATE는 문자열을 날짜데이터로 변환하는 변환함수
```

SELECT TO_DATE('1996-01-31', 'YYYY-MM-DD') + 30, ADD_MONTHS('96-01-31', 1)
FROM dual;

TO_DATE('1996-01-31','YYYY-MM-DD')+30|ADD_MONTHS('96-01-31',1)|
-------------------------------------+------------------------+
              1996-03-01 00:00:00.000| 1996-02-29 00:00:00.000|


--------------------------------------------------------------------------------------------



두번째 인수값을 양수로 작성하면 이후 날짜, 음수로 작성하면 이전 날짜가 연산된다.
SELECT ADD_MONTHS('96-01-31', -11)
FROM dual;

ADD_MONTHS('96-01-31',-11)|
--------------------------+
   1995-02-28 00:00:00.000|
```

### NEXT_DAY 함수
- 특정 날짜 다음 오는 특정 요일 연산하는 함수
```
sql*plus, sql Developer 기준
SELECT NEXT_DAY('01-SEP-95', 'FRIDAY')
FROM dual;


DBeaver 기준
요일은 일요일을 일주일의 첫날로 1로 사용한다. (화 - 2, 수 - 3, …)
SELECT NEXT_DAY('95-09-01', 6)
FROM dual;

NEXT_DAY('95-09-01',6) |
-----------------------+
1995-09-08 00:00:00.000|



-----------------------------------------------------------------------------------



SELECT NEXT_DAY('22-04-07', 6)
FROM dual;

NEXT_DAY('22-04-07',6) |
-----------------------+
2022-04-08 00:00:00.000|
```

### LAST_DAY함수
- 입력한 날짜가 속한 월의 마지막 날을 돌려주는 함수
```

SELECT LAST_DAY('95-02-01')
FROM dual;

LAST_DAY('95-02-01')   |
-----------------------+
1995-02-28 00:00:00.000|
```

### ROUND/TRUNC 함수
- 날짜 데이터에 대한 반올림/버림 연산을 하는 함수
- 숫자의 반올림/버림 연산을 하는 함수와 이름이 같으나 입력되는 데이터가 다르므로 구분 가능
- 날짜 데이터의 요소 중 반올림/버림연산을 할 수 있는 단위로 'YEAR', 'MONTH'를 사용할 수 있음.
```
'MONTH'단위로 반올림하는 경우 월보다 작은 단위인 일/시/분/초/…를 고려하여 절반여부에 따라 올림 또는 버림을 연산하게 된다.
SELECT SYSDATE, ROUND(SYSDATE, 'MONTH')
FROM dual;

SYSDATE                |ROUND(SYSDATE,'MONTH') |
-----------------------+-----------------------+
2022-04-07 10:31:49.000|2022-04-01 00:00:00.000|


----------------------------------------------------------------------------------


SELECT TO_DATE('2022-04-16','YYYY-MM-DD'), 
ROUND(TO_DATE('2022-04-16','YYYY-MM-DD'), 'MONTH') DD
FROM dual;

TO_DATE('2022-04-16','YYYY-MM-DD')|DD                     |
----------------------------------+-----------------------+
           2022-04-16 00:00:00.000|2022-05-01 00:00:00.000|



----------------------------------------------------------------------------------


SELECT SYSDATE, ROUND(SYSDATE, 'YEAR')
FROM dual;

SYSDATE                |ROUND(SYSDATE,'YEAR')  |
-----------------------+-----------------------+
2022-04-07 10:44:07.000|2022-01-01 00:00:00.000|



------------------------------------------------------------------------------------




반올림 단위를 'YEAR'로 하는 경우 년도 단위보다 작은 월/일/시/분/초/…의 값을 고려하여 절반여부에 따라 올림 또는 버림 연산을 하게된다.

SELECT SYSDATE, 
ROUND(TO_DATE('2022-06-25', 'YYYY-MM-DD'), 'YEAR') ROUND
FROM dual;

SELECT TO_DATE('2022-07-05', 'YYYY-MM-DD'), 
ROUND(TO_DATE('2022-07-05', 'YYYY-MM-DD'), 'YEAR') ROUND
FROM dual;



TO_DATE('2022-07-05','YYYY-MM-DD')|ROUND                  |
----------------------------------+-----------------------+
           2022-07-05 00:00:00.000|2023-01-01 00:00:00.000|



-----------------------------------------------------------------------------------------



TRUNC 함수는 입력한 날짜에 대해서 연산 단위보다 작은 요소값들을 가장 작은값으로 연산한 결과를 출력한다.
SELECT TO_DATE('2022-07-05', 'YYYY-MM-DD'), 
TRUNC(TO_DATE('2022-07-05', 'YYYY-MM-DD'), 'YEAR') trunc
FROM dual;


TO_DATE('2022-07-05','YYYY-MM-DD')|TRUNC                  |
----------------------------------+-----------------------+
           2022-07-05 00:00:00.000|2022-01-01 00:00:00.000|


```
```
SELECT employee_id, hire_date, 
MONTHS_BETWEEN(SYSDATE, hire_date) TENURE,
ADD_MONTHS(hire_date, 6) REVIEW,
NEXT_DAY(hire_date, 6), LAST_DAY(hire_date)
FROM employees
WHERE MONTHS_BETWEEN(SYSDATE, hire_date) < 170;


EMPLOYEE_ID|HIRE_DATE              |TENURE                                  |REVIEW                 |NEXT_DAY(HIRE_DATE,6)  |LAST_DAY(HIRE_DATE)    |
-----------+-----------------------+----------------------------------------+-----------------------+-----------------------+-----------------------+
        128|2008-03-08 00:00:00.000|168.982735588410991636798088410991636798|2008-09-08 00:00:00.000|2008-03-14 00:00:00.000|2008-03-31 00:00:00.000|
        165|2008-02-23 00:00:00.000|169.498864620669056152927120669056152927|2008-08-23 00:00:00.000|2008-02-29 00:00:00.000|2008-02-29 00:00:00.000|
        166|2008-03-24 00:00:00.000|168.466606556152927120669056152927120669|2008-09-24 00:00:00.000|2008-03-28 00:00:00.000|2008-03-31 00:00:00.000|
        167|2008-04-21 00:00:00.000|167.563380749701314217443249701314217443|2008-10-21 00:00:00.000|2008-04-25 00:00:00.000|2008-04-30 00:00:00.000|
        173|2008-04-21 00:00:00.000|167.563380749701314217443249701314217443|2008-10-21 00:00:00.000|2008-04-25 00:00:00.000|2008-04-30 00:00:00.000|




-------------------------------------------------------------------------------------------------------------------------------------------------------




SELECT employee_id, hire_date, 
ROUND(hire_date, 'MONTH'), 
TRUNC(hire_date, 'MONTH')
FROM employees
WHERE hire_date LIKE '%04';

EMPLOYEE_ID|HIRE_DATE              |ROUND(HIRE_DATE,'MONTH')|TRUNC(HIRE_DATE,'MONTH')|
-----------+-----------------------+------------------------+------------------------+
        157|2004-03-04 00:00:00.000| 2004-03-01 00:00:00.000| 2004-03-01 00:00:00.000|
        179|2008-01-04 00:00:00.000| 2008-01-01 00:00:00.000| 2008-01-01 00:00:00.000|
        192|2004-02-04 00:00:00.000| 2004-02-01 00:00:00.000| 2004-02-01 00:00:00.000|

```



연습문제
```
입사일(hire_date)로부터 6개월 뒤 다음 오는 첫 주 금요일은 몇 일인가?


SELECT hire_date, 
NEXT_DAY(LAST_DAY(ADD_MONTHS(hire_date, 6)), 6) review_date
FROM employees
WHERE department_id = 90;

HIRE_DATE              |REVIEW_DATE            |
-----------------------+-----------------------+
2003-06-17 00:00:00.000|2004-01-02 00:00:00.000|
2005-09-21 00:00:00.000|2006-04-07 00:00:00.000|
2001-01-13 00:00:00.000|2001-08-03 00:00:00.000|

```

## 변환함수
- 숫자, 문자, 날짜 데이터 타입간 변환을 할 수 있는 함수
- 모든 데이터를 무조건 변환을 지원하지는 않는다.
- 변환할 수 있는 형식을 갖춘 데이터에 대해서 변환이 가능
- 변환함수를 사용해서 데이터타입을 변환하는 방식을 명시적 형변환.
- 변환함수 없이 데이터베이스가 자동으로 인식해서 형변환하는 방식은 암시적 형변환

![unnamed](https://user-images.githubusercontent.com/95197594/162115633-dd28b738-bb1d-434b-9246-cc4bbb6c764b.png)


### TO_CHAR(날짜 → 문자)
- 날짜 데이터가 가지고 있는 요소(년,월,일,시,…)들을 원하는 형식의 문자열로 변환하여 출력하는 함수
- TO_CHAR(날짜데이터, '형식문자')
```
SELECT employee_id, hire_date,
TO_CHAR(hire_date, 'MM/YY') Month_Hired
FROM employees
WHERE last_name = 'Higgins';

EMPLOYEE_ID|HIRE_DATE              |MONTH_HIRED|
-----------+-----------------------+-----------+
        205|2002-06-07 00:00:00.000|06/02      |
```

### TO_CHAR 형식문자
- 온전한 단어형태의 형식문자
> - YEAR	년도의 영문 스펠링형태
> - MONTH	월의 영문 스펠링형태
> - DAY	요일의 영문 스펠링형태
```
SELECT TO_CHAR(SYSDATE, 'YEAR / MONTH / DAY')
FROM dual;


TO_CHAR(SYSDATE,'YEAR/MONTH/DAY')
--------------------------------------------------------------------------------
TWENTY TWENTY-TWO / APRIL     / THURSDAY
```

- 요소를 3자리의 약어형태로 출력하는 형식
> - MON	월을 3자리의 약어로 표현
> - DY	요일을 3자리의 약어로 표현
```
SELECT TO_CHAR(SYSDATE, 'MON / DY')
FROM dual;


TO_CHAR(SYSDATE,'MON/DY')
---------------------------
APR / THU
```

- 요소 단위의 첫 글자가 반복되는 형식
> - YYYY	년도를 4자리의 숫자로 표현
> - YY	년도를 2자리의 숫자로 표현
> - MM	월을 2자리의 숫자로 표현
> - DD	일을 2자리의 숫자로 표현
> - D	요일을 1자리의 숫자로 표현 (1~7 → 일~토)
```
SELECT TO_CHAR(SYSDATE, 'YYYY / MM / DD')
FROM dual;


TO_CHAR(SYSDAT
--------------
2022 / 04 / 07

SQL> SELECT TO_CHAR(SYSDATE, 'YY / MM / DD')
  2  FROM dual;


--------------------------------------------------------------------


TO_CHAR(SYSD
------------
22 / 04 / 07

SELECT TO_CHAR(SYSDATE, 'YY / MM / D')
FROM dual;


---------------------------------------------------------------------


TO_CHAR(SYS
-----------
22 / 04 / 5
```



---

# [오후수업] JAVA 31차

## Thread
- 프로그램(Program)
  - 디스크에 설치되어 있는 실행되기 전 상태의 소프트웨어

- 프로세스(Process)
  - 프로그램을 실행하여 메모리에 로딩된 상태(= 실행 중인 프로그램)
  - 멀티태스킹(Multi Tasking)
	  - 프로세스가 여러개일 때 해당 프로세스들이 동시에 수행되는 것 (정확히는 CPU가 빠른 속도로 프로세스들을 번갈아가며 처리함 = 진짜 동시 x)
	    - 동영상을 재생하면서 웹페이지를 표시하고, 자바 코드를 실행하는 것

쓰레드(Thread)
- 프로세스 내에서 작업의 최소 단위
- 하나의 프로세스 내에 쓰레드가 한 개(= Single Thread)일 때 해당 프로세스 내에서 동시에 수행 가능한 작업은 단 하나 뿐이다.
- 하나의 프로세스 내에서 동시에 수행 가능한 작업을 늘리려면 멀티 쓰레딩(= Multi Thread)을 구현 해야한다!
  - (ex. 메신저에서 파일 전송과 함께 메세지 송신, 수신을 동시에 수행하는 것)

- 일반적인 프로그래밍은 싱글 쓰레드(Single Thread)로 동작하므로 하나의 작업이 끝난 후에야 그 다음 작업이 수행 될 수 있다!

```java
package thread;

public class Ex1 {

	public static void main(String[] args) {

		NoThread nt1 = new NoThread("★★A작업★★", 1000000);
		NoThread nt2 = new NoThread("○○B작업○○", 1000000);
		NoThread nt3 = new NoThread("◈◈C작업◈◈", 1000000);
		
		nt1.run();	// A 작업이 100만번 수행이 끝나면
		nt2.run();	// B 작업이 실행되고, B 작업이 100만번 수행이 끝나면
		nt3.run();	// C 작업이 실행됨
		// => 즉, 기본적인 프로그램은 앞의 코드가 실행이 끝나야만 다음 코드가 실행된다!


		
		
	}

}

class NoThread {
	String str;
	int count;
	public NoThread(String str, int count) {
		this.str = str;
		this.count = count;
	}
	
	public void run() {
		// count 횟수만큼 str 반복 출력
		for(int i = 1; i <= count; i++) {
			System.out.println(i + " : " + str);
			
		}
	}
	
}
```

### 멀티 쓰레딩(Multi Threading)
- 하나의 프로세스 내에서 두가지 이상의 작업을 동시에 처리하는 것
- 실제로 두 가지 이상의 작업을 동시에 수행하는 것이 아니며 CPU가 빠른 속도로 여러 작업을 번갈아가며 수행하기 때문에 동시에 수행되는 것처럼 느껴짐 = Round Robin 방식 이라고 함
- 멀티쓰레딩으로 처리되는 작업 순서는 고정이 아닌 계속 바뀌므로 항상 실행결과가 달라질 수 있다!
  
< 멀티 쓰레딩 구현 방법 >
1. Thread 클래스를 상속 받는 서브클래스를 정의하는 방법
- 1) 멀티쓰레딩 코드가 포함될 서브클래스에서 Thread 클래스를 상속
- 2) Thread 클래스의 run() 메서드를 오버라이딩하여 멀티쓰레딩으로 처리할 코드를 기술
- 3) 멀티쓰레딩 클래스 인스턴스 생성
- 4) *** start() 메서드를 호출하여 멀티쓰레딩 시작 ***
  - 주의! run() 메서드 호출이 아닌 start() 메서드를 호출해야한다! (run() 메서드 호출 시 멀티 쓰레딩이 아닌 싱글 쓰레딩으로 동작함)

```java
package thread;

public class Ex2 {

	public static void main(String[] args) {
		
		MyThread mt1 = new MyThread("★★A작업★★", 1000000);
		MyThread mt2 = new MyThread("★★B작업★★", 1000000);
		MyThread mt3 = new MyThread("★★C작업★★", 1000000);

		// run() 메서드를 호출하면 멀티쓰레딩이 아닌 싱글쓰레딩으로 처리되므로 주의!
//		mt1.run();
//		mt2.run();
//		mt3.run();
		
		mt1.start();
		mt2.start();
		mt3.start();
	}

}

class MyThread extends Thread {

	String str;
	int count;
	
	public MyThread(String str, int count) {
		this.str = str;
		this.count = count;
	}

	// 오버라이딩 단축키 : Alt + Shift + S -> V
	@Override
	public void run() {
		for(int i = 0; i <= count; i++) {
			System.out.println(i + " : " + str);
		}
	}
	
}
```      

연습문제
```java
package thread;

public class Test2 {

	public static void main(String[] args) {
		// 메세지 전송과 파일 전송을 동시에 수행할 경우
		// 1. 싱글쓰레드로 구현한 경우

		SendMessage sm = new SendMessage("안녕하세요", 10);
		FileTransfer ft = new FileTransfer("a.java", 100000);

		ft.run();
		sm.run();

		System.out.println("======================================");

		// 2. 멀티쓰레드로 구현한 경우

		// 객체 생성 (SendMessageThread, FileTransferThread, ReceiveMessageThread)
		SendMessageThread smt = new SendMessageThread("안녕하세요", 100000);
		FileTransferThread ftt = new FileTransferThread("a.java", 100000);
		ReceiveMessageThread rmt = new ReceiveMessageThread("Re:안녕하세요", 100000);

		// 반드시 start() 메서드 호출 필수!
		smt.start();
		ftt.start();
		rmt.start();
	}

}

class SendMessage {
	String str;
	int count;

	public SendMessage(String str, int count) {
		this.str = str;
		this.count = count;
	}

	public void run() {
		for (int i = 0; i <= count; i++) {
			System.out.println(i + " : " + str + "메시지 전송");
		}
	}
}

	class FileTransfer {
		String str;
		int count;

		public FileTransfer(String str, int count) {
			this.str = str;
			this.count = count;
		}

		public void run() {
			for (int i = 0; i <= count; i++) {
				System.out.println(i + " : " + str + "파일 전송");
			}
		}
	}

// 메세지 전송 클래스를 Thread 클래스를 상속받아 정의하는 SendMessageThread
	class SendMessageThread extends Thread {
		String str;
		int count;

		public SendMessageThread(String str, int count) {
			this.str = str;
			this.count = count;
		}

		public void run() {
			for (int i = 0; i <= count; i++) {
				System.out.println(i + " : " + str + "메시지 전송");
			}
		}
	}

// 파일 전송 클래스를 Thread 클래스를 상속받아 정의하는 FileTransferThread

	class FileTransferThread extends Thread {
		String str;
		int count;

		public FileTransferThread(String str, int count) {
			this.str = str;
			this.count = count;
		}

		public void run() {
			for (int i = 0; i <= count; i++) {
				System.out.println(i + " : " + str + "파일 전송");
			}
		}
	}
// 메세지 수신 클래스를 Thread 클래스를 상속받아 정의하는 ReceiveMessageThread

class ReceiveMessageThread extends Thread {
	String str;
	int count;
	
	public ReceiveMessageThread(String str, int count) {
		this.str = str;
		this.count = count;
	}
	
	@Override
	public void run() {
		for(int i = 0; i <= count; i++) {
			System.out.println(i + " : " + str + "메시지 수신");
		}
	}
}
```

### Runnable
< 멀티 쓰레딩 구현 방법 >
2. Runnable 인터페이스를 구현하는 서브클래스를 정의하는 방법
- 기존에 다른 클래스를 상속중인 서브클래스는 Thread 클래스 상속이 불가능하므로 Thread 클래스 상속 대신 Runnable 인터페이스를 구현하면 된다.
  - (Thread 클래스는 Runnable 인터페이스의 구현체이다)
- Runnable 인터페이스 내에는 start() 메서드가 존재하지 않으며 존재 하더라도 추상 메서드 형태이기 때문에 start() 메서드 호출이 불가능하다!
  - 따라서, Thread 클래스를 통해 간접적으로 start() 메서드를 호출해야한다!
1) 멀티쓰레딩 코드가 포함될 서브클래스에서 Runnable 인터페이스를 구현(implements)
2) 추상메서드인 run() 메서드를 구현(오버라이딩)하여 멀티쓰레딩 코드 기술
3) 멀티쓰레딩 클래스 인스턴스 생성

        ----------------- 주의! Runnable 인터페이스 내에 start() 메서드가 존재하지 않음 -----------------

4) start() 메서드를 갖고 있는 Thread 클래스의 인스턴스 생성
  - 생성자 파라미터로 Runnable 구현체 객체 전달
5) Thread 인스턴스 통해 간접적으로 start() 메서드 호출

```java
package thread;

public class Ex3 {

	public static void main(String[] args) {
		

//		YourThread yt1 = new YourThread("★★A작업★★", 100000);
//		YourThread yt2 = new YourThread("○○B작업○○", 100000);
//		YourThread yt3 = new YourThread("◈◈C작업◈◈", 100000);
//		Runnable r = new YourThread("◈◈C작업◈◈", 100000);	// YourThread -> Runnable 업캐스팅
		
		// Thread 클래스의 인스턴스 생성 시 생성자 파라미터로 Runnable 구현체 객체 전달 후
		// Thread 객체를 통해 start() 메서드를 호출하여 간접적으로 멀티쓰레딩 수행 
//		Thread t1 = new Thread(yt1);
//		Thread t2 = new Thread(yt2);
//		Thread t3 = new Thread(yt3);
		
		// -----------------------------------------------------
		// YourThread 타입 변수는 (yt1 ~ yt3) 쓰레드 클래스 생성자에 전달하는 작업 외에 사용되지 않는다.
		// 따라서, 1회성 변수를 선언하기보다 Thread 생성자에 
		// YourThread 객체 생성 코드를 직접 전달하여 변수 없이 객체 자체를 바로 전달 가능
//		YourThread yt1 = new Thread(new YourThread("★★A작업★★", 100000));
//		YourThread yt2 = new Thread(new YourThread("○○B작업○○", 100000));
//		YourThread yt3 = new Thread(new YourThread("◈◈C작업◈◈", 100000));
		
//		t1.start();
//		t2.start();
//		t3.start();

		// -------------------------------------------------------------------
		new Thread(new YourThread("★★A작업★★", 100000)).start();
		new Thread(new YourThread("○○B작업○○", 100000)).start();
		new Thread(new YourThread("◆◆C작업◆◆", 100000)).start();

		
	}

}

class A {}
class YourThread extends A implements Runnable {
	
	String str;
	int count;
	
	
	public YourThread(String str, int count) {
		this.str = str;
		this.count = count;
	}


	// Runnable 인터페이스에서 상속된 run() 메서드 오버라이딩 필수!
	@Override
	public void run() {
		for(int i = 1; i <= count; i++) {
			System.out.println(i + " : " + str);
		}
	}
}

```


연습문제
```java
package thread;

public class Test3 {

	public static void main(String[] args) {
		GugudanThread gt1 = new GugudanThread(2);
		GugudanThread gt2 = new GugudanThread(5);
		GugudanThread gt3 = new GugudanThread(9);

//		gt1.start();
//		gt2.start();
//		gt3.start();

		System.out.println("===================================");

		// GugudanRunnable 인스턴스 3개 생성(각각 다른 단을 생성자에 전달)
		// start() 메서드 호출
//		GugudanRunnable g1 = new GugudanRunnable(2);
//		GugudanRunnable g2 = new GugudanRunnable(5);
//		GugudanRunnable g3 = new GugudanRunnable(9);
//		
//		Thread t1 = new Thread(g1);
//		Thread t2 = new Thread(g2);
//		Thread t3 = new Thread(g3);

		Thread t1 = new Thread(new GugudanRunnable(2));
		Thread t2 = new Thread(new GugudanRunnable(5));
		Thread t3 = new Thread(new GugudanRunnable(9));

		t1.start();
		t2.start();
		t3.start();

		new Thread(new GugudanRunnable(7)).start();
	}

}

/*
 * GugudanRunnable 클래스 정의 - Runnable 인터페이스 구현 - 멤버변수(int dan) - 생성자 정의(int형 변수
 * dan을 초기화하는 생성자) - run() 메서드 내부에서 dan에 해당하는 구구단을 100번 반복 출력 => 멀티쓰레드
 */

class GugudanRunnable implements Runnable {

	int dan;

	public GugudanRunnable(int dan) {
		this.dan = dan;
	}

	@Override
	public void run() {
		for (int count = 1; count <= 1000; count++) {
			for (int i = 1; i <= 9; i++) {
				System.out.printf("%d단 : %d * %d = %d \n", dan, dan, i, dan * i);
			}
		}
	}

}

class GugudanThread extends Thread {

	int dan;

	public GugudanThread(int dan) {
		this.dan = dan;
	}

	@Override
	public void run() {
		for (int count = 1; count <= 1000; count++) {
			for (int i = 1; i <= 9; i++) {
				System.out.printf("%d단 : %d * %d = %d \n", dan, dan, i, dan * i);
			}
		}
	}

}
  
```

### 멀티쓰레딩 구현 코드의 변형
- 실제 프로그래밍에서 더 많이 쓰는 형식 
- Runnable 인터페이스를 구현하는 구현체 클래스를 별도로 정의하지 않고 Thread 클래스의 생성자에 Runnable 인터페이스 객체 생성 코드를 바로 작성
  - Runnable 인터페이스의 임시 객체 형태를 Thread 생성자에 전달 
- Runnable 객체를 담는 변수가 없으므로 Runnable 객체에는 접근 불가능. 만약, Thread 객체의 변수도 제거하는 경우 Thread 객체도 접근 불가능 
  - 일회용 객체(= 임시 객체)라고 한다!
```
* < 기본 문법 >
Thread t = new Thread(new Runnable(){
		@Override
		public void run(){
			// 멀티 쓰레딩으로 구현할 코드...
		}
});
t.start();
=> 위의 두문장을 하나로 결합하여 Thread 객체도 임시 객체로 사용 가능
new Thread(new Runnable(){...}).start();
```
```java
package thread;

public class Ex4 {

	public static void main(String[] args) {
		

		Thread t = new Thread(new Runnable() {

			@Override
			public void run() {
				for (int i = 1; i <= 100000; i++) {
					System.out.println(i + ": A작업");
				}
			}
		});
		t.start();

		// --------------------------------------------
		// 위의 작업을 하나의 문장으로 결합
		// => Thread 클래스의 변수를 제거 후 객체 생성 코드만 기술하고,
		// start() 메서드를 바로 호출
//		new Thread().start();

//		new Thread(new Runnable() {
//
//			@Override
//			public void run() {
//				for (int i = 1; i <= 100000; i++) {
//					System.out.println(i + ": A작업");
//				}
//
//			}
//		}).start();

		new Thread(new Runnable() {

			@Override
			public void run() {
				for (int i = 1; i <= 100000; i++) {
					System.out.println(i + ": A작업");
				}

			}
		}).start();

		new Thread(new Runnable() {

			@Override
			public void run() {
				for (int i = 1; i <= 100000; i++) {
					System.out.println(i + ": B작업");
				}

			}
		}).start();

		new Thread(new Runnable() {

			@Override
			public void run() {
				for (int i = 1; i <= 100000; i++) {
					System.out.println(i + ": C작업");
				}

			}
		}).start();

		// 람다 맛보기
		new Thread(() -> {
			for(int i = 1; i <= 100000; i++) {
				System.out.println(i + ": 람다 작업");
			}
		}).start();
		
		/*
		 * 참고. Thread 클래스만으로 멀티쓰레딩을 구현하는 방법 (위의 방법보다 사용빈도 낮음)
		 * < 기본 문법 >
		 * new Thread(){
		 * 		@Override
		 * 		public void run(){
		 * 			// 멀티 쓰레딩으로 처리할 코드
		 * 		}
		 * }.start();
		 * 
		 */
		Thread t2 = new Thread() {
			
			@Override
			public void run() {
				for(int i = 1; i <= 100000; i++) {
					System.out.println(i + " : D작업");
				}
			}
			
		};
		
		t2.start();
		
		new Thread() {
			@Override
			public void run() {
				for(int i = 1; i <= 100000; i++) {
					System.out.println(i + " : E작업");
				}
			}
		}.start();
		
		
	}

}
```


연습문제
```java
package thread;

public class Test4 {

	public static void main(String[] args) {
		// main() 메서드 (main 쓰레드) 내에서
		// "메세지 송신", "메세지 수신", "파일 전송" 작업을 동시에 100번씩 수행
		new Thread(new Runnable() {

			@Override
			public void run() {
				for (int i = 0; i <= 100; i++) {
					try {
						Thread.sleep(500);
					} catch (InterruptedException e) {
						e.printStackTrace();
					}
					System.out.println("메세지 송신");
				}
			}
		}).start();

		new Thread(new Runnable() {

			@Override
			public void run() {
				for (int i = 0; i <= 100; i++) {
					try {
						Thread.sleep(500);
					} catch (InterruptedException e) {
						e.printStackTrace();
					}
					System.out.println("메세지 수신");
				}
			}
		}).start();

		new Thread(new Runnable() {

			@Override
			public void run() {
				for (int i = 0; i <= 100; i++) {
					try {
						Thread.sleep(500);
					} catch (InterruptedException e) {
						e.printStackTrace();
					}
					System.out.println("파일 전송");
				}
			}
		}).start();

		// 람다식
//		new Thread(() -> { for(int i = 0; i <= 100; i++) System.out.println("메세지 송신"); }).start();
//		new Thread(() -> { for(int i = 0; i <= 100; i++) System.out.println("메세지 수신"); }).start();
//		new Thread(() -> { for(int i = 0; i <= 100; i++) System.out.println("파일 전송"); }).start();

//		
	}

}

```

```java
package thread;

public class Ex5 {

	public static void main(String[] args) {

		Thread t1 = new Thread(new Runnable() {

			@Override
			public void run() {
				for (int i = 1; i <= 100; i++) {
					System.out.println("A작업");
				}

			}
		});
		t1.start();

		Thread t2 = new Thread(new Runnable() {

			@Override
			public void run() {
				for (int i = 1; i <= 100; i++) {
					System.out.println("B작업");
				}

			}
		});
		t2.start();

		System.out.println("현재 쓰레드명 : " + Thread.currentThread().getName());
		System.out.println("t1 쓰레드명 : " + t1.getName());
		System.out.println("t2 쓰레드명 : " + t2.getName());

		Thread t3 = new Thread(new Runnable() {

			@Override
			public void run() {
				// 현재 수행중인 쓰레드 객체를 가져오는 방법
				// => Thread 클래스의 static 메서드 currentThread() 메서드 호출
				Thread t = Thread.currentThread();
				System.out.println("C작업! 현재 쓰레드 : " + t.getName());

			}
		});
		t3.setName("t3 쓰레드"); // 저장된 쓰레드명 변경
		t3.start();
		System.out.println("t3.getName() : " + t3.getName());

		System.out.println("=======================================");

		MyThread2 mt = new MyThread2("<<< 내 쓰레드 >>>", 1000);
		mt.start();
	}

}

class MyThread2 extends Thread {
	int count;

	public MyThread2(String name, int count) {
		setName(name); // 외부로부터 쓰레드명을 전달받아 초기화 가능
		this.count = count;
	}

	@Override
	public void run() {
		for (int i = 1; i <= count; i++) {
			System.out.println(i + " : " + getName());
		}
	}

}
```

