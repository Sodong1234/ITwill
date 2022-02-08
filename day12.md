# [오전수업] 자바 7차
## 생성자

```java
package constructor;

class DefaultPerson {
	String name;

	// 생성자를 아무것도 정의하지 않으면 컴파일러에 의해 자동으로 기본 생성자가 생성됨
	// => 기본생성자 : 파라미터 없음, 중괄호 블록 내에 아무 코드도 없음
	// => 아무것도 전달받지 않으며, 아무작업도 수행하지 않음
	public DefaultPerson() {
	}
}

class DefaultPerson2 {
	String name;

	public DefaultPerson2() {
		System.out.println("DefaultPerson2() 생성자 호출됨!");
		name = "홍길동";
	}
}

class ParameterPerson {
	String name;

	public ParameterPerson(String name) {
		System.out.println("ParameterPerson(String) 생성자 호출됨!!");
		this.name = name;
	}
}

class ParameterPerson2 {
	String name;
	int age;
	boolean isHungry;

	public ParameterPerson2(String name, int age) {
		this.name = name;
		this.age = age;
	}

	// ** 생성자 자동 생성 단축키 : Alt + Shift + S -> O **
	public ParameterPerson2(String name, int age, boolean isHungry) {
		this.name = name;
		this.age = age;
		this.isHungry = isHungry;
	}

	
	
}

public class Ex1 {

	public static void main(String[] args) {

		DefaultPerson dp = new DefaultPerson();
		System.out.println("DefaultPerson의 name : " + dp.name);
		System.out.println("=====================================");

		DefaultPerson2 dp2 = new DefaultPerson2();
		System.out.println("DefaultPerson의 name : " + dp2.name);
		System.out.println("=====================================");

		ParameterPerson p = new ParameterPerson("홍길동");
		System.out.println("ParameterPerson의 name : " + p.name);
		System.out.println("=====================================");

		ParameterPerson2 p2 = new ParameterPerson2("소지섭", 40, true);
		System.out.println("ParameterPerson2의 name : " + p2.name);
		System.out.println("ParameterPerson2의 age : " + p2.age);
		System.out.println("ParameterPerson2의 isHungry : " + p2.isHungry);

		// ParameterPerson 클래스의 인스턴스 생성 시 기본 생성자를 호출하는 경우
		// => 기존에 파라미터를 전달받는 생성자를 정의해 놓은 경우
		// 컴파일러가 기본 생성자를 자동으로 생성하지 않으므로
		// 기본 생성자 호출 코드가 존재하는 경우 오류 발생!
//		ParameterPerson2 p3 = new ParameterPerson2();
		
		
		
		

	} // main 메서드 끝

} // Ex1 클래스 끝
```

문제출제
**Account 클래스 정의**
- 멤버변수 : 계좌번호(accountNo, 문자열), 예금주(ownerName, 문자열), 현재잔고(balance, 정수)
 
< 생성자 >
- 기본생성자와 동일한 Account() 생성자 정의
  - 생성자 내에서 멤버변수를 다음과 같이 초기화
- 계좌번호(accountNo) : "111-1111-111"
- 예금주(ownerName) : "홍길동"
- 현재잔고(balance) : 100000 

- 파라미터 3개 (계좌번호, 예금주, 현재잔고) 를 전달받아 초기화하는 생성자 정의
```java
package constructor;


class Account {
	String accountNo;
	String ownerName;
	int balance;
	
	public Account() {
		accountNo = "111-1111-111";
		ownerName = "홍길동";
		balance = 100000;
	}
	public Account(String accountNo, String ownerName, int balance) {
		this.accountNo = accountNo;
		this.ownerName = ownerName;
		this.balance = balance;
	}
	
}

public class Test1 {

	public static void main(String[] args) {
		Account acc = new Account("111-1111-111","홍길동", 100000);
		System.out.println("계좌번호 : " + acc.accountNo);
		System.out.println("예금주 : " + acc.ownerName);
		System.out.println("현재잔고 : " + acc.balance + "원");
    System.out.println("================================================");
		Account acc2 = new Account("222-2222-222", "양윤석", 999999999);
		System.out.println("계좌번호 : " + acc2.accountNo);
		System.out.println("예금주 : " + acc2.ownerName);
		System.out.println("현재잔고 : " + acc2.balance + "원");

	}

}
```

## 오버로딩
- 파라미터가 다른 생성자를 여러번 정의하는 것
```java
package constructor;

class OverloadingPerson {
	String name;
	int age;
	boolean isHungry;

	// 기본 생성자
	public OverloadingPerson() {
		System.out.println("OverloadingPerson 생성자 호출됨!!");
		name = "홍길동";
		age = 28;
		isHungry = true;

	}

	// 생성자 오버로딩
	// 이름, 나이, 배고픔을 전달받아 멤버변수를 초기화 하는 생성자 정의
	public OverloadingPerson(String name, int age, boolean isHungry) {
		System.out.println("OverloadingPerson(String, int, boolean) 생성자 호출됨!!");
		this.name = name;
		this.age = age;
		this.isHungry = isHungry;
	}
	
	public void print() {
		System.out.println("이름 : " + name);
		System.out.println("나이 : " + age);
		System.out.println("배고픔 : " + isHungry);
	}

}

public class Ex2 {

	public static void main(String[] args) {
		/*
		 * 
		 * 생성자 오버로딩(Overloading) - 파라미터가 다른 생성자를 여러번 정의하는 것
		 */
		
		OverloadingPerson op = new OverloadingPerson();
		op.print();
		
		System.out.println("=========================================");
		OverloadingPerson op2 = new OverloadingPerson("이순신", 44, false);
		op2.print();
	}

}
```
문제출제2
**Account2 클래스 정의**
1. 파라미터가 없는 기본 생성자 정의, 다음과 같이 멤버변수 초기화
- 계좌번호 : 111-1111-111
- 예금주명 : 홍길동
- 현재잔고 : 0

2. 계좌번호를 전달받아 초기화하는 생성자 정의, 나머지는 다음과 같이 초기화
- 예금주명 : 홍길동
- 현재잔고 : 0

3. 계좌번호, 예금주명을 전달받아 초기화하는 생성자 정의, 나머지는 다음과 같이 초기화
- 현재잔고 : 0

4. 계좌번호, 예금주명, 현재잔고를 전달받아 초기화하는 생성자 정의


5. showAccountInfo() 메서드 정의
- 파라미터 X, 리턴값 X, 멤버변수에 접근하여 다음과 같이 출력
	- 계좌번호 : XXX-XXXX-XXX
	- 예금주명 : XXX
	- 현재잔고 : XXXX원

```java
package constructor;


class Account2 {
	String accountNo;	// 계좌번호
	String ownerName;	// 예금주명
	int balance;		// 현재잔고
	
	public Account2() {
		accountNo = "111-1111-111";
		ownerName = "홍길동";
		balance = 0;
	}
	
	public Account2(String accountNo) {
		this.accountNo = accountNo;
		ownerName = "홍길동";
		balance = 0;
		
	}
	
	public Account2(String accountNo, String ownerName) {
		this.accountNo = accountNo;
		this.ownerName = ownerName;
		balance = 0;
	}
	
	public Account2(String accountNo, String ownerName, int balance) {
		this.accountNo = accountNo;
		this.ownerName = ownerName;
		this.balance = balance;
	}
	
	void showAccountInfo() {
		System.out.println("계좌번호 : " + accountNo);
		System.out.println("예금주명 : " + ownerName);
		System.out.println("현재잔고 : " + balance + "원");
	}
	
	
}

public class Test2 {

	public static void main(String[] args) {
		
		Account2 acc = new Account2();
		acc.showAccountInfo();
		System.out.println("====================================================");
		
		Account2 acc2 = new Account2("222-2222-222");
		acc2.showAccountInfo();
		System.out.println("====================================================");
		
		Account2 acc3 = new Account2("222-2222-222", "이순신");
		acc3.showAccountInfo();
		System.out.println("====================================================");
		
		Account2 acc4 = new Account2("222-2222-222", "이순신", 999999);
		acc4.showAccountInfo();
		
		
		

	}

}
```
## this문
### this 키워드
1. 레퍼런스(Reference) this
- 자신의 인스턴스 주소가 저장되는 레퍼런스(= 참조변수)
- 개발자가 임의로 생성할 수 없으며, 인스턴스 생성 시 자동으로 주소가 저장됨
	- 각 인스턴스 마다 this에 저장되는 주소가 달라짐
- 일반적인 참조변수와 동일한 방법으로 사용가능
- 주로 생성자나 메서드 내에서 멤버변수와 로컬변수의 이름이 동일할 경우 멤버변수를 구별할 목적으로 사용
- 또한, 자신의 인스턴스 내의 다른메서드를 호출하는 데에도 사용 가능
	- (일반적으로 메서드 이름은 로컬변수처럼 중복되지 않으므로 this 생략)
```
< 기본 문법 >
this.멤버변수명
this.메서드명()
```
```java
package constructor;

class Person {
	String name;
	int age;

//	public void setName(String name) {
	// 파라미터로 전달받은 데이터가 로컬변수 name에 저장되며
	// name = name 코드에 의해 로컬변수 name 값이 다시 자신(로컬변수 name)에 저장됨
//		name = name; // 경고! 아무런 효과가 없는 코드
//	}

	// 1번 방법 : 멤버변수와 로컬변수의 이름을 다르게 선언
//	public void setName(String newName) {
//		// 파라미터로 전달받은 이름이 매개변수 newName에 저장됨 = 로컬변수 = 지역변수
//		// => 로컬변수 newName 값을 인스턴스 멤버변수 name에 저장
//		name = newName;

		
	// 2번 방법 : 레퍼런스 this를 사용하여 멤버변수를 지정
		public void setName(String name) {
			this.name = name;
			
	}
}




class Hero {
	String name;
	int hp;
	int mp;
	int damage;
	
	public Hero(String name, int hp, int mp, int damage) {
		this.name = name;
		this.hp = hp;
		this.mp = mp;
		this.damage = damage;
	}
	
	public void attack(Hero h) {
		int enemyHp = h.hp;
		System.out.println(h.name + "의 hp" + h.hp);
		System.out.println(this.name + "의 hp" + this.hp);
	}
}

public class Ex3 {

	public static void main(String[] args) {
		
		Hero 뽀삐 = new Hero("뽀삐", 520, 120, 50);
		Hero 가렌 = new Hero("가렌", 700, 0, 45);
		
		뽀삐.attack(가렌);
		System.out.println("===================================");
		가렌.attack(뽀삐);
		


	}

}
```

2. 생성자 this()
- 하나의 생성자에서 자신의 클래스 내의 "또 다른" 생성자를 호출하는코드
- 생성자 오버로딩 수행할 경우 멤버변수 초기화 코드가 중복 될 수 있다!
	- 따라서, 코드 중복을 제거하기 위해 하나의 생성자에 작업 처리 코드를 기술하고 다른 생성자에서는 해당 생성자를 호출하여 데이터만 전달한 뒤 간접적으로 멤버변수를 초기화 하도록 한다! => 중복 제거
- this() 형식으로 호출하며, 생성자에 전달할 파라미터를 소괄호() 안에 기술
	- 해당 파라미터 타입 및 갯수가 일치하는 생성자가 호출됨
- 주의사항! 생성자 this()는 생성자 내에서 반드시 첫문장으로 실행되어야 한다! (주석은 제외)

```java
package constructor;

class Person2 {
	String name;
	int age;
	
	public Person2() {
		this("홍길동");
		System.out.println("Person2() 생성자 호출됨!");
//		name = "홍길동";
//		age = 20;
	}
	

	public Person2(String name) {
		this(name, 20);
		System.out.println("Person2(String) 생성자 호출됨!");
//		this.name = name;
//		age = 20;
	}
	
	public Person2(String name, int age) {
		System.out.println("Person2(String, int) 생성자 호출됨!");
		this.name = name;
		this.age = age;
	}
	
}

public class Ex4 {

	public static void main(String[] args) {
	
		
		Person2 p = new Person2();
		System.out.println("이름 : " + p.name);
		System.out.println("나이 : " + p.age);
		System.out.println("=====================================");
		
		Person2 p2 = new Person2("이순신");
		System.out.println("이름 : " + p2.name);
		System.out.println("나이 : " + p2.age);
		System.out.println("=====================================");
		
		Person2 p3 = new Person2("강감찬", 44);
		System.out.println("이름 : " + p3.name);
		System.out.println("나이 : " + p3.age);
		System.out.println("=====================================");

	}

}

console-----------------------


Person2(String, int) 생성자 호출됨!
Person2(String) 생성자 호출됨!
Person2() 생성자 호출됨!
이름 : 홍길동
나이 : 20
=====================================
Person2(String, int) 생성자 호출됨!
Person2(String) 생성자 호출됨!
이름 : 이순신
나이 : 20
=====================================
Person2(String, int) 생성자 호출됨!
이름 : 강감찬
나이 : 44
=====================================
```

