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
### INSERT 구문

```jsp

-----------------------------------------------jdbc_insert_form2_join-----------------------------------------------
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<h1>회원 가입</h1>
	<form action="jdbc_insert_pro2.jsp" name="fr">
		<table border="1">
			<tr><td>이름</td><td><input type="text" name="name" required="required"></td></tr>
			<tr>
				<td>ID</td>
				<td>
					<input type="text" name="id" placeholder="4 ~ 8글자 사이 입력" required="required">
					<input type="button" value="ID중복확인">
					<span id="checkIdResult"></span>
				</td>
			</tr>
			<tr>
				<td>비밀번호</td>
				<td>
					<input type="password" name="passwd" placeholder="8 ~ 16글자 사이 입력" required="required">
					<span id="checkPasswdResult"></span>
				</td>
			</tr>
			<tr>
				<td>비밀번호확인</td>
				<td>
					<input type="password" name="passwd2" required="required">
					<span id="checkConfirmPasswdResult"></span>
				</td>
			</tr>
			<tr>
				<td>주민번호</td>
				<td>
					<input type="text" name="jumin1" required="required"> -
					<input type="password" name="jumin2" required="required">
				</td>
			</tr>
			<tr>
				<td>E-Mail</td>
				<td>
					<input type="text" name="email1" required="required">@
					<input type="text" name="email2" required="required">
					<select name="emailDomain">
						<option value="">직접입력</option>
						<option value="naver.com">naver.com</option>
						<option value="nate.com">nate.com</option>
						<option value="daum.net">daum.net</option>
					</select>
				</td>
			</tr>
			<tr>
				<td>직업</td>
				<td>
					<select name="job" required="required">
						<option value="">항목을 선택하세요</option>
						<option value="개발자">개발자</option>
						<option value="DB엔지니어">DB엔지니어</option>
						<option value="관리자">관리자</option>
						<option value="기타">기타</option>
					</select>
				</td>
			</tr>
			<tr>
				<td>성별</td>
				<td>
					<input type="radio" name="gender" value="남">남
					<input type="radio" name="gender" value="여">여
				</td>
			</tr>
			<tr>
				<td>취미</td>
				<td>
					<input type="checkbox" name="hobby" value="여행">여행
					<input type="checkbox" name="hobby" value="독서">독서
					<input type="checkbox" name="hobby" value="게임">게임
					<input type="checkbox" name="check_all" value="전체선택">전체선택
				</td>
			</tr>
			<tr>
				<td>가입동기</td>
				<td><textarea name="content" cols="40" rows="10"></textarea></td>
			</tr>
			<tr>
				<td colspan="2">
					<input type="submit" value="가입">
					<input type="reset" value="초기화">
					<input type="button" value="돌아가기">
				</td>
			</tr>
		</table>
	</form>
</body>
</html>

-----------------------------------------------jdbc_insert_pro2_join-----------------------------------------------


<%@page import="java.sql.PreparedStatement"%>
<%@page import="java.sql.DriverManager"%>
<%@page import="java.sql.Connection"%>
<%@ page language="java" contentType="text/html; charset=UTF-8"
	pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<%
	request.setCharacterEncoding("UTF-8");

	String name = request.getParameter("name");
	String id = request.getParameter("id");
	String passwd = request.getParameter("passwd");
	// 주민번호와 이메일의 경우 두 파라미터 결합 필요
	String jumin = request.getParameter("jumin1") + "-" + request.getParameter("jumin2");
	String email = request.getParameter("email1") + "@" + request.getParameter("email2");
	String job = request.getParameter("job");
	String gender = request.getParameter("gender");

	// 주의! 체크박스 등을 통해 동일한 이름으로 복수개의 파라미터가 전달될 경우
	// request.getParameter() 가 아닌 request.getParameterValues() 메서드를 통해 배열로 전달받아야함
	//	 	String hobby = request.getParameter("hobby"); // 첫번째 항목만 전달받게 됨
	String[] arrHobby = request.getParameterValues("hobby");
	String hobby = "";
	// for문을 사용하여 arrHobby 배열에 차례대로 접근하여 저장되어 있는 데이터를 hobby 변수에 결합
	// => 주의! 체크박스 체크 항목이 하나도 체크되지 않은 경우 null 값이 전달되므로 반복문 사용 불가(오류 발생)
	// => 따라서, 배열이 null 이 아닐 때만 반복을 수행하도록 조건문 사용
	if (arrHobby != null) {
		for (int i = 0; i < arrHobby.length; i++) {
			hobby += arrHobby[i];
		}
	}
	String content = request.getParameter("content");

	
	
	String Driver = "com.mysql.jdbc.Driver";
	String url = "jdbc:mysql://localhost:3306/study_jsp3";
	String user = "root";
	String password = "1234";

	Class.forName(Driver);
	out.println("<h3>1단계 완료!</h3>");

	Connection con = DriverManager.getConnection(url, user, password);
	out.println("<h3>2단계 완료!</h3>");

	String sql = "INSERT INTO test4 VALUES(?,?,?,?,?,?,?,?,?)";

	PreparedStatement pstmt = con.prepareStatement(sql);

	pstmt.setString(1, name);
	pstmt.setString(2, id);
	pstmt.setString(3, passwd);
	pstmt.setString(4, jumin);
	pstmt.setString(5, email);
	pstmt.setString(6, job);
	pstmt.setString(7, gender);
	pstmt.setString(8, hobby);
	pstmt.setString(9, content);

	int InsertCount = pstmt.executeUpdate();
	out.println("<h3>추가완료</h3>");
	%>

</body>
</html>

```

### SELECT 구문

```jsp
<%@page import="java.sql.ResultSet"%>
<%@page import="java.sql.PreparedStatement"%>
<%@page import="java.sql.DriverManager"%>
<%@page import="java.sql.Connection"%>
<%@ page language="java" contentType="text/html; charset=UTF-8"
	pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<%
	String Driver = "com.mysql.jdbc.Driver";
	String url = "jdbc:mysql://localhost:3306/study_jsp3";
	String user = "root";
	String password = "1234";

	Class.forName(Driver);
	out.println("<h3>1단계 완료!</h3>");

	Connection con = DriverManager.getConnection(url, user, password);
	out.println("<h3>2단계 완료!</h3>");

	// 3단계. SQL 구문 작성 및 전달
	// test3 테이블의 모든 레코드 조회
	String sql = "SELECT * FROM test3";
	PreparedStatement pstmt = con.prepareStatement(sql);
	
	/* 
	4단계. SQL 구문 실행 및 결과 처리 => SELECT 에서 매우 중요!
	int executeUpdate() : INSERT, UPDATE, DELETE 등 데이터베이스 조작에 필요한 작업을 수행하는 메서드
	=> 실행 수 영향을 받은 레코드 수가 int 타입으로 리턴됨(단, DML 만 실제 값이 리턴됨)
	
	ResultSet executeQuery() : SELECT 구문 전용 메서드
	=> 실행 후 조회된 테이블 형태를 관리하는 ResultSet 타입 객체 리턴됨
	*/
	
	ResultSet rs = pstmt.executeQuery(); // 조회 후 결과를 ResultSet 타입으로 저장
	/*
	조회 결과가 다음 형태의 테이블로 조회되며, ResultSet 타입 객체로 리턴됨
	또한, 커서(Cursor) 라는 개념의 포인터가 조회된 테이블 최상단(데이터보다 윗쪽)에 위치함
	+----------+--------+------+--------+
	| no       | name   | age  | gender | <= 현재 포인터(커서) 위치
	+----------+--------+------+--------+
	| 11111111 | 이순신 |   20 | 남     |
	|        2 | 가나다 |   30 | 남     |
	|        3 | 라마바 |   40 | 남     |
	|        4 | 사아자 |   50 | 남     |
	|        5 | 차카타 |   20 | 여     |
	|        6 | 파하하 |   30 | 여     |
	+----------+--------+------+--------+
	현재 커서가 위치한 곳은 데이터가 있는 레코드의 윗쪽이므로
	커서를 한 줄씩 아래로 이동시키면서 각 레코드에 접근해야함
	=> ResultSet 객체의 next() 메서드를 호출하면 커서를 한 줄 아래로 이동시킨 후 
	   레코드가 존재하면 true 값을 리턴하고, 존재하지 않으면 false 값 리턴
	=> 따라서, 커서를 다음 레코드가 존재할 동안 이동시키면서 각 레코드에 접근 가능 
	=> 반복 횟수가 정해져 있지 않으므로 while 문을 활용하여 rs.next() 결과가 true 동안 반복하는것이 유리함
	   (조회 결과 레코드가 1개밖에 없을 경우 while 문 대신 if 문으로 교체 가능)
	*/
// 	while(rs.next()) { // 다음 레코드가 존재할 동안 반복(= 다음 레코드가 존재하지 않으면 반복 종료됨)	
// 		/*
// 		커서를 이동시킨 후 해당 레코드에서 각 컬럼에 접근하기 위해
// 		ResultSet 객체의 getXXX() 메서드를 호출하여 컬럼에 접근 가능
// 		=> 이 때, getXXX() 메서드의 XXX 은 가져올 컬럼에 대한 자바 데이터타입명을 지정
// 			ex) 가져올 컬럼의 타입이 varchar 타입일 경우 호출할 메서드 이름 : getString()
// 			ex) 가져올 컬럼의 타입이 int 타입일 경우 호출할 메서드 이름 : getInt()
// 		=> getXXX() 메서드의 파라미터로 컬럼 인덱스 번호(int 타입) 또는 컬럼명(String 타입) 사용
// 		   인덱스 번호는 1번부터 컬럼 순서대로 자동으로 부여됨
// 		   ex) "varchar 타입"의 "두번째" 컬럼인 "name" 컬럼에 접근할 경우 : getString(2) 또는 getString("name")
// 		*/
// 		// 첫번째 컬럼(1번 인덱스)의 타입이 varchar 타입이고 컬럼명이 "no"인 컬럼에 접근하여 데이터 가져와서 변수 no 에 저장하기
// // 		String no = rs.getString(1); // 인덱스 번호(int 타입)를 사용하여 컬럼에 접근 시 => 인덱스 범위 주의!
// // 		out.println("인덱스로 접근 : " + no + "<br>");
		
// // 		String no = rs.getString("noo"); // 컬럼명(String 타입)를 사용하여 컬럼에 접근 시 => 오타 주의!
// // 		out.println("컬럼명으로 접근 : " + no + "<br>");

// 		// 4개 컬럼 데이터를 모두 변수에 저장 후 한꺼번에 출력
// // 		int no = rs.getInt(1); // int 타입 첫 번째 컬럼(컬럼명 "no");
// 		int no = rs.getInt("no"); // 컬럼명 "no"인 컬럼
// 		String name = rs.getString("name"); // 또는 rs.getString(2)
// 		int age = rs.getInt("age"); // 또는 rs.getInt(3)
// 		String gender = rs.getString("gender"); // 또는 rs.getString(4)
		
// 		out.println(no + ", " + name + ", " + age + ", " + gender + "<br>");

// 	}
	%>
	
	
	<!-- 조회된 테이블 정보를 table 태그를 사용하여 표시하기 위해 테이블 생성 -->
	<!-- while 문을 사용하여 ResultSet 객체를 반복하기 전 제목열 까지는 직접 생성하고, 데이터만 반복 -->
	<table border="1">
		<!-- 제목을 표시하기 위한 행(tr) 생성 -->
		<tr>
			<th width="150">학번</th>
			<th width="150">이름</th>
			<th width="100">나이</th>
			<th width="100">성별</th>
		</tr>
		<!-- 데이터를 표시하기 위한 행(tr) 생성 => while 문을 통해 반복 작업 필요 -->
		<%
		// while 문을 사용하여 ResultSet 객체의 다음 레코드가 존재할 동안 반복
		while(rs.next()) {
			// no, name, age, gender 변수 선언 및 데이터	 가져와서 저장
			int no = rs.getInt(1);
			String name = rs.getString(2);
			int age = rs.getInt(3);
			String gender = rs.getString(4);
			
			// tr 태그와 td 태그를 조합하여 1개 레코드(tr)의 각 데이터(td)를 표시
			// out.println() 메서드를 통해 자바코드로 행, 열을 출력할 경우
// 			out.println("<tr>");
// 			out.println("<td>" + no + "</td>");
// 			out.println("<td>" + name + "</td>");
// 			out.println("<td>" + age + "</td>");
// 			out.println("<td>" + gender + "</td>");
// 			out.println("</tr>");	

			// 스크립틀릿 사이에 HTML 태그를 출력할 경우
		%>
			<!-- 이 부분은 HTML 태그 작성이 가능하며 while문 내부이므로 반복됨 -->
			<tr>
				<td><%=no %></td>
				<td><%=name %></td>
				<td><%=age %></td>
				<td><%=gender %></td>
			</tr>
		<%
		}
		%>
	</table>
</body>
</html>
```

주석 없는 버전
```jsp
<%@page import="java.sql.ResultSet"%>
<%@page import="java.sql.PreparedStatement"%>
<%@page import="java.sql.DriverManager"%>
<%@page import="java.sql.Connection"%>
<%@ page language="java" contentType="text/html; charset=UTF-8"
	pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<%
	String Driver = "com.mysql.jdbc.Driver";
	String url = "jdbc:mysql://localhost:3306/study_jsp3";
	String user = "root";
	String password = "1234";

	Class.forName(Driver);

	Connection con = DriverManager.getConnection(url, user, password);

	String sql = "SELECT * FROM test3";
	PreparedStatement pstmt = con.prepareStatement(sql);

	ResultSet rs = pstmt.executeQuery();
	%>


	<table border="1">
		<tr>
			<th width="150">학번</th>
			<th width="150">이름</th>
			<th width="100">나이</th>
			<th width="100">성별</th>
		</tr>
		<%
		while(rs.next()) {
			int no = rs.getInt(1);
			String name = rs.getString(2);
			int age = rs.getInt(3);
			String gender = rs.getString(4);
		%>
		<tr>
			<td><%=no%></td>
			<td><%=name%></td>
			<td><%=age%></td>
			<td><%=gender%></td>
		</tr>
		<%
		}
		%>
	</table>

</body>
</html>
```

