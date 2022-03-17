# [오전수업] JAVA 21차

## try catch
오류(Error)
- 프로그램 자체 또는 JVM 등 치명적은 원인으로 인해 발생하는 문제점
  - (예기치 못한 상황에서 발생하는 의도치 않은 문제)
- 오류가 발생하면 프로그램은 강제로 종료됨
- 개발자가 수정이 불가능한 문제

예외(Exception)
- 사용자의 잘못된 조작 실수 또는 개발자의 잘못된 코딩 등으로 인해 발생하는 문제점
- 예외가 발생해도 프로그램은 즉시 강제 종료됨
- 개발자가 예외 상황이 발생했을 때 대처 방안을 기술 할 수 있음
  - 프로그램이 정상적으로 종료 될 수 있도록 처리 가능

```
< 예외 관련 용어 >
1. 예외가 발생했을 때 예외 발생 여부를 감지하는 대상 : 리스터(Listener)
2. 예외가 발생했을 때 정상적으로 종료될 수 있도록 처리하는 것 : 예외처리
   => 예외가 발생하지 않도록 미리 원인을 제거하는 것은 예외 처리가 아니며, 
      예외가 발생할 상황을 미리 탐지하여 예외가 발생했을 때
      프로그램이 정상 종료되도록 처리하는 것을 예외 처리라고 한다.
```

```java
package try_catch;

public class Ex1 {

	public static void main(String[] args) {

		System.out.println("프로그램 시작");
		int num1 = 10, num2 = 0;
		
		// 자바에서는 연산 시 두 번째 피연산자가 0일 경우 나눗셈 과정에서 예외 발생함
//		System.out.println(num1 + " / " + num2 + " = " + (num1 / num2)); // 예외 발생!	
		// => 예외 발생 코드 아래쪽의 나머지 코드는 실행되지 못한 채 비정상 종료됨
		
		/*
		 * 예외가 발생할 경우 예외 상황이 메시지로 출력됨
		 * Exception in thread "main" java.lang.ArithmeticException: / by zero
		 * at try_catch.Ex1.main(Ex1.java:31)
		 * 
		 * 1) Exception in thread "main" java.lang.ArithmeticException:
		 * 	  => 예외 발생 상황 정보 및 예외 발생을 탐지한 예외 클래스명
		 * 		 (예외 발생을 탐지하는 리스너 객체 : ArithmeticException)
		 * 2) / by zero
		 * 	  => 예외 발생 원인 메세지 (0에 의한 나눗셈으로 인해 예외 발생했다는 의미)
		 * 
		 * 3) at try_catch.Ex1.main(Ex1.java:30)
		 * 	  => 예외 발생 위치(Ex.java 클래스의 31번 라인에서 예외 발생
		 */
		
		// 만약, 예외가 발생하지 않도록 if문을 통해 피연산자 값 검사하는 경우
		// 예외처리가 아님
//		if(num2 != 0) {
//			System.out.println(num1 + " / " + num2 + " = " + (num1 / num2));
//		}
		
//		String str = null;	// 참조 변수에 저장된 주소값이 없음
		// null 값을 참조하는 객체에 접근하여 작업을 수행하려 할 경우
		// 접근이 불가능하므로 NullPointerException 예외가 발생
//		System.out.println(str.length());
		
		int[] arr = new int[2];
		// 배열 인덱스 사용 시 존재하지 않는 인덱스 사용할 경우
		// ArrayIndexOutOfBoundsException 예외가 발생하게 된다!
//		arr[2] = 10;
		
		
		System.out.println("프로그램 종료");
	}

}
```
### 예외(Exception)
- 개발자가 의도하지 않는 상황에서 발생하는 문제
- 예외 발생 시 프로그램은 해당 지점에서 비정상적으로 종료됨
  - 예외 발생 코드 아래쪽의 코드들은 실행되지 못함
- 오류(Error)와 달리 심각도가 낮으며 예외처리(Exception HandLing)를 통해 예외 발생 시 해결책을 기술하여 프로그램이 정상적으로 종료되도록 처리 기능
	- (=> 오류는 JVM 등의 문제로 인한 것으로 개발자가 처리 불가능)
- 예외 처리를 위해 try ~ catch 문을 사용하여 처리 작업 수행
  - try 블록 내에서 예외 발생 가능성이 있는 코드들을 감시하고 예외가 발생했을 경우 JVM이 던지는 예외 객체를 전달받아 catch 블록 중 해달 객체 타입과 일치하는 catch 블록을 실행하여 예외를 처리
  - 예외가 발생하지 않으면 catch블록 실행되지 않음

예외 종류 분류
1) Compile Checked Exception 계역
	- 컴파일(번역) 시점에서 예외 발생 가능성을 판별 가능하므로 예외 처리가 되어있지 않으면 컴파일 에러 발생함(예외처리 강제성 있음)
	- 대표적인 예외 : IOException, SQLException, ClassNotFoundException 등
2) Compile Unchecked Exception
	- 컴파일(번역) 시점에서 예외 발생 가능성을 판별 할 수 없으며 실행 시점에서 예외 발생 여부가 판별되므로 예외 처리 여부를 별도로 감시하지 않음(예외 처리 강제성이 없음)
    - 예외가 발생할 것으로 예상되는 코드를 찾아서 예외처리를 수행해야함
	- 대표적인 예외 : RuntimeException 계열
		- ArithmetixException, ArrayIndexOfBoundsException, NullPointerException, ClassCastException 등
- java.lang 패키지 내에 Exception 클래스와 서브클래스들이 제공되며 각 예외는 "자신의 슈퍼클래스 타입으로 업캐스팅" 도 가능함

```
< 기본 문법 >
try {
    // 일반적인 코드 및
    // 예외 발생 가능성이 있는 코드들
} catch(예외클래스명 변수명){
    // 예외클래스에 해당하는 예외가 발생할 경우 처리할 코드들 기술
}
```

```java
package try_catch;

public class Ex2 {

	public static void main(String[] args) {
		
		System.out.println("프로그램 시작");
		
		// 예외가 발생할 것으로 예상되는 코드(또는 코드 범위)를
		// try{} 블록으로 감싸기
		
		try {
			System.out.println("try 블록 시작!");
		
			int num1 = 3;
			int num2 = 0;
			
			if(num2 != 0) {} // 예외가 발생하지 않도록 코드를 수정하는 것은 예외처리 X
			// => 예외가 발생하더라도 나머지 코드들이 정상적으로 수행되도록 하는 것
			
			System.out.println(num1 / num2);	// 예외(AritmeticException)가 발생하는 코드
			// 현재 예외가 발생한 코드의 아래쪽 나머지 코드들(try블록 내의 코드들)은
			// 실행되지 못하고, catch 블록으로 가서 예외 객체에 해당하는 클래스를 찾아
			// 해당 catch블록 내의 코드들을 실행하게 된다
			System.out.println("try 블록 끝!");
		} catch(ArithmeticException e) {
			
			// ArithmeticException 예외가 try 블록 내에서 발생하면
			// JVM에 의해 ArithmeticException 예외 객체가 생성되고 던져짐
			// 따라서, catch 블록의 소괄호 안에 해당 예외 클래스와 일치하는 참조변수를 선언하여
			// 던져지는 예외 객체를 전달 받아야함(catch)
			// => catch 블록 중괄호 내에서 예외 발생 시 수행할 작업들을 명시
			System.out.println("ArithmeticException 예외 발생! 0으로 나눌 수 없습니다!");
		}
		
		// try문에서 예외가 발생하더라도 try문 내의 아래쪽 코드 실행만 불가능하고
		// catch블록 코드 실행 후 try ~ catch문 밖으로 나와 나머지 코드들을 정상적으로 실행함
		// => 즉, 예외가 발생하더라도 프로그램램이 정상적으로 종료되도록 처리됨
		System.out.println("프로그램 종료");
		
		System.out.println("=============================");
		
		System.out.println("프로그램 시작");
		
		try {
			System.out.println("try 블록 시작!");
			int[] arr = {1, 2, 3};
			System.out.println(arr[3]); // ArrayIndexOutOfBoundsException 예외 발생!
			System.out.println("try 블록 끝!"); // 예외 발생 시 실행되지 못하는 코드
		} catch(ArrayIndexOutOfBoundsException e) { 
			// ArrayIndexOutOfBoundsException 예외가 발생했을 경우 수행할 코드
			System.out.println("ArrayIndexOutOfBoundsException 예외 발생!");
			e.printStackTrace();
		}
		
		System.out.println("프로그램 종료");
		
		System.out.println("=============================");

		System.out.println("프로그램 시작!");
		
		try {
			System.out.println("try 블록 시작!");
			String str = null;
			System.out.println(str.length());
			System.out.println("try 블록 끝!");
		} catch(NullPointerException e) {
			System.out.println("NullPointerException 예외 발생!");
		}
		
		System.out.println("프로그램 종료!");
	}

}
```


연습문제
```java
package try_catch;

import java.util.Random;

public class Test2 {

	public static void main(String[] args) {
		/*
		 * 반복문을 사용하여 0 ~ 9 까지의 난수 발생 => 10번 반복 => 발생한 난수를 정수형 변수 num에 저장 후 10 / num 결과를
		 * 화면에 출력 (ex. 10 / 3 = 3 출력) => Exception 발생 시 "피연산자가 0이므로 연산에서 제외됩니다!" 출력
		 * 
		 */

		Random r = new Random();

		for (int i = 1; i < 10; i++) {
			int num = r.nextInt(10); // 0 ~ 9 사이 난수 발생
			try {
				System.out.println("10 / " + num + " = " + (10.0 / num));
			} catch (ArithmeticException e) {
				System.out.println("피연산자가 0이므로 연산에서 제외됩니다!");
			}

		}

	}

}
```

### 다중 catch
하나의 try 블록에서 복수개의 예외를 처리하는 경우
- try 블록 내에서 처리해야하는 예외가 두 종류 이상일 경우 catch 블록을 해당 예외 종류만큼 작성하거나 하나의 catch 블록에서 복수개의 예외를 모두 처리하는 클래스를 사용
- 복수개의 catch 블록은 첫번째 catch 블록부터 차례대로 탐색
  - 만약, 끝까찌 탐색했음에도 일치하는 catch 블록이 없으면 실행 시 예외 발생함
- 만약, 복수개의 catch 블록 지정 시 하위 타입부터 상위 타입순으로 나열해야함
  - 상위 타입이 먼저 기술되면 하위 타입 예외는 실행 될 수 없음
  - (ex. ArithmeticException 보다 Exception 클래스가 위에 있을 수 없음)
- 만약, 하나의 catch 블록으로 복수개의 예외를 처리하려면

1) catch 블록의 클래스를 복수개의 예외클래스의 상위 타입으로 지정
  - (ex. 모든 얘외를 처리하기 위해서는 Exception 클래스 지정)
2) catch 블록의 클래스에 | 기호를 사용하여 복수개의 클래스를 기술 가능
  - 유사한 성격을 갖는 예외를 하나의 catch 블록에서 묶어서 처리 가능
	  - (ex. FileNotFoundException 과 ClassNotFoundException 은 유사한 기능이므로
	  - catch(FileNotFoundException | ClassNotFoundException e) {} 형태로 기술

```
< 기본 문법 >
try {
		// 예외발생1코드
		// 예외발생2코드
		// 예외발생3코드

} catch(예외발생1클래스 변수명){

		// 예외발생1 코드에서 예외 발생 시 처리할 코드들...

} catch(예외발생2클래스 변수명){

		// 예외발생2 코드에서 예외 발생 시 처리할 코드들...

} catch(Exception e){

		// 위의 모든 예외가 일치하지 않을 경우 나머지 예외를 모두 처리하는 코드
	  // 마치 if문에서 else문(switch-case문에서 default문)과 같은 역할 수행
}
```

```java
package try_catch;

import java.io.FileInputStream;
import java.io.FileNotFoundException;

public class Ex3 {

	public static void main(String[] args) {
			
		System.out.println("프로그램 시작!");
		
		try {
			// 첫번째 예외 : ArithmeticException
			int num1 = 3, num2 = 1;
			System.out.println(num1 / num2);
			
			// 두번째 예외 : NullPointerException
//			String str = null;
			String str = "홍길동";
			System.out.println(str.length());
			
			// 세번째 예외 : ArrayIndexOutOfBoundsException
			int[] arr = {1};
			System.out.println(arr[1]);
		} catch(ArithmeticException e) {
			System.out.println("예외 발생 : 0으로 나눌 수 없습니다!");
		} catch(NullPointerException e) {
			System.out.println("예외 발생 : null 값을 참조할 수 없습니다!");
		} catch(ArrayIndexOutOfBoundsException e) {
			System.out.println("예외 발생 : 잘못된 인덱스를 참조합니다!");
		} catch (Exception e) { // 마지막으로 확인하는 catch 블록
			// 위의 세가지 예외에 해당하지 않는 예외를 모두 Exception 타입으로 catch 함
			// => if문에서 else문 역할과 동일함
			System.out.println("예외 발생 : 나머지 예외를 모두 처리합니다!");
		}
		
		System.out.println("프로그램 종료!");
		
		try {
			// ClassNotFoundException 발생 예상 코드...
			Class.forName("");
			
			// FileNotFoundException 발생 예상 코드...
			FileInputStream fis = new FileInputStream("");
		} catch (FileNotFoundException | ClassNotFoundException e) {
			System.out.println("FileNotFoundException 또는 ClassNotFoundException 발생!");
		}
		
		
	}

}
```

```java
package try_catch;

public class Ex4 {

	public static void main(String[] args) {
		/*
		 * Exception 객체의 정보 활용
		 * - 예외 발생 시 해당 예외를 처리할 수 있는 예외 객체를 생성하여 전달되어짐
		 * 	 => 이 때, catch 블록에서 해당 예외 객체를 전달 받을 수 있다!
		 * - 전달받은 객체의 여러 메서드를 통해 객체 정보를 활용 가능
		 * 
		 * 1. void printStackTrace()
		 * 	  => 예외 발생 시 코드들(메서드)의 호출 순서 및 예외 발생 상황을 출력
		 * 	  ex) java.lang.ArithmeticException: / by zero
		 * 		   at try_catch.Ex4.main(Ex4.java:20)
		 * 			1) java.lang.ArithmeticException : 예외 발생 시 던져지는 예외 객체(클래스)
		 * 			2) / by zero : 예외 발생 원인 메세지
		 * 			3) at try_catch.Ex4.main(Ex4.java:20) : 예외 발생 위치(클래스 및 메서드, 코드라인 정보)
		 * 			   => 예외 발생 위치는 메서드 호출 과정에 따라 복수개 출력 될 수 있음
		 * 
		 *  2. String getMessage()
		 *     => 예외 발생 시 전달되는 원인메세지를 문자열로 리턴
		 *     	  ex) ArithmeticException 발생 시 원인 메세지인 "/ by zero" 문자열을 리턴
		 *     
		 *  3. String getLocalizedMessage
		 *     => 기본적으로 getMessage() 메서드와 기능은 동일하나
		 *     	  주로, 예외 원인 메세지를 수정하기 위해 오버라이딩해서 메세지 수정하여
		 *     	  수정된 메세지를 출력하기 위해 사용하는 메서드
		 *     	  (오버라이딩 하지 않을 경우 getMessage() 메서드와 동일함)
		 */
		
		try {
			System.out.println("try 블록 시작!");
			
			int num1 = 3;
			int num2 = 0;
			
			System.out.println(num1 / num2);	// ArithmeticException)가 발생하는 코드
			System.out.println("try 블록 끝!");
		} catch (Exception e) { // Exception 예외를 전달받은 catch문 e를 활용
//			e.printStackTrace();
//			System.out.println(e.getMessage());
			System.out.println(e.getLocalizedMessage());
		}

	}

}
```

### try ~ catch ~ finally 구문
- 기본적으로 try, catch 블록의 역할은 동일하나 finally 블록에 기술된 코드들은 예외 발생 여부와 관계없이 실행됨
- 일반적인 try 구문 바깥의 코드도 예외 발생 여부와 관계없이 실행되는 것은 동일하나 try ~ catch 블록 내에서 return 문의 사용으로 인한 메서드 수행이 종료되더라도 반드시 finally 블록의 코드를 실행한 후에 메서드가 종료된다!
- 주로, 데이터베이스 작업(JDBC)이나 파일 작업을 위해 DB서버 등의 자원 사용시 예외가 발생하더라도 자원을 반환하기 위한 코드는 무조건 실행되어야 하므로 finally 블록 내에 자원 반환 코드를 기술하면 예외 발생 여부와 관계없이 실행됨
  
```  
< 기본 문법 >
  try {
    // 예외 발생할 가능성이 있는 코드...
  } catch {
    // 예외 발생 시 처리할 코드들...
  } finally {
    // 예외 발생 여부와 관계없이 실행할 코드들...
  }
```
```java
package try_catch;

public class Ex5 {

	public static void main(String[] args) {
		
		try {
			System.out.println("1 - try 블록 시작!");
			
			int num1 = 3;
			int num2 = 1;
			System.out.println(num1 / num2);
			
			System.out.println("2 - 예외 미발생. try 블록 끝!");
			
		} catch(Exception e) {
			
			System.out.println("3 - catch 블록 시작! 원인 : " + e.getMessage());
			
		} finally {
			
			System.out.println("4 - finally 블록 시작! 언제나 실행되는 코드!");
			
		}
		
		System.out.println("5 - try 구문 끝!");	
		
		System.out.println("======================================");
		
		System.out.println("메서드 호출 시작!");
		
		method();
		
		System.out.println("메서드 호출 끝!");
		
		
		
		
	} // main() 메서드 끝
	
	public static void method() {
		
		/*
		 * 1. 예외가 발생하지 않을 경우
		 * 		1 -> 2 -> return문 (아직 실행X) -> 4(finally블록 실행) -> return
		 * 
		 * 2. 예외가 발생했을 경우
		 *		1 -> 예외 발생 -> 3(catch블록 실행) -> 4(finally블록 실행) -> 5 		
		 * 
		 * */
		
		try {
			System.out.println("1 - try 블록 시작!");
			
			int num1 = 3;
			int num2 = 0;
			System.out.println(num1 / num2);
			
			System.out.println("2 - 예외 미발생. try 블록 끝!");
			
			return; // 메서드 수행을 중단하고 호출한 곳으로 돌아감
			
		} catch(Exception e) {
			
			System.out.println("3 - catch 블록 시작! 원인 : " + e.getMessage());
			
		} finally {
			
			System.out.println("4 - finally 블록 시작! 언제나 실행되는 코드!");
			
		}
		
		System.out.println("5 - try 구문 끝!");
	}

}

```

# [오후수업] JSP 28차
> 27차에서 학습한 내용 복습 실시 및 추가분 커밋

### select 구문
```jsp
<%@page import="jsp10_javabeans.Test3DTO"%>
<%@page import="java.util.ArrayList"%>
<%@page import="jsp10_javabeans.Test3DAO"%>
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%
// Test3DAO 클래스의 인스턴스 생성 후 select() 메서드를 호출하여 학생 목록 정보 조회 요청
// => 파라미터 : 없음	리턴타입 : ?(임시로 없다고 가정)
Test3DAO dao = new Test3DAO();
ArrayList dtoList = dao.select();

%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<h1>학생 목록 정보</h1>
	<table border="1">
		<tr>
			<th width="100">학번</th>
			<th width="100">이름</th>
			<th width="100">나이</th>
			<th width="100">성별</th>
		</tr>
		<%
		// 배열과 마찬가지로 ArrayList 객체의 크기(size()) 크기보다 작을동안 반복
		for(int i = 0; i < dtoList.size(); i++) {
			// dtoList(ArrayList) 객체에는 Test3DTO 객체가 여러개 저장되어 있음
			// => ArrayList 객체의 get() 메서드를 호출하여 저장된 객체를 꺼낼 수 있음
			//	  이 때, 파라미터로 객체의 인덱스 지정
			// => get() 메서드를 호출하여 리턴되는 객체를 Test3DTO 타입 변수에 저장해야함
			//	  단, ArrayList 객체에 저장 시 adD() 메서드는 Object 타입 파라미터를 사용하므로
			// 	  Test3DTO -> Object 타입으로 업캐스팅 되어있으며
			//	  이를 다시 Test3DTO 타입으로 저장하려면 다운캐스팅이 필수!
// 			Test3DTO dto = dtoList.get(i); // 오류 발생! Object -> Test3DTO 타입으로 그냥 저장 불가
			Test3DTO dto = (Test3DTO)dtoList.get(i); // Object -> Test3DTO 타입으로 다운캐스팅
			
			// 테이블에 데이터 표시를 위해 td 태그 내에 Test3DTO 객체의 getXXX() 메서드를 호출하여
			// 저장되어 있는 컬럼 데이터 출력
			%>
			<tr>
				<td><%=dto.getNo() %></td>
				<td><%=dto.getName() %></td>
				<td><%=dto.getAge() %></td>
				<td><%=dto.getGender() %></td>
			</tr>
			<%
		}
		%>
		
	</table>
</body>
</html>
```
