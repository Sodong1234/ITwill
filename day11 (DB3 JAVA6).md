# [오전수업] DB 4차

> 가상머신 내보내기 & 불러오기
> - 내보내기 : Oracle VM VirtualBox 실행 -> 파일 -> 가상시스템 내보내기 -> 내보내기 할 가상머신 선택 -> MAC 주소정책을 '모든 네트워크 어댑터 MAC 주소 제외' 선택 -> 내보내기 끝
> - 불러오기 : Oracle VM VirtualBox 실행 -> 파일 -> 가상시스템 가져오기 -> 불러오기 할 가상머신 선택 -> MAC 주소정책을 '모든 네트워크 어댑터 MAC 주소 생성' 선택 -> 불러오기 끝
>   - 본인이 설정해놓은 가상머신을 어느 컴퓨터에서든 자유롭게 사용 가능


## 가상머신 터미널 원격접속하기
- 가상머신 실행 후 root 입력 -> 비밀번호 1234 입력-> 로그인 성공했다면 ip add show 입력 -> 본인의 장치명과 아이피 확인 -> ssh root@아이피주소 입력 -> 비밀번호 입력
![캡처](https://user-images.githubusercontent.com/95197594/152708890-ee36e657-6bf3-479f-bf06-999056c93b54.PNG)

## MySQL (CLI 환경에서 접속)
- mysql -u root -p 입력 -> 비밀번호 입력

### 데이터베이스 목록 조회 & 생성 & 선택 & 삭제 명령어
- show databases; : 목록 조회
- CREATE DATABASE test; : test 데이터베이스 생성
- use test; : test 데이터베이스 선택
- DROP DATABASE test; : test 데이터베이스 삭제

## 테이블 생성 및 수정
```
-문법

CREATE TABLE 테이블명 (
컬럼명1      데이터타입1(크기) [DEFAULT] [제약조건] [,
컬럼명2 …]
);
```
- 데이터타입
  - 숫자 : INT(정수) / float(실수) 
  - 문자 : CHAR(고정크기) / VARCHAR(가변크기)
    - 고정크기 : 10바이트를 설정해주었으면 한 글자만 입력하고 끝내도 10바이트로 인식됨
    - 가변크기 : 10바이트를 설정해주었으면 한 글자당 1바이트로 인식됨. VARCHAR이 CHAR보다 좀 더 효율적 
  - 날짜 : DATE, TIMESTAMPE 
    - 크기 지정 하지 않음. 

### 생성
```
CREATE TABLE test_table ( 입력
idx INT(10), 입력
name  VARCHAR(10) 입력
); 입력
```
### 테이블 목록 및 구조 조회
- show tables; 입력 -> 생성한 테이블이 확인됨.
- DESC test_table 입력 -> 생성한 테이블의 구조가 확인됨.
```
+-------+-------------+------+-----+---------+-------+
| Field | Type        | Null | Key | Default | Extra |
+-------+-------------+------+-----+---------+-------+
| idx   | int         | YES  |     | NULL    |       |
| name  | varchar(10) | YES  |     | NULL    |       |
+-------+-------------+------+-----+---------+-------+
```
- Null : 아직 입력되지 않은 값. (현재 비어있음) YES는 입력을 허용한다는 뜻!
  - 숫자 0, 문자 공백과 전혀 다른 값
- Default : 기본값. 컬럼(열)에 데이터 입력 값을 주지 않은 경우 해당 컬럼에 NULL값이 입력.

### 테이블 수정 (ALTER TABLE)
- 컬럼 추가
```
-문법

ALTER TABLE 테이블명
ADD   컬럼명 데이터타입(크기);
```
```
ALTER TABLE text_table 입력
ADD Salary INT(20); 입력


+--------+-------------+------+-----+---------+-------+
| Field  | Type        | Null | Key | Default | Extra |
+--------+-------------+------+-----+---------+-------+
| idx    | int         | YES  |     | NULL    |       |
| name   | varchar(10) | YES  |     | NULL    |       |
| salary | int         | YES  |     | NULL    |       |
+--------+-------------+------+-----+---------+-------+
```
- 컬럼 이름 변경
```
-문법

ALTER TABLE 테이블명
CHANGE 기존컬럼명 변경할컬럼명 데이터타입(크기)
```
```
ALTER TABLE test_table 입력
CHANGE idx test_number INT(10); 입력


+-------------+-------------+------+-----+---------+-------+
| Field       | Type        | Null | Key | Default | Extra |
+-------------+-------------+------+-----+---------+-------+
| test_number | int         | YES  |     | NULL    |       |
| name        | varchar(10) | YES  |     | NULL    |       |
| salary      | int         | YES  |     | NULL    |       |
+-------------+-------------+------+-----+---------+-------+
```
- 컬럼 데이터타입 변경
```
-문법

ALTER TABLE 테이블명
MODIFY 컬럼명 변경할데이터타입(크기)
```
```
ALTER TABLE test_table 입력
MODIFY name CHAR(10); 입력


+-------------+----------+------+-----+---------+-------+
| Field       | Type     | Null | Key | Default | Extra |
+-------------+----------+------+-----+---------+-------+
| test_number | int      | YES  |     | NULL    |       |
| name        | char(10) | YES  |     | NULL    |       |
| salary      | int      | YES  |     | NULL    |       |
+-------------+----------+------+-----+---------+-------+
```
- 컬럼 삭제
```
-문법

ALTER TABLE 테이블명
DROP 컬럼명;
```
```
ALTER TABLE test_table 입력
DROP salary; 입력


+-------------+----------+------+-----+---------+-------+
| Field       | Type     | Null | Key | Default | Extra |
+-------------+----------+------+-----+---------+-------+
| test_number | int      | YES  |     | NULL    |       |
| name        | char(10) | YES  |     | NULL    |       |
+-------------+----------+------+-----+---------+-------+
```
- 테이블명 변경
```
-문법

ALTER TABLE 기존테이블명
RENAME 변경할테이블명;
```
```
ALTER TABLE test_table 입력
RENAME test_itwill; 입력


+-------------+----------+------+-----+---------+-------+
| Field       | Type     | Null | Key | Default | Extra |
+-------------+----------+------+-----+---------+-------+
| test_number | int      | YES  |     | NULL    |       |
| name        | char(10) | YES  |     | NULL    |       |
+-------------+----------+------+-----+---------+-------+
```
- 테이블 삭제
```
-문법

DROP TABLE 테이블명;
```
```
DROP TABLE test_itwill;


mysql> show tables;
Empty set (0.00 sec)
```

### 테이블 생성 연습
```
테이블 명 : student
컬럼명1 : stu_no
데이터타입1 : INT
컬럼명2 : last_name
데이터타입2 : VARCHAR(20)
컬럼명3 : birth_date
데이터타입3 : DATE



CREATE TABLE student (
stu_no INT,
last_name VARCHAR(20),
birth_date DATE
);




mysql> DESC student;

+------------+-------------+------+-----+---------+-------+
| Field      | Type        | Null | Key | Default | Extra |
+------------+-------------+------+-----+---------+-------+
| stu_no     | int         | YES  |     | NULL    |       |
| last_name  | varchar(20) | YES  |     | NULL    |       |
| birth_date | date        | YES  |     | NULL    |       |
+------------+-------------+------+-----+---------+-------+
```

## DDL (Data Definition Language) / 데이터 정의어
- 데이터 저장 구조를 다루는 문법
	- CREATE : 오브젝트 생성
	- ALTER : 오브젝트 변형
	- DROP : 오브젝트 삭제

키워드 : 기능 할당 된 단어
키워드 + 요소 = 절
절 + 절 = 문법
---

 

# [오후수업] 자바 6차
## 클래스의 정의
- 객체를 나타내는 설계도!
- 표현하려는 대상 객체의 이름을 지정하여 클래스를 정의
- 클래스 내의 멤버변수는 인스턴스 생성 시 각 인스턴스마다 별도의 공간을 할당 받음
  - 이 때, 각 멤버변수는 자동으로 기본값 초기화가 수행됨
-	클래스 내의 메서드는 인스턴스 생성 후 호출을 통해 작업을 수행하게 됨.
- 클래스 정의 후에는 반드시 인스턴스화를 통해 메모리 상에 실제 객체를 생성해야함 

```
< 기본 문법 >
		[제한자] class 클래스명 {
 		// 멤버변수 (= 객체의 정보를 저장할 변수 = 속성)
 		// 생성자 (= 객체의 정보를 초기화하는 역할)
 		// 메서드 (= 객체의 수행할 동작 = 기능)
```
```java
ex)
public class person {
	String name; // 이름
	int age; // 나이
	boolean isHungry; // 배고픔 유무

	public void eat() {
		System.out.println("먹다!");
	}

	public void talk() {
		System.out.println("말하다!");
	}
}
```

### 클래스 인스턴스 생성
- 클래스의 객체(인스턴스 = instance) 생성 = 인스턴스화
- new 키워드를 사용하여 인스턴스 생성
  - Heap 공간의 특정 위치에 클래스에 대한 인스턴스가 생성됨. 또한, 생성된 공간의 주소값이 리턴됨

```
 < 클래스 인스턴스 생성 기본 문법 >
클래스명 변수명;	              ex) int a;
변수명 = new 클래스명();	      ex) a = 10;
=> 위의 두 문장을 한 문장으로 결합할 경우
클래스명 변수명 => new 클래스명();     ex) int a = 10;
```

```java
//		1. Person 타입 변수 p를 선언 (int a)
//		Person p; // 참조 데이터타입 변수 p를 선언
//		System.out.println(p); // 초기화가 되어 있지 않아서 출력 불가
		
		// 2. Person 클래스의 인스턴스를 생성하여 변수 p에 해당 인스턴스 주소 전달
//		p = new person(); // Heap 공간에 생성도니 인스턴스 주소가 리턴되어 p에 저장 (a = 10)
		
		// 위의 두 문장 1,2 를 한 문장으로 결합 (int a = 10;)
		Person p = new Person();
		// 참조변수를 통해 인스턴스에 접근하여 멤버변수 값을 출력하고, 메서드 호출
		// 1. 멤버변수값 출력
		//		-> 인스턴스 생성 시 멤버변수들은 기본값으로 자동으로 초기화 수행됨
		System.out.println("이름(p.name): " + p.name);
		System.out.println("나이(p.age): " + p.age);
		System.out.println("배고픔유무(p.isHungry): " + p.isHungry);
		
		// 2. 메서드 호출
		p.eat();    // "먹다!" 호출
		p.talk();   // "말하다!" 호출
```

ex 1)
```java
// 1. Person 타입 변수 p를 선언 (int a)
//		Person p; // 참조 데이터타입 변수 p를 선언
//		System.out.println(p); // 초기화가 되어 있지 않아서 출력 불가
		
		// 2. Person 클래스의 인스턴스를 생성하여 변수 p에 해당 인스턴스 주소 전달
//		p = new person(); // Heap 공간에 생성도니 인스턴스 주소가 리턴되어 p에 저장 (a = 10)
		
		// 위의 두 문장 1,2 를 한 문장으로 결합 (int a = 10;)
		Person p = new Person();
		// 참조변수를 통해 인스턴스에 접근하여 멤버변수 값을 출력하고, 메서드 호출
		// 1. 멤버변수값 출력
		//		-> 인스턴스 생성 시 멤버변수들은 기본값으로 자동으로 초기화 수행됨
		System.out.println("이름(p.name): " + p.name);
		System.out.println("나이(p.age): " + p.age);
		System.out.println("배고픔유무(p.isHungry): " + p.isHungry);
		
		// 2. 메서드 호출
		p.eat();
		p.talk();
		
		System.out.println("=====================================");
		
		// 인스턴스의 멤버변수 초기화(값 저장)
		// 참조변수명.멤버변수명 = 값;
		p.name = "홍길동";
		p.age = 20;
		p.isHungry = true;
		System.out.println("이름(p.name): " + p.name);
		System.out.println("나이(p.age): " + p.age);
		System.out.println("배고픔유무(p.isHungry): " + p.isHungry);
		
		System.out.println("=====================================");
		
		
		Person p2 = new Person();
		// 인스턴스 p2의 이름을 "이순신", 나이를 44로 설정 후 출력
		
		p2.name = "이순신";
		p2.age = 44;
		
		System.out.println("이름(p.name) : " + p2.name);
		System.out.println("나이(p.age) : " + p2.age);
```


문제출제 1.
```
 - 은행계좌(Account) 클래스 정의
 - 멤버변수
 - 1) 계좌번호(accountNo, "xxx-xxxx-xxx" = 문자열)
 - 2) 예금주명(ownerName, "xxx" = 문자열)
 - 3) 현재잔고(balance, XXXXXX = 정수)
 
 

- 은행계좌 Account 클래스의 인스턴스 생성
- 다음과 같이 출력되도록 초기화 및 출력
 
 < 출력 예시 >
 계좌번호 : 111-1111-111
 예금주명 : 홍길동
 현재잔고 : 100000원
 ```
```java
package oop;


/*
 * 은행계좌(Account) 클래스 정의
 * - 멤버변수
 * 		1) 계좌번호(accountNo, "xxx-xxxx-xxx" = 문자열)
 * 		2) 예금주명(ownerName, "xxx" = 문자열)
 * 		3) 현재잔고(balance, XXXXXX = 정수)
 */

class Account {
	String accountNo;
	String ownerName;
	int balance;
	
}


public class Test1 {

	public static void main(String[] args) {
		/*
		 * 은행계좌 Account 클래스의 인스턴스 생성
		 * 다음과 같이 출력되도록 초기화 및 출력
		 * 
		 * < 출력 예시 >
		 * 계쫘번호 : 111-1111-111
		 * 예금주명 : 홍길동
		 * 현재잔고 : 100000원
		 * 
		 */
		Account acc = new Account();
		
		
		acc.accountNo = "111-1111-111";
		acc.ownerName = "홍길동";
		acc.balance = 100000; 
		
		System.out.println("계좌번호 : " + acc.accountNo);
		System.out.println("예금주명 : " + acc.ownerName);
		System.out.println("현재잔고 : " + acc.balance + "원");
				
	}

}
```
ex 2)
```java
package oop;

// 하나의 소스파일(.java) 내의 복수개의 클래스 정의 가능
// 단, 파일명과 동일한 클래스에만 public 접근제한자를 붙이고
// 나머지 클래스에는 public를 붙이지 않도록 한다!

//Animal 클래스 정의
// - 멤버변수 : 이름(name, 문자열), 나이(age, 정수)
// - 메서드 : cry() -파라미터와 리턴값은 없음. "동물 울음 소리!" 출력
class Animal {
	String name;
	int age;

	void cry() {
		System.out.println("동물 울음 소리!");
	}
}

public class Ex2 {

	public static void main(String[] args) {

		// Animal 인스턴스 생성(ani)
		// 이름 : 멍멍이, 나이 : 3

		Animal ani = new Animal();
		ani.name = "멍멍이";
		ani.age = 3;
		System.out.println("이름 : " + ani.name + ", 나이 : " + ani.age);
		ani.cry();

		// Animal 인스턴스 생성(ani2)
		// 이름 : 야옹이, 나이 : 2

		Animal ani2 = new Animal();
		ani2.name = "야옹이";
		ani2.age = 2;
		System.out.println("이름 : " + ani2.name + ", 나이 : " + ani.age);
		System.out.println("==========================================");
		
		Student s1 = new Student();
		System.out.println("이름 : " + s1.name);
		System.out.println("학번 : " + s1.id);
		System.out.println("나이 : " + s1.age);
		
		// 또 다른 Student 인스턴스 생성
		// => 서로 다른 공간에 생성되는 인스턴스이지만, 이름과 학번이 지정된 값으로 동일
		Student s2 = new Student(); 
		System.out.println("이름 : " + s2.name);
		System.out.println("학번 : " + s2.id);
		System.out.println("나이 : " + s2.age);
		
		System.out.println("==========================================");
		s1.name = "이순신";
		s1.id = "a1234567";
		s1.kor = 100;
		s1.eng = 80;
		s1.mat = 50;
		
		s1.showStudentInfo();
		s2.showStudentInfo();
		
		System.out.println("==========================================");
		System.out.println("합계 : " + s1.getTotal());
		System.out.println("평균 : " + s1.getAverage());
	}

}


/*
 * Student 클래스 정의
 * - 멤버변수 : 이름(name, 문자열), 학번(id, 문자열), 나이(age, 정수)
 * 	       국어점수(kor, 정수), 영어점수(eng, 정수), 수학점수(mat, 정수)
 */

class Student {
	// 멤버변수(인스턴스변수)는 초기값을 지정해도 되고, 생략도 가능
	// -> 만약, 초기값을 지정할 경우 인스턴스 생성할 때마다 해당 초기값으로 초기화
	String name = "홍길동"; // 모든 인스턴스에서 name의 기본값을 "홍길동" 으로 갖게 됨
	String id = "a0000000"; // 모든 인스턴스에서 id의 기본값을 "a0000000" 으로 갖게 됨
	int age;
	int kor;
	int eng;
	int mat;
	
	// showStudentInfo() 메서드 정의
	// 파라미터, 리턴값 없음. 다음과 같이 출력
	// 이름 : XXX
	// 학번 : XXX
	// 나이 : XXX
	// 국어점수 : XXX점...
	
	void showStudentInfo() {
		// 자신의 클래스에서 선언된 멤버변수는 클래스 내의 모든 메서드에서 접근이 가능
		// => 이 때, 별도의 주소 지정 없이 변수명만으로 접근이 가능
		// => 각 인스턴스에서 동일한 메서드를 호출하더라도 
		//	  인스턴스마다 멤버변수가 다르므로 실행 시 실제 저장된 데이터들은 서로 다름
		System.out.println("이름 : " + name);
		System.out.println("학번 : " + id);
		System.out.println("나이 : " + age);
		System.out.println("국어점수 : " + kor + "점");
		System.out.println("영어점수 : " + eng + "점");
		System.out.println("수학점수 : " + mat + "점");
		
		
	}
	
	int getTotal(){
		int sum = kor + eng + mat;
		return sum;
	}
	double getAverage() {
//		double avg = (kor + eng + mat) / 3.0;
//		return avg;
		
//		return (kor + eng + mat) / 3.0;
		
		// 합계 계산 코드를 직접 작성하지 않고, 기존의 getTotal() 메서드를 재사용
		// => 메서드 내에서 자신의 클래스 내에 있는 다른 메서드를 호출할 경우
		//	  멤버변수 접근 방법과 동일하게 호출하면 됨
		return getTotal() / 3.0;
	}
}




```

문제출제
```
 * 은행계좌(Account2) 클래스 정의
 * - 멤버변수
 * 	1) 계좌번호(accountNo, "xxx-xxxx-xxx" = 문자열)
 * 	2) 예금주명(ownerName, "XXX" = 문자열)
 * 	3) 현재잔고(balance, XXXXXXX = 정수)
 * 
 * - 메서드 
 * 	1)	showAccountInfo(): 계좌정보를 출력
 * 		- 매개변수 X, 리턴값 X
 * 		- 다음과 같이 출력
 * 		<출력 예시 >
 * 		계좌번호 : 111-1111-111
 * 		예금주명 : 홍길동
 * 		현재잔고 : 100000원
 * 	2)	deposit() : 입금기능
 * 		- 매개변수 : 입금금액(정수, amount), 리턴값 : X
 * 		- 입금할 금액(amount)을 전달받아 현재잔고(balance)에 누적
 * 		- 입금할 금액과 입금 후의 잔고를 다음과 같이 출력
 * 			입금금액 : XXXX원
 * 			현재잔고 : XXXX원
 * 	3)	withdraw() : 출금기능
 * 		- 매개변수 : 출금할 금액(정수, amount), 리턴값 : 출금 성공 시 출금된 금액
 * 		- 출금할 금액(amount)을 전달 받아 다음 조건에 따라 출금 수행
 * 			a. 출금할 금액보다 현재잔고가 작을 경우 (출금이 불가능할 경우)
 * 				출력) 출금할 금액 : XXXX원
 * 					  잔액이 부족하여 출금할 수 없습니다! (현재잔고 : XXX원)
 * 			b. 출금할 금액보다 현재잔고가 크거나 같을 경우 (출금이 가능할 경우)
 * 				출금할 금액만큼 현재 잔고에서 차감 후 다음과 같이 출력
 * 					  출금할 금액 : XXXX원
 * 					  현재잔고 : XXXXX원 -> 출금하고 남은 금액만큼 출력
 * 					  출금할 금액만큼 리턴
 * 			- 외부에서 리턴값을 전달받아 "출금금액 : XXXX원" 출력 
```

```java
package oop;

class Account2 {
	String accountNo;
	String ownerName;
	int balance;

	void showAccountInfo() {
		System.out.println("계좌번호 : " + accountNo);
		System.out.println("예금주명 : " + ownerName);
		System.out.println("현재잔고 : " + balance + "원");
	}

	void deposit(int amount) {
		balance += amount;
		System.out.println("입금금액 : " + amount + "/ 현재잔고 : " + balance + "원");
	}

	int withdraw(int amount) {

		int result = 0;
		if (balance < amount) {

			System.out.println("출금할 금액 : " + amount + "원" + " / 잔액이 부족하여 출금 할 수 없습니다! (현재 잔고 : " + balance + "원)");
			result = 0;
		} else {

			balance -= amount;

			System.out.println("출금할 금액 : " + amount + "원" + " / 현재잔고 : " + balance + "원)");
			result = amount;
		}

		return result;
	}

}

public class Test2 {

	public static void main(String[] args) {

		Account2 acc = new Account2();

		acc.accountNo = "111-1111-111";
		acc.ownerName = "홍길동";
		acc.balance = 100000;
		
		acc.showAccountInfo();
		System.out.println("==========================");
		acc.deposit(50000);
		System.out.println("==========================");
		acc.withdraw(800000);

	}

}
```



