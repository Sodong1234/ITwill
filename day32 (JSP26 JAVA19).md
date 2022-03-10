# [오전수업] JAVA 26차

## SELECT 구문 연습

```jsp
---------------------------------------------jdbc_select3.jsp---------------------------------------------
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

	String sql = "SELECT * FROM test4";
	PreparedStatement pstmt = con.prepareStatement(sql);

	ResultSet rs = pstmt.executeQuery();
	%>


	<table border="1">
		<tr>
			<th width="150">이름</th>
			<th width="150">ID</th>
			<th width="150">비밀번호</th>
<!-- 			<th width="150">주민번호</th> -->
			<th width="150">E-Mail</th>
<!-- 			<th width="150">직업</th> -->
<!-- 			<th width="150">성별</th> -->
<!-- 			<th width="150">취미</th> -->
<!-- 			<th width="150">가입동기</th> -->
			<th></th>
		</tr>
		<%
		while(rs.next()) {
			String name = rs.getString("name");
	 		String id = rs.getString("id"); 
			String passwd = rs.getString("passwd");
			String jumin = rs.getString("jumin");
			String email = rs.getString("email");
			String job = rs.getString("job");
			String gender = rs.getString("gender");
			String hobby = rs.getString("hobby");
			String reason = rs.getString("reason");
		%>
		<tr>
			<td><%=name%></td>
			<td><%=id%></td>
			<td><%=passwd%></td>
<%-- 			<td><%=jumin%></td> --%>
			<td><%=email%></td>
<%-- 			<td><%=job%></td> --%>
<%-- 			<td><%=gender%></td> --%>
<%-- 			<td><%=hobby%></td> --%>
<%-- 			<td><%=reason%></td> --%>
			<td>
			<!-- 상세정보 버튼 클릭 시 jdbc_select3_detail.jsp 페이지로 이동(URL에 id 파라미터 전달) -->
			<input type="button" value="상세정보" onclick="location.href='jdbc_select3_detail.jsp?id=<%=rs.getString("id")%>'">
			<input type="button" value="수정">
			</td>
		</tr>
		<%
		}
		%>
	</table>

</body>
</html>

---------------------------------------------jdbc_select3_detail.jsp---------------------------------------------
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
	<h1>회원 상세정보</h1>

	<%
	
	//URL로 전달받은 파라미터값 가져오기
	String id = request.getParameter("id");
	
	// SELECT 구문을 사용하여 URL에 전달된 id 값에 해당하는 레코드를 조회하여
	// form 태그 내의 각 테이블 컬럼에 데이터 표시(패스워드 제외)
	
	String Driver = "com.mysql.jdbc.Driver";
	String url = "jdbc:mysql://localhost:3306/study_jsp3";
	String user = "root";
	String password = "1234";

	Class.forName(Driver);

	Connection con = DriverManager.getConnection(url, user, password);

	String sql = "SELECT * FROM test4 WHERE id=?";
	PreparedStatement pstmt = con.prepareStatement(sql);
	pstmt.setString(1, id);
	
	// SELECT 구문 실행 결과를 전달받을 ResultSet 타입 변수 선언 및 executeQuery() 메서드 실행 결과 리턴받기
	ResultSet rs = pstmt.executeQuery();
	
	if(rs.next()) {
		String name = rs.getString("name");
// 		String id = rs.getString("id"); // 파라미터로 이미 전달받아 저장되어 있음
		String passwd = rs.getString("passwd");
		String jumin = rs.getString("jumin");
		String email = rs.getString("email");
		String job = rs.getString("job");
		String gender = rs.getString("gender");
		String hobby = rs.getString("hobby");
		String reason = rs.getString("reason");
		
		// 이메일 주소를 계정과 도메인으로 분리시키려면 "@" 기호를 기준으로 양 쪽의 문자열에 대한 분리 필요
		// => String 클래스의 split() 메서드 필요(메서드 파라미터로 분리에 사용될 문자 지정(= 구분자 = Delimeter 라고 함))
		// => 주의! 분리된 문자열은 복수개의 문자열이 되므로 String[] 타입으로 모든 문자열이 관리됨
		String[] arrEmail = email.split("@");
	%>
	<form action="jdbc_update_pro2.jsp" name="fr" onsubmit="return checkForm()">
	<input type="hidden" name="id" value="<%=id %>">
		<table border="1">
			<tr>
				<td>이름</td>
				<td>
					<!-- 이름 데이터 표시할 곳 --><%=name %></td>
			</tr>
			<tr>
				<td>ID</td>
				<td>
					<!-- 아이디 데이터 표시할 곳 --><%=id %>
				</td>
			</tr>
			<tr>
				<td>비밀번호</td>
				<td><input type="password" name="passwd"
					placeholder="기존 비밀번호 입력" required="required"></td>
			</tr>
			<tr>
				<td>변경할 비밀번호</td>
				<td><input type="password" name="newPasswd"
					placeholder="새 비밀번호 입력"></td>
			</tr>
			<tr>
				<td>주민번호</td>
				<td>
					<%=jumin%><!-- 주민번호 표시할 곳 -->
				</td>
			</tr>
			<tr>
				<td>E-Mail</td>
				<td>
				<!-- @ 기호를 기준으로 분리된 arrEmail 배열의 0번 인덱스에 계정, 1번 인덱스에 도메인이 저장되어 있음 -->
				<input type="text" name="email1" required="required" value="<%=arrEmail[0]%>">@
					<input type="text" name="email2" required="required" value="<%=arrEmail[0]%>"> <select
					name="emailDomain">
						<option value="">직접입력</option>
						<option value="naver.com">naver.com</option>
						<option value="nate.com">nate.com</option>
						<option value="daum.net">daum.net</option>
				</select></td>
			</tr>
			<tr>
				<td>직업</td>
				<td><select name="job" required="required">
						<!-- 존재하는 값에만 selected="selected" 속성이 실행되도록 if문 사용 필요 -->
						<option value="개발자"> <%if(job.equals("개발자")) {%>	selected="selected" <%} %>>개발자</option>
						<option value="DB엔지니어"> <%if(job.equals("DB엔지니어")) {%>selected="selected" <%} %>DB엔지니어</option>
						<option value="관리자"> <%if(job.equals("관리자")) {%>selected="selected" <%} %>관리자</option>
						<option value="기타"> <%if(job.equals("기타")) {%>selected="selected" <%} %>기타</option>
				</select></td>
			</tr>
			<tr>
				<td>성별</td>
				<td><input type="radio" name="gender" value="남"> <%if(gender.equals("남")) {%> checked="checked" <%} %>남 <input
					type="radio" name="gender" value="여"> <%if(gender.equals("여")) {%> checked="checked" <%} %>여</td>
			</tr>
			<tr>
				<td>취미</td>
				<td>
				<!-- 복수개의 취미 항목이 하나의 문자열로 연결되어 있으므로, 특정 항목이 포함되었는지 판별 = contains() 메서드 활용 -->
				<!-- 문자열.contains("찾을 문자열") => boolean 타입 결과가 리턴됨 -->
				<input type="checkbox" name="hobby" value="여행" <%if(hobby.contains("여행")) {%> checked="checked" <%} %>>여행 <input
					type="checkbox" name="hobby" value="독서" <%if(hobby.contains("독서")) {%> checked="checked" <%} %>>독서 <input
					type="checkbox" name="hobby" value="게임" <%if(hobby.contains("게임")) {%> checked="checked" <%} %>>게임 <input
					type="checkbox" name="check_all" value="전체선택">전체선택</td>
			</tr>
			<tr>
				<td>가입동기</td>
				<td><textarea name="reason" cols="40" rows="10"><%=reason %></textarea></td>
			</tr>
			<tr>
				<td colspan="2"><input type="submit" value="정보수정"> <input
					type="button" value="돌아가기"></td>
			</tr>
		</table>
	</form>
	<%} else { %>
	<!-- 전달받은 id 가 존재하지 않을 경우 자바스크립트를 사용하여 오류 메시지 출력 및 이전 페이지로 돌아가기 -->
	<script type="text/javascript">
		alert("잘못된 접근입니다");
		history.back();
	</script>
	<%} %>
</body>
</html>

---------------------------------------------jdbc_update_pro2.jsp---------------------------------------------

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
	// 한글 처리 필수
	request.setCharacterEncoding("UTF-8");
		
// 	String name = request.getParameter("name");
	String id = request.getParameter("id");
	String passwd = request.getParameter("passwd"); // 기존 패스워드
	String newPasswd = request.getParameter("newPasswd"); // 변경할 새 패스워드(값이 없을 수도 있음)
	// 주민번호와 이메일의 경우 두 파라미터 결합 필요
// 	String jumin = request.getParameter("jumin1") + "-" + request.getParameter("jumin2");
	String email = request.getParameter("email1") + "@" + request.getParameter("email2");
	String job = request.getParameter("job");
	String gender = request.getParameter("gender");
	
	String[] arrHobby = request.getParameterValues("hobby");
	String hobby = "";
	if(arrHobby != null) {
		for(String item : arrHobby) {
			hobby += item + "/";
		}
	} 
		
	String reason = request.getParameter("reason");
	%>
	<%-- 	<h3>이름 : <%=name %></h3> --%>
	<h3>
		ID :
		<%=id %></h3>
	<h3>
		패스워드 :
		<%=passwd %></h3>
	<h3>
		변경할 패스워드 :
		<%=newPasswd %></h3>
	<%-- 		<h3>주민번호 : <%=jumin %></h3> --%>
	<h3>
		E-Mail :
		<%=email %></h3>
	<h3>
		직업 :
		<%=job %></h3>
	<h3>
		성별 :
		<%=gender %></h3>
	<h3>
		취미 :
		<%=hobby %></h3>
	<h3>
		가입동기 :
		<%=reason %></h3>

	<%
	String Driver = "com.mysql.jdbc.Driver";
	String url = "jdbc:mysql://localhost:3306/study_jsp3";
	String user = "root";
	String password = "1234";

	Class.forName(Driver);

	Connection con = DriverManager.getConnection(url, user, password);
	
	// id 가 일치하는 레코드의 패스워드, E-mail, 직업, 성별, 취미, 가입동기를 모두 수정
	// => 단, 패스워드도 검사해야하므로 id와 패스워드가 모두 일치하는 레코드를 수정해야함
	// => 단, newPAsswd 값이 널스트링("")일 경우 패스워드는 변경하지 않도록 해야함
// 	if(newPasswd.equals("")) {
// 	String sql = "UPDATE test4 SET email=?, job=?, gender=?, hobby=?, reason=? WHERE id=? AND passwd=?";
// 	PreparedStatement pstmt = con.prepareStatement(sql);
// 	pstmt.setString(1, email);
// 	pstmt.setString(2, job);
// 	pstmt.setString(3, gender);
// 	pstmt.setString(4, hobby);
// 	pstmt.setString(5, reason);
// 	pstmt.setString(6, id);
// 	pstmt.setString(7, passwd);

// 	int updateCount = pstmt.executeUpdate();
// } else {
// 	String sql = "UPDATE test4 SET passwd=?, email=?, job=?, gender=?, hobby=?, reason=? WHERE id=? AND passwd=?";
// 	PreparedStatement pstmt = con.prepareStatement(sql);
// 	pstmt.setString(1, newPasswd); // 변경해야하는 새 패스워드
// 	pstmt.setString(2, email);
// 	pstmt.setString(3, job);
// 	pstmt.setString(4, gender);
// 	pstmt.setString(5, hobby);
// 	pstmt.setString(6, reason);
// 	pstmt.setString(7, id);
// 	pstmt.setString(8, passwd);
	
// 	int updateCount = pstmt.executeUpdate();
// }

	// 새 패스워드 존재 유무에 따라 UPDATE 항목에 passwd 변경 문자열을 추가할지 말지 결정 => 삼항연산자를 통해 문자열 추가 선택 가능
	String sql = "UPDATE test4 SET email=?, job=?, gender=?, hobby=?, reason=?" + 
			(newPasswd.equals("") ? "" : "") +  
			" WHERE id=? AND passwd=?";
	
	out.println(sql);
	
	PreparedStatement pstmt = con.prepareStatement(sql);
	pstmt.setString(1, email);
	pstmt.setString(2, job);
	pstmt.setString(3, gender);
	pstmt.setString(4, hobby);
	pstmt.setString(5, reason);
	// 새 패스워드가 있을 경우와 없을 경우 다른 파라미터 순서로 전달해야함
	if(newPasswd.equals("")) { // 새 패스워드가 없을 경우 패스워드를 제외한 나머지 파라미터 지정
		pstmt.setString(6, id);
		pstmt.setString(7, passwd);
	} else { // 새 패스워드가 있을 경우 새 패스워드를 포함하여 파라미터 지정
		pstmt.setString(6, passwd);		
		pstmt.setString(7, id);
		pstmt.setString(8, passwd);
	}
	
	int updateCount = pstmt.executeUpdate();
	
	if(updateCount > 0) { // 수정 성공
		%>
		<script type="text/javascript">
		alert("정보 수정 성공!");
		// jdbc_select3_detail.jsp 로 이동 => id 파라미터 전달
		location.href = "jdbc_select3_detail.jsp?id=" + id
	</script>
	<%
	} else { // 수정 실패
	%>
		<script type="text/javascript">
		alert("수정 권한이 없습니다!");
		history.back();
	</script>
	<%
	}
	%>
</body>
</html>
```


# [오후수업] JAVA 19차


## 추상 메서드 (Abstract Method)
- 메서드 구현부(= 바디{})가 없는 메서드 (미완성 메서드)	
- 메서드의 동작을 기술하는 구현부가 없는 추상메서드는 실행할 수 있는 코드가 없으므로 외부로부터 호출하면 안됨
	- 따라서, 추상메서드를 갖는 클래스는 "반드시" 추상클래스 선언되어야 한다!
- 메서드 정의 시 선언부의 접근제한자 뒤에 abstract 키워드를 붙여서 정의
	- abstract 키워드가 붙은 메서드는 추상메서드이므로 바디{}를 가질 수 없음
		- (따라서, 메서드 선언부 마지막에 바디{} 대신 세미콜론(;) 으로 끝냄)
- 서브클래스에서 반드시 오버라이딩 하도록 강제성을 부여해야하는 메서드는 슈퍼클래스에서 추상메서드로 선언하면 서브클래스가 무조건 오버라이딩 필수! 
	- 강제성을 통해 업캐스팅 시 코드의 통일성을 더욱 향살 시킬 수 있음
```
< 추상메서드 정의 기본 문법 >
[접근제한자] abstract [리턴타입] 메서드명([매개변수...]);
```


## 추상 클래스(Abstract Class)
- 객체를 생성할 수 없는 클래스(미완성 클래스)
	- 따라서, new 연산자를 통해 생성자 호출 불가능
- 추상메서드를 가질 수 있는 클래스
	- 상속받는 서브클래스에서 반드시 오버라이딩 해야한다!
- 추상클래스는 추상메서드, 일반메서드, *생성자, 멥버변수를 모두 가질 수 있음
- 상속을 통해 서브클래스에서 사용 가능하며, 업캐스팅을 통한 다형성 활용 가능
- 구현의 강제성 부여를 통해 코드의 통일성이 향상됨
	- (일부 메서드에 대한 구현의 강제)
```
< 추상클래스 정의 기본 문법 >
abstract class 클래스명 {
		// 멤버변수(인스턴스 멤버, 클래스(static) 멤버)
		// *생성자
		// 메서드 (abstract 추상 메서드 포함!)
}
```

```java
package abstract_method;

abstract class Animal {
	public abstract void cry();
}

class Dog extends Animal {
	@Override
	public void cry() {
	}
}

class Cat extends Animal {
	@Override
	public void cry() {
	}
}





public class Ex1 {

	public static void main(String[] args) {
		
		// 추상클래스의 인스턴스 생성
//		new AbstractClass ac = new AbstractClass();
		// 추상클래스는 인스턴스 생성이 불가능한 클래스
		
		// 추상클래스를 상속받아 추상메서드를 구현한 서브클래스의 인스턴스 생성
		SubClass sc = new SubClass();
		sc.abstractMethod();
		
		// 추상클래스는 인스턴스 생성은 불가능하지만, 다형성 활용은 가능
		// 즉, 변수의 클래스 타입으로 사용할 수 있다!
		AbstractClass ac = sc; // SubClass -> AbstractClass 업캐스팅
		ac.abstractMethod();
			
		
		
	} // main() 메서드 끝
	
} // Ex1 끝

abstract class AbstractClass {
	// 주의! abstract 키워드가 붙은 메서드는 추상메서드이므로 바디를 가질 수 없음
	// 바디를 정의할 경우 오류 발생!
	
//	public abstract void abstractMethod() { // 오류 발생!
//		System.out.println("일반 메서드!");
//	}
	
	public abstract void abstractMethod();
	// ================================================================
	// 추상클래스는 멤버변수와 일반 메서드(static 메서드 포함), 생성자도 가질 수 있음
	int a;			// 인스턴스 멤버변수
	static int b;	// static 멤버변수
	public AbstractClass() {}		// 생성자
	public void print() {}			// 일반메서드
	public static void print2() {}	// static 메서드
	
	
}

// 추상클래스를 상속받는 서브클래스 정의(= 실체(구현체) 클래스)
// => 추상클래스는 인스턴스 생성은 불가능하지만 상속은 가능
class SubClass extends AbstractClass{
	// 추상클래스를 상속받은 서브클래스는 추상클래스에 추상메서드가 존재할 경우
	// 반드시 오버라이딩을 통해 추상메서드의 구현부(바디{})를 정의해야함
	// => 오버라이딩을 통해 메서드 바디를 정의하면 나머지 모든 상속이 적용됨
	// => 만약, 오버라이딩을 하지 않을 경우 상속이 불가능하며
	// 	  현재의 서브클래스도 추상클래스로 선언해야함
	@Override
	public void abstractMethod() {
		// 메서드 오버라이딩을 통해 바디만 구현하면 메서드 내의 코드는 서브클래스에서 자유롭게 기술 가능
		System.out.println("서브클래스에서 오버라이딩을 통해 구현한 추상메서드!");
	}
	
}

abstract class MiddleClass extends AbstractClass {
	// 추상메서드를 포함하는 추상클래스를 상속 받은 경우 오버라이딩이 필수지만
	// 만약, 오버라이딩을 하지 않을 경우 서브클래스도 추상클래스로 선언해야함
	// => 추상클래스로 선언하게 되면 해당 서브클래스도 인스턴스 생성 불가
	//	  현재 클래스를 상속받는 다른 서브클래스를 통해 추상메서드 구현 강제
}

class SubClass2 extends MiddleClass {
	// 추상클래스의 서브클래스가 추상클래스로 선언된 상태에서
	// 해당 클래스를 상속받은 서브클래스에서 추상메서드를 구현
	@Override
	public void abstractMethod() {
		System.out.println("AbstractClass -> Middle -> SubClass2 상속을 통해 구현");
	}
	
	
}
```



연습문제
```java
package abstract_method;

public class Test1 {

	public static void main(String[] args) {

		// 추상클래스인 Flyer 클래스는 인스턴스 생성 불가!
//		Flyer f = new Flyer(); // 오류 발생!
		
		Bird b = new Bird();
		b.fly();
		SuperMan s = new SuperMan();
		s.fly();
		Airplane a = new Airplane();
		a.fly();
		System.out.println("-------------------------------------");
		// Flyer 클래스 인스턴스 생성은 불가능하지만, 참조변수 타입으로 활용 가능(다형성)
		// Bird -> Flyer 업캐스팅 후 fly() 메서드 호출
		Flyer f;
		f = new Bird();
		f.fly();
		// SuperMan -> Flyer 업캐스팅 후 fly() 메서드 호출
		f = new SuperMan();
		f.fly();
		
		// Airplane => Flyer 업캐스팅 후 fly() 메서드 호출
		f = new Airplane();
		f.fly();
		
		System.out.println("-------------------------------------");

		// Flyer 배열 (크기 : 3) 만들어서 각각 업캐스팅 후 
		// 향상된 for문으로 fly() 메서드 호출
		
		Flyer[] fArr = {new Bird(), new SuperMan(), new Airplane()};
		
		for(Flyer flyer : fArr) {
			flyer.fly();
		}
		
		// main 메서드 시작부터 끝까지 Bird는 몇마리 일까요? (메모리에 Bird 타입이 몇개 있을까요?) 
		// 답 : 2
		// 업캐스팅 시 Stack에 있는 주소가 bird에서 Superman으로 이동 할 때 bird의 값이 사라짐
		


	}
	
}
// 날아다니는 것(Flyer)
// fly() 추상메서드

// Bird 클래스 정의 - Flyer 클래스 상속
// => 추상메서드 fly() 오버라이딩 하여 구현("Bird 비행!" 출력)

// SuperMan 클래스 정의 - Flyer 클래스 상속
// => 추상메서드 fly() 오버라이딩 하여 구현("SuperMan 비행!" 출력)

// Airplane 클래스 정의 - Flyer 클래스 상속
// => 추상메서드 fly() 오버라이딩하여 구현("Airplane 비행!" 출력)

abstract class Flyer {
	public abstract void fly();
}

class Bird extends Flyer {
	@Override
	public void fly() {
		System.out.println("Bird 비행!");
	}
}

class SuperMan extends Flyer {
	@Override
	public void fly() {
		System.out.println("SuperMan 비행!");
	}
}

class Airplane extends Flyer {
	@Override
	public void fly() {
		System.out.println("Airplane 비행!");
	}
}
```

## 인터페이스
> 차후 설명 추가 예정 

```java
package interface_statement;

interface A {
	int a = 10;
	public static final int b = 20;
	public static int c = 30;

	// 추상메서드
	void turnOn();

	String methodA();

	String methodB(int a, String b);
}

interface B {
	void methodBB();
}

interface C extends A, B {} // 다중상속 가능

class ImplClass implements A, B { // 다중구현 가능

	@Override
	public void turnOn() {
	}

	@Override
	public String methodA() {
		return null;
	}

	@Override
	public String methodB(int a, String b) {
		return null;
	}

	@Override
	public void methodBB() {
		
	}
}

class SubClass extends ImplClass implements A, B {

}




public class Ex1 {

	public static void main(String[] args) {

	}

}
```
