# [오전수업] JAVA 33차
### 2. Supplier 계열(공급자)
 - 파라미터를 받지 않는다.
 - 자체적으로 리턴할 값을 생성한다.

```java
package Lambda;

import java.util.Random;
import java.util.function.IntSupplier;
import java.util.function.Supplier;

public class Ex1 {

	public static void main(String[] args) {
		
		// 익명의 내부클래스 방식
		Supplier<String> supplier = new Supplier<String>() {
			
			@Override
			public String get() {
				return "Hello";
			}
		};
		
		System.out.println(supplier.get());
		
		// ===========================================
		// 람다식 변수명 : supplier2, 리턴값 :  "Hello2" -> 출력
//		Supplier<String> supplier2 = () -> { return "Hello2"; };
//		System.out.println(supplier2.get());
		
		// === supplier2 수정 생략 가능 최소 문법 ===
		// 파라미터없음 : 소괄호() 생략 불가
		// 단순한 리턴문 1개 : 중괄호{} 생략 가능, return 생략 가능
		// <T>의 값이 return 타입을 결정
		Supplier<String> supplier2 = () -> "Hello2";
		System.out.println(supplier2.get());
		
		// PSupplier
		// P: Boolean, Integer, Long, Double을 의미
		
		IntSupplier supplier3 = () -> {
			Random random = new Random();
			return random.nextInt(6);
		};
		System.out.println(supplier3.getAsInt());
		
		IntSupplier supplier4 = () -> new Random().nextInt(10) + 1;
		System.out.println(supplier4.getAsInt());
	}
	
}
```

### 3. Function 계열
- 파라미터 리턴 모두 존재

```java
package Lambda;

import java.util.function.BiFunction;
import java.util.function.Function;

public class Ex2 {

	public static void main(String[] args) {
  
		// 람다식
		FunctionTest ft = new FunctionTest();
		ft.addPerson((name, age) -> new Person(name, age), "홍길동", 10);
		System.out.println(ft.person);
		
		ft.printPerson((name) -> {
			
			if(ft.person.name.equals(name)) {
				return ft.person.toString();
			} else {
				return "unknown user";
			}
			
		}, "홍길동");
		
		ft.printPerson(name -> ft.person.name.equals(name) ? ft.person.toString() : "unknown user", "홍길동");
		
		System.out.println("-------------------------------");
		
		// 익명내부클래스로 코딩
		FunctionTest ft2 = new FunctionTest();
		ft2.addPerson(new BiFunction<String, Integer, Person>() {

			@Override
			public Person apply(String t, Integer u) {
				return new Person(t, u);
			}
		}, "이순신", 20);
		System.out.println(ft2.person);
		
		ft2.printPerson(new Function<String, String>() {

			@Override
			public String apply(String t) {
				
				if(ft2.person.name.equals(t)) {
					return ft2.person.toString();
				} else {
					return "unknown user";
				}
			}
			
		}, "이순신");
	}

}

class Person {
	String name;
	int age;
	
	// Alt + Shift + S -> O
	public Person(String name, int age) {
		this.name = name;
		this.age = age;
	}

	// Alt + Shift + S -> S
	@Override
	public String toString() {
		return "Person [name=" + name + ", age=" + age + "]";
	}
	
}

class FunctionTest {
	
	Person person;
	
	// String과 Integer를 매개변수로 받고 Person을 리턴하는
	// BiFunction 타입에 String, Integer를 받는 메서드 apply() 메서드 활용
	public void addPerson(BiFunction<String, Integer, Person> function, String name, Integer age) {
		person = function.apply(name, age);
	}
	
	// String을 파라미터로 받고 String을 리턴하는
	// Function 타입에 String을 받는 apply() 메서드 활용
	public void printPerson(Function<String, String> function, String name) {
		System.out.println(function.apply(name));
	}
}
```

### 4. Operation 계열
- Function 계열과 유사하게 파라미터와 리턴을 갖는다.
- 차이점은 Operator 계열은 매개변수탑이 곧 리턴타입이 된다.
- 매개변수를 전달받아 "어떠한 연산" 후 같은 타입을 리턴할때 사용

```java
package Lambda;

import java.util.function.DoubleBinaryOperator;
import java.util.function.UnaryOperator;

public class Ex3 {

	public static void main(String[] args) {
		/*
		 * 4. Operation 계열
		 * - Function 계열과 유사하게 파라미터와 리턴을 갖는다.
		 * - 차이점은 Operator 계열은 매개변수탑이 곧 리턴타입이 된다.
		 * - 매개변수를 전달받아 "어떠한 연산" 후 같은 타입을 리턴할때 사용
		 */
		UnaryOperator<Double> op1 = x -> Math.pow(x, 2);
		UnaryOperator<Double> op2 = new UnaryOperator<Double>() {

			@Override
			public Double apply(Double x) {
				return Math.pow(x, 2);
			}
		};
		
		System.out.println(op1.apply(20.0));
		System.out.println(op2.apply(10.0));

		DoubleBinaryOperator op3 = (x, y) -> Math.pow(x, y);
		DoubleBinaryOperator op4 = new DoubleBinaryOperator() {
			
			@Override
			public double applyAsDouble(double x, double y) {
				return Math.pow(x, y);
			}
		};
		
		System.out.println(op3.applyAsDouble(100, 2));
		System.out.println(op4.applyAsDouble(3, 3));
	}

}
```
### 5. Predicate 계열
- Function, Operator와 같이 파라미터와 리턴을 갖는다.
- 리턴타입이 boolean 으로 고정되어있다.
- 매개변수로 전달받은 데이터를 "어떠한 판단" 후 true/false 리턴

```java
package Lambda;

import java.util.function.IntPredicate;

public class Ex4 {

	public static void main(String[] args) {

		PredicateTest pt = new PredicateTest();
		pt.printSome(new IntPredicate() {
			
			@Override
			public boolean test(int value) {
				return value%3 == 0;
			}
		});
		pt.printSome(value -> value%3 == 0);
		pt.printSome(value -> value > 5);
		
	}

}

class PredicateTest {
	int[] nums = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
	
	public void printSome(IntPredicate pred) {
		for(int num : nums) {
			if(pred.test(num)) {
				System.out.println(num + " ");
			}
		}
		System.out.println();
		
	}
}
```

### 스트림(Stream)
- 여러 자료의 처리에 대한 기능을 구현해 놓은 클래스
- 배열, 컬렉션 등의 자료를 일관성 있게 처리할 수 있다
  - (자료형에 따라 기능을 각각 새로 구현하는것이 아닌 자료형에 상관없이 같은 방식으로 메서드를 호출할 수 있기 때문)

** 스트림의 특징
- 자료의 대상과 관계없이 동일한 연산을 수행한다.
- 한 번 생성하고 사용한 스트림은 재사용할 수 없다.
- 스트림의 연산은 기존 자료를 변경하지 않는다. (스트림 연산을 위해 사용하는 메모리 공간이 별도로 존재)
- 중간연산과 최종연산이 존재한다.
  - 중간연산은 여러개가 적용될 수 있고, 최종연산은 맨 마지막에 한 번 적용.
  - 최종연산이 호출되지 않으면 중간연산이 아무리 많더라도 적용되지 않는다. 이를 지연연산 'lazy evalutaion' 이라고 함.

```java
package Lambda;

import java.util.Arrays;

public class Ex5 {

	public static void main(String[] args) {
  
		// 일반적인 배열
		int[] arr = {1, 2, 3, 4, 5};
		for(int i = 0; i < arr.length; i++) {
			System.out.println(arr[i]);
		}

		// 스트림을 활용한 람다식
		Arrays.stream(arr).forEach(value -> System.out.println(value));
		
		/*
		 * 스트림의 연산
		 * - 중간연산: filter(), map()
		 * - 최종연산: forEach(), count(), sum(), reduce()
		 * 
		 */
		
		// filter : 조건이 참인 경우만 추출하는 경우 사용
		String[] str = {"아이티윌", "부산", "교육센터"};
		Arrays.stream(str).filter(s -> s.length() >= 3).forEach(s -> System.out.println(s));
		
		// map : 요소들을 순회하여 다른형식으로 변환
		Arrays.stream(arr).map(n -> n+10).forEach(n -> System.out.println(n));
		
		// 중간연산은 해당 조건이나 함수에 맞는 결과를 추출해 내는 중간 역할
		// 결국, 최종연산이 있어야한다.
		
		System.out.println(Arrays.toString(arr));
		System.out.println(Arrays.toString(str));
	}

}


```

### 람다식 사용의 장단점
```java
package Lambda;

import java.util.Arrays;
import java.util.List;

public class Ex6 {

	public static void main(String[] args) {
		/*
		 * < 람다식 사용의 장점 >
		 * 1. 코드의 간결성
		 * - 기본적으로 "익명의 내부클래스" 형태보다 간결함
		 * - 실행문이 간단할 경우 그 효과가 더욱 부각됨
		 * 	 => 어떤 로직인지 바로바로 알아보기 편하다. (가독성이 좋다)
		 * 
		 * 2. "지연 연산" (Lazy Evaluation)을 이용하여 향상된 퍼포먼스를 보여줄 수 있다.
		 * - 연산은 나중으로 미루어 두었다가 한꺼번에 연산하는 방식
		 * - 메모리상의 효율성 및 불필요한 연산은 배제가 가능
		 *
		 * < 람다식 사용의 단점 >
		 * 1. 모든 원소를 순회하는 경우 어떤방법으로 작성하더라도 람다식이 조금 느릴 수 밖에 없다.
		 *    (최종 출력되는 bytecode는 단순 반복문 보다 몇 단계를 더 거칠 수 밖에 없다.)
		 *    
		 * 2. 디버깅 시 추적이 극도로 어렵다.
		 * 	  (거의 불가능에 가깝다
		 * 	  -> 람다식으로 작성한 로직은 실수없이 완벽해야한다.
		 * 	  -> 간단한 로직이어야한다.)
		 * 
		 * 3. 람다식이 남용되면 오히려 코드를 이해하기 어려울 수 있다. (가독성이 떨어진다.)
		 * 
		 * < 최종 결론 >
		 * - 단순하고 간결한 로직이 그때 그때(일회성) 사용될 경우 람다식 O
		 * 	 => 어떠한 경우에서만 로직이 조금 수정될 경우 메서드로 호출하면 호출하고 있는 포인트를 전부 찾아
		 * 	    일일이 수정하거나 수정된 부분만 새로운 메서드를 만들어서 호출하도록 변경해야한다.
		 * 		(결국, 메서드를 재사용하지 않고 상황별 메서드가 따로 생기게 됨)
		 * 
		 * - 로직이 조금 복잡하거나(실행문이 많을 경우) 가독성이 떨어지는 경우 람다식 X
		 * 	 => 복잡한 로직 = 가독성이 안 좋음
		 * 	 => 실행문이 많다 = 디버깅할 일이 생긴다!
		 */

		// 지연 연산 예
		// 잘못 해석)
		// - 6보다 작은 수를 모두 filter
		// - filter된 결과에서 짝수인 것들만 filter
		// - filter된 결과 모두 10을 곱한다.
		// - 그 중 제일 첫번째 요소를 return 한다.
		
		// 실제동작)
		// - 6보다 작은지 판단
		// - 짝수인지 판단
		// - 10을 곱한다.
		// - 위 모든 판단들을 만족하는 첫번째 요소에다가 10을 곱하는 연산을 한 후 더 이상 연산하지 않는다.
		// 즉, 불필요한 연산을 하지 않는다.
		
		// 6보다 작은 값 -> 짝수인 것만 -> 결과에 모두 10을 곱한 후 -> 제일 첫번째 요소
		List<Integer> list = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);
		
		// 람다식
		int n = list.stream().filter(i -> i < 6)	// 6보다 작은것만
							 .filter(i -> i%2 == 0)	// 짝수인것만
							 .map(i -> i * 10)		// 10을 곱학고
							 .findFirst().get();
		System.out.println(n);
		
		// 일반적인 for문으로 구현
		int result = 0;
		for(Integer i : list) {
			if(i < 6) {
				if(i % 2 == 0) {
					i *= 10;
					result = i;
					break;
				}
			}
		}
		System.out.println(result);
	}

}
```


연습문제
```java
package Lambda;

import java.util.ArrayList;
import java.util.Comparator;
import java.util.List;

public class Ex7 {

	public static void main(String[] args) {
		/*
		 * 1. Customer 객체 5개 생성
		 * 2. ArrayList에 추가
		 */
		Customer customer1 = new Customer("홍길동", 40, 100);
		Customer customer2 = new Customer("이순신", 50, 200);
		Customer customer3 = new Customer("세정", 30, 20);
		Customer customer4 = new Customer("송강", 20, 30);
		Customer customer5 = new Customer("박민영", 25, 50);

		List<Customer> list = new ArrayList<Customer>();
		list.add(customer1);
		list.add(customer2);
		list.add(customer3);
		list.add(customer4);
		list.add(customer5);

		
		System.out.println("=== 고객 이름 추가된 순서대로 출력 ===");
		for(Customer customer : list) {
			System.out.println(customer.getName());
		}
		System.out.println("-------------------------------------------");
		list.stream().forEach(customer -> System.out.println(customer.getName()));

		int sum = 0;
		for(Customer customer : list) {
			sum += customer.getPrice();
		}
System.out.println("총 요금은: "+sum+" 입니다." );
		
		int sum2 = list.stream().mapToInt(c -> c.getPrice()).sum();
		System.out.println("총 요금은: " + sum2 + " 입니다." );
		
		System.out.println("===== 30세 이상 고객중 요금(price)이 큰순으로 출력 =====");
		list.stream().filter(c -> c.getAge() >= 30)
					 .sorted((o1, o2) -> (o2.getPrice() - o1.getPrice()))
					 .forEach(s -> System.out.println(s));
			
		System.out.println("===== 가장 어린 사람 출력 =====");
		int minAge = Integer.MAX_VALUE;
		for(Customer customer : list) {
			if(minAge > customer.getAge()) {
				minAge = customer.getAge();
			}
		}
		for(Customer customer: list) {
			if(customer.getAge() == minAge) {
				System.out.println(customer);
				break;
			}
		}
		
		Customer customer = list.stream().min((o1, o2) -> o1.getAge() - o2.getAge()).get();
		System.out.println(customer);
		
		System.out.println("=== 가장 요금이 적은 사람 출력 ===");
		customer = list.stream().min((o1, o2) -> o1.getPrice() - o2.getPrice()).get();
		System.out.println(customer);

		System.out.println("== 가장 요금이 많은 사람 출력 ===");
		customer = list.stream().max((o1, o2) -> o1.getPrice() - o2.getPrice()).get();
		System.out.println(customer);
	}

}

/*
 * Customer 클래스 정의
 * - 멤버변수 : 이름(문자열, name), 나이(정수, age), 요금(정수, price)
 * - 생성자 : 모든 멤버변수를 초기화하는 생성자
 * - 모든 멤버변수에 대한 Getter/Setter 메서드 정의
 * - 모든 멤버변수에 대한 toString() 메서드 오버라이딩
 * 
 */

class Customer {
	String name;
	int age;
	int price;
	
	public Customer(String name, int age, int price) {
		this.name = name;
		this.age = age;
		this.price = price;
	}

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}

	public int getAge() {
		return age;
	}

	public void setAge(int age) {
		this.age = age;
	}

	public int getPrice() {
		return price;
	}

	public void setPrice(int price) {
		this.price = price;
	}

	@Override
	public String toString() {
		return "Customer [name=" + name + ", age=" + age + ", price=" + price + "]";
	}
	
	
}

```

---

# [오후수업] JSP 45차

## 서블릿
> 새로운 Workspace(JSP_model2) 생성

> ServletTest.jsp, ServletTest2.jsp는 src/main/java 폴더에 생성

```jsp
------------------------------------------------------------------index.jsp------------------------------------------------------------------
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<h1>index.jsp</h1>
	
	<!-- GET 방식의 요청을 수행하는 방법 3가지 -->
	<!-- 1. 웹브라우저 주소창에 직접 URL 을 입력하여 요청 -->
	
	<!-- 2. 하이퍼링크를 사용하여 "test" 주소(= 서블릿 주소) 요청 -->
	<a href="test">test 서블릿 주소 요청</a>
	
	<!-- 3. form 태그에서 method="get" 으로 명시하거나 method 속성을 명시하지 않은 상태로 요청 -->
	<!-- form 태그를 사용하여 submit 버튼("test 서블릿 주소 요청(GET)")에 "test" 주소 GET 방식 요청 -->
	<form action="test" method="get">
		<input type="submit" value="test 서블릿 주소 요청(GET)">
	</form>

	
	<!-- POST 방식의 요청을 수행하는 방법 - 1가지 -->
	<!-- form 태그에서 method="post" 로 명시하여 요청 -->
	<!-- form 태그를 사용하여 submit 버튼("test 서블릿 주소 요청(GET)")에 "test" 주소 POST 방식 요청 -->
	<form action="test" method="post">
		<input type="submit" value="test 서블릿 주소 요청(POST)">
	</form>
</body>
</html>



------------------------------------------------------------------ServletTest.jsp------------------------------------------------------------------
import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

@WebServlet("/test222")
public class ServletTest extends HttpServlet { // 서블릿 클래스 정의

	public void doGet(HttpServletRequest request, HttpServletResponse response) throws IOException {
		// => GET 방식 요청 시 자동으로 호출되는 메서드

//		System.out.println("doGet() 메서드 호출됨!");

		response.setContentType("text/html");
		response.setCharacterEncoding("UTF-8");
		PrintWriter out = response.getWriter();
		out.println("<h1>doGet() 메서드 호출됨!</h1>");

	}

	public void doPost(HttpServletRequest request, HttpServletResponse response) throws IOException {

		response.setContentType("text/html");
		response.setCharacterEncoding("UTF-8");
		PrintWriter out = response.getWriter();
		out.println("<h1>doPost() 메서드 호출됨!</h1>");
	}

}


------------------------------------------------------------------Servlet2.jsp------------------------------------------------------------------
import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class ServletTest2 extends HttpServlet { // 서블릿 클래스 정의

	public void doGet(HttpServletRequest request, HttpServletResponse response) throws IOException {
		// => GET 방식 요청 시 자동으로 호출되는 메서드

//		System.out.println("doGet() 메서드 호출됨!");

		response.setContentType("text/html");
		response.setCharacterEncoding("UTF-8");
		PrintWriter out = response.getWriter();
		out.println("<h1>ServletTest2 - doGet() 메서드 호출됨!</h1>");

	}

	public void doPost(HttpServletRequest request, HttpServletResponse response) throws IOException {

		response.setContentType("text/html");
		response.setCharacterEncoding("UTF-8");
		PrintWriter out = response.getWriter();
		out.println("<h1>ServletTest2 - doPost() 메서드 호출됨!</h1>");
	}

}

------------------------------------------------------------------web.xml------------------------------------------------------------------
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://xmlns.jcp.org/xml/ns/javaee" xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd" id="WebApp_ID" version="3.1">
  <display-name>Test</display-name>
  <welcome-file-list>
    <welcome-file>index.html</welcome-file>
    <welcome-file>index.jsp</welcome-file>
    <welcome-file>index.htm</welcome-file>
    <welcome-file>default.html</welcome-file>
    <welcome-file>default.jsp</welcome-file>
    <welcome-file>default.htm</welcome-file>
    <welcome-file>main2.htm</welcome-file>
  </welcome-file-list>
  
  <servlet-mapping>
  	<servlet-name>itwillServlet</servlet-name>
  	<url-pattern>/itwill</url-pattern>
  </servlet-mapping>
  
  <servlet>
  	<servlet-name>itwillServlet</servlet-name>
  	<servlet-class>ServletTest2</servlet-class>
  </servlet>
</web-app>
```
