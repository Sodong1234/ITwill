# [오전수업] DB 6차

> DB 5차 수업때 실시한 SELECT 내용을 보충해 놓았음

## WHERE절
- 조건을 통해서 출력을 원하는 행을 선택
- 행을 제한하는 절
```
- 문법
WHERE 조건컬럼 연산자 조건값
```

```
- HR_EMPLOYEES 테이블의 employee_id, last_name, department_id의 데이터를 출력하되, department_id는 데이터가 90인 것만을 출력

SELECT employee_id, last_name, department_id 입력
FROM HR_EMPLOYEES 입력
WHERE department_id = 90; 입력

+-------------+-----------+---------------+
| employee_id | last_name | department_id |
+-------------+-----------+---------------+
|         100 | King      |            90 |
|         101 | Kochhar   |            90 |
|         102 | De Haan   |            90 |
+-------------+-----------+---------------+


-------------------------------------------------------------------------------------------------------------------------


- HR_EMPLOYEES 테이블의 employee_id, last_name, department_id의 데이터를 출력하되, last_name은 데이터가 king인 것만을 출력

SELECT employee_id, last_name, department_id 입력
FROM HR_EMPLOYEES 입력
WHERE last_name = 'King'; 입력


+-------------+-----------+---------------+
| employee_id | last_name | department_id |
+-------------+-----------+---------------+
|         100 | King      |            90 |
|         156 | King      |            80 |
+-------------+-----------+---------------+
```

## 비교연산자
```
=   :  같은 값
>   :  보다 크다, 초과
<   :  보다 작다, 미만
>=  :  보다 크거나 같다, 이상
<=  :  보다 작거나 같다, 이하
!=  :  같지 않다
<>  :  같지 않다
^=  :  같지 않다
```

```
- HR_EMPLOYEES 테이블의 employee_id, last_name, department_id의 데이터를 출력하되, employee_id는 데이터가 110 이하인 것만을 출력

SELECT employee_id, last_name, department_id 입력
FROM HR_EMPLOYEES 입력
WHERE employee_id <= 110; 입력


+-------------+-----------+---------------+
| employee_id | last_name | department_id |
+-------------+-----------+---------------+
|         100 | King      |            90 |
|         101 | Kochhar   |            90 |
|         102 | De Haan   |            90 |
|         103 | Hunold    |            60 |
|         104 | Ernst     |            60 |
|         105 | Austin    |            60 |
|         106 | Pataballa |            60 |
|         107 | Lorentz   |            60 |
|         108 | Greenberg |           100 |
|         109 | Faviet    |           100 |
|         110 | Chen      |           100 |
+-------------+-----------+---------------+



-------------------------------------------------------------------------------------------------------------------------

- HR_EMPLOYEES테이블에서 salary의 값이 17000달러 이상인 사원을 조회해서 employee_id, last_name, salary, department_id의 컬럼을 출력

SELECT employee_id, last_name, salary, department_id 입력
FROM HR_EMPLOYEES 입력
WHERE salary >= 17000; 입력


+-------------+-----------+--------+---------------+
| employee_id | last_name | salary | department_id |
+-------------+-----------+--------+---------------+
|         100 | King      |  24000 |            90 |
|         101 | Kochhar   |  17000 |            90 |
|         102 | De Haan   |  17000 |            90 |
+-------------+-----------+--------+---------------+

```

# [오후수업] JAVA 12차
> JAVA 11차에 실시한 다리 건너기 문제의 메서드버전과 객체지향버전 추가분을 커밋해놓았음

## OOP is A.P.I.E

- E(Encapsulation = 캡슐화)
  - 객체 내부 구조를 숨기고(은닉), 메서드를 통해 메세지만으로 데이터를 주고 받는것
  - 클래스 내의 멤버변수 접근제한자를 private으로 선언하여 외부 접근을 제한하고, Getter/Setter 메서드를 정의하여 간접적으로 데이터를 주고 받도록 하는 것
    - 이 때, Getter/Setter는 누구나 접근 가능하도록 public 적용

- I(Inheritance = 상속)
- P(Polymorphism = 다형성)
- A(Abstraction = 추상화)

### 캡슐화
- 캡슐화(Encapsulation) = 은닉성
- 객체 내부의 구조(멤버변수 및 메서드 처리과정)를 외부로부터 숨기고 메세지만으로 객체와 통신하도록 하는 것
- 클래스 정의 시 멤버변수를 private 접근제한자를 사용하여 외부로 접근을 차단하고 public 접근제한자가 선언된 Getter/Setter 메서드를 정의하여 간접적으로 객체 내의 멤버변수에 접근하도록 하는 것
- 모든 Getter/Setter 메서드는 누구나 접근 가능하도록 접근제한자 public을 사용
- Getter 메서드는 내부 멤버변수 값을 외부로 리턴하는 역할을 수행하여 메서드 이름은 getXXX() 형태(XXX : 리턴할 멤버변수 이름)로 하고 파라미터는 없고, 리턴타입은 리턴할 멤버변수의 데이터타입을 지정
  - ex) 멤버변수 name에 대한 Getter 메서드명 : public String getName(){}
- Setter 메서드는 외부로부터 전달받은 값을 내부 멤버변수에 저장하는 역할을 수행하며 메서드 이름은 setXXX() 형태(XXX : 데이터를 저장할 멤버변수 이름) 파라미터는 전달받은 데이터의 데이터타입을 지정하고, 리턴타입은 없으므로 void 지정
  - ex) 멤버변수 name에 대한 Setter 메서드명 : public String setName(String name){}    
- Getter/Setter 자동 생성 단축키 : Alt + Shift + S -> R
  - 주의! 멤버변수가 있는 클래스 내부에 커서를 두고 단축키를 사용! 

```java
package method;


class Student {
	// 멤버변수 : 이름(name, 문자열), 나이(age, 정수), 점수(score, 정수)
	private String name;
	private int age;
	private int score;
	
	// 멤버변수 name 값을 외부로 리턴하는 Getter 메서드 정의
	// 파라미터 : 없음, 리턴타입 : String(이름(name) 리턴)
	public String getName() {
		return name;
	}
	
	// 이름(name)을 외부로 전달받아 내부 멤버변수 name에 저장하는 Setter 메서드 정의
	// 파라미터 : 이름(String타입, name), 리턴타입 : 리턴값이 없으므로 void
	public void setName(String name) {
		this.name = name;
	}
	
//	// 나이(age)에 대한 Getter 메서드 정의
//	public int getAge() {
//		return age;
//	}
//	
//	
//	// 나이(age)에 대한 Setter 메서드 정의
//	public void setAge(int age) {
//		this.age = age;
//	}
	
	
	
}

public class Ex1 {

	public static void main(String[] args) {


		Student s = new Student();
		s.setName("홍길동");
		System.out.println(s.getName());
		
		
		s.setAge(15);
		System.out.println(s.getAge());
		
	}

}
```


연습문제
```java
package method;

/* 은행계좌(Account) 클래스 정의
 * - 멤버변수(단, 모든 멤버변수를 private 접근제한자로 선언
 * 	1) 계좌번호(accountNo, "xxx-xxxx-xxx" = 문자열)
 * 	2) 예금주명(ownerName, "xxx" = 문자열)
 * 	3) 현재잔고(balance, XXX = 정수)
 * - 모든 멤버변수에 대한 Getter / Setter 메서드 정의
 * 	 (단, setBalance 대신에 deposit() 메서드 정의)
 *	 // => 파라미터 : 입금할 금액(amount, 정수), 리턴타입 : void
 *	 //	   입금금액을 전달받아 현재잔고(balance)에 누적하고 다음과 같이 출력
 *	 //	   입금할 금액 : XXX원
 *	 //	   입금 후 현재잔고 : XXX원
 */
class Account {
	private String accountNo;
	private String ownerName;
	private int balance;
	
	public String getAccountNo() {
		return accountNo;
	}
	
	public String getOwnerName() {
		return ownerName;
	}
	
	public int getBalance() {
		return balance;
	}
	
	public void setAccountNo(String accountno) {
		this.accountNo = accountno;
	}
	
	public void setOwnerName(String ownername) {
		this.ownerName = ownername;
	}
	
	public void setBalance(int balance) {
		this.balance = balance;
	}
	
	public void deposit(int amount) {
		balance += amount;
		System.out.println("입금할 금액 : " + amount);
		System.out.println("입금 후 현재 잔고 : " + balance);
	}
}

public class Test1 {

	public static void main(String[] args) {
		/*
		 * 은행계쫘 Account 클래스의 인스턴스 생성
		 * 다음과 같이 출력되도록 초기화 및 출력
		 * 
		 * 
		 * < 출력 예시 >
		 * 계좌번호 : 111-1111-111
		 * 예금주명 : 홍길동
		 * 현재잔고 : 100000원
		 * 
		 */
		// 은행계좌 기능 중 잔고설정(setBalance())은 적합하지 않은 기능이므로
		// 입금기능(deposit())을 통해 처리하도록 구현
		
		Account acc = new Account();
		acc.setAccountNo("111-1111-111");
		acc.setOwnerName("홍길동");
		acc.deposit(100000);
		
		System.out.println("계좌번호 : " + acc.getAccountNo());
		System.out.println("예금주명 : " + acc.getOwnerName());
		System.out.println("현재잔고 : " + acc.getBalance() + "원");
		
		acc.deposit(10000);


	}

}
```
## 메서드 오버로딩(Method Overloading)
- 메서드 오버로딩(Method Overloading) = 메서드 다중정의
- 동일한 이름의 매개변수가 다른 메서드를 여러개 정의하는 것 
- 동일한 이름으로 서로 다른 파라미터를 전달받아 다른 작업을 처리하도록 하는 것

> - 규칙1. 메서드명 동일 
> - 규칙2. 매개변수의 타입이나 갯수가 달라야함 
> - 규칙3. 리턴타입이 다른 것은 오버로딩과 무관함 
> - 규칙4. 매개변수의 변수명만 다른 것은 오버로딩과 무관함 
>   - 즉, 메서드 호출 시점에서 전달되는 데이터에 따라 각각 다른 메서드를 호출하도록 "구분" 되어야함!


```java
package method;

class AbsNum {
	// 수치 데이터를 전달받아 절대값을 리턴하는 메서드 정의
	// 1. int형 정수 1개(num)를 전달받아 절대값을 리턴

	public int abs(int num) {
		return num < 0 ? -num : num;
	}
	
	// 2. double형 실수 1개(num)를 전달받아 절대값을 리턴
	public double dAbs(double num) {
		return num < 0 ? -num : num;
	}
	
	// 3. long형 정수 1개(num)를 전달받아 절대값을 리턴
	public long lAbs(long num) {
		return num < 0? -num : num;
	}
}

// 메서드 오버로딩을 사용하여 클래스 정의
// => 1) 매개변수의 데이터타입이 다른 오버로딩 메서드 정의
class OverloadingAbsNum {
	// 1. 정수 1개(num)를 전달받아 절대값을 리턴하는 메서드 abs() 정의
	public int abs(int num) {
		return num < 0? -num : num;
	}
	
	// 주의! 매개변수의 타입 또는 갯수가 아닌 매개변수 이름만 다른 것은 오버로딩이 아님!
//	public int abs(int num2) {}
	// 주의! 이름과 매개변수가 같고 리턴타입만 다른 것은 오버로딩이 아님!
//	public double abs(int num) {}
	
	// 2. 실수 1개(num)를 전달받아 절대값을 리턴하는 메서드 abs() 정의 - 오버로딩 적용
	public double abs(double num) {
		return num < 0? -num : num;
	}
	
	// 3. long타입 정수 1개(num)를 전달받아 절대값을 리턴하는 메서드 abs() 정의 - 오버로딩 적용
	public long abs(long num) {
		return num < 0? -num : num;
	}
	
}	
	
class Fork {}
class Chopstick {}
class Spoon {}
class Eat {
	// 오버로딩을 사용하지 않은 경우
	public void eatUsingFork(Fork fork) {
		System.out.println("포크를 사용하여 식사!");
	}
	
	public void eatUsingSpoon(Spoon spoon) {
		System.out.println("스푼을 사용하여 식사!");
	}
	
	//====================================================
	// 오버로딩을 사용한 경우
	
	public void eat(Fork fork) {
		System.out.println("포크를 사용하여 식사!");
	}
	
	public void eat(Spoon spoon) {
		System.out.println("숟가락을 사용하여 식사!");
	}
	
	public void eat(Chopstick chopstick) {
		System.out.println("젓가락을 사용하여 식사!");
	}
}



public class Ex2 {

	public static void main(String[] args) {
		/*
		 * 
		 * 메서드 오버로딩(Method Overloading) = 메서드 다중정의 - 동일한 이름의 매개변수가 다른 메서드를 여러개 정의하는 것 -
		 * 동일한 이름으로 서로 다른 파라미터를 전달받아 다른 작업을 처리하도록 하는 것 - 규칙1. 메서드명 동일 규칙2. 매개변수의 타입이나
		 * 갯수가 달라야함 규칙3. 리턴타입이 다른 것은 오버로딩과 무관함 규칙4. 매개변수의 변수명만 다른 것은 오버로딩과 무관함 => 즉, 메서드
		 * 호출 시점에서 전달되는 데이터에 따라 각각 다른 메서드를 호출하도록 "구분" 되어야함!
		 */
		
		AbsNum absNum = new AbsNum();
		int num = absNum.abs(-5);
		System.out.println("-5의 절대값 : " + num);
		System.out.println("-3.14의 절대값 : " + absNum.dAbs(3));
		System.out.println("-100L의 절대값 : " + absNum.lAbs(-100L));
		
		System.out.println("==================================");
		
		OverloadingAbsNum oan = new OverloadingAbsNum();
		System.out.println("정수 10의 절대값 : " + oan.abs(10));
		System.out.println("실수 3.14의 절대값 : " + oan.abs(3.14));
		System.out.println("정수 100L의 절대값 : " + oan.abs(100L));
			
		// 대표적인 예 : println();
		System.out.println("==================================");
		
		Eat eat = new Eat();
		// eat() 메서드를 호출 할 때 식사 도구를 파라미터로 전달
		// => 이 때, 매개변수 타입이 클래스 타입이므로 인스턴스를 전달하게 됨
		Fork fork = new Fork();
		Chopstick chopstick = new Chopstick();
		Spoon spoon = new Spoon();
		
		eat.eat(fork);
		eat.eat(chopstick);
		eat.eat(spoon);
		
	}

}
```

연습문제
```java
package method;

class OverloadingMethod {
	// int형 정수 2개를 전달받아 덧셈 결과를 출력하는 add() 메서드 정의 - 오버로딩
	public void add(int num1, int num2) {
		System.out.println(num1 + num2);
	}
	// double형 실수 2개를 전달받아 덧셈 결과를 출력하는 add() 메서드 정의 - 오버로딩
	public void add(double num1, double num2) {
		System.out.println(num1 + num2);
	}
	// long형 정수 2개를 전달받아 덧셈 결과를 출력하는 add() 메서드 정의 - 오버로딩
	public void add(long num1, long num2) {
		System.out.println(num1 + num2);
	}
}
public class Test2 {

	public static void main(String[] args) {
		// OverloadingMethod 클래스의 인스턴스 생성 후
		// 각 add() 메서드에 해당하는 데이터를 전달하여 덧셈결과 출력
		
		OverloadingMethod olm = new OverloadingMethod();
		
		olm.add(10, 20);
		olm.add(3.5, 6.2);
		olm.add(324234, 2626236);

		
	}

}
```

메서드 오버로딩 예제 2.
```java
package method;
/*
 * 
 * 계산기(Calculator) 클래스 정의
 * 덧셈, 뺄셈, 곱셈, 나눗셈 기능을 모두 calc() 메서드로 처리
 * => 첫번째 파라미터는 연산자(기호, char 타입 opr)를 전달하고
 * 	  두번째 파라미터부터는 피연산자(숫자, int 타입 numX)을 2 ~ 4까지 전달하여
 * 	  연산자에 따라 각각의 연산을 누적하여 결과를 출력
 * 
 */

class Calculator {
	
	public void calc(char opr, int num1, int num2) {
		if (opr == '+') {
			System.out.println(num1 + num2);
		} else if (opr == '-') {
			System.out.println(num1 - num2);
		} else if (opr == '*') {
			System.out.println(num1 * num2);
		} else if (opr == '/') {
			System.out.println(num1 / num2);
		}
	}

	public void calc(char opr, int num1, int num2, int num3) {
		if (opr == '+') {
			System.out.println(num1 + num2 + num3);
		} else if (opr == '-') {
			System.out.println(num1 - num2 - num3);
		} else if (opr == '*') {
			System.out.println(num1 * num2 * num3);
		} else if (opr == '/') {
			System.out.println(num1 / num2 / num3);
		}

//	public void clac(char opr, int num2, int num3) {
//		
//		} // 불가능! 매개변수의 타입 또는 갯수가 아닌 매개변수 이름만 다른 것은 오버로딩이 아님!

//	public void calc (int num3, int num3, char opr) {
//		
//	} // 가능! public void calc(char opr, int num1, int num2) 와는 전혀 다른 식

	}

	public void calc(char opr, int num1, int num2, int num3, int num4) {
		if (opr == '+') {
			System.out.println(num1 + num2 + num3 + num4);
		} else if (opr == '-') {
			System.out.println(num1 - num2 - num3 - num4);
		} else if (opr == '*') {
			System.out.println(num1 * num2 * num3 * num4);
		} else if (opr == '/') {
			System.out.println(num1 / num2 / num3 / num4);
		}
	}

}

public class Ex3 {

	public static void main(String[] args) {
		Calculator cal = new Calculator();
		cal.calc('+', 10, 20);
		cal.calc('-', 10, 10, 10);
		cal.calc('*', 10, 10, 10, 10);
//		cal.calc('/', 10, 10, 10, 10, 10); // 정수형 매개변수 5개는 오버로딩 하지 않음
	}

}
```

## 비정형 인자
- 비정형 인자(= 가변인자, Variable Arguments)
- 메서드 파라미터 갯수가 정해져 있지 않을 때 다양한 갯수의 파라미터를 모두 전달받을 수 있는 인자
- 메서드 정의시 매개변수 데이터타입과 변수명 사이에 ... 기호를 붙여서 표기
- 전달되는 모든 데이터는 해당 변수명으로 된 "배열" 관리되며 0개 이상의 파라미터를 전달할 수 있음
```
< 기본 문법 >
[접근제한자] 리턴타입 메서드명(데이터타입... 변수명)
```
package method;

public class Ex4 {

	public static void main(String[] args) {

		int dan = 2;
		int i = 3;
		System.out.printf("%d * %d = %d\n", dan, i, dan*i);
		System.out.printf("%d %d %d %d %d\n", 1, 2, 3, 4, 5, 6, 7, 8, 9);
		System.out.println("=================");
		// 전달할 파라미터의 갯수가 정해져 있지 않을 경우
		// 1. 배열을 사용한 매개변수를 선언 시 해당 배열 내의 데이터 갯수는 무제한이므로
		//	  파라미터의 갯수가 정해져 있지 않더라도 배열타입으로 모두 처리 가능
		int[] arr = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
		ArrayArguments aa = new ArrayArguments();
		aa.print(arr);
		System.out.println("=================");
		VariableArguments va = new VariableArguments();
		va.print(1, 2, 3, 4);
		va.print(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);
		va.print(); // 심지어 파라미터 없이도 메서드 호출이 가능
		
	}

}

class VariableArguments {
	// 메서드 정의 시 가변인자 타입으로 매개변수를 선언하면
	// 갯수 제한없이 0개부터 무한대의 파라미터를 한꺼번에 전달 받을 수 있다!
	public void print(int... nums) {
		// 전달받은 모든 파라미터는 nums라는 이름의 배열로 관리됨
		// 따라서, 배열 접근 방법을 그대로 사용 가능
		for(int i = 0; i < nums.length; i++) {
			System.out.print(nums[i] + " ");
			
		}
		System.out.println();
	}
	
	
	
	
	
	// 가변인자 사용시 주의사항!
	// 복수개의 매개변수 선언시 가변인자가 마지막에 사용되면 오류가 발생하지 않음 (= 가변인자가 먼저 오면 오류 발생)
	public void print(String str, int... nums) {}
	
//	public void print(int... nums, String str) {} // 오류 발생!
	// 가변인자는 이 메서드의 마지막 파라미터로 사용되어야 한다!
//	public void print(int...nums, String...str) {} // 오류 발생!
	// 가변 인자는 단 한번만 사용 가능(= 마지막 파라미터로 사용되어야한다는 사유에 포함)
	
	
	
}




class ArrayArguments {
	public void print(int[] arr) {
		for(int i = 0; i < arr.length; i++) {
			System.out.println(arr[i]);
		}
	}
}
```

연습문제
```java
package method;

/*
 * 계산기(Calculator2) 클래스 정의
 * - 덧셈, 뺄셈, 곱셈, 나눗셈 기능을 모드 calc() 메서드로 처리
 * => 첫번째 파라미터로 연산자(기호, char 타입 opr)를 전달하고
 * 	  두번째 파라미터부터 피연산자(숫자, int 타입 가변인자 nums)을 2개 이상 전달하여
 * 	  연산자에 따라 각각의 연산을 누적하여 결과를 출력
 * 
 */
class Calculator2 {
	public void calc(char opr, int...nums) {
		int result = 0;
		for(int i = 0; i < nums.length; i++) {
			if(opr == '+') {
				result += nums[i];
			} else if(opr == '-') {
				result -= nums[i];
			} else if(opr == '*') {
				result *= nums[i];
			} else if(opr == '/') {
				result /= nums[i];
			}
		}
			
	}
}



public class Test4 {

	public static void main(String[] args) {
		Calculator2 cal = new Calculator2();
		cal.calc('+', 1, 2, 3);
		
		
		
		
	}

}
```

// 추후 추가 
