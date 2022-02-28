# [오전수업] DB 테스트 실시
> 오전 DB 테스트 실시

---

# [오후수업] JAVA 15차
> JAVA 14차에서 실시한 연습문제 이어서 풀이

```java
// Q5) 아침 출근길에 김씨는 4,000원을 내고 별다방에서 아메리카노를 사 마셨습니다
//	   이씨는 콩다방에서 4,500원을 내고 라떼를 사 마셨습니다.
//	   이 과정을 객체지향으로 프로그래밍 해보세요.
    
package book;

public class Ex1 {

	public static void main(String[] args) {

		Customer 김씨 = new Customer("김씨", 10000);
		Customer 이씨 = new Customer("이씨", 10000);
		
		CoffeeShop 별다방 = new CoffeeShop("별다방");
		CoffeeShop 콩다방 = new CoffeeShop("콩다방");
		
		System.out.println("-------- 커피 구매 전 ---------");


		System.out.println("김씨 돈 : " + 김씨.money);
		System.out.println("김씨 돈 : " + 김씨.coffee);
		System.out.println();
		System.out.println("이씨 돈 : " + 이씨.money);
		System.out.println("이씨 커피 : " + 이씨.coffee);
		System.out.println();
		System.out.println(별다방.name + "의 오늘 매출액 : " + 별다방.money);
		System.out.println(콩다방.name + "의 오늘 매출액 : " + 콩다방.money);
		
		// 커피 구매
		김씨.buyCoffee(별다방, "아메리카노", 4000);
		이씨.buyCoffee(콩다방, "라떼", 4500);
		
		System.out.println("-------- 커피 구매 후 ---------");
		
		System.out.println("김씨 돈 : "  + 김씨.money);
		System.out.println("김씨 커피 종류 : "  + 김씨.coffee.kind);
		System.out.println("김씨 커피 가격 : "  + 김씨.coffee.price);
		System.out.println();
		System.out.println("이씨 돈 : "  + 이씨.money);
		System.out.println("이씨 커피 종류 : "  + 이씨.coffee.kind);
		System.out.println("이씨 커피 가격 : "  + 이씨.coffee.price);
		System.out.println();
		System.out.println(별다방.name + "의 오늘 매출액 : " + 별다방.money);
		System.out.println(콩다방.name + "의 오늘 매출액 : " + 콩다방.money);


	}

}

class Customer {
	String name;
	int money;
	Coffee coffee;

	// Alt + Shift + S -> O
	public Customer(String name, int money) {
		this.name = name;
		this.money = money;
	}
	
	public void buyCoffee(CoffeeShop coffeeshop, String kind, int price) {
		this.money -= price;
		this.coffee = coffeeshop.sellCoffee(kind, price);
	}

}

class CoffeeShop {
	String name;
	int money; // 오늘 매출액
	
	public CoffeeShop(String name) {
		this.name = name;
	}

	public Coffee sellCoffee(String kind, int money) { // 커피 판매 기능
		this.money += money;
		return new Coffee(kind, money);
	}
}

class Coffee {
	String kind; // 종류
	int price; // 가격

	public Coffee(String kind, int price) {
		this.kind = kind;
		this.price = price;
	}
}


// 정답은 없는 문제. 여러가지 풀이가 나올 수 있음.
```

---

## 상속

- 슈퍼클래스(부모)의 모든 멤버를 서브클래스(자식)에서 물려받아 선언 없이 사용하는 것 
	- 상속을 받은 서브클래스에서 별도의 선언 및 정의 없이도 슈퍼클래스가 가진 멤버변수나 메서드 등을 자신의 멤버처럼 사용 가능 
	- 상속을 활용하면 코드 중복이 제거되며, 유지 보수에 용이해짐 
- 슈퍼클래스(Super class) = 부모클래스(조상클래스) = 상위클래스 
- 서브클래스(Subclass) = 자식클래스(자손클래스) = 하위클래스 = 파생클래스 
- 서브클래스 정의 시 서브클래스명 뒤에 extends 키워드를 사용하고 extends 키워드 뒤에 슈퍼클래스의 이름을 명시함 
	- 슈퍼클래스가 가진 멤버를 물려받아 서브클래스에서 멤버를 추가하므로 기존클래스를 확장(extends)하는 개념으로 사용됨

- private 접근제한자가 적용된 멤버는 상속 대상에서 제외됨 
- 생성자는 상속되지 않음 (생성자는 자신의 클래스이름과 동일해야하는데 생성자가 상속되면 이름이 다름) 
- 자바는 단일 상속만 지원하므로 동시에 2개 이상의 클래스를 상속 받을 수 없음 (class 서브클래스명 extends 슈퍼클래스1, 슈퍼클래스2 {} 형태로 상속 불가능!) 
- 클래스 정의 시 별도의 extends 키워드를 사용하지 않으면(상속 명시하지 않으면) 자동으로 java.lang.Object 클래스를 상속받게 됨 
	- 따라서, object 클래스는 모든 자바클래스의 최상위 클래스. 
	- 즉, 모든 클래스에서 Object 클래스의 멤버에 접근 가능

```
< 상속 기본 문법 - 서브클래스 정의 >

class 서브클래스명 extends 슈퍼클래스명 {}
```


```java
package inheritance;

public class Ex1 {

	public static void main(String[] args) {

		System.out.println("-------- 아버지 --------");
		아버지 아버지 = new 아버지();
		System.out.println(아버지.car);
		아버지.drawWell();

		// 슈퍼클래스인 할아버지의 멤버 접근(아버지 인스턴스를 통해 접근)
		System.out.println(아버지.house);
		아버지.singWell();

		System.out.println("-------- 나 --------");

		나 나 = new 나();
		System.out.println(나.pc);
		나.programmingWell();

		// 슈퍼클래스인 아버지와 그의 슈퍼클래스인 할아버지의 모든 멤버에 접근 가능!
		System.out.println(나.house); // 할아버지
		System.out.println(나.car); // 아버지
		나.singWell(); // 할아버지
		나.drawWell(); // 아버지
	}
}






class 할아버지 {
	String house = "이층집";

	public void singWell() {
		System.out.println("노래를 잘한다!");
	}
}

// 아버지 클래스 정의 - 할아버지 클래스로부터 상속
// => 멤버변수 : car(문자열) -> "자동차" 로 초기화
// => 메서드 : drawWell() 메서드 정의(파라미터 없음, 리턴값 없음)

class 아버지 extends 할아버지 {
	String car = "자동차";

	public void drawWell() {
		System.out.println("그림을 잘 그린다!");
	}
}

// 아버지 클래스를 상속받는 나 클래스 정의
// => 슈퍼클래스가 아버지 될 경우 아버지의 슈퍼클래스인 할아버지의 멤버도 상속됨
class 나 extends 아버지 {
	String pc = "컴퓨터";

	public void programmingWell() {
		System.out.println("프로그래밍을 잘 한다!");
	}
}
```

연습문제

```java

	String accountNo; // 계좌번호
	String ownerName; // 예금주명
	int balance; // 계좌잔고

	// 모든 멤버 정보를 출력하는 showAccountInfo() 메서드 작성
	// 입금 가능(전달받은 입금금액(ammount)을 현재잔고 balance에 누적) deposit 메서드
	// 출금 기능(매개변수 금액을 전달받아, 금액만큼 리턴) withdraw 메서드 작성
	// 단, 잔고보다 큰 금액을 출금하려고 하면 출금불가! / 출금이 가능한 경우 : 출금 금액만큼 리턴
	
	
	
/*
 * Account 클래스를 상속 받는 ItwillBank 클래스 정의 
 * => 은행계좌 기본기능 + 보험 관리 기능 추가 
 * - 멤버변수 : 보험명(insureName, 문자열) 
 * - 메서드 : contract() 메서드 (파라미터 보험명(insureName), 리턴값 없음) 
 * 		  => 전달받은 보험명을 멤버변수에 저장하고 "계약하신 보험명 : XXX보험" 출력
 */

----------------------------------------------------------------------------------------------
package inheritance;

public class Test1 {

	public static void main(String[] args) {

		
		
		itwillBank ib = new itwillBank();
		ib.accountNo = "111-1111-111";
		ib.ownerName = "홍길동";
		ib.balance = 100000;
		
		ib.showAccoutInfo();
		ib.deposit(50000);
		System.out.println("출금된 금액 : " + ib.withdraw(100000));
		System.out.println("-------------------");
		ib.contract("자동차");
		
	}

}

class Account {

	public void showAccoutInfo() {
		System.out.println("계좌번호 : " + accountNo);
		System.out.println("예금주명 : " + ownerName);
		System.out.println("계좌잔고 : " + balance);
	}

	public void deposit(int amount) {
		balance += amount;
		System.out.println("입금금액 : " + amount + "원");
		System.out.println("현재잔고 : " + balance + "원");
	}

	public int withdraw(int amount) {
		if (amount > balance) {
			System.out.println("출금할 금액 : " + amount + "원");
			System.out.println("잔액이 부족하여 출금 불가! (현재잔고 : " + balance + "원)");
			return 0;
		} else {
			balance -= amount;
			System.out.println("출금할 금액 : " + amount + "원");
			System.out.println("출금 후 현재 잔고 :" + balance + "원");
			return amount;
		}
	}
}

class itwillBank extends Account {
	String insureName;
	public void contract(String insureName) {
		this.insureName = insureName;
		System.out.println("계약하신 보험명 : " + insureName + "보험");
	}
	
}
```

### private 상속
```java
package inheritance;

public class Ex2 {

	public static void main(String[] args) {
		// private 접근제한자를 갖는 멤버는 상속 대상에서 제외됨
		// => private 접근제한자는 자신의 클래스내에만 접근 가능하고
		//	  다른 클래스에서 접근 불가능 하도록 제어하므로
		//	  상속받는 자식도 접근 불가능하기 때문에 상속되지 않는다!
		spiderMan sm = new spiderMan();
		sm.name = "피터 파커";
		sm.age = 20;
		sm.nickName = "친절한 이웃 스파이더맨";
		sm.eat();
		sm.jump();
		sm.fireWeb();
//		sm.normalLife(); // 오류! private 접근제한자로 선언된 메서드는 상속 불가!
		
		// 생성자도 상속 대상에서 제외됨
		// => 생성자의 이름은 클래스의 이름과 동일해야하는데
		//	  생성자를 상속 받은 경우 자신의 이름이 아닌 부모 클래스의 이름이 사용되므로
		//	  생성자 작성 규칙을 위반하게 되므로 생성자는 상속되지 않음
		Person p = new Person("홍길동", 20);
//		SpiderMan sm2 = new SpiderMan("피터 파커, 20"); // 오류!
		// 생성자도 상속되지 않으므로, 서브클래스 타입의 파라미터 생성자 호출 불가능!
	}

}

class Person {
	String name;
	int age;
	
	// 기본생성자
	public Person() {}
	public Person(String name, int age) {
		this.name = name;
		this.age = age;
	}
	
	// 메서드 정의
	public void eat() {
		System.out.println("밥 먹기!");
	}
	
	public void jump() {
		 System.out.println("점프!");
	}
	
	public void normalLife() {
		 System.out.println("평범한 삶!");
	}
	
}

class spiderMan extends Person {
	String nickName;
	
	public void fireWeb() {
		System.out.println("거미줄 발사!");
	}
}
```
