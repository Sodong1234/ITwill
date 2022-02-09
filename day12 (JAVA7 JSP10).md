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

# [오후수업] JSP 10차

> **변수 사용 연습 및 var,let,const 차이점 비교를 day10 커밋에 추가하였음**

## 내장함수
- 자바스크립트에서 제공하는(내장된) 함수로, 이미 특정 기능을 구현하여 제공
- 내장 함수 호출을 통해 구현된 기능을 사용 가능(=> 자바의 메서드와 거의 동일함)
	- 별도의 객체 등이 없이 함수 이름만으로 호출 가능함
		- alert(), prompt(), confirm() 등
```javascript
// alert() 함수 : 파라미터로 전달된 데이터를 별도의 팝업창에 출력 
// 	alert("안녕하세요!");
	
	// 자신의 이름을 저장하는 변수 name 을 선언
	var name = "양윤석";
	// "제 이름은 XXX 입니다" 형식으로 출력
// 	alert("제 이름은 " + name + " 입니다");
	
	// -----------------------------------------------------------------------------------
	// prompt() 함수 : 웹브라우저에서 별도의 팝업창을 통해 데이터를 입력
	//				   (= 다이얼로그 = 모달(Modal)창)
	// - 입력된 데이터를 그대로 리턴
	// - 데이터를 입력하지 않고 확인 버튼 클릭 시 널스트링("") 값 입력됨
	// - 취소 버튼 클릭 시 null 값 입력됨
	// - 입력되는 데이터는 무조건 문자열(string 타입)로 취급됨
	//	 (수치데이터 등을 사용해야 하는 경우 별도의 형변환 필요)
	// 기본문법1 : prompt("입력 다이얼로그에 표시할 메세지");
	// 기본문법2 : prompt("입력 다이얼로그에 표시할 메세지", "기본 입력값")
// 	var inputData = prompt(); // 잘 사용하지 않음
// 	var inputData = prompt("이름을 입력하세요."); // "이름을 입력하세요" 텍스트가 표시됨
// 	var inputData = prompt("이름을 입력하세요.", "기본값"); // 기본값 데이터를 미리 표시함
// 	document.write("입력 데이터 : " + inputData);
	// -----------------------------------------------------------------------------------
	// confirm() 함수 : 사용자로부터 확인 및 취소 버튼을 통해 확인 작업을 수행하는 모달창 표시
	// => 확인 클릭 시 true, 취소 클릭 시 false 값 리턴됨
	var isExit = confirm("종료하시겠습니까?");
	document.write("선택 결과 : " + isExit);
```

## if문
- 자바에서의 if문과 문법 구조 동일
```javascript
[ 기본 문법 ]
	if(조건식1) {
		// 조건식1 판별 결과가 true 일 때 실행할 문장들..
	} else if(조건식2) {
		// 조건식1 판별 결과가 false 이고, 조건식2 판별 결과가 true 일 때 실행할 문장들...
	}	else {
		// 모든 조건식 판별 결과가 false 일 때 실행할 문장들...
	}
```
```javascript
// 사용자의 나이(age)를 입력받아 판별하는 코드 작성
// 	var age = prompt("나이를 입력하세요.");
// 	document.write(age)
	
	// 입력받은 나이에 대해 10대인지 여부 판별하는 if문 작성
	// => 10대일 경우 "10대입니다!" 출력하고, 아니면 "10대가 아닙니다!" 출력"
	// => 단, prompt() 함수로 입력받은 값은 string 타입
	//	  숫자 형식의 데이터타입(number)과 연산 수행할 경우 자동으로 number 타입으로 변환 일어남
	// => 나이(age)가 이상이고 나이(age)가 19 이하
// 	if(age >= 10 && age <= 19) {
// 		document.write("10대입니다!")
// 	} else {
// 		document.write("10대가 아닙니다!")
// 	}
	
	// --------------------------------------------
	// 정수 1개(num)를 입력받아 양수, 음수, 0 판별
	
	var num = prompt("정수를 입력하세요.")
	
	if(num > 0) {
		document.write("양수입니다!")
	} else if(num < 0) {
		document.write("음수입니다!")
	} else {
		document.write("0입니다!")
	}
```
## switch-case문
- 비교할 값을 case 문을 통해 지정(범위 지정 불가, 단일값만 지정 가능)
- case 문 수행 후 중단해야할 경우 break 문 필수
	- 주의! prompt() 함수로 정수 입력 시 string 타입이 case 문으로 비교 시 문자열 형태로 비교 필수!
```javascript
// switch-case 문을 통해 정수값이 1, 2, 3 인지 판별하여 결과 출력
	// 단, 모든 조건이 일치하지 않을 경우 "일치하는 값이 없습니다" 출력
 	var num = prompt("정수를 입력하세요.")
 	switch(num) { // 대상 데이터를 switch 문에 전달
 		
 		case "1" : 
 			document.write("입력된 값은 1 입니다.")
 			break;
 		case "2" : 
 			document.write("입력된 값은 2 입니다.")
 			break;
 		case "3" :
 			document.write("입력된 값은 3 입니다.")
 			break;
		default : 
			document.write("일치하는 값이 없습니다.")
 	}
```

## 반복문
- 주어진 조건에 따라 특정 문장을 반복 실행하는 문
- for문은 주로 반복 횟수가 정해져 있을 때 주로 사용하고, while문은 반복 횟수가 정해져 있지 않을 때 주로 사용

```javascript
1. for문
	[ 기본 문법 ]
	for(초기식; 조건식; 증감식) {
		// 조건식 판별 결과가 true일 때 반복 실행할 문장들...
	}
	*/
	
	// document.write() 함수를 사용하여 "Hello, World!" 문자열 세 번 출력
	for(var i = 1; i <= 3; i++) {
		document.write("Hello, World!<br>");
	}
	// i 값이 1 ~ 3 까지 반복 후 i++ 에 의해 4가 되면 i <= 3 조건식이 false 가 되므로
	document.write("for문 종료 후 i값 : " + i + "<br>"); // 종료 후 i값은 4
	// => 참고! var 로 선언한 변수는 블록과 상관없이 사용 가능함(범위 제한 없음)
		
	
	----------------------------------------------------------------------------------
2. while문
	/*
	[ 기본 문법]
	초기식; // 위치 유동적
	
	while(조건식) {
		// 조건식 판별 결과가 true 일 때 반복 실행할 문장들...
		// 증감식(위치 유동적)
	}
	*/
	
	//while 문을 사용하여 "ITWILL" 문자열 5번 반복 출력
	
	i = 1;
	while(i <= 5) { // 조건식
		document.write("ITWILL<br>"); // 실행문
		i++;
	}
```
## 형변환
- 함수와 연산자에 사용되는 값은 대부분 적합한 데이터타입으로 자동 형변환이 일어남 
- 상황에 따라 개발자가 의도한 타입으로 변환하는 것을 명시적 형변환(= 강제 형변환) 이라고 함
- 자바스크립트의 데이터타입 첫글자를 대문자로 지정하여 형변환 수행해야함
	- (String, Number, Boolean)

```javascript
< 명시적 형변호나 기본 문법 >
	데이터타입(데이터)
	*/
	
	// 1. 문자(string) 타입으로 변환
	// => 참고. alert() 함수 파라미터는 string 타입이며, 다른 데이터타입 전달 시 자동으로 변환됨
	// => 변환 방법 : String(데이터);
	var value1 = 1; // 현재 value1 변수의 데이터타입은 number 타입
	document.write(value1 + " => 변환 전 타입 : " + typeof(value1) + "<br>")
	
	// number 타입인 value1 변수의 타입을 문자 타입(string) 으로 변환할 경우
	value1 = String(value1) // 첫 글자 대문자 S 필수!
	document.write(value1 + " => 변환 후 타입 : " + typeof(value1) + "<br>")
	
	var value2 = true; // 현재 value2 변수의 데이터타입은 boolean
	document.write(value2 + " => 변환 전 타입 : " + typeof(value2) + "<br>")
	
	// boolean 타입인 value2 변수의 타입을 문자 타입(string) 으로 변환할 경우
	value2 = String(value2)
	document.write(value2 + " => 변환 후 타입 : " + typeof(value2) + "<br>")
	
	
	document.write("<hr>")
	// ===================================================================================
	// 2. 숫자(number) 타입으로 변환
	// - 수학 관련 함수나 연산자를 사용한 연산 과정에서는 자동으로 숫자 타입 형변환 일어남
	//	 ex) 문자형숫자(string) / 문자형숫자(string) = 숫자(number) 타입으로 변환 후 연산됨
	// - 변환 방법 : Number(데이터);
	// - 주로, prompt() 함수 등을 사용하여 데이터 입력 시 문자로 취급되므로 변환 시 사용
	document.write("10"/"5") // string / string => number / number 로 변환 후 연산
	document.write("<br>")
	var strNum = prompt("정수를 입력하세요.")
	document.write(strNum + " => 변환 전 타입 : " + typeof(strNum) + "<br>")
	var num = Number(strNum)
	document.write(num + " => 변환 후 타입 : " + typeof(num) + "<br>")
	
	// switch-case 문에서 case문 사용 시 문자 데이터일 경우 숫자로 자동 변환이 일어나지 않으므로
	// 강제 형변환을 통해 number 타입으로 변환해야만 정수 형태로 비교 가능
	switch(Number(strNum)) { // 변환된 변수를 사용하는 switch(num)과 동일함
 		case 1 : 
 			document.write("입력된 값은 1 입니다.")
 			break;
 		case 2 : 
 			document.write("입력된 값은 2 입니다.")
 			break;
 	}
	
	// =====================================================================================================
	// 주의! string -> number 로 변환 시 숫자 데이터 외에 다른 데이터가 섞여 있을 경우 
	// 변환 과정에서 number 타입으로 변환은 일어나지만 데이터에 NaN 이라는 특수한 값이 저장됨
	// => NaN : Not a Number 의 약자로 숫자가 아닌 데이터라는 의미
		
	strNum = "1234a";
	document.write(strNum + " => 변환 전 타입 : " + typeof(strNum) + "<br>")
	
	num = Number(strNum) // "1234a" 를 숫자로 변환 불가능하므로 NaN 값이 저장됨(타입은 number)
	document.write(num + " => 변환 후 타입 : " + typeof(num) + "<br>")
```

## 사용자 정의 함수
- 자바스크립트에서 제공되는 함수 외에 개발자가 직접 정의하는 함수
	- 자바의 메서드와 달리 리턴타입을 명시하지 않으며, 파라미터로 변수 지정은 거의 동일함
	- <script> 태그 내의 자바스크립트 코드는 기본적으로 페이지 로딩 시점에서 바로 실행되지만 함수를 정의할 경우 반드시 호출되어야만 실행 가능함(단, 호출하지 않고도 실행되는 함수도 있음)
	- 정의된 함수는 <script> 태그 내의 아무 위치에서나 호출 가능
	
```javascript
	< 함수 정의 기본 문법>
	function 함수이름([매개변수...]) {
		// 외부로부터 함수가 호출되면 실행할 코드들...
	}
	*/
	
// 	// showMessage() 함수 호출
// 	showMessage(); // 함수 선언부보다 윗쪽에서 호출도 가능함
	
	// showMessage() 함수 정의 => 파라미터 없음
	function showMessage() {
		alert("showMessage() 함수 호출됨!")
	}
	
// 	// showMessage() 함수 호출
// 	showMessage();
	
	// -------------------------------------------------------------------------------------------------
	// sum() 함수 정의 => 파라미터 없음
	// => 1 ~ 10 까지 합을 반복문을 통해 계산 후 alert() 함수를 통해 결과(55) 출력
	
	var total = 0;
	function sum() {
		for(var i = 1; i <= 10; i++) {
			total += i;
		}
		alert(total)
	}
		sum();
				      
	</script>
<!-- 외부 자바스크립트파일(test.js)을 불러오기 -->
	<script type="text/javascript"src="test.js"></script>
	</head>
	<body>
		<h1>사용자 정의 함수 - test.html</h1>
		<input type="button" value="함수호출" onclick="showMessage()"><br>
		<input type="button" value="함수호출2" onclick="printName()"><br>
	
	</body>
	</html>			      
```
