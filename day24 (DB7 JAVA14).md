# [오전수업] DB 7차

> DB 6차 복습 수업 실시


## 조건이 여러가지인 WHERE 
- 비교연산자인 '='은 한번에 하나의 조건값만 입력받을 수 있음

```
- department_id컬럼의 값이 90 또는 100의 값을 가지고 있는 행을 출력

SELECT employee_id, last_name, department_id 
FROM HR_EMPLOYEES 
WHERE department_id = 90;

+-------------+-----------+---------------+
| employee_id | last_name | department_id |
+-------------+-----------+---------------+
|         100 | King      |            90 |
|         101 | Kochhar   |            90 |
|         102 | De Haan   |            90 |
+-------------+-----------+---------------+

-------------------------------------------------------------------------------

SELECT employee_id, last_name, department_id 
FROM HR_EMPLOYEES 
WHERE department_id = 100;
```

## IN 연산자
- IN 연산자는 '='연산자와 비슷하게 조건값과 동일한 값을 가진 행을 찾는 연산자
  - 다만 입력받을 수 있는 조건값은 여러개를 동시에 받을 수 있음
```
SELECT employee_id, last_name, department_id 
FROM HR_EMPLOYEES 
WHERE department_id IN (90, 100);

+-------------+-----------+---------------+
| employee_id | last_name | department_id |
+-------------+-----------+---------------+
|         100 | King      |            90 |
|         101 | Kochhar   |            90 |
|         102 | De Haan   |            90 |
|         108 | Greenberg |           100 |
|         109 | Faviet    |           100 |
|         110 | Chen      |           100 |
|         111 | Sciarra   |           100 |
|         112 | Urman     |           100 |
|         113 | Popp      |           100 |
+-------------+-----------+---------------+
```

## AND / OR
```
- AND의 연산자를 양옆의 조건식을 동시에 만족하는 행을 결과로 출력

SELECT employee_id, last_name, salary, department_id
FROM HR_EMPLOYEES
WHERE department_id IN (90, 100) AND salary >= 9000;

+-------------+-----------+--------+---------------+
| employee_id | last_name | salary | department_id |
+-------------+-----------+--------+---------------+
|         100 | King      |  48000 |            90 |
|         101 | Kochhar   |  17000 |            90 |
|         102 | De Haan   |  17000 |            90 |
|         108 | Greenberg |  12000 |           100 |
|         109 | Faviet    |   9000 |           100 |
+-------------+-----------+--------+---------------+


-------------------------------------------------------------------------------


SELECT employee_id, last_name, salary, department_id
FROM HR_EMPLOYEES
WHERE department_id = 90 OR department_id = 100;

+-------------+-----------+--------+---------------+
| employee_id | last_name | salary | department_id |
+-------------+-----------+--------+---------------+
|         100 | King      |  48000 |            90 |
|         101 | Kochhar   |  17000 |            90 |
|         102 | De Haan   |  17000 |            90 |
|         108 | Greenberg |  12000 |           100 |
|         109 | Faviet    |   9000 |           100 |
|         110 | Chen      |   8200 |           100 |
|         111 | Sciarra   |   7700 |           100 |
|         112 | Urman     |   7800 |           100 |
|         113 | Popp      |   6900 |           100 |
+-------------+-----------+--------+---------------+


-------------------------------------------------------------------------------

SELECT employee_id, last_name, salary, department_id 
FROM HR_EMPLOYEES 
WHERE department_id = 90 OR salary >= 10000;

+-------------+-----------+--------+---------------+
| employee_id | last_name | salary | department_id |
+-------------+-----------+--------+---------------+
|         100 | King      |  48000 |            90 |
|         101 | Kochhar   |  17000 |            90 |
|         102 | De Haan   |  17000 |            90 |
|         108 | Greenberg |  12000 |           100 |
|         114 | Raphaely  |  11000 |            30 |
|         145 | Russell   |  14000 |            80 |
|         146 | Partners  |  13500 |            80 |
|         147 | Errazuriz |  12000 |            80 |
|         148 | Cambrault |  11000 |            80 |
|         149 | Zlotkey   |  10500 |            80 |
|         150 | Tucker    |  10000 |            80 |
|         156 | King      |  10000 |            80 |
|         162 | Vishney   |  10500 |            80 |
|         168 | Ozer      |  11500 |            80 |
|         169 | Bloom     |  10000 |            80 |
|         174 | Abel      |  11000 |            80 |
|         201 | Hartstein |  13000 |            20 |
|         204 | Baer      |  10000 |            70 |
|         205 | Higgins   |  12000 |           110 |
+-------------+-----------+--------+---------------+


-------------------------------------------------------------------------------


HR_EMPLOYEES테이블에서 급여가 10000이 넘으면서 소속부서가 80 또는 50인 사원을 출력

SELECT employee_id, last_name, salary, department_id 
FROM HR_EMPLOYEES 
WHERE department_id = 80 OR department_id = 50 AND salary >= 10000;

+-------------+------------+--------+---------------+
| employee_id | last_name  | salary | department_id |
+-------------+------------+--------+---------------+
|         145 | Russell    |  14000 |            80 |
|         146 | Partners   |  13500 |            80 |
|         147 | Errazuriz  |  12000 |            80 |
|         148 | Cambrault  |  11000 |            80 |
|         149 | Zlotkey    |  10500 |            80 |
|         150 | Tucker     |  10000 |            80 |
...
|         172 | Bates      |   7300 |            80 |
|         173 | Kumar      |   6100 |            80 |
|         174 | Abel       |  11000 |            80 |
|         175 | Hutton     |   8800 |            80 |
|         176 | Taylor     |   8600 |            80 |
|         177 | Livingston |   8400 |            80 |
|         179 | Johnson    |   6200 |            80 |
+-------------+------------+--------+---------------+



-------------------------------------------------------------------------------


SELECT employee_id, last_name, salary, department_id 
FROM HR_EMPLOYEES 
WHERE (department_id = 80 OR department_id = 50) AND salary >= 8000;

+-------------+------------+--------+---------------+
| employee_id | last_name  | salary | department_id |
+-------------+------------+--------+---------------+
|         120 | Weiss      |   8000 |            50 |
|         121 | Fripp      |   8200 |            50 |
|         145 | Russell    |  14000 |            80 |
|         146 | Partners   |  13500 |            80 |
...
|         174 | Abel       |  11000 |            80 |
|         175 | Hutton     |   8800 |            80 |
|         176 | Taylor     |   8600 |            80 |
|         177 | Livingston |   8400 |            80 |
+-------------+------------+--------+---------------+


-------------------------------------------------------------------------------


SELECT employee_id, last_name, salary, department_id 
FROM HR_EMPLOYEES 
WHERE salary >= 5000 AND salary <= 8000;

+-------------+-----------+--------+---------------+
| employee_id | last_name | salary | department_id |
+-------------+-----------+--------+---------------+
|         104 | Ernst     |   6000 |            60 |
|         111 | Sciarra   |   7700 |           100 |
|         112 | Urman     |   7800 |           100 |
...
|         179 | Johnson   |   6200 |            80 |
|         202 | Fay       |   6000 |            20 |
|         203 | Mavris    |   6500 |            40 |
+-------------+-----------+--------+---------------+
```

> 이후 다음주에 있을 테스트를 대비해 배운 내용들을 전체적으로 요약 복습 실시 


# [오후수업] JAVA 14차

## 기본형 변수 & 참조형 변수

### 기본형(Primitive Type) 변수와 참조형(Reference Type) 변수의 차이
- 기본형 변수는 실제값(리터럴)을 저장하며, 참조형 변수는 실제값이 저장된 "인스턴스의 주소값"(참조값 = 레퍼런스)을 저장
- 기본형 변수와 참조형 변수의 값을 복사(전달) 할 때 차이점
  - 기본형 변수의 값을 복사할 경우    
	  - 변수에 저장된 실제 값을 복사(전달) = Pass by value
    - 실제 값을 복사하게 되면 원본 값과 동일한 값이 별도로 생성되어 전달되므로 복사된 값을 변경하더라도 원본값과 상관이 없기 때문에 원본값은 변경되지 않음
  - 참조형 변수의 값을 복사할 경우
    - 변수에 저장된 인스턴스 주소값을 복사(전달) = Pass by reference
    - 주소 값을 복사하게 되면 원본 값에 저장된 주소와 동일한 주소가 전달되므로 실제 인스턴스 하나를 함께 공유. 따라서, 한 쪽에서 인스턴스에 접근하여 저장된 값을 변경할 경우 동일한 주소값을 참조하는 쪽에도 영향을 받음
    - 즉, 한 쪽에서 값을 변경하면 다른 쪽의 값도 함께 변경된 효과를 갖게 됨

```java
class Num {
	int x;
	int y;

}

class MyDate {
	int year;
	int month;
	int day;
	
	public MyDate(int year, int month, int day) {
		this.year = year;
		this.month = month;
		this.day = day;
	}
	
	public void print() {
		System.out.println(year + "년 " + month + "월 " + day + "일 ");
	}
	
}


public class Ex1 {

	public static void main(String[] args) {
		/*
		 *   
		 * 기본형 변수와 참조형 변수의 값을 복사(전달) 할 때 차이점
		 * 1. 기본형 변수의 값을 복사할 경우    
		 * 	- 변수에 저장된 실제 값을 복사(전달) = Pass by value
		 * 	- 실제 값을 복사하게 되면 원본 값과 동일한 값이 별도로 생성되어 전달되므로
		 * 	  복사된 값을 변경하더라도 원본값과 상관이 없기 때문에 원본값은 변경되지 않음
		 * 
		 * 2. 참조형 변수의 값을 복사할 경우
		 *  - 변수에 저장된 인스턴스 주소값을 복사(전달) = Pass by reference
		 *  - 주소 값을 복사하게 되면 원본 값에 저장된 주소와 동일한 주소가 전달되므로
		 *    실제 인스턴스 하나를 함께 공유
		 *    따라서, 한 쪽에서 인스턴스에 접근하여 저장된 값을 변경할 경우
		 *    동일한 주소값을 참조하는 쪽에도 영향을 받음
		 *    => 즉, 한 쪽에서 값을 변경하면 다른 쪽의 값도 함께 변경된 효과를 갖게 됨
		 * 
		 * 
		 */
		
		int x = 10; // 기본형(정수형) 변수 x의 값을 10으로 초기화
		int y = x; // 기본형 변수 x의 값(실제값 10)을 복사하여 기본형 변수 y에 전달
		System.out.println("x = " + x + ", y = " + y);
		
		// 기본형 변수 y의 값을 99로 변경 후 출력 
		y = 99;
		System.out.println("x = " + x + ", y = " + y);
		// => 기본형 변수 y의 값을 변경하더라도
		// 	  원본 데이터 기본형 변수 x의 값은 변경되지 않음
		
		System.out.println("--------------------------------------");
	
		Num n = new Num();
		n.x = 10;
		n.y = n.x; // 인스턴스 내의 기본형 변수도 값의 복사가 일어남
		System.out.println("n.x = " + n.x + ", n.y = " + n.y);
		
		n.y = 99;
		System.out.println("n.x = " + n.x + ", n.y = " + n.y);
		// => Num 클래스의 인스턴스 내에 있는 기본형 변슈 y의 값을 변경하더라도
		// 	  원본 데이터 기본형 변수 x의 값은 변경되지 않음
		System.out.println("--------------------------------------");
		
		Num n2 = new Num();
		n2.x = 10;
		n2.y = 10;
		
		Num n3 = n2;	// Num 타입 참조형 변수 n3에 참조형 변수 n2의 값 복사(= 주소값 복사)
		// => 참조형 변수 n2가 가리키는 인스턴스 주소값을 n3에 복사(전달) 했으므로
		//	  n2와 n3가 가리키는(참조하는) 인스턴스가 동일함
		System.out.println("n2.x = " + n2.x + ", n2.y = " + n2.y);
		System.out.println("n3.x = " + n3.x + ", n3.y = " + n3.y);
		
		n3.y = 99;
		
		System.out.println("n2.x = " + n2.x + ", n2.y = " + n2.y);
		System.out.println("n3.x = " + n3.x + ", n3.y = " + n3.y);
		// => n2와 n3 모두 하나의 인스턴스를 참조하고 있으므로
		//	  한 쪽에서 인스턴스 내의 변수값을 변경하면
		//	  다른 쪽 인스턴스도 동일하므로 똑같이 변경된 사항이 적용됨
		System.out.println("--------------------------------------");
		
		System.out.println("----- 변경 전");
		MyDate d1 = new MyDate(2022, 02, 24);
		d1.print();
		
		MyDate d2 = d1;
		
		System.out.println("----- 변경 후");
		
		d2.year = 1999;
		
		d1.print();
		d2.print();
		
		System.out.println("--------------------------------------");

		PassValue pv = new PassValue();
		
		int xNum = 10;
		System.out.println("changeValue() 메서드 호출 전 x : " + xNum);
		pv.changeValue(xNum); // 실제 값의 복사를 통해 값을 전달함
		System.out.println("changeValue() 메서드 호출 후 x : " + xNum);
		// => 메서드 내에서 값을 변경하고 돌아오더라도 원본 변수 값에는 아무런 영향 없음
		
		System.out.println("--------------------------------------");

		Num num = new Num();
		num.x = 10;
		
		System.out.println("changeReferenceValue() 메서드 호출 전 num : " + num.x);
		pv.changeReferenceValue(num);
		System.out.println("changeReferenceValue() 메서드 호출 후 num : " + num.x);
		// 다른 인스턴스 내의 메서드에서 전달받은 참조변수가 가리키는 인스턴스 값을 변경하면
		// 동일한 인스턴스를 참조하는 나 자신도 변경된 값을 적용 받게 됨
		
		/*
		 * 
		 * 결론!
		 * 변수 값을 변경하는 경우 '누가' 바꾸는지는 중요하지 않고
		 * '무엇을' 바꾸는지가 중요하다!
		 * => 기본형 변수값을 변경하는 경우 값의 복사로 인해 원본은 영향 없음
		 * => 참조형 변수값을 변경하는 경우 주소값의 복사로 인해 원본도 영향 있음
		 * 
		 */
		
	}

}


class PassValue {
	public void changeValue(int x) {
		System.out.println("changeValue 메서드의 변경 전 : " + x);
		x = 999;
		System.out.println("changeValue 메서드의 변경 전 : " + x);

	}
	
	public void changeReferenceValue(Num num) {
		System.out.println("changeReferenceValue() 메서드의 변경 전 " + num.x);
		num.x = 999;
		System.out.println("changeReferenceValue() 메서드의 변경 후 " + num.x);

	}
}
```

## 패키지

- 윈도우에서의 폴더(Folder), 리눅스에서의 디렉토리(Directory)에 해당하는 개념
- 자바에서 클래스 파일들을 모아놓는 공간(물리적으로 폴더와 동일)
  - 서로 다른 패키지에는 같은 이름의 클래스가 중복될 수 있음 (= 중복 가능)
	  - = 단, 하나의 패키지에는 같은 이름의 클래스가 중복될 수 없음
- 자바에서 패키지를 생성하면, 실제 물리적인 폴더가 생성됨
  - 만약, 패키지 생성 생략 시, 물리적 폴더가 없는 default package가 생성됨
- 특정 클래스 파일은 하나의 패키지에'만' 소속되어야 함
- 원칙적으로 클래스에 접근하는 방법은 패키지명과 클래스명을 동시에 지정하여 접근
  - '패키지명.클래스명' 형태로 지정
	- 만약, 패키지에 계층 구조로 이루어져 있을 경우에는 '상위패키지명.하위패키지명.클래스명' 형태로 지정

> 단, java.lang 패키지는 기본적으로 포함되는 유일한 패키지이므로 java.lang 패키지 내의 클래스는 클래스 명만으로 접근 가능

- 실제 폴더 구조처럼 상위, 하위로 구분하여 패키지를 작성하며 이 때, 패키지 이름이 중복될 수 있으므로 도메인네임을 사용하여 패키지명을 지정
  - 단, 도메인네임을 역순으로 지정하여 포괄적인 이름이 상위패키지명이 되도록 함
  > - ex) itwillbs.co.kr의 도메인일 경우 패키지명을 kr.co.itwillbs로 지정
  > - ex) nate.co.kr의 도메인일 경우 패키지명을 kr.co.nate로 지정
  >	- ex) google.co.kr -> kr.co.google

    - 상위 패키지 -> 하위 패키지 순으로 생성되므로 kr 폴더 내에 co 폴더 내에 itwillbs, nate, google 폴더가 생성됨
    - (즉, 동일한 성격에 따라 같은 폴더(패키지)로 관리되므로 유지보수가 용이)

### package문
- 특정 클래스 파일의 첫번째 라인에 해당 클래스가 소속된 패키지를 명시하는데 사용
  - 주석을 제외한 소스코드에서 가장 먼저 실행되어야 하는 코드(맨 윗 줄에 위치)
- 실제 클래스 파일이 위치한 패키지와 지정된 위치가 다를 경우 오류 발생
- 클래스 파일 내에서 단 한번만 사용 가능
- default package에 위치한 클래스는 실제 패키지가 없으므로 package문 생략

```java
< package 키워드 사용 기본 문법 >
소스코드 첫 번째 라인에서 
package 상위패키지명.하위패키지명;
```

### import문
- 특정 패키지 또는 패키지 내의 클래스를 현재 클래스 내에 포함시키는 키워드
- 자신과 동일한 패키지에 존재하는 클래스가 아닌 다른 패키지의 클래스는 직접 이름만으로 접근이 불가능하며, 반드시 패키지명을 포함하여 지정해야함
  - 원래 클래스명을 지정할 때 패키지명.클래스명 형태로 지정해야하지만 패키지명을 생략하고 싶은 경우 해당 패키지명을 import 문으로 등록 시키면 해당 패키지명을 생략하고 클래스명만으로 사용 가능해짐
- package 문과 달리 여러번 사용할 수 있으며 package 문 아래쪽, 다른 코드들보다 윗쪽에 위치해야함
  - (= package 문을 제외하면 가장 윗쪽 라인에 위치해야함)
- 클래스명 지정 시 자동완성 기능을 사용하여 import문을 자동 생성하거나 단축키를 사용하여 import문을 자동 생성 가능 (Ctrl + Shift + o)   
  - 동일한 이름의 클래스가 여러 패키지에 존재할 경우 선택창 표시됨

```java
import java.io.*;
import java.util.*;

import javax.swing.JFrame;

public class Ex3 {

	public static void main(String[] args) {
		
		// String 클래스 사용방법
		String s1 = "홍길동"; // 클래스 명만으로 사용했었음
		
		// 정상적인 STring 클래스 사용 방법 -> java.lang 패키지명을 포함하여 기술함
		java.lang.String s2 = "홍길동";
		// => String 클래스가 포함된 java.lang 패키지는 기본적으로 포함된 패키지이므로
		//	  패키지명을 생략한 채 클래스 명만으로 객체 사용 가능
		
//		Random r = new Random(); // 패키지명을 포함하지 않을 경우 클래스를 찾지 못하므로
		// 존재하지 않는 클래스 지정으로 인한 오류 발생
		
		// 패키지명을 포함하여 클래스 지정해야함
		java.util.Random r = new java.util.Random();
		
		Date d = new Date(); // java.util 패키지의 Date를 import 했으므로 이름만으로 접근 가능
		
		Calendar cal;
		
		InputStream is;
		BufferedReader buffer;
		
		JFrame f;
		
		// 기존의 import 구문을 통해 java.util.Date 클래스가 포함되어 있는 경우
		// 다른 패키지인 java.sql.Date 클래스를 사용하려면 import문 중복이 불가능하므로
		// 패키지명을 포함하여 클래스명을 지정해야한다!
		
		java.sql.Date date;
		
		
		
	}

}
```

연습문제
```java
package book;

public class Ex1 {

	public static void main(String[] args) {
		/*
		 * 나이가 40살, 이름이 James라는 남자가 있습니다.
		 * 이 남자는 결혼을 했고, 자식이 셋 있습니다
		 * 
		 * < 출력 결과 >
		 * 이 사람의 나이
		 * 이 사람의 이름
		 * 이 사람의 결혼여부
		 * 이 사람의 자녀수
		 * 
		 * 클래스 이름 : Person
		 * 
		 */
		Person p = new Person();
		
		System.out.println("이 사람의 나이 : " + p.age);
		System.out.println("이 사람의 이름 : " + p.name);
		System.out.println("이 사람의 결혼여부 : " + p.married);
		System.out.println("이 사람의 자녀수 : " + p.children);
		
		
		System.out.println("=======================================");
		
		/*
		 * 쇼핑몰에 주문이 들어왔습니다. 주문 내용은 다음과 같습니다.
		 * 
		 * 주문번호 : 201803120001
		 * 주문자 아이디 : abc123
		 * 주문자 이름 : 홍길순
		 * 주문 상품 번호 : PD0345-12
		 * 배송 주소 : 서울시 영등포구 여의도동 20번지
		 * 
		 * 위 주문 내용에 대한 클래스를 만들고 주문 내용을 인스턴스로 생성한 후 위와 같은 형식으로 
		 * 주문 내용을 출력
		 * 
		 */
		
		shopping sp = new shopping();
		System.out.println("주문번호 : " + sp.num);
		System.out.println("주문자 아이디 : " + sp.id);
		System.out.println("주문자 이름 : " + sp.name);
		System.out.println("주문 상품 번호 : " + sp.snum);
		System.out.println("배송 주소 : " + sp.address);

		
	}

}






class Person {
	int age = 40;
	String name = "James";
	boolean married = true;
	int children = 3;

}


class shopping {
	String num = "201803120001";
	String id = "abc123";
	String name = "홍길순";
	String snum = "PD0345-12";
	String address = "서울시 영등포구 여의도동 20번지";
}
```
```java
package book;

import java.util.ArrayList;

public class Ex3 {

	public static void main(String[] args) {
		// char 배열에 10개의 알파벳(대소문자 섞어서)을
		// 초기화한 후 대소문자를 변환해 출력

		char[] a = {'a', 'b', 'c', 'd', 'e', 'f', 'g', 'H', 'I'};
		for(char ch : a) {
			if(ch >= 'a' && ch <= 'z') {
				ch -= 32;
			} else {
				ch += 32;
			}
			System.out.print(ch + "");
		}
		
		// 배열의 요소를 대소문자 변환하는 version
//		for(int i = 0; i < a.length; i++) {
//			if(a[i] >= 'a' && a[i] <= 'z') {
//				a[i] -= 32;
//			} else {
//				a[i] += 32;
//			}
//		}
//		
//		for(int i = 0; i < a.length; i++) {
//			System.out.print(a[i] + "");
//		}

		
//=====================================================================
		
		
		// 배열 길이가 5인 정수형 배열을 선언하고,
		// 1~10 중 짝수만 배열에 저장한 후 그 합을 출력
		ArrayList<Integer> arr = new ArrayList<Integer>();
		for(int i = 1; i <= 10; i++) {
			if(i % 2 == 0) {
				arr.add(i);
			}
		}
		for(int num : arr) {
			System.out.print(num + "");
		}
		
		
//		int[] num = new int[5];
//		int index = 0;
//		for(int i = 1; i < 11; i++) {
//			if(i % 2 == 0) {
//				num[index] = i;
//				index++;
//			}
//		}
//		
//		for(int n : num) {
//			System.out.print(n + "");
//		}

		
//=====================================================================
		
		

		// 다음과 같이 Dog 클래스가 있습니다.
		// 배열 길이가 5인 Dog[] 배열을 만든 후 Dog인스턴스를 5개 생성하여 배열에 추가합니다.
		// for문 (향상된 for문)으로 showDogInfo() 메서드를 사용하여 배열에 추가한 Dog 정보를 모두 출력하세요
		
		Dog[] dogs = new Dog[5];
		dogs[0] = new Dog("뽀삐", "시츄");
		dogs[1] = new Dog("토토", "말티즈");
		dogs[2] = new Dog("맥스", "골든리트리버");
		dogs[3] = new Dog("누누", "페키니즈");
		dogs[4] = new Dog("똥강아지", "잡종");
		
		for(Dog dog : dogs) {
			System.out.println(dog.showDogInfo());
		}


	}

}

class Dog {
	private String name;
	private String type;
	
	public Dog(String name, String type) {
		this.name = name;
		this.type = type;
	}

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}

	public String getType() {
		return type;
	}

	public void setType(String type) {
		this.type = type;
	}
	
	public String showDogInfo() {
		return name + ", " + type;
	}

}
```
```java
package book;

public class Ex4 {

	public static void main(String[] args) {
		// Q1) 클래스 내부에서 자신의 주소를 가리키는 예약어를 XXX 라고 합니다.
		// : this
		
		// Q2) 클래스에 여러 생성자가 오버로드되어 있을 경우에 하나의 생성자에서
		// 	   다른 생성자를 호출 할 때 XXX 를 사용합니다.
		// : this()
		
		// Q3) 클래스 내부에 선언하는 static 변수는 생성되는 인스턴스마다 만들어지는 것이 아닌
		// 	   여러 인스턴스가 공유하는 변수입니다. 따라서 클래스에 기반한 유일한 변수라는 의미로
		//	   XXX 라고도 합니다.
		// 클래스변수
		
		// Q4) 지역 변수는 함수나 메서드 내부에서만 사용할 수 있고 XXX 메모리에 생성됩니다.
		//	   멤버 변수 중 static 예약어를 사용하는 static은 XXX 메모리에 생성됩니다.
		// : 스택, 스태틱영역&메소드영역
		
		
	

	}

}
```
