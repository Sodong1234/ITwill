# [오전수업] DB 24차

> 23차 수업 복습 실시


## DML(데이터 조작어 / Data Manipulation Language)
- 테이블의 데이터를 조작하는데 사용하는 문법
- 새로운 데이터 입력 (INSERT)
- 기존 데이터 갱신(UPDATE)
- 기존 데이터 삭제(DELETE)

### INSERT 구문

![1](https://user-images.githubusercontent.com/95197594/165093075-3bf0826d-47de-4adf-b4bf-4e969b84d9ac.png)


- insert into 절 : 데이터를 입력할 테이블명과 입력 값의 컬럼의 목록
- values 절 : 입력할 값들의 목록 (작성 시 컬럼에 대한 데이터타입 크기에 맞춰 작성)
```
- 컬럼의 목록을 작성한경우 VALUES절의 값은 목록의 컬럼 순서에 맞춰 작성해야한다.

INSERT INTO departments (department_id, department_name, manager_id, location_id)
VALUES (280, 'Prog', 100, 1700);


DEPARTMENT_ID|DEPARTMENT_NAME     |MANAGER_ID|LOCATION_ID|
-------------+--------------------+----------+-----------+
…
          240|Government Sales    |          |       1700|
          250|Retail Sales        |          |       1700|
          260|Recruiting          |          |       1700|
          270|Payroll             |          |       1700|
          280|Prog                |       100|       1700|
```

NULL값이 입력되는 경우
```
- 아래와 같이 일부 컬럼에만 입력값을 작성한 경우 입력값이 없는 나머지 컬럼에는 기본적으로 NULL값이 입력된다.
manager_id, location_id 컬럼에는 NULL이 입력되었음.
INSERT INTO departments (department_id, department_name)
VALUES (290, 'shopping');


DEPARTMENT_ID|DEPARTMENT_NAME     |MANAGER_ID|LOCATION_ID|
-------------+--------------------+----------+-----------+
…
          260|Recruiting          |          |       1700|
          270|Payroll             |          |       1700|
          280|Prog                |       100|       1700|
          290|shopping            |          |           |



------------------------------------------------------------------------------------------------------------------------



- 컬럼의 목록을 생략하는 경우 테이블의 모든 컬럼에 대한 입력값을 VALUES절에 작성해야 한다.
- 특정 컬럼에 대해서 입력할 값이 없어 NULL을 입력을 원하는 경우 값을 생략할 수 없으므로 명시적으로 NULL값을 입력해줘야 한다.
- 명시적으로 NULL값을 입력하는 경우 컬럼에 설정된 기본값을 무시하고 NULL값이 입력된다.

INSERT INTO departments
VALUES (300, 'Tetris', NULL, NULL);


DEPARTMENT_ID|DEPARTMENT_NAME     |MANAGER_ID|LOCATION_ID|
-------------+--------------------+----------+-----------+
…
          260|Recruiting          |          |       1700|
          270|Payroll             |          |       1700|
          280|Prog                |       100|       1700|
          290|shopping            |          |           |
          300|Tetris              |          |           |
```

서브쿼리로 데이터 입력하기
```
실습 테이블 생성
CREATE TABLE sales_reps
    AS
        ( SELECT employee_id id, last_name   name, salary, commission_pct
        FROM employees
        WHERE 1 = 2
        );


desc sales_reps

이름             널?       유형           
-------------- -------- ------------ 
ID                      NUMBER(6)    
NAME           NOT NULL VARCHAR2(25) 
SALARY                  NUMBER(8,2)  
COMMISSION_PCT          NUMBER(2,2)  



SELECT * FROM sales_reps;

ID|NAME|SALARY|COMMISSION_PCT|
--+----+------+--------------+



------------------------------------------------------------------------------------------------------------------



- VALUES절 대신 서브쿼리 작성
- 이 경우 서브쿼리의 출력 컬럼의 순서와 insert into절의 컬럼의 순서, 데이터타입에 주의!


INSERT INTO sales_reps (
    id, name, salary, commission_pct
)
    SELECT employee_id, last_name, salary, commission_pct
    FROM employees
    WHERE job_id LIKE '%REP%';


SELECT * FROM sales_reps;

ID |NAME      |SALARY|COMMISSION_PCT|
---+----------+------+--------------+
150|Tucker    | 10000|           0.3|
151|Bernstein |  9500|          0.25|
152|Hall      |  9000|          0.25|
153|Olsen     |  8000|           0.2|
154|Cambrault |  7500|           0.2|
155|Tuvault   |  7000|          0.15|
156|King      | 10000|          0.35|
157|Sully     |  9500|          0.35|
158|McEwen    |  9000|          0.35|
…
```

### UPDATE 구문
- 기존 행의 데이터를 다른 값으로 갱신하는 구문

![2](https://user-images.githubusercontent.com/95197594/165093086-5342d94f-8dda-4fd0-8616-49f01e958e0e.png)

- UPDATE절	: 값이 갱신될 테이블명시
- SET절		: 갱신될 값을 작성하는 절
- WHERE절	: 테이블에서 갱신될 행을 선택하는 조건. 옵션절


```
- 실습용 emp2 테이블 생성 / 기존 employees테이블을 통으로 복사

CREATE TABLE emp2
    AS
        ( SELECT *
        FROM employees
        );


SELECT employee_id, last_name, department_id
FROM emp2
WHERE employee_id = 113;

EMPLOYEE_ID|LAST_NAME|DEPARTMENT_ID|
-----------+---------+-------------+
        113|Popp     |          100|



---------------------------------------------------------------------------------------------------------



113 사원의 department_id컬럼의 값을 50으로 갱신
UPDATE emp2
SET department_id = 50
WHERE employee_id = 113;

EMPLOYEE_ID|LAST_NAME|DEPARTMENT_ID|
-----------+---------+-------------+
        113|Popp     |           50|
```

WHERE절 없이 갱신
```
- 현재 emp2 테이블의 department_id컬럼의 값은 12가지이다.

SELECT DISTINCT department_id
FROM emp2;

DEPARTMENT_ID|
-------------+
          100|
           30|
             |
           90|
           20|
           70|
          110|
           50|
           80|
           40|
           60|
           10|



------------------------------------------------------------------------------------------------------------------------



emp2테이블의 모든 행에 대해서 department_id 컬럼의 값을 110으로 갱신
UPDATE emp2
SET department_id = 110;


SELECT DISTINCT department_id
FROM emp2;

DEPARTMENT_ID|
-------------+
          110|
```

서브쿼리를 활용한 UPDATE구문 작성
```
SELECT employee_id, salary, job_id
FROM emp2
WHERE employee_id IN (113, 205);


EMPLOYEE_ID|SALARY|JOB_ID    |
-----------+------+----------+
        113|  6900|FI_ACCOUNT|
        205| 12008|AC_MGR    |



------------------------------------------------------------------------------------------------------------------------



- 113번 사원에 대해서 205번 사원의 job_id, salary컬럼의 값을 갱신하는 구문

UPDATE emp2
SET job_id = (
    SELECT job_id
    FROM employees
    WHERE employee_id = 205
), salary = (
    SELECT salary
    FROM employees
    WHERE employee_id = 205
)
WHERE employee_id = 113;


SELECT employee_id, salary, job_id
FROM emp2
WHERE employee_id IN (113, 205);


EMPLOYEE_ID|SALARY|JOB_ID|
-----------+------+------+
        113| 12008|AC_MGR|
        205| 12008|AC_MGR|
```

### DELETE구문
- 기존 테이블의 데이터를 삭제하는 구문

![3](https://user-images.githubusercontent.com/95197594/165093087-0a548b6d-584a-4bdb-b8a6-8e7e1e9795dc.png)

- DELETE 절 : 데이터를 삭제 할 테이블 명시. FROM 키워드는 기능이 없으며 생략가능하다(Oracle DB).
- WHERE 절 : 삭제 할 데이터를 선택하는 조건절. 옵션

```

DEPARTMENT_ID|DEPARTMENT_NAME     |MANAGER_ID|LOCATION_ID|
-------------+--------------------+----------+-----------+
…
          260|Recruiting          |          |       1700|
          270|Payroll             |          |       1700|
          280|Prog                |       100|       1700|
          290|shopping            |          |           |



--------------------------------------------------------------------------------------------------------------------------



DELETE FROM departments
WHERE department_name = 'shopping';

DEPARTMENT_ID|DEPARTMENT_NAME     |MANAGER_ID|LOCATION_ID|
-------------+--------------------+----------+-----------+
…
          260|Recruiting          |          |       1700|
          270|Payroll             |          |       1700|
          280|Prog                |       100|       1700|
          300|Tetris              |          |           |
```

where 절 생략한 DELETE구문
- WHERE절이 생략된 경우 테이블의 모든 행이 삭제 된다.

```
DELETE FROM emp2;


SELECT * FROM emp2;

EMPLOYEE_ID|FIRST_NAME|LAST_NAME|EMAIL|PHONE_NUMBER|HIRE_DATE|JOB_ID|SALARY|COMMISSION_PCT|MANAGER_ID|DEPARTMENT_ID|
-----------+----------+---------+-----+------------+---------+------+------+--------------+----------+-------------+
```

## 트랜잭션
- 트랜잭션이 진행 중인 경우 작업의 내용은 데이터베이스에 저장되지 않는다.
- 한번에 처리되어야 할 논리적인 작업단위
- 여러 개의 DML구문이 모여서 트랜잭션을 구성할 수 있다.
- 하나의 DDL, DCL구문이 트랜잭션을 구성한다.

![4](https://user-images.githubusercontent.com/95197594/165093088-ee5e54dc-1482-45f9-a6aa-091f2970e96b.png)

### TCL(Transaction Control Language)
- DML구문은 사용자가 TCL구문을 통해서 직접 트랜잭션을 종료할 수 있다.
- COMMIT : 트랜잭션의 작업을 DB에 반영
- ROLLBACK : 트랜잭션의 작업을 취소
- SAVEPOINT : 트랜잭션 진행 중 돌아갈 수 있는 지점을 생성

![5](https://user-images.githubusercontent.com/95197594/165093091-38b27704-09d5-479a-a17d-2f67e56f3f9a.png)


```
SELECT * FROM sales_reps;
ID |NAME      |SALARY|COMMISSION_PCT|
---+----------+------+--------------+
…
177|Livingston|  8400|           0.2|
178|Grant     |  7000|          0.15|
179|Johnson   |  6200|           0.1|
202|Fay       |  6000|              |
203|Mavris    |  6500|              |
204|Baer      | 10000|              |



--------------------------------------------------------------------------------------------------------------------------


- 200번대 사번 사원 데이터 삭제

DELETE FROM sales_reps
WHERE id > 200;

SAVEPOINT after_del_200;

SELECT * FROM sales_reps;
ID |NAME      |SALARY|COMMISSION_PCT|
---+----------+------+--------------+
157|Sully     |  9500|          0.35|
158|McEwen    |  9000|          0.35|
159|Smith     |  8000|           0.3|
160|Doran     |  7500|           0.3|
161|Sewall    |  7000|          0.25|
162|Vishney   | 10500|          0.25|



--------------------------------------------------------------------------------------------------------------------------


- 160번대 초과 사번의 사원 삭제
DELETE FROM sales_reps
WHERE id > 160;

SELECT * FROM sales_reps;
ID |NAME      |SALARY|COMMISSION_PCT|
---+----------+------+--------------+
157|Sully    |  9500|          0.35|
158|McEwen   |  9000|          0.35|
159|Smith    |  8000|           0.3|
160|Doran    |  7500|           0.3|



--------------------------------------------------------------------------------------------------------------------------

- 선택 savepoint 지점으로 롤백
ROLLBACK TO SAVEPOINT after_del_200;

SELECT * FROM sales_reps;
ID |NAME      |SALARY|COMMISSION_PCT|
---+----------+------+--------------+
159|Smith     |  8000|           0.3|
160|Doran     |  7500|           0.3|
161|Sewall    |  7000|          0.25|
162|Vishney   | 10500|          0.25|
163|Greene    |  9500|          0.15|
164|Marvins   |  7200|           0.1|
165|Lee       |  6800|           0.1|
```
---

# [오후수업] JAVA 37차

- 연습문제1
```java
package book;

public class Ex1 {

	public static void main(String[] args) {
		// 386p
		// Q1)
		// 두 개의 인스턴스가 메모리는 다르더라도 논리적으로 동일하다는 것을 
		// 구현하는 Object의 메서드는 eXXX 입니다. 정답: equals()
		
		// Q2)
		// String 클래스는 멤버로 가지는 문자열 변수가 final이어서 변하지 않습니다. 다음과 같이 두 개의
		// String 변수를 연결할 때 힙 메모리에 생성되는 String 인스턴스를 그려보세요.
//		String a = new String("abc");
//		String b = new String("def");
//		a = a + b;
//		String c = new String("abc");
//		String d = "abc";
		
		// ------------------------------------
//		String a = "abc";
//		String b = "def";
//		a = a + b;
//		String c = "abc";
//		System.out.println(a == c);
		
		// Q3)
		// 기본 자료형을 멤버 변수로 포함하여 메서드를 제공함으로써 기본 자료형의 객체를 
		// 제공하는 클래스를 WXXX 라고 합니다. 정답: Wrapper Class
		
		// Q4)
		MyDog dog = new MyDog("멍멍이", "진돗개");
		System.out.println(dog);	// 출력결과: 진돗개 멍멍이 
		
	}

}

//Q4)
//다음 코드의 결과가 '진돗개 멍멍이' 가 되도록 MyDog클래스를 수정하세요.
class MyDog {
	String name;
	String type;
	
	public MyDog(String name, String type) {
		this.name = name;
		this.type = type;
	}

	@Override
	public String toString() {
		return type + " " + name;
	}
}
```

- 연습문제2
```java
package book;

import java.util.HashSet;
import java.util.Iterator;

public class Ex2 {

	public static void main(String[] args) {
		// 446p
		// Q1)
		// 자료구조를 사용하기 편리하도록 자바에서 제공하는 라이브러리를 컬XXX 라고 합니다.
		// 정답: 컬렉션 프레임워크, 컬렉션 라이브러리, 컬렉션 API
		
		// Q2)
		// 클래스에서 여러 자료형을 사용할 때 자료형을 명시하지 않고 자료형을 의미하는
		// 문자로 선언한 후 실제 클래스를 생성할 때 자료형을 명시하는 프로그래밍 방식을 제XXX 이라고 합니다.
		// 정답: 제네릭
		
		// Q3)
		// Collection 인터페이스를 구현한 클래스를 순회하기 위해 사용하는 인터페이스는 IXXX 입니다.
		// 정답: Iterator
		
		// Q5)
		// 출력결과가 다음과 같도록 Student 클래스를 구현해 보세요.
		HashSet<Student> set = new HashSet<Student>();
		set.add(new Student("100", "홍길동"));
		set.add(new Student("200", "강감찬"));
		set.add(new Student("300", "이순신"));
		set.add(new Student("400", "정약용"));
		set.add(new Student("100", "송중기"));
		
		System.out.println(set);
		// < 출력 결과 > (출력 순서는 무관)
		// [100:홍길동, 200:강감찬, 300:이순신, 400:정약용, 100:송중기]
	}

}

class Student {
	String num;
	String name;
	
	public Student(String num, String name) {
		this.num = num;
		this.name = name;
	}

	@Override
	public String toString() {
		return num + ":" + name;
	}
	
}
```

- 연습문제3
```java
package book;

import java.util.ArrayList;
import java.util.List;

public class Ex3 {

	public static void main(String[] args) {
		// 484p
		// Q1)
		// 지역 내부 클래스에서 외부 클래스 메서드의 지역변수를 사용할 수 있지만,
		// 그 값을 변경하면 오류가 발생합니다. 이때 사용하는 지역 변수는 fXXX 변수가 되기 때문입니다.
		// 정답: final
		
		// Q2)
		// 내부 클래스 중 클래스 이름 없이 인터페이스나 추상 클래스 자료형에 직접 대입하여
		// 생성하는 클래스를 익XXX 라고 합니다.
		// 정답: 익명클래스
		
		// Q3)
		// 자바에서 제공하는 함수형 프로그래밍 방식으로 인터페이스의 메서드를 직접 구현하는
		// 코드를 람XXX 라고 합니다.
		// 정답: 람다식
		
		// Q4)
		// 람다식으로 구현할 수 있는 인터페이스는 메서드를 하나만 가져야합니다.
		// 이러한 인터페이스를 함XXX 라고 합니다.
		// 정답: 함수형 인터페이스 @FunctionalInterface
		
		// Q5)
		// 다음과 같이 두 정수를 매개변수로 하는 메서드가 인터페이스에 정의되어 있습니다.
		// 두 정수의 합을 반환하는 람다식을 구현하고 호출해보세요.
		
		// 구현
		Calc cal = (num1, num2) -> num1 + num2;
		// 호출
		System.out.println(cal.add(10, 20));
		
		// Q6)
		List<Book> bookList = new ArrayList<Book>();
		bookList.add(new Book("자바", 25000));
		bookList.add(new Book("파이썬", 15000));
		bookList.add(new Book("안드로이드", 30000));
		
		// 위와 같을 때 스트림을 활용하여 다음처럼 책 목록을 출력해보세요.
		// 1. 모든 책의 가격의 합
		int total = bookList.stream().mapToInt(book -> book.getPrice()).sum();
		System.out.println(total);
		
		// 2. 책의 가격이 20000원 이상인 책의 이름을 정렬하여 출력
		bookList.stream().filter(book -> book.getPrice() >= 20000)
						 .map(book -> book.getName())
						 .sorted().forEach(b -> System.out.println(b));
	}

}

// Q5)
interface Calc {
	public int add(int num1, int num2);
}

//Q6)
class Book {
	private String name;
	private int price;
	
	public Book(String name, int price) {
		this.name = name;
		this.price = price;
	}

	public String getName() {
		return name;
	}

	public int getPrice() {
		return price;
	}
	
}
```

### 자바의 기본 데이터 입출력
- DataInputStream, DataOutputStream 사용
- 자바의 기본데이터타입 8가지 + 문자열(String)타입 처리 가능
  - readXXX(), writeXXX() 메서드 사용하며 XXX은 기본데이터타입의 이름 사용
    - ex) int형 데이터 출력시 : writeInt()
  	- ex) double형 데이터 입력 시 : readDouble()
  - 주의! String 타입은 XXX 부분에 String 대신 UTF 사용
    - ex) readString() 메서드 (X) => readUTF() 메서드 (O)
- 주의사항!
  - 반드시 두 객체 끼리만 데이터 입출력 가능
	- 입출력 시 각각의 순서에 따라 처리해야함
    - ex) int, char, String 순으로 출력 시 int, char, String 순으로 입력해야함!
```
< 기본 문법 >
1. 기본 데이터 출력
		DataOutputStream dos = new DataOutputStream(바이트 스트림 객체);
		dos.writeXXX(데이터);
2. 기본 데이터 입력
		DataInputStream dis = new DataInputStream(바이트 스트림 객체);
		dis.readXXX(데이터);   
```

```java
package io;

import java.io.DataInputStream;
import java.io.DataOutputStream;
import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.FileOutputStream;
import java.io.IOException;

public class Ex1 {

	public static void main(String[] args) {
		
		// 자바 기본 데이터를 파일로 출력하기
		// 1. FileOutputStream 객체를 생성하여 출력할 파일 위치 및 파일명 지정
//		FileOutputStream fos = new FileOutputStream("C:\\temp\\data.txt");
		// 2. DataOutputSream 객체를 생성하여 FileOutputStream 객체를 전달
//		DataOutputStream dos = new DataOutputStream(fos);
		
//		try (DataOutputStream dos = new DataOutputStream(new FileOutputStream("C:\\temp\\data.txt"))){
//			// DataOutputStream 객체를 통해 출력되는 데이터는
//			// C 드라이브 temp 폴더 내의 data.txt 파일에 출력됨(기록됨)
//			dos.writeInt(100);		// int형 정수 출력
//			dos.writeDouble(3.14);	// double형 실수 출력
//			dos.writeUTF("홍길동");	// 문자열 출력(주의! writeString() 메서드 아님!)
//			
//		} catch (FileNotFoundException e) {
//			// FileOutputStream 에서 지정한 경로가 존재하지 않을 경우 예외 발생
//			e.printStackTrace();
//		} catch (IOException e) {
//			e.printStackTrace();
//		}
		
		System.out.println("=====================================================");
		
		// 파일에 출력된 자바 기본데이터를 읽어와서 화면에 출력하기
		try(DataInputStream dis = new DataInputStream(new FileInputStream("C:\\temp\\data.txt"))){
			
			// dis.readXXX() 메서드 호출하여 데이터 읽어오기(입력)
			// 읽어들일 데이터는 반드시 출력된 데이터 순으로 읽어야 한다!
			// => 출력 순서: int -> double -> String 이므로, 입력 순서도 동일해야함
			// => 순서가 바뀔 경우 EOFException 예외가 발생하므로 주의!
			int num = dis.readInt();
			double dNum = dis.readDouble();
			String str = dis.readUTF();
			
			// 입력받은 데이터 출력
			System.out.println("int형 정수: " + num);
			System.out.println("double형 실수: " + dNum);
			System.out.println("문자열: " + str);
			
		} catch(FileNotFoundException e) {
			e.printStackTrace();
		} catch (IOException e) {
			e.printStackTrace();
		}
		
		
	}

}
```

### 객체 직렬화(Serialization) & 역직렬화(Deserialization)
- 자바에서 사용하는 객체에 영속성을 부여하여 파일 또는 네트워크 등으로 내보내는 것을 직렬화라고 하며, 반대로 파일이나 네트워크로부터 데이터를 읽어 객체로 변환하는 것을 역직렬화라고 함
- ObjectInputStream, ObjectOutputStream 클래스 사용
- 주의! 직렬화 대상이 되는 클래스를 정의할 때는 반드시 Serialization 인터페이스 상속 필수!  
- 만약, 직렬화 클래스 내에서 출력 대상으로부터 제외시킬 변수가 있을 경우 해당 변수 선언부 앞에 transient 키워드를 사용하면 출력 대상에서 제외됨

```java
package io;

import java.io.File;
import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.ObjectInputStream;
import java.io.ObjectOutputStream;
import java.io.Serializable;

public class Ex2 {

	public static void main(String[] args) {
		
		// Person 객체 생성
//		Person p = new Person("홍길동", 20, "901010-1234567");
//		
//		// File 경로 관리를 위한 File 객체 생성
		File f = new File("C:\\temp\\person.txt");
//		
//		// try ~ resource 구문으로 Person 객체 직렬화(= Serialization)
//		// => ObjectOutputStream 객체를 생성하여 FileOutputStream 객체 연결
//		try(ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream(f))){
//			
//			oos.writeObject(p);
//			
//		} catch (FileNotFoundException e) {
//			e.printStackTrace();
//		} catch (IOException e) {
//			e.printStackTrace();
//		}
//		
//		System.out.println("객체 출력 완료!");
		
		System.out.println("==============================================");
		
		// 외부 폴더(C드라이브 - temp - person.txt) 에 저장되어 있는 파일 내의
		// Person 객체를 ObjectInputStream 객체를 사용하여 읽어오기
		// => 역직렬화(Deserialization)
		try(ObjectInputStream ois = new ObjectInputStream(new FileInputStream(f))){
			
			// ObjectInputStream 객체의 readObject() 메서드를 호출하여
			// 파일에 저장된 객체를 Object 타입으로 읽어오기
			// => 리턴타입이 Object 타입이므로 실제 객체 사용을 위해 다운캐스팅 필요
//			Person person = (Person)ois.readObject();
			
			Object o = ois.readObject();
			if(o instanceof Person) {
				Person person = (Person)o;
				// toString() 메서드가 오버라이딩 되어 있으므로 변수명 바로 전달
				System.out.println(person);
			}
			
		} catch (FileNotFoundException e) {
			e.printStackTrace();
		} catch (IOException e) {
			e.printStackTrace();
		} catch (ClassNotFoundException e) {
			e.printStackTrace();
		}
		
		


		
	}

}

// 직렬화를 위한 Person 클래스 정의
// => 주의! 직렬화 대상이 되는 클래스를 정의할 때 반드시 Serializable 인터페이스 상속 필수!
// 별도의 추상메서드가 없는 단순히 마커(Marker)용도의 인터페이스
class Person implements Serializable {
	String name;
	int age;
	String jumin;
	
	// Alt + Shift + S -> O
	public Person(String name, int age, String jumin) {
		this.name = name;
		this.age = age;
		this.jumin = jumin;
	}

	// toString() 오버라이딩
	// Alt + Shift + S -> S
	@Override
	public String toString() {
		return "Person [name=" + name + ", age=" + age + ", jumin=" + jumin + "]";
	}
	
}
```

### 키보드로 부터 입력받은 데이터를 파일로 출력
- 키보드로부터 입력받기
1) System.in을 통해 키보드로부터 입력받는 입력스트림을 InputStream 객체로 연결
    - byte 단위로 처리
2) InputStream -> InputStreamReader 객체로 변환하여 char단위로 처리
3) InputStreamReader -> BufferedReader 객체로 변환하여 String 단위로 처리
4) BufferedReader 객체로부터 입력스트림 한줄(Line) 단위로 읽어와서 출력

- 파일로 출력하기
1) File 객체를 사용하여 출력할 파일 위치 및 이름을 지정
2) FileWriter 객체를 사용하여 char 단위로 처리(File 객체 전달)
3) FileWriter -> PrintWriter 객체로 변환하여 출력


```java
package io;

import java.io.BufferedReader;
import java.io.File;
import java.io.FileWriter;
import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.io.PrintWriter;

public class Ex3 {

	public static void main(String[] args) {
  
//		InputStream is = System.in;
//		InputStreamReader isr = new InputStreamReader(is);
//		BufferedReader buffer = new BufferedReader(isr);
		
		// 위문장을 한줄로 표현하면 Scanner sc = new Scanner(System.in);
//		BufferedReader buffer = new BufferedReader(new InputStreamReader(System.in));
		
		File f = new File("C:\\temp\\readme.txt");
//		FileWriter fw = new FileWriter(f);
//		PrintWriter pw = new PrintWriter(fw);
		
		// 위 문장을 한줄로 표현하면
//		PrintWriter out = new PrintWriter(new FileWriter(f));
		
		try(BufferedReader buffer = new BufferedReader(new InputStreamReader(System.in));
				PrintWriter out = new PrintWriter(new FileWriter(f))){
			
			String str = buffer.readLine();
			out.println(str);
			
		} catch (IOException e) {
			e.printStackTrace();
		}
		
		
	}

}
```

- 연습문제1
```java
package io;

import java.io.BufferedReader;
import java.io.File;
import java.io.FileWriter;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.PrintWriter;

public class Test3 {

	public static void main(String[] args) {
		/*
		 * 키보드로 부터 입력받은 내용을 readme.txt 파일에 출력
		 * - 여러 줄을 입력 가능하도록 반복 입력 처리
		 * - 단, ":wq" 문자열이 입력되면 입력을 종료
		 * */
		
		System.out.println("데이터를 입력하세요. (종료 시 :wq 입력)");
		
		File f = new File("C:\\temp\\readme.txt");
		
		try(BufferedReader buffer = new BufferedReader(new InputStreamReader(System.in));
				PrintWriter out = new PrintWriter(new FileWriter(f))){
			
			String str = buffer.readLine();
			
			// 입력 문자열이 ":wq" 가 아닐동안 반복
			while(!str.equals(":wq")) {
				out.println(str);		// 입력데이터 한줄 출력
				str = buffer.readLine();// 새로운 한줄 읽어오기
			}
			
			
		} catch (IOException e) {
			e.printStackTrace();
		}
		
		
	}

}
```

- 연습문제2
```java
package io;

import java.io.BufferedReader;
import java.io.File;
import java.io.FileReader;
import java.io.FileWriter;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.PrintWriter;

public class Test4 {

	public static void main(String[] args) {
		/*
		 * temp 폴더에 저장된 source.txt 파일을 읽어들여
		 * 각 라인에 라인번호를 추가하여 콘솔(화면)에 출력
		 * - FileReader, BufferedReader 사용
		 *   (키보드로 입력받는 InputStreamReader 대신 File로 부터 입력받는 FileReader 사용)
		 * 
		 * */
//		try(BufferedReader buffer = new BufferedReader(new InputStreamReader(System.in));
//				PrintWriter out = new PrintWriter(new FileWriter(f))){
//			
//			String str = buffer.readLine();
//			
//			// 입력 문자열이 ":wq" 가 아닐동안 반복
//			while(!str.equals(":wq")) {
//				out.println(str);		// 입력데이터 한줄 출력
//				str = buffer.readLine();// 새로운 한줄 읽어오기
//			}
//			
//			
//		} catch (IOException e) {
//			e.printStackTrace();
//		}
		
		File f = new File("C:\\temp\\source.txt");
		try(BufferedReader buffer = new BufferedReader(new FileReader(f))){
			
			int count = 1;
			
			String str = buffer.readLine();
			while(str != null) {
				System.out.println(count + " " + str);	// 라인번호 붙여 출력
				str = buffer.readLine();	// 새로운 한 줄 읽어오기
				count++;	// 라인번호 증가
			}
			
		} catch (IOException e) {
			e.printStackTrace();
		}
		
		
		
		
	}

}
```

