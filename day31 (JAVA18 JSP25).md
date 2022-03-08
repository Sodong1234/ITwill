# [오전수업] JAVA 18차

## 동적 바인딩
- 컴파일(번역) 시점에서 실행될 것으로 예상되는 코드와 실제 실행 시점에서 해당 객체의 타입 기준으로 메서드가 달라지는 것

```java
package Polymorphism;

public class Ex1 {

	public static void main(String[] args) {

		Employee emp = new Employee("홍길동", 3000);
		System.out.println("emp.getEmployee() : " + emp.getEmployee());
		System.out.println("-----------------------------------");
		
		// 서브클래스(Manager) 인스턴스 생성
		// => 파라미터 생성자 호출하여 "이순신", 4000, "개발팀" 전달
		
		Manager man = new Manager("이순신", 4000, "개발팀");
		System.out.println("man.getEmployee() : " + man.getEmployee());
		System.out.println("-----------------------------------");

		// Manager -> Employee 업캐스팅
		emp = man; // 업캐스팅은 자동 형변환 가능(묵시적 형변환) = 형변환 연산자 생략 가능
		// 업캐스팅 후에는 참조영역에 대한 축소가 발생
		// 단, 현재 Manager 클래스에는 별도의 추가 메서드가 없으므로 기능 동일
		System.out.println("emp.getEmployee() : " + emp.getEmployee());
		// => 코드상(컴파일시점)으로는 Employee 클래스의 getEmployee() 메서드가 호출될 것으로 보이지만
		// 	  실제 emp 변수에 저장된 인스턴스(객체) Manager 클래스의 인스턴스이므로
		//	  실행 시점에는 Manager 클래스에 오버라이딩 된 getEmployee() 메서드가 실행 됨!
		
		System.out.println("-----------------------------------");
		
		// 서브클래스(Engineer) 인스턴스 생성
		// => "강감찬", 2000, "자바개발" 로 초기화
		Engineer eng = new Engineer("강감찬", 2000, "자바개발");
		System.out.println("eng.getEmployee() : " + eng.getEmployee());
		
		// Enginner(eng) -> Employee(emp2) 업캐스팅
		Employee emp2 = eng;
		System.out.println("emp2.getEmplyee() : " + emp2.getEmployee());
		
		// Manager 인스턴스를 생성하여 Employee(emp2) 로 업캐스팅
		
		emp2 = new Manager("이순신", 5000, "개발팀");
		
		// 출력문에서 실행되는 메서드는 문법적으로 차이가 없지만
		// 실제 참조하는 인스턴스가 달라지므로 결과값이 다름
		System.out.println("emp2.getEmployee() : " + emp2.getEmployee());
	}

}


/*
 * 사원 클래스(Employee) 정의
 * 멤버변수 : 사원명(name, 문자열), 연봉(salary, 정수)
 * 생성자
 * - 기본 생성자
 * - 파라미터 생성자 : 사원명과 연봉을 전달받아 초기화
 * 메서드
 * - getEmployee() 메서드 : 파라미터 없음, 리턴타입 String(이름과 연봉을 문자열로 결합하여 리턴)
 * 						  			  ex) "홍길동, 3000" 리턴
 * 
 * */

class Employee {
	String name;
	int salary;
	
	public Employee() {}
	
	public Employee(String name, int salary) {
		this.name = name;
		this.salary = salary;
	}
	
	public String getEmployee() {
		return name + ", " + salary;
		
	}
	
}

/*
 * Manager 클래스 정의 - Employee 클래스 상속
 * 멤버변수 : 부서명(depart, 문자열)
 * 생성자 : 파라미터생성자 - 사원명, 연봉, 부서명을 전달받아 초기화
 * 메서드 : getEmployee() 메서드 오버라이딩
 * 	 	  사원명, 연봉, 부서명을 문자열로 결합하여 리턴
 * 
 */

class Manager extends Employee {
	String depart;

	public Manager(String name, int salary, String depart) {
		super(name, salary);
		this.depart = depart;
	}
	
	@Override
	public String getEmployee() {
		return super.getEmployee() + ", " + depart;
	}
	
}

/*
 * Engineer 클래스 정의 - Employee 클래스 상속
 * 멤버변수 : 기술명(skill, 문자열)
 * 생성자 : 파라미터 생성자 - 사원명, 연봉, 기술명을 전달받아 초기화
 * 메서드 : getEmployee() 메서드 오버라이딩
 * 	 	  사원명, 연봉, 기술명을 문자열로 결합하여 리턴
 */ 

  class Engineer extends Employee {
  String skill;

public Engineer(String name, int salary, String skill) {
	super(name, salary);
	this.skill = skill;
}

@Override
public String getEmployee() {
	return super.getEmployee() + ", " + skill;
}

  }
```

## 다형성(Polymorphism)
- 하나의 클래스 타입 참조변수로 여러 인스턴스를 참조할 수 있는 특성
- 서브클래스 타입 인스턴스들을 슈퍼클래스 타입으로 업캐스팅 하여 공통된 방법으로 여러 인스턴스의 멤버에 접근
  - ex) Employee 타입 변수로 Manager와 Engineer 인스턴스를 참조하여 각 객체 내의 메서드(getEmployee() 등) 를 공통으로 다루는 것
- 슈퍼클래스 타입 배열에 서브클래스 타입 인스턴스를 저장하거나 메서드 정의 시 매개변수 하나에 여러 타입의 인스턴스를 파라미터로 전달 할 때 활용

```
< 주의사항! >
업캐스팅 후 슈퍼클래스 타입으로 서브클래스를 다룰 때 멤버변수는 참조변수 타입에 따라 결정되고, 메서드는 실제 인스턴스에 따라 결정됨
=> 메서드 오버라이딩 시 업캐스팅 후에도 오버라이딩 된 메서드만 호출되지만 
   변수에 대한 오버라이딩 시 업캐스팅 후에는 해당 변수에 접근하는 참조변수 타입에 따라 접근하는 변수가 달라짐
```

```java
package Polymorphism;

public class Ex2 {

	public static void main(String[] args) {


//		C c = new C();
//		System.out.println(c.name);
//		c.print();
//		
//		// 업캐스팅
//		P p = c;
//		System.out.println(p.name);
//		p.print();
		
		System.out.println("----------------------------------------------");
		
		Circle c = new Circle();
		Rectangle r = new Rectangle();
		Triangle t = new Triangle();
		
		// 만약, 상속을 통한 메서드 오버라이딩 없이 각 클래스 자율적으로 메서드를 정의할 경우
		// => 각각의 클래스의 메서드가 다르므로 코드의 통일성이 사라짐
		c.circleDraw();
		r.paint();
		
		// 그러나, 추상화를 통해 슈퍼클래스인 Shape 클래스를 정의하고
		// 상속을 통해 Shape 클래스의 draw() 메서드를 오버라이딩 하게 되면
		// 각 인스턴스에서 호출하는 메서드가 동일하므로 코드의 통일성이 향상됨
		c.draw();
		r.draw();
		t.draw();
		
		System.out.println("----------------------------------------------");
		Shape s = c;
		s.draw();
		
		s = r;
		s.draw();
		
		s = t;
		s.draw();
		
		System.out.println("----------------------------------------------");

		// 다형성을 배열에 적용시키는 경우
		Shape[] sArr = new Shape[3];
		
		// Shape 타입 배열 각 인덱스에 Circle, Rectangle, Triangle 인스턴스 생성하여 저장
		// => Shape 타입 배열은 Shape 클래스 타입에 해당하는 주소를 저장하는 배열이며
		// 	  Circle, Rectangle, Triangle 클래스의 인스턴스는 업캐스팅되어 주소가 저장됨
		sArr[0] = new Circle();
		sArr[1] = new Rectangle();
		sArr[2] = new Triangle();
		
		for(int i = 0; i< sArr.length; i++) {
			sArr[i].draw();
		}
		
		// 향상된 for문
				for(int i = 0; i < sArr.length; i++) {
					Shape shape = sArr[i];
					shape.draw();
				}
				
				for(Shape shape : sArr) {
					shape.draw();
				}
		
		System.out.println("----------------------------------------------");

		
		
		
		
	}
	
	public static void polymorphism(Shape s) {
		// 만약, 각 도형에 따라 추가적인 작업이 필요한 경우
		// instanceof 연산자를 사용하여 각 객체를 구별한 후
		// 객체마다 다른 작업을 수행할 수 있다!
		if(s instanceof Circle) {
			System.out.println("Circle 객체!");
			Circle c = (Circle)s;
			c.circleDraw(); // 서브클래스에서 정의한 메서드
			c.draw();		// 상속받은 메서드(오버라이딩)
			
		} else if (s instanceof Rectangle) {
			System.out.println("Rectangle 객체!");
			Rectangle r = (Rectangle)s;
			r.paint();		// 서브클래스에서 정의한 메서드
			r.draw();		// 상속받은 메서드(오버라이딩)
		} else if(s instanceof Triangle) {
			System.out.println("Triangle 객체!");
			Triangle t = (Triangle)s;
			t.draw();
		}
	}
}

// 도형(Shape) 클래스 정의
class Shape {
	public void draw() {
		System.out.println("도형 그리기!");
	}
	
}
// 원(Circle) 클래스 정의 - 도형(Shape)클래스 상속
// => 메서드 오버라이딩 : "원 그리기!" 출력
class Circle extends Shape {
	
	public void circleDraw() {
		System.out.println("원 그리기!");
	}
	@Override
	public void draw() {
		System.out.println("원 그리기!");
	}
}

// 사각형(Revtangle) 클래스 정의 - 도형(Shape)
// => 메서드 오버라이딩 : "사각형 그리기!" 출력
class Rectangle extends Shape {
	
	public void paint() {
		System.out.println("사각형 그리기!");
	}

	@Override
	public void draw() {
		System.out.println("사각형 그리기!");
}

}
// 삼각형(Triangle) 클래스 정의 - 도형(Shape) 클래스 상속
// => 메서드 오버라이딩 : "삼각형 그리기!" 출력
class Triangle extends Shape {
	@Override
	public void draw() {
		System.out.println("삼각형 그리기!");
}

}
	
	
	

class P {
	String name = "P";
	public void print() {
		System.out.println("부모");
	}
}

class C extends P {
	String name = "C";
	
	@Override
	public void print() {
		System.out.println("자식");
	}
}
```

## final 키워드
- 클래스, 메서드, 멤버변수에 지정 가능
- 정말 마지막 이라는 뜻

1) 멤버변수에 final이 붙은 경우
- 변수값 변경 불가 = 상수로 취급됨
  - 즉, 기존에 저장된 값을 사용하는 것만 가능하고, 값을 변경할 수는 없음
- 기본문법 : final 데이터타입 변수명;
	- ex) 원주율 계산을 위한 파이(PI) 값은 변경되면 안되므로 변수에 final 표기 

2) 메서드에 final이 붙은 경우
- 메서드 변경 불가 = 오버라이딩 금지 (재정의 X)
	- 즉, 슈퍼클래스의 메서드를 상속받아 사용하는 것은 가능하나 오버라이딩을 통해 슈퍼클래스의 메서드를 변경(수정) 할 수 없음
- 기본문법 : [접근제한자] final 리턴타입 메서드명(매개변수...){}

3) 클래스에 final이 붙은 경우
- 클래스 변경 불가 = 클래스를 더 이상 확장 할 수 없음 = 상속 X
	- 즉, 특정 클래스 자체를 그대로 사용하는 것은 가능하나 다른 클래스에서 해당 클래스를 상속 받을 수 없음
	- 어떤 클래스 자체로 이미 완전한 클래스 기능을 수행하는 경우 상속을 금지시킴
  - ex) String 클래스, Math 클래스 등
- final 메서드보다 더 광범위한 제한을 두도록 할 때 사용
- 기본 문법 : [접근제한자] final class 클래스명 {}


```java
package final_statement;

public class Ex1 {

	public static void main(String[] args) {

		FinalMemberVariable fmv = new FinalMemberVariable();
		fmv.normalVar = 99;
//		fmv.finalVar = 999; // 오류 발생! final로 선언된 멤버변수는 값 변경 불가!
		System.out.println(fmv.normalVar);
		System.out.println(fmv.finalVar);

	}

}

// ----- final 멤버변수 -----
class FinalMemberVariable {
	int normalVar = 10;
	final int finalVar = 20;
}
//----- final 메서드 -----
class FinalMethod {
	public void normalMethod() {
		System.out.println("normalMethod()");
	}
	
	public final void finalMethod() {
		System.out.println("finalMethod()");
	}
}

class SubClassFinalMethod extends FinalMethod {
	@Override
	public void normalMethod() {
		System.out.println("서브 클래스에서 오버라이딩 된 normalMethod()");
	}
	
//	@Override
//	public final void finalMethod() {
//		System.out.println("~~~");
		// 오류발생! final 메서드는 서브클래스에서 오버라이딩 불가!
//	}
}


//----- final 클래스 -----
final class FinalClass {}

//class SubClassFinalClass extends FinalClass {
//	// 오류 발생! final 클래스 상속 불가!
//}

class OtherClass {
	// final 클래스는 상속(is-a관계)은 불가능 하나
	// 포함(has-a)은 가능하다!
	FinalClass fc = new FinalClass();
}

//final 클래스의 대표적인 예 : String 클래스
//class OtherClass2 extends String {} // 상속 불가!

```

# [오후수업] JSP 25차
