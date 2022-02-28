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
