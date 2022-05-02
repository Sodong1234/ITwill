# [오전수업] DB 26차

> 25차 수업 복습 실시

## 데이터 딕셔너리
```
SELECT *
FROM dictionary
WHERE table_name = 'USER_OBJECTS';

TABLE_NAME  |COMMENTS                 |
------------+-------------------------+
USER_OBJECTS|Objects owned by the user|
```

## 데이터정의어(DDL)
- 오브젝트의 구조를 정의하는 문법
- CREATE(생성), ALTER(변형), DROP(삭제), TRUNCATE(절단)
```
- 테이블
행으로 구성되어 있는, 데이터 저장의 기본 단위
- 뷰
하나 이상의 테이블에 대한 논리적으로 표현된 부분집합
- 시퀀스
숫자값 생성 오브젝트
- 인덱스
일부 쿼리 구문의 성능을 높여줌. 제약조건(PK, UK)
- 시노님
동의어. 기존 오브젝트에 반영구적인 별명을 부여
```

## FOREIGN KEY
![1](https://user-images.githubusercontent.com/95197594/166177495-ed3706f5-fb98-482a-bc46-8fa25d347332.PNG)

``` 
CREATE TABLE test_pk
(id NUMBER PRIMARY KEY, 
name VARCHAR2(30));

CREATE TABLE test_fk
(emp_id NUMBER REFERENCES test_pk(id),
emp_name varchar2(30));
```
![2](https://user-images.githubusercontent.com/95197594/166177557-53db965f-f9a7-4ca4-a3df-7e6d6abe76d5.PNG)
```
INSERT INTO test_pk
VALUES (10, 'ten');
INSERT INTO test_pk
VALUES (20, 'tenten');


COMMIT;


SELECT * FROM test_pk;
ID|NAME  |
--+------+
10|ten   |
20|tenten|



--------------------------------------------------------------------------------------------------------------------



INSERT INTO test_fk
VALUES (10, 't');
INSERT INTO test_fk
VALUES (5, 'f');

- 두번째 구문에서 5의 값은 부모컬럼인 test_pk의 id컬럼에는 없는 값으로 입력할수 없는 값을 입력해서 에러가 발생

INSERT INTO test_fk
VALUES (10, 'f');


SELECT * FROM test_fk;

EMP_ID|EMP_NAME|
------+--------+
    10|t       |
    10|f       |
```

### check 제약조건
- id 컬럼에 0이상의 값만 입력 허용하는 check제약조건 설정
```
CREATE TABLE test_ck
(id NUMBER CHECK (id >= 0),
name VARCHAR2(30));

INSERT INTO test_ck
VALUES (5, null);
INSERT INTO test_ck
VALUES (-5, null);


SQL Error [2290] [23000]: ORA-02290: 체크 제약조건(HR.SYS_C007728)이 위배되었습니다
```

## VIEW
- 쿼리구문의 출력결과를 가상의 테이블로 활용할 수 있게 해주는 문법
- 특정 연산을 하는 구문을 반복적으로 사용해야 할 일이 있는 경우 활용한다면 유용
- 뷰의 결과는 저장되지 않으며 뷰를 구성하는 SELECT구문만 데이터딕셔너리에 저장되어 있다.
- 뷰의 내용을 구성하는 데이터들의 테이블을 base table이라고 한다.
- 뷰를 통한 DML작업을 하는 경우 뷰의 데이터는 base table의 데이터를 사용하는 것이므로 결국 base table의 데이터에 작업이 이루어진다.
- 뷰의 DML 작업은 권한이나 제약조건에 의해서 일부 제한 될 수 있다.
```
SELECT employee_id, last_name, salary, salary * 12 an_sal
FROM employees;


- 뷰 생성 시 표현식, 함수를 사용한 경우 column alias를 필수로 사용해야 한다.
CREATE VIEW ann_salary AS 
SELECT employee_id, last_name, salary, salary * 12 an_sal
FROM employees;


SELECT * FROM ann_salary;


UPDATE ann_salary
SET salary = salary * 2
WHERE employee_id = 100;

SELECT employee_id, salary
FROM employees;

CREATE VIEW emp_dept AS 
SELECT employee_id, last_name, 
d.department_id, department_name
FROM employees e, departments d
WHERE e.department_id = d.department_id;


SELECT * FROM emp_dept;


- 뷰의 내용을 재정의해야 하는 경우 OR REPLACE 키워드를 추가하여 뷰를 재정의할 수 있다.
CREATE OR REPLACE VIEW emp_dept AS 
SELECT employee_id, last_name, salary, 
d.department_id, department_name
FROM employees e, departments d
WHERE e.department_id = d.department_id;


SELECT * FROM emp_dept;


```

## 시퀀스
- 고유한 숫자값 생성 오브젝트

> CREATE SEQEUNCE 시퀀스명

### 옵션
```
INCREMENT BY n  : 생성 숫자의 증감간격. 기본값 1
START WITH n    : 생성 시작값 지정. 기본값 1
MAXVALUE n      : 생성 최대값 지정
NOMAXVALUE      : 최대값을 지정하지 않음. 
MINVALUE n      : 생성 최소값 지정
NOMINVALUE      : 최소값을 지정하지 않음.
CYCLE           : 한계값 도달 시 초기값으로 돌아감
NOCYCLE         : 한계값 도달 시 값의 생성을 멈춤
CACHE n         : 값을 미리 생성해두는 옵션. 기본값 6
NOCACHE         : 값을 미리 생성하지 않음.
```

---

# [오후수업] JAVA 40차

이벤트 처리 4단계
```java
package event_handling;

import java.awt.event.WindowAdapter;
import java.awt.event.WindowEvent;
import java.awt.event.WindowListener;

import javax.swing.JFrame;

public class Ex1 {

	public Ex1() {
		showFrame();
	}
	
	public void showFrame() {
		JFrame f = new JFrame("이벤트처리 - 4");
		f.setBounds(600, 400, 300, 300);
		
		/*
		 * < 이벤트 처리 5단계 >
		 * 4단계. 익명 내부클래스(Anonymous Inner Class) 형태로 정의
		 * - 리스너 인터페이스 또는 어댑터 클래스를 구현하는 핸들러를 별도로 정의하지 않고
		 * 	 해당 리스너 또는 어댑터의 이름을 그대로 사용하여
		 *   변수 선언 및 인스턴스 생성과 추상메서드 구현까지 한꺼번에 수행하는 방법
		 * - 개발자가 별도의 핸들러 이름을 부여하지 않으므로
		 *   이름이 없다는 뜻의 익명클래스라는 의미가 붙게됨
		 * - 3단계 위치와 동일하며 클래스 정의 방법만 달라짐
		 * 
		 */
		
		// 로컬 내부 클래스 형태로 정의
		// => 로컬변수와 동일한 범위에서만 접근 가능
		WindowAdapter listener = new WindowAdapter() {

			@Override
			public void windowClosing(WindowEvent e) {
				System.out.println("로컬 windowClosing");
				System.exit(0);
			}
			
		};
		
		// 람다식 WindowAdapter가 Interface가 아니므로
		// 람다식 적용 X
//		WindowAdapter listener2 = (e) -> {
//			System.out.println("로컬 windowClosing");
//			System.exit(0);
//		};
		
		f.addWindowListener(listener);
		
		f.setVisible(true);
	}
	
	
	public static void main(String[] args) {
		new Ex1();
	}

	// 이벤트 처리 4단계. 익명 내부 클래스 (Anonymous Inner Class) 형태로 정의
	// => WindowAdapter 또는 WindowListener 이름을 그대로 사용하여
	//	  참조변수선언, 객체생성, 클래스바디 내의 추상메서드 정의까지 한꺼번에 수행
	//
	// 멤버 내부 클래스 형태로 정의
	// => 인스턴스 멤버와 동일한 위치에 정의하므로 인스턴스 내부 클래스라고도 함
	// => 인스턴스 내의 여러 메서드에서 공유 가능
	WindowAdapter listener = new WindowAdapter() {

		@Override
		public void windowClosing(WindowEvent e) {
			System.out.println("멤버 windowClosing");
			System.exit(0);
		}
		
	};
	
	
	// WindowListener 인터페이스를 익명 내부 클래스 형태로 정의할 경우
	WindowListener listener2 = new WindowListener() {
		
		@Override
		public void windowOpened(WindowEvent e) {
			// TODO Auto-generated method stub
			
		}
		
		@Override
		public void windowIconified(WindowEvent e) {
			// TODO Auto-generated method stub
			
		}
		
		@Override
		public void windowDeiconified(WindowEvent e) {
			// TODO Auto-generated method stub
			
		}
		
		@Override
		public void windowDeactivated(WindowEvent e) {
			// TODO Auto-generated method stub
			
		}
		
		@Override
		public void windowClosing(WindowEvent e) {
			// TODO Auto-generated method stub
			
		}
		
		@Override
		public void windowClosed(WindowEvent e) {
			// TODO Auto-generated method stub
			
		}
		
		@Override
		public void windowActivated(WindowEvent e) {
			// TODO Auto-generated method stub
			
		}
	};
	
	
	
}


```


연습문제
```java
package event_handling;

import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.awt.event.WindowAdapter;

import javax.swing.JButton;
import javax.swing.JFrame;

public class Test1 {
	
	public Test1() {
		showFrame();
	}
	
	public void showFrame() {
		
		JFrame f = new JFrame("이벤트 처리 연습 - 4");
		f.setBounds(800, 400, 300, 300);
		f.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
		
		// JButton 객체 생성 및 JFrame 객체에 부착
		JButton btn = new JButton();
		f.add(btn);
		
		// 이벤트 처리4. 익명 내부 클래스 형태로 정의
		// JButton 컴포넌트에 대한 이벤트 처리 담당 리스너 : ActionListener 인터페이스
		ActionListener listener = new ActionListener() {
			
			@Override
			public void actionPerformed(ActionEvent e) {
				System.out.println("버튼 클릭!");
			}
		};
		
		// 람다식
		ActionListener listener2 = e -> System.out.println("로컬(람다식) 버튼 클릭!");
		
		// 생성한 JButton 객체에 이벤트 연결
		btn.addActionListener(listener2);
		
		f.setVisible(true);
	}
		
	public static void main(String[] args) {
		new Test1();
	}
	
	// 이벤트 처리4. 익명 내부 클래스 형태로 정의
	// JButton 컴포넌트에 대한 이벤트 처리 담당 리스너 : ActionListener 인터페이스
	ActionListener listener = new ActionListener() {
		
		@Override
		public void actionPerformed(ActionEvent e) {
			System.out.println("멤버 버튼 클릭!");
		}
	};
	
	// 람다식
	ActionListener listener2 = e -> System.out.println("멤버(람다식) 버튼 클릭!");
}

```

이벤트 처리 5단계
```java
package event_handling;

import java.awt.event.WindowAdapter;
import java.awt.event.WindowEvent;

import javax.swing.JFrame;

public class Ex2 {
	
	public Ex2() {
		showFrame();
	}
	
	public void showFrame() {
		JFrame f = new JFrame("이벤트 처리 - 5");
		f.setBounds(600, 400, 300, 300);
		
		/*
		 * < 이벤트 처리 5단계 >
		 * 5단계. 익명 내부클래스(Anonymous Inner Class)의 임시 객체 형태로 사용
		 * - 기본적인 개념은 익명 내부클래스와 동일하나 이벤트 처리 대상이 하나뿐일 경우
		 * 	 별도의 변수가 필요없으므로 변수 선언부를 제외하고
		 * 	 리스너 연결을 위한 addXXXListener() 메서드 파라미터로
		 * 	 익명 내부클래스를 구현하는 코드를 바로 기술하여 객체를 전달
		 * 	 별도의 변수가 없으므로 두 개 이상의 컴포넌트에 리스너 연결이 불가능함
		 * 	 => 즉, 1회용 리스너 객체가 됨
		 * - 리스너 인스턴스 생성 및 추상클래스 구현과 함께 연결 작업까지 한꺼번에 수행
		 * - 동일한 이벤트를 다른 대상에 적용해야할 경우 중복되는 코드가 발생함
		 *   => 4단계 방법이 더 효율적
		 *   
		 */
		f.addWindowListener(new WindowAdapter() {
			
			@Override
			public void windowClosing(WindowEvent e) {
				System.out.println("windowClosing");
				System.exit(0);
			}
		});
		
		// 주의! 새로운 프레임 객체에 WindowListener를 연결해야 하는 경우
		// 다시 리스너 구현 코드를 작성해야한다!

//		f2.addWindowListener(new WindowAdapter() {
//			// 추상메서드 구현...
//		});
		
		// 만약, 4단계 사용 시 객체 재사용이 가능하므로 리스너 구현체 변수만 전달하면 됨
//		f2.addWindowListener(listener);
		
		f.setVisible(true);
	}

	public static void main(String[] args) {
		new Ex2();
	}

	WindowAdapter listener = new WindowAdapter() {

		@Override
		public void windowClosing(WindowEvent e) {
			System.out.println("windowClosing");
			System.exit(0);
		}
		
	};
}
```


연습문제
```java
package event_handling;

import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;

import javax.swing.JButton;
import javax.swing.JFrame;

public class Test2 {

	public Test2() {
		showFrame();
	}
	
	public void showFrame() {
		JFrame f = new JFrame("이벤트처리 연습 - 5");
		f.setBounds(600, 400, 300, 300);
		f.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
		
		JButton btn = new JButton("버튼");
		f.add(btn);
		
		// 이벤트 처리5. 익명 내부클래스의 임시객체 형태로 이벤트 처리
		// JButton 컴포넌트에 대한 이벤트 처리 담당 리스너 : ActionListener 인터페이스
		// 1. JButton 객체의 addActionListener() 메서드를 호출하여 구현체 객체 전달
		btn.addActionListener(new ActionListener() {
			
			@Override
			public void actionPerformed(ActionEvent e) {
				System.out.println("버튼 클릭!");
			}
		});
		
		// 2. 람다식
		btn.addActionListener(e -> System.out.println("람다식 버튼 클릭!"));
	
		
		
		f.setVisible(true);
	}
	
	public static void main(String[] args) {
		new Test2();
	}

}
```
