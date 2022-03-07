# [오전수업]
> 강사님께서 공유해주신 Oracle 이미지파일 다운로드 후 가져오기 실시함.

## SELECT 구문
- 테이블의 저장된 데이터를 조회하는데 사용하는 문법

### 키워드(예약어)
- SQL 문법 / 접속 프로그램 상에서 미리 기능이 예약되어 있는 단어

```
키워드 + 요소 → 절
ex) SELECT + 컬럼리스트 → SELECT절
    FROM + 테이블명 → FROM절
절 + 절 → 구문
SELECT절 + FROM절 → SELECT구문

SELECT	  : 테이블에서 출력할 컬럼의 목록, 표현식, 함수를 작성하는 키워드
FROM	  : 데이터를 가져올 테이블명 작성
DISTINCT  : 데이터의 중복값을 제거하여 출력
```

### 테이블 구조 조회
```
SELECT department_id, location_id
FROM HR_DEPARTMENTS;

+---------------+-------------+
| department_id | location_id |
+---------------+-------------+
|            60 |        1400 |
|            50 |        1500 |
|            10 |        1700 |
|            30 |        1700 |
...
|            20 |        1800 |
|            40 |        2400 |
|            80 |        2500 |
|            70 |        2700 |
+---------------+-------------+


--------------------------------------------------------------------------------------------------
SELECT location_id, city, postal_code
FROM HR_LOCATIONS;

+-------------+---------------------+-------------+
| location_id | city                | postal_code |
+-------------+---------------------+-------------+
|        1000 | Roma                | 00989       |
|        1100 | Venice              | 10934       |
|        1200 | Tokyo               | 1689        |
|        1300 | Hiroshima           | 6823        |
...
|        2900 | Geneva              | 1730        |
|        3000 | Bern                | 3095        |
|        3100 | Utrecht             | 3029SK      |
|        3200 | Mexico City         | 11932       |
+-------------+---------------------+-------------+
```
- SELECT절에 '*' 기호를 단독으로 작성하는 경우 테이블의 모든 컬럼을 출력

```

SELECT *
FROM HR_LOCATIONS;

+-------------+------------------------------------------+-------------+---------------------+-------------------+------------+
| LOCATION_ID | STREET_ADDRESS                           | POSTAL_CODE | CITY                | STATE_PROVINCE    | COUNTRY_ID |
+-------------+------------------------------------------+-------------+---------------------+-------------------+------------+
|        1000 | 1297 Via Cola di Rie                     | 00989       | Roma                | NULL              | IT         |
|        1100 | 93091 Calle della Testa                  | 10934       | Venice              | NULL              | IT         |
|        1200 | 2017 Shinjuku-ku                         | 1689        | Tokyo               | Tokyo Prefecture  | JP         |
...
|        3000 | Murtenstrasse 921                        | 3095        | Bern                | BE                | CH         |
|        3100 | Pieter Breughelstraat 837                | 3029SK      | Utrecht             | Utrecht           | NL         |
|        3200 | Mariano Escobedo 9991                    | 11932       | Mexico City         | Distrito Federal, | MX         |
+-------------+------------------------------------------+-------------+---------------------+-------------------+------------+
```

### 표현식

- 간단한 수식의 작성으로 연산된 결과를 출력할 수 있는 문법
- 사칙연산 연산자 → 연산자 우선순위도 동일하게 적용
  - 연산 우선순위가 낮은 연산을 먼저 연산하고 싶은 경우 괄호()를 활용하여 순위를 바꿀 수 있음
  - 괄호를 중첩하여 사용하는 경우 안쪽의 괄호부터 연산

```
+	: 덧셈
-	: 뺄셈
*	: 곱셈
/	: 나눗셈
```

### NULL
- 테이블에 값 입력 시 컬럼의 입력값을 입력하지 않은 경우 기본적으로 입력되는 값
- 숫자, 문자, 날짜도 아닌 데이터타입으로 데이터타입 구문없이 어느 컬럼이건 입력이 가능
- 다만,연산에 영향을 미치는 부분이 있으므로 적극적인 활용은 피하는 것이 좋음
- NULL값이 포함된 표현식의 결과는 표현식의 내용과는 상관없이 NULL값으로 만들어짐

```
SELECT last_name, 12*salary*commission_pct, commission_pct FROM HR_EMPLOYEES;


+-------------+--------------------------+----------------+
| last_name   | 12*salary*commission_pct | commission_pct |
+-------------+--------------------------+----------------+
| Davies      |                     NULL |           NULL |
| Matos       |                     NULL |           NULL |
| Vargas      |                     NULL |           NULL |
| Russell     |                        0 |              0 |
| Partners    |                        0 |              0 |
| Errazuriz   |                        0 |              0 |
…
```

### column alias
- 결과 출력 시 컬럼이나 표현식의 이름을 다른 이름으로 대체하여 출력하는 문법
- alias는 구문이 실행되는 동안에만 유지 (영구 변환 X)
- 컬럼명 [AS] 별명
    - AS 키워드는 옵션으로 기능은 특별히 있지 않고 생략도 가능

```
SELECT last_name AS name, commission_pct comm
FROM HR_EMPLOYEES;

+-------------+------+
| name        | comm |
+-------------+------+
| King        | NULL |
| Kochhar     | NULL |
| De Haan     | NULL |


--------------------------------------------------------------------------------------------------

column alias에 공백을 포함하고 싶은 경우 ""기호로 묶어서 작성


SELECT last_name AS "last name", commission_pct comm FROM HR_EMPLOYEES;

+-------------+------+
| last name   | comm |
+-------------+------+
| King        | NULL |
| Kochhar     | NULL |
| De Haan     | NULL |
…
```

# [오후수업] JAVA 17차

## 생성자

### 상속에서의 생성자
- 생성자는 상속되지 않음
  (생성자의 이름은 클래스명과 동일해야하는데, 부모의 생성자는 자식과 이름이 다름)
- 서브클래스의 객체(인스턴스) 생성 시, 먼저 슈퍼클래스의 인스턴스를 생성한 후 서브클래스의 인스턴스가 생성됨
  - 이 때, 서브클래스의 생성자 내에서 먼저 자동으로 슈퍼클래스의 기본생성자를 호출
    - (생성자 super() 코드가 생략되어 있을 경우에도 암묵적으로 호출됨)
  - 슈퍼클래스의 생성자 내에서 작업이 모드 끝나면 다시 서브클래스의 생성자로 돌아와서 다음 코드들을 실행하게 됨
    - 즉, 슈퍼클래스 생성자의 코드가 먼저 실행된 후 서브클래스 생성자 코드가 실행됨

### 생성자 super()
- 서브클래스의 생성자 내에서 슈퍼클래스의 생성자를 명시적으로 호출하는 코드
- 서브클래스의 생성자 내에 생성자 super() 코드가 생략될 경우 컴파일러에 의해 슈퍼클래스의 기본생성자를 호출하는 super() 가 자동으로 추가됨
- 생성자 super()도 생성자 this()와 마찬가지로 생성자 내에서 반드시 첫번째로 실행되어야함 (주석문 제외)
  - 따라서, 생성자 this()와 생성자 super()는 하나의 생성자에서 기술 불가!
	- 만약, 오버로딩 생성 호출 this()와 슈퍼클래스를 호출하는 super()가 함께 실행되어야 하는 경우 생성자 this()를 통해 다른 생성자를 먼저 호출 후 해당 오버로딩 생성자 내에서 생성자 super()를 통해 슈퍼클래스에 접근 해야함
- 주로, 슈퍼클래스의 기본생성자가 없는 상태에서 파라미터 생성자만 정의했을 경우 서브클래스에서 슈퍼클래스 기본생성자를 호출하게 되면 오류기 발생하므로 이 때, 슈퍼클래스의 파라미터 생성자를 명시적으로 호출하는 용도로 사용

```java

public class Ex1 {

	public static void main(String[] args) {

		Child c = new Child();
		
	}

}

class Parent {
		String name;
		public Parent(String name) {
			this.name = name;
			System.out.println("Parent 생성자 호출됨!");
		}
}

class Child extends Parent {
	public Child() {
		super("홍길동");
		System.out.println("Child 생성자 호출됨!");
	}	
	
}

// ---------------------------------------------------------

class Person {
	String name;
	int age;
	
	public Person() {}
	public Person(String name, int age) {
		this.name = name;
		this.age = age;
	}
	
}

class SpiderMan extends Person {
	boolean isSpider;
	
	public SpiderMan(String name, int age, boolean isSpider) {
//		this.name = name;	// super.name = name; 과 동일
//		this.age = age;		// super.age = age; 와 동일
		
		// 이처럼 슈퍼클래스의 생성자와 상속받은 멤버변수에 대한 초기화 코드가 중복될 경우
		// 서브클래스의 생성자에서 직접 초기화 하지 않고 슈퍼클래스의 생성자를 호출하여
		// 간접적으로 상속받은 멤버변수에 대해 초기화를 수행하고,
		// 서브클래스에서 선언한 멤버변수만 직접 초기화하면 코드 중복 제거가 가능
		super(name, age);
		this.isSpider = isSpider;
	}
	
	
	/*
	 * 슈퍼클래스를 통해 간접적으로 초기화하도록 생성자를 자동 정의하는 단축키
	 * 1. Alt + Shift + S -> O 누른 후 슈퍼클래스의 파라미터 생성자를 선택하여 자동 생성
	 * 	  => 슈퍼클래스의 멤버변수를 슈퍼클래스에서, 서브클래스의 멤버변수를 서브클래스에서 초기화하는 생성자가 자동으로 생성됨
	 * 2. Alt + Shift + S - > C 누른 후 슈퍼클래스의 생성자 중 원하는 생성자 선택하면 
	 *    슈퍼클래스의 생성자와 동일한 형태의 생성자가 자동으로 생성됨
	 *    (주로, 서브클래스에 멥버변수가 없을 때 생성자 정의 시 사용)
	 * 
	 */
}
```

## 레퍼런스 형변환
- 참조형(레퍼런스 타입) 끼리의 형변환 (상속 관계에서만 사용 가능)
- 참조형 변수를 사용하여 다른 타입의 인스턴스 (객체)를 참조하기 위해 변환하는 것
- 업캐스팅(Up casting)과 다운캐스팅(Down casting)으로 분류됨

### 업캐스팅 & 다운캐스팅
1. 업캐스팅(Up casting)
- 슈퍼클래스의 레퍼런스가 서브클래스의 인스턴스를 참조하는 것
  - 서브클래스의 인스턴스를 슈퍼클래스 타입으로 변환하는 것
- 묵시적 형변환(자동 형변환)이 일어남
- 참조 가능 영역에 대한 축소 발생
  - 슈퍼클래스로부터 상속됨 멤버만 접근 가능하고, 서브클래스의 멤버는 접근 불가
- 일반적인 클래스간의 형변환은 대부분 업캐스팅을 의미함
- 업캐스팅을 통해 다형성(Polymorphism) 활용이 가능

2. 다운캐스팅 (Down casting)
- 서브클래스의 레퍼런스가 슈퍼클래스의 인스턴스를 참조하는 것(가리키는 것)
  - 슈퍼클래스의 인스턴스를 서브클래스 타입으로 변환하는 것
- 참조 가능한 영역에 대한 확대 발생
- 자동 형변환이 일어나지 않으므로, 명시적(강제)형변환이 필수
  - 즉, 형변환 연산자를 통해 좌변(서브클래스)의 데이터타입을 명시해야함

> - (중요!) 명시적 형변환을 통해 문법적(구문)오류가 해결되더라도 실제 실행 시점에서 오류가 발생할 수도 있음
>   - 참조 가능 영역의 확대로 인해 존재하지 않는 영역에 대한 참조의 위험성 때문

```java
package ref_casting;

public class Ex1 {

	public static void main(String[] args) {
		
//		Parent p = new Parent();		// 자동형변환 = 업캐스팅
//		Child c = (Child)new Parent();	// 강제형변환 = 다운캐스팅
		
//		int a = 10;
//		long l = a;
//		long l = (long)a;
		
		// -----------------------------------------------------------
		// 1. 참조 데이터타입에서의 묵시적(자동) 형변환 = 업캐스팅
		// -----------------------------------------------------------
		Child c = new Child();
		// Child 타입 참조변수로 접근 가능한 메서드 : 2개
		c.parentPrn();	// Parent꺼
		c.childPrn();	// Child꺼
		
		// 슈퍼클래스 타입 레퍼런스 변수 선언 
		Parent p;
		p = c; 	//	서브클래스 인스턴스 주소를 슈퍼클래스 타입 변수에 전달
		// => 슈퍼클래스 타입 변수(p)가 서브클래스 인스턴스(c)를 가리킴
		// => 이 때, 자동으로 Child -> Parent 변환이 일어나므로 업캐스팅
		
		p.parentPrn();
//		p.childPrn();	//	컴파일에러 발생! 서브클래스에서 정의한 메서드는 접근 불가능
		
		System.out.println("==================================");
		
		// -----------------------------------------------------------
		// 2. 참조 데이터타입에서의 명시적(강제) 형변환 = 다운캐스팅
		// -----------------------------------------------------------
				
		Parent p2 = new Parent();
		// 슈퍼클래스 타입 Parent 변수 p2로 접근 가능한 메서드 : 1개
		p2.parentPrn();
//		p2.childPrn();
		
		// 서브클래스 타입 레퍼런스 변수(c2) 선언
		Child c2;
//		c2 = p2;	//	컴파일에러 발생! 컴파일러에 의한 자동 형변환이 제공되지 않음
		//				따라서, Parent -> Child 타입으로 변환 시 명시적 형변환이 필수!
//		c2 = (Child)p2;
		
		// 만약, 변환이 가능했다고 가정했을 때 
		// 서브클래스 타입 레퍼런스 변수(c2)로 접근 가능한 메서드 : 2개
//		c2.parentPrn();	// Parent꺼
//		c2.childPrn();	// 실제 Parent 인스턴스에 "존재하지 않는" 메서드
		// => 주의! ChildPrn() 메서드는 실제 Parent 인스턴스에는 존재하지 않는 메서드이며
		//	  Child 클래스가 이 메서드를 갖고 있으므로 컴파일 시점에는 참조 가능한 메서드
		//	  따라서, 다운캐스팅 후에는 존재하지 않는 영역에 대한 참조 위험성이 발생하므로
		//	  자바에서는 대부분의 다운캐스팅은 인정하지 않는다! = 실행 시 오류 발생!
		System.out.println("==============================");
		// -----------------------------------------------------------
		// 3. 다운캐스팅이 가능한 경우
		// => 이전에 이미 업캐스팅 된 인스턴스를 다시 다운캐스팅 하는 경우
		// -----------------------------------------------------------
		Parent p3 = new Child();	//	Child -> Parent 로 업캐스팅(자동 형변환)
		// 슈퍼클래스(Parent) 타입 변수 p3로 접근 가능한 메서드 : 1개
		p3.parentPrn();
//		p3.childPrn();
		
		Child c3 = (Child)p3;
		
		c3.parentPrn();
		c3.childPrn();
		
		/*
		 * 다운캐스팅 (Down casting) - 결론
		 * - 서브클래스의 레퍼런스가 슈퍼클래스의 인스턴스를 참조하는 것
		 * 	- 슈퍼클래스의 인스턴스를 서브클래스 타입으로 변환하는 것
		 * - 참조 가능한 영역에 대한 확대 발생
		 * - 자동 형변환이 일어나지 않으므로, 명시적(강제) 형변환이 필수!
		 * 	 => 즉, 형변환 연산자를 통해 좌변(서브클래스)의 데이터타입을 명시!
		 * 	 --------------------------------------------
		 * => 최종결론
		 * 	  예전에 이미 업캐스팅 된 레퍼런스를 다시 다운캐스팅 하는 경우에만 안전함
		 * 	  (그 외의 다운캐스팅은 모두 인정되지 않음)
		 * 
		 */
		
		
	}

}



class Parent {
	public void parentPrn() {
		System.out.println("슈퍼클래스의 parentPrn()");
	}
}

class Child extends Parent {
	public void childPrn() {
		System.out.println("서브클래스의 childPrn()");
	}
}

```

캐스팅 연습
```java
package ref_casting;

public class Ex2 {

	public static void main(String[] args) {
		// 업캐스팅과 다운캐스팅 연습
		// 서브클래스(스마트폰) 타입 인스턴스 생성
		
		스마트폰 갤럭시노트 = new 스마트폰();
		갤럭시노트.전화();
		갤럭시노트.문자();
		갤럭시노트.카톡();
		
		// 슈퍼클래스(핸드폰) 타입 인스턴스 생성
		핸드폰 어머니폰 = new 핸드폰();
		어머니폰.전화();
		어머니폰.문자();
//		어머니폰.카톡();
		
		System.out.println("---------------------------------");
		// 업캐스팅 (스마트폰 -> 핸드폰)
		// 스마트폰(갤럭시노트)을 어머니께 드리는 경우
		어머니폰 = 갤럭시노트;	// 업캐스팅 = 자동 형변환 가능(별도의 형변환 연산자 필요 없음)
		어머니폰.전화();	// 핸드폰에서 사용 가능한 기능
		어머니폰.문자();	// 핸드폰에서 사용 가능한 기능
//		어머니폰.카톡();	// 핸드폰에서 사용 불가능한 기능!
		
		// 만약, 동생의 스마트폰(아이폰)을 어머니께 드리는 경우
		스마트폰 아이폰 = new 스마트폰();
		어머니폰 = 아이폰;
		어머니폰.전화();
		어머니폰.문자();
//		어머니폰.카톡()
		
		System.out.println("---------------------------------");
		
		// 다운캐스팅(핸드폰 -> 스마트폰)
		// 어머니 새 스마트폰을 사드렸을 경우
		어머니폰 = new 스마트폰();
		어머니폰.전화();
		어머니폰.문자();
//		어머니폰.카톡();
		
		// 어머니 스마트폰을 내가 가져다 쓰는 경
		스마트폰 내폰 = (스마트폰)어머니폰;	// 자동 형변환 불가능 => 명시적 형변환 필수!
		내폰.전화();
		내폰.문자();
		내폰.카톡();
		
		System.out.println("------------ 다운캐스팅 오류 케이스 ------------");
		핸드폰 아버지폰 = new 핸드폰();
		// 아버지 2G폰 내가 가져다 쓰는 경우
		내폰 = (스마트폰)아버지폰;
		내폰.전화();
		내폰.문자();
		내폰.카톡();
	}

}

class 전화기 {
	public void 전화() {
		System.out.println("전화 걸기!");
	}
}

class 핸드폰 extends 전화기 {
	public void 문자() {
		System.out.println("문자 전송!");
	}
}

class 스마트폰 extends 핸드폰 {
	public void 카톡() {
		System.out.println("카톡!");
	}
}
```

## 클래스들의 관계
### Has-a (포함 관계)
- 어떤 객체가 다른 객체에 포함되는 관계
- 대부분의 클래스들의 관계는 Has-a 관계 적용됨
- 자동차 has a 타이어, 스마트폰 has a 스피커, 영웅 has a 무기

	1) 집합 관계
		- 객체가 다른 객체에 포함될 때 해당 객체가 없어도 동작에 문제가 없는 관계
    - 객체 상호간의 라이프 사이클이 다른 관계
      - 자동차 has a 라디오 => 자동차는 라디오가 없어도 자동차
      - 컴퓨터 has a 스피커 => 컴퓨터는 스피커가 없어도 컴퓨터
      - 영웅  has a  무기   => 영웅은 무기가 없어도 영웅
   
	2) 구성 관계
		- 객체가 다른 객체에 포함될 때 해당 객체 없이는 동작이 불가능한 관계
		- 객체가 다른 객체에 포함될 때 해당 객체 없이는 동작이 불가능한 관계
		  - 자동차 has a 엔진, 컴퓨터 has a CPU

### Is-a (상속 관계 kind of)
- 비슷한 속성 및 동작을 갖는 객체 사이의 관계
	- ex) 초등학생, 중학생, 고등학생 객체들의 공통점은 학생
	  - 이 때, 학생은 초등학생, 중학생, 고등학생의 상위 개념이므로 모두를 포함
	  - 초등학생 is a 학생 => 학생의 모든 구성요소는 초등학생이 갖고 있음
	  - 스마트폰 is a 핸드폰 => 핸드폰의 모든 구성요소는 스마트폰이 갖고 있음
- Is-a 관계가 성립하는 경우 좌변의 객체는 우변의 객체를 상속받아 정의한 객체 정립
- Is-a 관계를 판별하는데 사용하는 연산자 : Instanceof 연산자

### instanceof 연산자
- 좌변의 객체(참조변수)가 우변(클래스) 타입인지 판별하는 연산자
  - (=> Is-a 관계를 판별하는 연산자)
- 판별 결과는 boolean 타입으로 리턴되며, 결과값이 true 이면 형변환 가능한 관계
  - (=> 업캐스팅 또는 다운캐스팅 모두 true 가 리턴됨)
	  - 만약, false가 리턴될 경우 어떤 경우에도 형변환이 불가능!

```
< 기본 문법 >
if(A instanceof B){ // => A는 참조변수, B는 클래스 또는 인터페이스
	// A is a B 가 성립 될 때 실행할 코드들...(업캐스팅 또는 다운캐스팅 등)
}	
```

```java
package Polymorphism;

public class Ex1 {

	public static void main(String[] args) {

		SmartPhone sp = new SmartPhone("010-1234-5678", "안드로이드");
		// SmartPhone 타입으로 접근 가능한 메서드
		sp.call();
		sp.sms();
		sp.kakaoTalk();
		
		// instanceof 연산자 활용
		// sp 는 SmartPhone 입니까?
		if(sp instanceof SmartPhone) {
			System.out.println("sp는 SmartPhone이다!");
		} else {
			System.out.println("sp는 SmartPhone이 아니다다!");
		}
		
		System.out.println("---------------------------------------");
		
		if(sp instanceof HandPhone) {
			System.out.println("sp는 HandPhone이다!");
		} else {
			System.out.println("sp는 HandPhone이 아니다!");
		}
		
		System.out.println("---------------------------------------");
		
		// 서브클래스(SmartPhone) 타입 변수 sp를
		// 슈퍼클래스(HandPhone) 타입으로 변환 가능한지 여부 판별을 위한 질문
		
		HandPhone hp = new HandPhone("010-1111-2222");
		
		// hp는 SmartPhone 입니까?
		if(hp instanceof SmartPhone) {
			System.out.println("hp는 SmartPhone이다!");
		} else {
			System.out.println("hp는 SmartPhone이 아니다!");
			System.out.println("그러므로, hp는 SmartPhone으로 형변환이 불가능!");
			System.out.println("hp는 SmartPhone이 가진 기능을 모두 포함하지 않는다!");
		}
		
		System.out.println("---------------------------------------");
		
		// SmartPhone -> HandPhone 타입으로 업캐스팅
		HandPhone hp2 = new SmartPhone("010-1234-5678", "안드로이드");
		hp2.call();
		hp2.sms();
//		hp2.kakaoTalk();	//	사용불가능한 기능
		
		if(hp2 instanceof SmartPhone) {
			System.out.println("hp2는 SmartPhone이다!");
		} else {
			System.out.println("hp2는 SmartPhone이 아니다!");
		}
		
		System.out.println("---------------------------------------");

		if(hp2 instanceof HandPhone) {
			System.out.println("hp2는 HandPhone이다!");
		} else {
			System.out.println("hp2는 HandPhone이 아니다!");
		}
		
		
		
		
	}

}

class HandPhone {
	String number;
	
	public HandPhone(String number) {
		this.number = number;
	}
	
	public void call() {
		System.out.println("전화 기능!");
	}
	
	public void sms() {
		System.out.println("문자 기능!");
	}
}

class SmartPhone extends HandPhone {
	String osName;

	public SmartPhone(String number, String osName) {
		// 슈퍼 클래스(HandPhone) 생성자 중 기본생성자가 없으므로
		// 반드시 파라미터 생성자 명시적으로 호출해야함
		super(number);
		this.osName = osName;
	}
	
	public void kakaoTalk() {
		System.out.println("카톡 기능!");
	}
	
}
```
