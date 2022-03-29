# [오전수업] DB 15차
> DB 14차에서 수업한 내용 복습 실시

### 테이블 생성 연습
```
테이블명 : member

컬럼명    / 데이터타입 / 크기 / 제약조건
member_id / NUMBER    / 5    / PRIMARY KEY
last_name / VARCHAR2  / 30   / NOT NULL
phone_no  / VARCHAR2  / 25   / UNIQUE



CREATE TABLE MEMBER
(member_id	NUMBER(5) PRIMARY KEY,
last_name	VARCHAR2(30) NOT NULL,
phone_no	VARCHAR2(25) UNIQUE);

```


연습문제
```
# employees 테이블로부터 커미션을 받지 않는 모든 사원의 last_name, salary, commission_pct를 출력하되
salary를 기준으로 내림차순 정렬기


SELECT last_name, salary, commission_pct
FROM employees 
WHERE commission_pct IS NULL
ORDER BY salary DESC;


LAST_NAME  |SALARY|COMMISSION_PCT|
-----------+------+--------------+
King       | 24000|              |
Kochhar    | 17000|              |
De Haan    | 17000|              |
Hartstein  | 13000|              |
Higgins    | 12008|              |
Greenberg  | 12008|              |
Raphaely   | 11000|              |
…
```


## 단일행 함수(Function)
- 연산을 보조해주는 오브젝트
- 단일행 함수는 행단위로 함수의 연산 결과를 출력

![unnamed](https://user-images.githubusercontent.com/95197594/160322817-988bb101-9919-449b-a580-318b549402f7.png)

![unnamed (1)](https://user-images.githubusercontent.com/95197594/160322820-e1caef79-62d2-4dba-ada1-05e88767e350.png)



## 문자함수
### 대소문자 변환 함수
- higgins의 성을 가진 사원은 있으나 결과로 출력되지 않음
- 문자열 값 비교 시 대소문자까지 일치하지 않으면 결과로 출력되지 않음

```

SELECT employee_id, last_name, department_id
FROM employees
WHERE last_name = 'higgins';

EMPLOYEE_ID|LAST_NAME|DEPARTMENT_ID|
-----------+---------+-------------+



- last_name 컬럼의 값에 LOWER함수를 적용하여 연산된 내용과 조건값인 'higgins' 비교하여 결과를 출력

SELECT employee_id, last_name, department_id 
FROM employees
WHERE LOWER(last_name) = 'higgins';

EMPLOYEE_ID|LAST_NAME|DEPARTMENT_ID|
-----------+---------+-------------+
        205|Higgins  |          110|
        
        
        

SELECT 'The job id for ' || UPPER(last_name) || ' is ' || LOWER(job_id)
AS "EMPLOYEE DETAILS"
FROM employees;

EMPLOYEE DETAILS                      |
--------------------------------------+
The job id for ABEL is sa_rep         |
The job id for ANDE is sa_rep         |
The job id for ATKINSON is st_clerk   |
The job id for AUSTIN is it_prog      |
The job id for BAER is pr_rep         |
The job id for BAIDA is pu_clerk      |
The job id for BANDA is sa_rep        |
…
```

---

# [오후수업] JAVA 26차

## enum 
- 상수를 사용하여 한정된 데이터를 관리하는 경우 간편한 선언을 통해 관리할 수 있는 장점이 있으나 해당 값의 범위를 벗어나는 값을 사용할 경우 컴파일 시점에서 오류 발견이 불가능하므로 추가적인 작업을 통해 범위 내의 값인지 판별하는 작업이 별도로 필요함
  - 이를 해결하기 위해 열거형(enum type) 필요
```
< 열거타입 정의 방법 >
[접근제한자] enum 열거타입명 {
		// 열거타입에서 사용할 값(상수)을 차례대로 나열
}

- 기본적으로 클래스, 인터페이스 정의 문법과 유사함
- 단, 열거타입 중괄호 내에는 상수로 사용될 이름만 지정

------------------------------------------------------------------

< 열거타입 사용 방법 >
1. 열거타입 변수 선언 방법(클래스, 인터페이스와 동일)
- 열거타입명 변수명
예)	Car	  car

2. 열거타입 상수 접근 방법 (일반 상수와 동일)
	- 열거타입명.상수명
예) Car.MAX_SPEED

3. 열거타입 비교 방법
- if문 사용 시 동등비교 연산자 사용(==)
- switch문 사용 가능 (단, case 문에서 열거타입명 없이 상수만 지정 필수!)
	 switch(열거타입변수){
			case 열거타입상수1 : 수행할 작업들...;
			case 열거타입상수2 : 수행할 작업들...;
			case 열거타입상수3 : 수행할 작업들...;
```

```java
package enum_statement;

import java.time.LocalDate;
import java.time.Month;

public class Ex1 {

	public static void main(String[] args) {

		BadWeek badWeek = new BadWeek();
		// Setter 호출하여 파라미터로 요일 전달 시 정확한 정수값은 몰라도
		// 상수 호출만으로 해당 정수 사용 및 전달이 가능
		badWeek.setMyWeek(BadWeek.WEEK_MONDAY); // 월요일(0) 설정
		
		if(badWeek.getMyWeek() == BadWeek.WEEK_MONDAY) {
			System.out.println("오늘은 월요일 입니다!");
		}
		
		// 요일 정보를 상수로 관리하는 경우의 문제점
		// => 상수 데이터들이 정수일 경우 해당 정수를 전달받은 메서드(Setter) 호출 시
		//	  상수 범위 값이 아닌 다른 값을 전달해도 컴파일에러(문법적 오류)가 발생하지 않는다!
		badWeek.setMyWeek(10); // 10이라는 정수값 갖는 요일은 없으나
		// int 타입 파라미터 이므로 컴파일 시점에는 아무런 문제가 발생하지 않는다!
		// 그러나, 해당 데이터를 사용하기 위한 시점에는 문제가 발생할 수 있음
		// => 따라서, 별도로 해당 범위 내의 데이터인지 판별하여 추가적인 작업을 수행해야함
		System.out.println("===============================================");
		
		// 열거타입 사용
//		EnumWeek myWeek = new EnumWeek();	// 열거타입 인스턴스 생성 불가!
		EnumWeek myWeek = EnumWeek.FRIDAY;	// 금요일로 설정
		System.out.println("오늘은 " + myWeek + " 입니다.");
		if(myWeek == EnumWeek.FRIDAY) { // 상수 비교 방법이 동일함
			System.out.println("오늘은 금요일 입니다.");
		}
		
		System.out.println("----------------------------------------");
		
		// 열거타입을 활용하는 클래스
		GoodWeek gw = new GoodWeek();
		gw.setMyWeek(EnumWeek.FRIDAY);
		System.out.println("오늘의 요일 : " + gw.getMyWeek());
		gw.printWeek();
		
		// 열거타입 사용 시 장점
		// => 정의 시 지정된 상수 외의 다른값은 절대 전달 불가능!
//		gw.setMyWeek(5);
//		gw.setMyWeek("MONDAY");
		// => 반드시 열거타입명. 상수명으로 지정된 값만 전달해야함
		gw.setMyWeek(EnumWeek.SATURDAY);
		
		System.out.println("===========================================");
		
		// LocalDate 클래스의 getMonth() 메서드를 호출하면 열거타입 Month 객체 리턴됨
		LocalDate today = LocalDate.now();
		System.out.println(today);
		
		System.out.println("오늘은 " + today.getMonthValue() + "월 입니다!");
		
		Month month = today.getMonth();
		System.out.println("오늘은 " + month + "월 입니다!");
		
		switch (month) {
		case JANUARY:	System.out.println("오늘은 1월 입니다!"); break;
		case FEBRUARY:	System.out.println("오늘은 2월 입니다!"); break;
		case MARCH:		System.out.println("오늘은 3월 입니다!"); break;
			// 생략
		case DECEMBER:	System.out.println("오늘은 12월 입니다!"); break;
		}
	}

}



// 열거타입 정의
enum EnumWeek {
	// 열거형 정의 시 중괄호 내에는 값을 갖는 상수 이름만 나열함(별도의 값을 저장하지 않는다!)
	MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY, SUNDAY
}

// 열거타입을 사용하는 클래스
class GoodWeek {
	EnumWeek myWeek;	// 클래스 내에서 열거타입 변수 선언 가능(로컬변수로도 가능)
	
	public EnumWeek getMyWeek() {
		return myWeek;
	}
	
	public void setMyWeek(EnumWeek myWeek) {
		this.myWeek = myWeek;
		
	}
	
	public void printWeek() {
		// 저장된 요일(myWeek)을 판별하여 월요일~일요일 까지 출력 => switch ~ case 사용
		// - 주의사항! switch문에 enum 타입 변수를 전달할 경우
		//	 case 문에서 비교값으로 enum 타입 값을 지정할때 열거타입명 없이 상수 바로 사용!
		switch (myWeek) {
		case MONDAY:	System.out.println("오늘은 월요일 입니다!"); break;
		case TUESDAY:	System.out.println("오늘은 화요일 입니다!"); break;
		case WEDNESDAY:	System.out.println("오늘은 수요일 입니다!"); break;
		case THURSDAY:	System.out.println("오늘은 목요일 입니다!"); break;
		case FRIDAY:	System.out.println("오늘은 금요일 입니다!"); break;
		case SATURDAY:	System.out.println("오늘은 토요일 입니다!"); break;
		case SUNDAY:	System.out.println("오늘은 일요일 입니다!"); break;
		}
	}
	
}


// ========================================================================
// 열거타입을 사용하지 않고, 상수만 사용하는 경우
class BadWeek {
	// 요일 정보를 상수로 관리하는 경우
	public static final int WEEK_MONDAY 	= 0;
	public static final int WEEK_TUESDAY 	= 1;
	public static final int WEEK_WEDNESDAY 	= 2;
	public static final int WEEK_THUSDAY 	= 3;
	public static final int WEEK_FRIDAY 	= 4;
	public static final int WEEK_SATURDAY 	= 5;
	public static final int WEEK_SUNDAY 	= 6;

	private int myWeek;
	
	public int getMyWeek() {
		return myWeek;
	}
	
	public void setMyWeek(int myWeek) {
//		this.myWeek = myWeek;
		// 요일에 대한 정상 범위 판별 없이 저장작업을 수행할 경우
		// 실제 저장된 요일을 꺼내서 사용하는 시점에는 문제가 발생할 수 있음
		// 따라서, 조건문 등을 사용하여 전달받은 파라미터에 대한 검증이 추가적으로 필요함
		if(myWeek >= WEEK_MONDAY && myWeek <= WEEK_SUNDAY) {
			this.myWeek = myWeek;
		} else {
			System.out.println("요일 입력 에러!");
		}
	}

	
}
```

연습문제
```java
package enum_statement;

public class Test1 {

	public static void main(String[] args) {
		/*
		 * enum(Week) 과 switch ~ case 문을 활용하여
		 * 
		 * 평일이면(월~목) : "하..." 금요일이면 : "불금!" 주말이면 : "주말!" 출력
		 * 
		 */
		Week week = Week.SAT;

		// switch ~ case 문 활용
		switch (week) {
		case MON:
		case TUE:
		case WED:
		case THU: System.out.println("하..."); break;
		case FRI: System.out.println("불금!"); break;
		case SAT:
		case SUN: System.out.println("주말!"); break;
		}

	}

}

enum Week {
	MON, TUE, WED, THU, FRI, SAT, SUN
}
```

### enum 객체 메서드
- enum 타입들은 모두 java.lang.Enum 클래스를 암묵적으로 상속받음
  - 따라서, Enum 클래스에 정의된 메서드 활용 가능

```java
package enum_statement;

public class Ex2 {

	public static void main(String[] args) {
		/*
		 * 
		 * enum 객체의 메서드
		 * - enum 타입들은 모두 java.lang.Enum 클래스를 암묵적으로 상속받음
		 * 	=> 따라서, Enum 클래스에 정의된 메서드 활용 가능
		 */
		
		Ex2 ex2 = new Ex2();
		ex2.compareEnum(Season.SUMMER);
		
	}

	public void compareEnum(Season season) {// enum 타입(Season타입) 객체(season)를 전달받음
//		System.out.println(season.compareTo(Season.WINTER));
		// => season 객체의 ordinal 값 - 파라미터로 전달된 상수의 ordinal 값 결과 리턴
		
		System.out.println(season.name() + " : " + season.ordinal());
		
		if(season == Season.SPRING) {
			System.out.println("따뜻한 봄이네요!");
		} else if(season.equals(Season.SUMMER)) {
			System.out.println("더운 여름입니다!");
		} else if(season.compareTo(season.WINTER) >= -1) {
			// ex) season(FALL(2)) - Season.WINTER(3) = -1
			// ex) season(WINTER(3)) - Season.WINTER(3) = 0
			System.out.println("점점 추워지는 계절(가을 또는 겨울) 입니다!");
		}
	}

}

// enum 타입 Season 정의
// => 상수: SPRING, SUMMER, FALL, WINTER
enum Season {
	// enum 타입 내의 상수는 자동으로 ordinal 값(순서번호)이 부여됨(0부터 자동 부여)
	SPRING,	// ordinal : 0
	SUMMER,	// ordinal : 1
	FALL,	// ordinal : 2
	WINTER	// ordinal : 3
}
```

### enum 속성 추가
```
1. ordinal은 마치 배열처럼 0부터 시작한다
	  그리고, ordinal은 정수 데이터이다.
- 특정 데이터를 지정해 주고 싶다!
예) 지역번호: (서울: 02, 경기도: 031, 인천: 032, 부산: 051...)
	   URL: NAVER("https://www.naver.com"),
			DAUM("https://www.daum.net"),
			GOOGLE"https://www.google.com")

2. 시스템 유지보수중 enum의 멤버가 추가되거나 변경된다면?
- 기존 ordinal이 깨질 수 있다.
	 => ordinal로 계산하고 있던 모든 로직을 찾아 수정해야한다.
	 예) 평일만 관리하고 있었다. (월 화 수 목 금 -> 0, 1, 2, 3, 4)
		주말도 관리해야 할 것 같다 (일 월 화 수 목 금 토 -> 0, 1, 2, 3, 4, 5, 6)

3. 모두 같은 뜻인데 DB의 테이블마다 다른 데이터로 관리되고 있다.
예) 학교 데이터 (졸업여부 : Y/N)
	   공장 생산 관리 데이터 (입고완료여부 : 1/0)
	   					(출고완료여부 : true/false)
	   배민 (결제완료여부 : T/F)
```

```java
package enum_statement;

public class Ex3 {

	public static void main(String[] args) {
  
		// 1. 지역번호
		Area 부산 = Area.부산;
		System.out.println(부산.value);
		
		// 2. 사이트
		Site 구글 = Site.GOOGLE;
		System.out.println(구글.url);
		
		// 3. 같은 뜻인데 다른 데이터로 관리되고 있을 경우
		Status s = Status.Y;
		System.out.println(s);
		System.out.println(s.value1);
		System.out.println(s.value2);
		System.out.println(s.value3);
		
		s = Status.N;
		System.out.println(s);
		System.out.println(s.value1);
		System.out.println(s.value2);
		System.out.println(s.value3);


	}

}

// 1. 특정 데이터를 지정해 주고싶다!
enum Area {
	서울("02"), 경기도("031"), 인천("032"), 부산("051");
	
//	static final Area seoul = new Area("02");
	
	String value;
	private Area(String value) {
		this.value = value;
	}
}

// 2. 사이트
enum Site {
	NAVER("https://www.naver.com"),
	DAUM("https://www.daum.net"),
	GOOGLE("https://www.google.com");
	
	String url;
	private Site(String url) {
		this.url = url;
	}
}


// 3. 모두 같은 뜻인데 DB의 테이블마다 다른 데이터로 관리되고 있다.
enum Status {
	Y(1, true, "T"),
	N(0, false, "F");
	
	int value1;
	boolean value2;
	String value3;
	
	// Alt + Shift + O
	private Status(int value1, boolean value2, String value3) {
		this.value1 = value1;
		this.value2 = value2;
		this.value3 = value3;
	}
	
	
}
```
