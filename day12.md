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

### 문제출제
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
