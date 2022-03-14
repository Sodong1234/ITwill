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

![unnamed](https://user-images.githubusercontent.com/95197594/158099343-623b7dda-63b5-422e-ab85-788280244cfd.png)


### WHERE절
- 필수가 아닌 옵션절로 출력을 원하는 행의 조건을 설정할 수 있는 절
- WHERE의 요소로 조건식을 사용
```
WHERE 조건컬럼 연산자 조건값
```
- 조건값과 조건컬럼은 동일한 데이터 타입이어야함
```

SQL> SELECT employee_id, last_name, job_id, department_id
  2  FROM employees
  3  WHERE department_id = 90;



EMPLOYEE_ID LAST_NAME                 JOB_ID     DEPARTMENT_ID
----------- ------------------------- ---------- -------------
        100 King                      AD_PRES               90
        101 Kochhar                   AD_VP                 90
        102 De Haan                   AD_VP                 90
```
- 문자열의 경우 비교시 대소문자를 구분
```

SQL> SELECT last_name, job_id, department_id
  2  FROM employees
  3  WHERE last_name = 'Whalen';



LAST_NAME                 JOB_ID     DEPARTMENT_ID
------------------------- ---------- -------------
Whalen                    AD_ASST               10
```
```
SQL> SELECT last_name, hire_date
  2  FROM employees
  3  WHERE hire_date = '19-MAR-05';



LAST_NAME                 HIRE_DATE
------------------------- ---------
Hutton                    19-MAR-05
```


# [오후수업] JAVA 20차

## 인터페이스
> 19차에 실시한 인터페이스 보충분 

```java

public class Ex1 {

	public static void main(String[] args) {
		
		// 인터페이스는 인스턴스 생성 불가
//		고래인터페이스 고래 = new 고래인터페이스(); 
//		상어인터페이스 상어 = new 상어인터페이스();
		
		// 인터페이스 2개를 상속받아 구현한 서브클래스인 고래상어를 인스턴스 생성 후 사용
		고래상어 고래상어 = new 고래상어();
		고래상어.번식();
		
		
		
	}

}




abstract class 동물클래스 {
	public abstract void 번식();
}

class 고래클래스 extends 동물클래스 {
	@Override
	public void 번식() {
		System.out.println("새끼를 낳아 번식");
	}
	
}

class 상어클래스 extends 동물클래스 {
	@Override
	public void 번식() {
		System.out.println("알을 낳아 번식");
	}
		
}

// 만약, 고래클래스와 상어클래스를 동시에 상속받아 고래상어 클래스를 정의 한다면
// 다중 상속 시 부모의 메서드가 동일할 경우 메서드 접근의 모호성이 발생할 수 있으므로
// 자바에서는 다중상속을 허용하지 않음
//class 고래상어클래스 extends 고래클래스, 상어클래스 {
//	public void 부모의번식() {
//		번식(); // 이 번식 메서드는 고래와 상어 중 누구의 번식 메서드인가?
//		// 따라서, 자바에서는 모호한 상황을 차단하기 위해 애초부터 다중상속을 허용하지 않음
//	}
//}
// ======================================================================
interface 고래인터페이스 {
	public void 번식();
}

interface 상어인터페이스 {
	public void 번식();
}

// 인터페이스끼리 다중상속(extends)
interface 고래상어인터페이스 extends 고래인터페이스, 상어인터페이스 {
	
}

// 구현클래스에서는 다중 구현(implements)
class 고래상어 implements 고래상어인터페이스 {
	@Override
	public void 번식() {
		// 추상메서드인 번식() 메서드를 상속받아 구현하더라도 
		// 어느 부모의 번식() 메서드인지 구별할 필요가 없음 = 모두 추상메서드이기 때문
		// 실행할 내용을 서브클래스에서 직접 정의하면 되기 때문에 부모의 메서드는 무관함
		// => 모든 구현은 서브클래스에게 맡기고, 인터페이스는 메서드 형식(표준)만 규정하는 역할
		System.out.println("고래상어는 상어처럼 알을 낳아 번식한다!");
	}
	
}
```

```java

public class Ex2 {

	public static void main(String[] args) {
		
	}

}



interface IHello {
	public void sayHello(String name);
	
}

interface IGoodbye {
	public void sayGoodbye(String name);
}

// 인터페이스 끼리 상속을 받을 경우 extends 키워드를 사용해야함
// => 인터페이스가 가질 수 있는 메서드는 모두 추상메서드 이므로 
// 	  부모 인터페이스를 상속받아 구현 할 수 없기 때문에
//	  인터페이스끼리의 상속은 구현(implements)이 아닌 확장(extends)을 사용
// 또한, 클래스 상속과는 달리 2개 이상의 인터페이스를 상속 받을 수 있다!
// => 부모 인터페이스의 모든 메서드가 추상메서드이므로 
//	  부모 인터페이스들 중 누구의 메서드인지 구별할 필요가 없이
//	  서브인터페이스도 추상메서드 형태로 보관하기 때문
interface ITotal extends IHello, IGoodbye {
//	public void sayHello(String name) {} // 오류 발생! 추상메서드 구현이 불가능!
	public void greeting(String name);
}

// 2개의 인터페이스를 상속받은 ITotal 인터페이스를
// 서브클래스에서 상속받아 구현하면 모든 인터페이스의 내용을 구현하게 된다!
// 만약, 하나라도 추상메서드를 구현하지 않으려면 추상클래스로 선언해야한다!
class ISay implements ITotal {

	// 3개의 추상메서드를 구현 해야함
	@Override
	public void sayHello(String name) {
		System.out.println(name + "씨, 안녕하세요!");
	}

	@Override
	public void sayGoodbye(String name) {
		System.out.println(name + "씨, 안녕히 가세요!");
	}

	@Override
	public void greeting(String name) {
		System.out.println(name + ", 안녕!");
	}
	
}

// --------------------------------------------------------------------------

abstract class ISay2 implements ITotal {

	// 3개의 추상메서드를 구현 해야함
	@Override
	public void sayHello(String name) {
		System.out.println(name + "씨, 안녕하세요!");
	}

	@Override
	public void sayGoodbye(String name) {
		System.out.println(name + "씨, 안녕히 가세요!");
	}

//	@Override
//	public void greeting(String name) {
//		System.out.println(name + ", 안녕!");
//	}
	
}


class ISay2SubClass extends ISay2 {

	// 추상클래스인 ISay2 클래스를 상속 받더라도 미구현된 greeting() 메서드만 구현하면 된다!
	// => 다른 메서드 오버라이딩도 가능하지만, 강제성은 greeting() 메서드에만 부여됨
	@Override
	public void greeting(String name) {
		// TODO Auto-generated method stub
		
	}
	
}

// ---------------------------------------------------------------------------
// 추상클래스 끼리의 상속
abstract class AbstractClass1 {
	public abstract void abstractMethod1();
}

abstract class AbstractClass2 extends AbstractClass1 {
	public abstract void abstractMethod2();
}

class NormalClass extends AbstractClass2 {
	
	// 추상메서드 2개 갖는 AbstractClass2를 상속받으면 2개의 추상메서드 모두 오버라이딩 필수!
	// => 만약, 하나라도 구현하지 않을 경우 현재 클래스도 추상클래스가 되어야 한다!
	@Override
	public void abstractMethod2() {
		// TODO Auto-generated method stub
		
	}

	@Override
	public void abstractMethod1() {
		// TODO Auto-generated method stub
		
	}
	
}
```

### 인터페이스 필요성 (장점)

1. 구현의 강제성 부여 (= 통일성)
- 인터페이스를 상속받은 서브클래스는 반드시 추상메서드를 구현해야한다! (메서드 이름 통일)

2. 모듈 교체의 용이
- 부모 타입인 인터페이스 타입을 변수로 사용하여 각 서브클래스를 다룰 경우 특정 서브클래스가 또 추가되더라도 공통 코드의 변경이 필요없이 새로운 클래스만 정의하게 되면 언제든 인스턴스 교체를 통해 서브클래스를 다룰 수 있음
  - ex) JDBC 인터페이스(Conection 등)를 통해 MySQL과 Oracle 드라이버 교체만으로 각 데이터베이스를 동일한 방법으로 다룰 수 있도록 해준다!

3. 상속 관계가 없는 객체끼리의 관계를 부여하여 다형성 활용 가능 (Flyer -> Bird, SuperMan, Airplane)

4. 모듈간 독립적 프로그래밍으로 인한 개발 기간 단축
```java

public class Ex3 {

	public static void main(String[] args) {
	
		PrintClient pc = new PrintClient();
		pc.setPrinter(new DotPrinter());
		pc.print("Ex3.java");
		
		pc.setPrinter(new InkjetPrinter());
		pc.print("Test3.java");
		
		pc.setPrinter(new LaserPrint());
		pc.print("Ex4.java");
	}

}

// ----------------- 2. 모듈 교체의 용이 -----------------

interface Printer {
	public void print(String fileName);
}

class DotPrinter implements Printer {

	@Override
	public void print(String fileName) {
		System.out.println("DotPrinter로 출력 : " + fileName);
	}
	
}

class InkjetPrinter implements Printer {

	@Override
	public void print(String fileName) {
		System.out.println("InkjetPrinter로 출력 : " + fileName);
	}
	
}

class LaserPrint implements Printer {

	@Override
	public void print(String fileName) {
		System.out.println("LaserPrinter로 출력 : " + fileName);
	}
	
}
// ---------------------------------------------------
class PrintClient { // 사용자
	// PrintClient 클래스에서 Printer 인터페이스를 전달 받아 프린터를 컨트롤 하는 경우
	// 직접적인 XXXPrinter에 대한 코드는 기술하지 않아도 된다!
	private Printer printer;
	
	// Setter 메서드를 정의하여 외부로부터 Printer 타입 객체를 전달받아 저장
	public void setPrinter(Printer printer) {
		this.printer = printer;
	}
	
	// print() 메서드를 정의하여 외부로부터 출력할 파일명을 전달받으면
	// 현재 갖고 있는 Printer 타입 객체의 print() 메서드를 호출하면 인쇄가 가능
	public void print(String fileName) {
		printer.print(fileName); // 업캐스팅 된 Printer 타입 객체의 print() 메서드 호출
	}
}

```

```java

public class Ex4 {

	public static void main(String[] args) {

		// ---------- 3. 상속 관계가 아닐 경우 나쁜(불편) 예 ----------
		/*
		 * 
		 * HandPhone 클래스와 DigitalCamera는 특정 클래스와 상속관계까 아니므로 두 클래스의 유일한 공통클래스는 Object 클래스
		 * 뿐이다. 따라서, 업캐스팅을 통해 다형성을 적용하려면 Object 타입으로 업캐스팅해야하며, 업캐스팅 후에는 두 객체의 고유멤버 접근이
		 * 불가능하므로 다시 다운캐스팅을 통해 각 객체의 고유 멤버에 접근해야한다!
		 */
//		Object obj1 = new HandPhone();
//		Object obj2 = new DigitalCamera();
		// => Object 타입 배열에 HandPhone, DigitalCamera 인스턴스를 업캐스팅 하여 저장
		Object[] objs = {new HandPhone(), new DigitalCamera(), new HandPhone(), new HandPhone()};

		// 반복문을 사용하여 Object[] 배열 크기만큼 반복
		for (int i = 0; i < objs.length; i++) {
			Object obj = objs[i];
			if (obj instanceof HandPhone) {
				// instanceof 연산자를 사용하여 객체를 판별한 후 다시 다운캐스팅을 통해
				// 원래 객체 형태로 변환하여 사용해야함
				HandPhone hp = (HandPhone) obj;
				hp.charge();
			} else if (obj instanceof DigitalCamera) {
				// instanceof 연산자를 사용하여 객체를 판별한 후 다시 다운캐스팅을 통해
				// 원래 객체 형태로 변환하여 사용해야함
				DigitalCamera dc = (DigitalCamera) obj;
				dc.charge();
			}
		}
		System.out.println("----------------------------------");
		// ---------- 향상된 for문으로 위의 for문을 바꿔보기 ----------
		for (Object obj : objs) {
			if (obj instanceof HandPhone) {
				HandPhone hp = (HandPhone) obj;
				hp.charge();
			} else if (obj instanceof DigitalCamera) {
				DigitalCamera dc = (DigitalCamera) obj;
				dc.charge();
			}

		}
		System.out.println("---------------------------");
		// --------------- 인터페이스를 통해 상속관계가 아닌 클래스끼리 상속 관계 부여 ---------------
		/*
		 * HandPhone2 클래스와 DigitalCamera2 는 특정 클래스와 상속 관계가 아니지만
		 * 두 클래스와 유일한 공통클래스는 Object 클래스 외에
		 * Chargable 인터페이스를 정의하여 곹오 부모로 정의하면
		 * 업캐스팅 후에도 두 객체의 고유 멤버인 charge() 메서드가
		 * 인터페이스 내의 추상메서드로 정의되어 있기 때문에
		 * 타입 판별 없이, 다운캐스팅 없이도 메서드 호출이 가능 
		 */
		Chargeable[] chs = {new HandPhone2(), new DigitalCamera2(), new DigitalCamera2(), new HandPhone2()};

		for (Chargeable ch : chs) {
			// 공통 부모인 Chargable 인터페이스 타입으로 업캐스팅 한 뒤에도
			// 공통 메서드인 charge() 메서드에 직접 접근 가능 => instanceof 판별 필요없음
			ch.charge();
		}
		
		
		
		
		
		
		
	}
}

// ---------- 상속 관계가 아닐 경우 나쁜(불편) 예 ----------
class Phone {
}

class HandPhone extends Phone {
	public void charge() {
		System.out.println("HandPhone 충전");
	}
}

class Camera {
}

class DigitalCamera extends Camera {
	public void charge() {
		System.out.println("DigitalCamera 충전");
	}
}

// --------------- 인터페이스를 통해 상속관계가 아닌 클래스끼리 상속 관계 부여 ---------------
// 서로 상속 관계가 없는 HandPhone과 Digital 클래스에
// 인터페이스를 통한 상속 관계를 부여하면 다형성 확장하여 적용 가능
interface Chargeable {
	public void charge();
}

class Phone2 {}

class HandPhone2 extends Phone implements Chargeable {

	@Override
	public void charge() {
		System.out.println("HandPhone 충전");
	}
}

class Camera2 {}

class DigitalCamera2 extends Camera implements Chargeable {

	@Override
	public void charge() {
		System.out.println("DigitalCamera 충전");
	}
}
```

```java

public class Ex5 {

	public static void main(String[] args) {
  
		// ----------------- 4. 모듈간 독립적 프로그래밍으로 인한 개발 기간 단축 -----------------
    
		// 개발자 코드 동작 확인
		// => 디자이너측에서 생성하여 전달할 아이디, 패스워드를 임의로 입력하여 확인
		DeveloperLogin dl = new DeveloperLogin();
		String result = dl.login("admin", "12345");
		System.out.println("로그인 결과 : " + result);
		
		System.out.println("-----------------------------");
		
		// 디자이너 코드 동작 확인
		// => 개발자측에 아이디, 패스워드를 전달하고 리턴값을 리턴받아 확인
		DesinerLogin dl2 = new DesinerLogin();
		dl2.inputLoginInfo();
		
	}

}


// 개발자와 디자이너 수행해야하는 작업들의 공통 부분을 추상메서드로 정의하여
// 기본적인 틀을 제시하면 개발자와 디자이너는 각각 별도로 독립된 작업 수행이 가능
interface LoginInterface {
	// 디자이너는 입력받은 id, password를 문자열로 전달 -> 성공여부 리턴받아 alert
	// 개발자는 id, password를 전달받아 로그인 처리 후 결과를 문자열로 리턴
	public String login(String id, String password);
}

// 개발자가 수행해야하는 로그인 처리 작업
// => 공통 인터페이스를 상속받아 추상메서드를 구현하여 로그인 작업 수행 가능
//	  이 때, 디자이너쪽에서 넘어올 데이터는 임의의 데이터로 대체 가능 (테스트)
//	  상호간에 사용할 파라미터를 미리 매개변수로 정했기 때문
class DeveloperLogin implements LoginInterface {

	@Override
	public String login(String id, String password) {
		System.out.println("아이디 : " + id);
		System.out.println("패스워드 : " + password);
		String result = "";
		if(id.equals("admin") && password.equals("1234")) {
			result = "로그인 성공!";
		} else {
			result = "로그인 실패!";
		}
		return result;
	}
}

// 디자이너가 수행해야하는 로그인 정보 전달 및 전달 후 결과 처리 작업
// 개발자측에 전달한 데이터(아이디, 패스워드)를 생성하여
// 공통 추상메서드를 구현하여 파라미터로 전달했을 때 데이터가 잘 전달되었는지 확인하고,
// 리턴되는 결과값을 임의로 설정하여 리턴 후 리턴된 데이터를 전달받아 확인
class DesinerLogin implements LoginInterface {

	@Override
	public String login(String id, String password) {
		
		// 실제로는 개발자가 구현할 메서드 이지만
		// 개발자와 별개로 독립적인 작업을 위해 공통 메서드를 임시로 구현하여
		// 개발자 입장에서 처리할 코드는 생략하고
		// 디자이너가 전달하는 데이터가 잘 전달되는지만 확인 
		System.out.println("< 로그인 정보 파라미터 확인 >");
		System.out.println("아이디 : " + id);
		System.out.println("패스워드 : " + password);
		
		return "로그인 성공";
	}
	
	// 디자이너가 외부로부터 아이디, 패스워드를 생성하여 개발자에게 넘겨줄 메서드 정의
	public void inputLoginInfo() {
		// 임의의 로그인 정보를 입력받았다고 가정
		String id = "admin";
		String password = "1234";
		
		// 공통 메서드인 login() 메서드를 호출하여 파라미터가 잘 전달되었는지 확인
		// login() 메서드에 임의의 리턴값을 설정하여 리턴된 데이터가 잘 출력되는지 확인
		String result = login(id, password);
		System.out.println("결과 : " + result); // alert라고 가정
	}
	
}
```

### default 메서드
```java

public class Ex6 {

	public static void main(String[] args) {
		/*
		 * 인터페이스의 default 메서드
		 * 충돌 발생 시 예외처리
		 */
		
		// 1. (super)class vs interface (class Win!)
		C c = new C();
		c.method();
		
		// 2. interface vs interface (반드시 override)
		SubClass sub = new SubClass();
		sub.method();
		

	}

}


//1. (super)class vs interface (class Win!)
class A {
	void method() {
		System.out.println("class A");
	}
}

interface B {
	default void method() {
		System.out.println("interface B");
	}
}

class C extends A implements B {

	@Override
	public void method() {
		System.out.println("호출됨!");
		super.method();
	}
	
}

// 2. interface vs interface (반드시 override)
interface I1 {
	default void method() {
		System.out.println("I1의 메서드");
	}
}

interface I2 {
	default void method() {
		System.out.println("I2의 메서드");
	}
}

class SubClass implements I1, I2 {

	@Override
	public void method() {
		System.out.println("SubClass의 메서드");
	}
	
}
```
