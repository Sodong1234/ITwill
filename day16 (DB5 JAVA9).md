# [오전수업] DB 5차

> - DB 4차에 실시한 수업 복습 실시하였음.
> - 가상머신 구동 -> cmd에서  ssh root@ip로 로그인 -> mysql -u root -p로 로그인 -> show databses; 입력 -> 안에 있는 test 데이터베이스를 사용하기 위해 use test; 입력 -> 안에 있는 student 테이블의 구조를 확인하기 위해 DESC student; 까지 입력 실시하였음
> - DB 4차, UPDATE 항목 이어서 수업 실시. 

## UPDATE 문법
- 기존 데이터의 값을 갱신할 때 사용하는 문법

```
- UPDATE 테이블명
  SET 갱신컬럼 = 갱신할 값
  [WHERE 조건컬럼 = 조건값];
```
```
UPDATE student 입력
SET last_name = 'GIM' 입력
WHERE stu_no = 3; 입력

SELECT * FROM student; 입력

+--------+-----------+------------+
| stu_no | last_name | birth_date |
+--------+-----------+------------+
|      1 | hi        | NULL       |
|      2 | 6         | NULL       |
|      3 | Gim       | NULL       |
|      4 | bye       | NULL       |
|      5 | Buy       | 2022-02-10 |
|      6 | LEE       | 2022-02-14 |
+--------+-----------+------------+

```

- WHERE절을 생략하는 경우 테이블의 모든 행에 대해서 SET절의 갱신의 영향 받음
```
UPDATE student 입력
SET last_name = 'Name'; 입력

+--------+-----------+------------+
| stu_no | last_name | birth_date |
+--------+-----------+------------+
|      1 | Name      | NULL       |
|      2 | Name      | NULL       |
|      3 | Name      | NULL       |
|      4 | Name      | NULL       |
|      5 | Name      | 2022-02-11 |
|      6 | Name      | 2022-02-14 |
+--------+-----------+------------+
```
- stu_no의 순서를 1씩 더하기
```
UPDATE student 입력
SET stu_no = stu_no + 1; 입력

+--------+-----------+------------+
| stu_no | last_name | birth_date |
+--------+-----------+------------+
|      2 | Name      | NULL       |
|      3 | Name      | NULL       |
|      4 | Name      | NULL       |
|      5 | Name      | NULL       |
|      6 | Name      | 2022-02-11 |
|      7 | Name      | 2022-02-14 |
+--------+-----------+------------+

```

### 데이터 삭제
- 기존 테이블의 데이터를 삭제하는 문법
```
-문법
DELETE FROM 테이블명;
[WHERE 조건컬럼 연산자 조건절];
```

```
- student 테이블에서 stu_no컬럼의 값이 3의 값을 가진 행을 삭제하기

DELETE FROM student 입력
WHERE stu_no = 3; 입력

+--------+-----------+------------+
| stu_no | last_name | birth_date |
+--------+-----------+------------+
|      2 | Name      | NULL       |
|      4 | Name      | NULL       |
|      5 | Name      | NULL       |
|      6 | Name      | 2022-02-11 |
|      7 | Name      | 2022-02-14 |
+--------+-----------+------------+


---------------------------------------------------------------------------------------------


- birth_date 컬럼의 값이 NULL인 행을 삭제

DELETE FROM student 입력
WHERE birth_date IS NULL; 입력

+--------+-----------+------------+
| stu_no | last_name | birth_date |
+--------+-----------+------------+
|      6 | Name      | 2022-02-11 |
|      7 | Name      | 2022-02-14 |
+--------+-----------+------------+


---------------------------------------------------------------------------------------------


- WHERE절을 생략하는 경우 테이블의 모든 데이터가 삭제

DELETE FROM student; 입력

Empty set (0.00 sec)
```

## 데이터 조회(SELECT)
- 테이블에 저장된 데이터를 조회할 때 사용하는 문법
> - 데이터 조회용 샘플 데이터 추가하기 (https://antop.tistory.com/attachment/cfile22.uf@120E12034B315C3B3FCAA1.zip)
> - cmd 창으로 돌아와서 exit 입력 -> mkdir Downloads 입력 -> cd Downloads/ 입력 -> pwd 입력 -> 
> - 압축 푼 폴더 안에서 Shift + 마우스 오른쪽 클릭 -> 여기에 PowerShell 창 열기 클릭 -> PowerShell 창에서 scp hr.sql root@아이피:/root/Downloads/. 입력 -> 비밀번호 입력
> - cmd 창으로 돌아와서 ls 입력 -> hr.sql이 있으면 다운로드 완료
> - cmd에서 mysql -u root -p로 로그인 -> CREATE DATABASE hr; 입력 -> use hr; 입력 -> source /root/Downloads/hr.sql 입력 -> show tables; 입력으로 생성된 테이블 확인
```
+----------------+
| Tables_in_hr   |
+----------------+
| HR_COUNTRIES   |
| HR_DEPARTMENTS |
| HR_EMPLOYEES   |
| HR_JOBS        |
| HR_JOB_HISTORY |
| HR_LOCATIONS   |
| HR_REGIONS     |
+----------------+
```
```
DESC HR_DEPARTMENTS; 입력

+-----------------+-------------+------+-----+---------+-------+
| Field           | Type        | Null | Key | Default | Extra |
+-----------------+-------------+------+-----+---------+-------+
| DEPARTMENT_ID   | int         | NO   | PRI | NULL    |       |
| DEPARTMENT_NAME | varchar(30) | NO   |     |         |       |
| MANAGER_ID      | int         | YES  | MUL | NULL    |       |
| LOCATION_ID     | int         | YES  | MUL | NULL    |       |
+-----------------+-------------+------+-----+---------+-------+
DEPARTMENT_ID     : 부서번호
DEPARTMENT_NAME   : 부서명
MANAGER_ID        : 부서장 사원번호
LOCATION_ID       : 부서의 위치코드
```

```
-문법
SELECT 컬럼1 [, 컬럼2, ...]
FROM 테이블명
[WHERE 조건컬럼 연산자 조건값];
```
```
SELECT department_id, department_name, manager_id, location_id 입력
FROM HR_DEPARTMENTS; 입력

+---------------+----------------------+------------+-------------+
| department_id | department_name      | manager_id | location_id |
+---------------+----------------------+------------+-------------+
|            10 | Administration       |        200 |        1700 |
|            20 | Marketing            |        201 |        1800 |
|            30 | Purchasing           |        114 |        1700 |
|            40 | Human Resources      |        203 |        2400 |
|            50 | Shipping             |        121 |        1500 |
|            60 | IT                   |        103 |        1400 |
|            70 | Public Relations     |        204 |        2700 |
|            80 | Sales                |        145 |        2500 |
|            90 | Executive            |        100 |        1700 |
|           100 | Finance              |        108 |        1700 |
|           110 | Accounting           |        205 |        1700 |
|           120 | Treasury             |       NULL |        1700 |
|           130 | Corporate Tax        |       NULL |        1700 |
|           140 | Control And Credit   |       NULL |        1700 |
|           150 | Shareholder Services |       NULL |        1700 |
|           160 | Benefits             |       NULL |        1700 |
|           170 | Manufacturing        |       NULL |        1700 |
|           180 | Construction         |       NULL |        1700 |
|           190 | Contracting          |       NULL |        1700 |
|           200 | Operations           |       NULL |        1700 |
|           210 | IT Support           |       NULL |        1700 |
|           220 | NOC                  |       NULL |        1700 |
|           230 | IT Helpdesk          |       NULL |        1700 |
|           240 | Government Sales     |       NULL |        1700 |
|           250 | Retail Sales         |       NULL |        1700 |
|           260 | Recruiting           |       NULL |        1700 |
|           270 | Payroll              |       NULL |        1700 |
+---------------+----------------------+------------+-------------+


---------------------------------------------------------------------------------------------


SELECT department_id, department_name 입력
FROM HR_DEPARTMENTS; 입력

+---------------+----------------------+
| department_id | department_name      |
+---------------+----------------------+
|            10 | Administration       |
|            20 | Marketing            |
|            30 | Purchasing           |
|            40 | Human Resources      |
|            50 | Shipping             |
|            60 | IT                   |
|            70 | Public Relations     |
|            80 | Sales                |
|            90 | Executive            |
|           100 | Finance              |
|           110 | Accounting           |
|           120 | Treasury             |
|           130 | Corporate Tax        |
|           140 | Control And Credit   |
|           150 | Shareholder Services |
|           160 | Benefits             |
|           170 | Manufacturing        |
|           180 | Construction         |
|           190 | Contracting          |
|           200 | Operations           |
|           210 | IT Support           |
|           220 | NOC                  |
|           230 | IT Helpdesk          |
|           240 | Government Sales     |
|           250 | Retail Sales         |
|           260 | Recruiting           |
|           270 | Payroll              |
+---------------+----------------------+

```

- 컬럼 목록을 하나하나 입력하는 것이 아닌, '*' 기호만 작성하는 경우 테이블의 모든 컬럼을 출력
```
DESC HR_EMPLOYEES; 입력

+----------------+--------------+------+-----+---------+-------+
| Field          | Type         | Null | Key | Default | Extra |
+----------------+--------------+------+-----+---------+-------+
| EMPLOYEE_ID    | int          | NO   | PRI | NULL    |       |
| FIRST_NAME     | varchar(20)  | YES  |     | NULL    |       |
| LAST_NAME      | varchar(25)  | NO   |     |         |       |
| EMAIL          | varchar(25)  | NO   | UNI |         |       |
| PHONE_NUMBER   | varchar(20)  | YES  |     | NULL    |       |
| HIRE_DATE      | date         | NO   |     | NULL    |       |
| JOB_ID         | varchar(10)  | NO   | MUL |         |       |
| SALARY         | double(22,0) | YES  |     | NULL    |       |
| COMMISSION_PCT | double(22,0) | YES  |     | NULL    |       |
| MANAGER_ID     | int          | YES  | MUL | NULL    |       |
| DEPARTMENT_ID  | int          | YES  | MUL | NULL    |       |
+----------------+--------------+------+-----+---------+-------+

SELECT * 입력 
FROM HR_EMPLOYEES; 입력

+-------------+-------------+-------------+----------+--------------------+------------+------------+--------+----------------+------------+---------------+
| EMPLOYEE_ID | FIRST_NAME  | LAST_NAME   | EMAIL    | PHONE_NUMBER       | HIRE_DATE  | JOB_ID     | SALARY | COMMISSION_PCT | MANAGER_ID | DEPARTMENT_ID |
+-------------+-------------+-------------+----------+--------------------+------------+------------+--------+----------------+------------+---------------+
|         100 | Steven      | King        | SKING    | 515.123.4567       | 1987-06-17 | AD_PRES    |  24000 |           NULL |       NULL |            90 |
|         101 | Neena       | Kochhar     | NKOCHHAR | 515.123.4568       | 1989-09-21 | AD_VP      |  17000 |           NULL |        100 |            90 |
|         102 | Lex         | De Haan     | LDEHAAN  | 515.123.4569       | 1993-01-13 | AD_VP      |  17000 |           NULL |        100 |            90 |
|         103 | Alexander   | Hunold      | AHUNOLD  | 590.423.4567       | 1990-01-03 | IT_PROG    |   9000 |           NULL |        102 |            60 |

...

|         204 | Hermann     | Baer        | HBAER    | 515.123.8888       | 1994-06-07 | PR_REP     |  10000 |           NULL |        101 |            70 |
|         205 | Shelley     | Higgins     | SHIGGINS | 515.123.8080       | 1994-06-07 | AC_MGR     |  12000 |           NULL |        101 |           110 |
|         206 | William     | Gietz       | WGIETZ   | 515.123.8181       | 1994-06-07 | AC_ACCOUNT |   8300 |           NULL |        205 |           110 |
+-------------+-------------+-------------+----------+--------------------+------------+------------+--------+----------------+------------+---------------+

```

## 표현식(expression)
- 수식을 통해서 데이터에 대한 연산을 한 결과를 출력할 수 있는 식
- 사칙연산
  - 덧셈 : +
  - 뺄셈 : -
  - 곱셈 : *
  - 나눗셈 : /

```
- salary(월급)을 이용하여 연봉 계산하기

SELECT salary, salary * 12 입력
FROM HR_EMPLOYEES; 입력

+--------+-------------+
| salary | salary * 12 |
+--------+-------------+
|  24000 |      288000 |
|  17000 |      204000 |
|  17000 |      204000 |

...

|  10000 |      120000 |
|  12000 |      144000 |
|   8300 |       99600 |
+--------+-------------+
```

## column alias (컬럼의 별명)
- 결과 출력 시 기존의 이름이 아닌 별명으로 임시의 이름을 붙여 결과를 출력
- 실제 컬럼의 이름은 바뀌지 않음

```
SELECT salary, salary * 12 "annual salary" 입력
FROM HR_EMPLOYEES; 입력

+--------+---------------+
| salary | annual salary |
+--------+---------------+
|  24000 |        288000 |
|  17000 |        204000 |
|  17000 |        204000 |

...

|  10000 |        120000 |
|  12000 |        144000 |
|   8300 |         99600 |
+--------+---------------+
```


---
# [오후수업] JAVA 9차
## 다차원 배열
- 1차원 배열 여러개가 모여 2차원 이상의 배열을 이루는 것
- 일반적으로 다차원 배열 = 2차원 배열을 의미

### 2차원 배열
- 행, 열의 구조로 이루어진 배열
  - 실제 데이터가 저장되는 공간 = 열
  - 열 공간의 주소를 저장하는 공간 = 행
- 배열 크기
  1) 행 크기 : 배열명.length
  2) 열 크기 : 배열명[행인덱스].length

```
< 2차원 배열 선언 기본 문법 >
데이터타입[][] 변수명;

< 2차원 배열 생성 기본 문법 >
변수명 = new 데이터타입[행크기][열크기]

< 2차원 배열 인덱스 접근 기본 문법 >
변수명[행인덱스][열인덱스]

< 2차원 배열 선언 및 생성, 초기화 한꺼번에 수행하는 방법 >
int[][] arr = ( 
           {1, 2, 3}, 
           {4, 5, 6} 
)
```

```java
		// 배열선언 및 생성
//		int[][] arr;
//		arr = new int[2][3];
		
		// 위 표현을 1줄로 표현하면
		int[][] arr = new int[2][3];
		
		arr[0][0] = 1; arr[0][1] = 2; arr[0][2] = 3;
		arr[1][0] = 4; arr[1][1] = 5; arr[1][2] = 6;
		
		System.out.println(arr[0][0] + " " + arr[0][1] + " " + arr[0][2]);
		System.out.println(arr[1][0] + " " + arr[1][1] + " " + arr[1][2]);
```
연습문제1. 반복문을 활용하여 위와 같이 출력
```java
		for(int i = 0; i < arr.length; i++) { // 행
			for(int j = 0; j < arr[i].length; j++) { // 열
				System.out.print(arr[i][j] + " ");
			}
			System.out.println();
		}
```    
```java
// 배열의 크기
		System.out.println("배열 arr 의 행 크기 : " + arr.length);
		System.out.println("배열 arr 의 0번 행의 열 크기 : " + arr[0].length);
		System.out.println("배열 arr 의 1번 행의 열 크기 : " + arr[1].length);
    
    ------------------------------------------------------------------------
    
		배열 arr 의 행 크기 : 2
		배열 arr 의 0번 행의 열 크기 : 3
		배열 arr 의 1번 행의 열 크기 : 3
```    
```java
선언과 동시에 초기화
	// 선언과 동시에 초기화
int[][] arr2 = {
				{1, 2, 3},
				{4, 5, 6}
		};
				
		for(int i = 0; i < arr2.length; i++) { // 행
			for(int j = 0; j < arr2[i].length; j++) { // 열
				System.out.print(arr2[i][j] + " ");
			}
			System.out.println();
		}
```
- 2차원 배열에서 각 행의 열 갯수가 동일하지 않을 수 있음
  - 연습문제1. 재풀이 
```java
//		int[][] arr3 = {
//				{1, 2, 3},
//				{4, 5},
//				{6, 7, 8, 9, 10}
//		};
		
		int[][] arr3 = new int[3][];
		arr3[0] = new int[] {1, 2, 3};
		arr3[1] = new int[] {4, 5};
		arr3[2] = new int[] {6, 7, 8, 9, 10};
		
		for(int i = 0; i < 3; i++) { // 행
			for(int j = 0; j < arr3[i].length; j++) { // 열
				System.out.print(arr3[i][j] + " ");
			}
			System.out.println();
		}
```    
연습문제2. 
- 1차원 배열(names)에 5명 학생의 이름을 저장하고, 2차원 배열(score)에 5명 학생의 국어, 영어, 수학 점수 저장 후 출력
- 학생별 총점을 1차원 배열(total)에 저장하고 출력
```
 < 출력 예시 >
	 국어		 영어		 수학
홍길동	 80		 80		 80
이순신	 90		 80		 77
강감찬	 60		 50		 60
김태희	 100	 	 100	 	 100
전지현	 50		 80		 100
---------------------------------
< 학생별 총점 >
홍길동 : 240점
이순신 : 247점
강감찬 : 170점
김태희 : 300점
전지현 : 230점
```
```java
String[] names = {"홍길동", "이순신", "강감찬", "김태희", "전지현"};
		int[][] score = {
				{80, 80, 80},
				{90, 80, 77},
				{60, 50, 60},
				{100, 100, 100},
				{50, 80, 100}
		};
				
		System.out.println("\t국어\t영어\t수학");

		for(int i = 0; i < score.length; i++) {
			System.out.print(names[i] + "\t");
			for(int j = 0; j < score[i].length; j++) {
				System.out.print(score[i][j] + "\t");
			}
			System.out.println();
		}
		
			System.out.println("-------------------------------");
			System.out.println("< 학생별 총점 >");
			

//		int[] total = new int[5];
////			total[0] = score[0][0] + score[0][1] + score[0][2]; // 홍길동 총점
////			total[1] = score[1][0] + score[1][1] + score[1][2]; // 이순신 총점
////			total[2] = score[2][0] + score[2][1] + score[2][2]; // 강감찬 총점
////			total[3] = score[3][0] + score[3][1] + score[3][2]; // 김태희 총점
////			total[4] = score[4][0] + score[4][1] + score[4][2]; // 전지현 총점
//		for (int i = 0; i < score.length; i++) {
//			for (int j = 0; j < score[i].length; j++) {
//				total[i] += score[i][j];
//			}
//		}
//
////			System.out.println(names[0] + " : " + total[0] + "점");
////			System.out.println(names[1] + " : " + total[1] + "점");
////			System.out.println(names[2] + " : " + total[2] + "점");
////			System.out.println(names[3] + " : " + total[3] + "점");
////			System.out.println(names[4] + " : " + total[4] + "점");
//
//		for (int i = 0; i < total.length; i++) {
//			System.out.println(names[i] + " : " + total[i] + "점");
//		}

		// 메서드 version
		
		
		
		int[][] score2 = { 
				{ 80, 80, 80 }, 
				{ 90, 80, 77 }, 
				{ 60, 50, 60 }, 
				{ 100, 100, 100 }, 
				{ 50, 80, 100 } 
				
				
		};
		int [] total = getTotal(score2);
		for(int i = 0; i < total.length; i++) {
			System.out.println(total[i]);
		}
		

		
		
		
		
	} // main() 메서드 끝
	
	// score 배열을 전달하여 총점(total) 배열을 리턴하는 메서드 getTotal() 작성
	public static int[] getTotal(int [][] score) {
			int[] result = new int[score.length]; // 전달받은 score 배열의 행 크기만큼 1차원 배열을 생성
			
			for(int i = 0; i < score.length; i++) {
				for(int j = 0; j < score[i].length; j++) {
					result[i] += score[i][j];
				}
			}
			return result;
		};
	} // test1 클래스 끝
```

### 복습문제
> 지금까지 수업시간에 배운 것들을 활용하여 복습 문제 풀이 실시
1. operator 값이 +,-,*,/ 인 경우에 사칙 연산을 수행하는 프로그램을 if-else if문과 switch-case 문을 사용해 작성하기
```java
int num1 = 10;
		int num2 = 2;
		char operator = '+';
		int result = 0;
		
		// if문
		if(operator == '+') {
			result = num1 + num2;
		} else if (operator == '-') {
			result = num1 - num2;
		} else if (operator == '*') {
			result = num1 * num2;
		} else if (operator == '/') {
			result = num1 / num2;
		}
		
		System.out.println("if문 결과 : " + result);
		// switch - case문
		switch(operator) {
		case '+' : result = num1 + num2;
		break;
		case '-' : result = num1 - num2;
		break;
		case '*' : result = num1 * num2;
		break;
		case '/' : result = num1 / num2;
		break;
		}
		System.out.println("switch-case문 결과 : " + result);
		
		} // main() 메서드 끝
	
	//int형 정수 2개와 char형 연산자를 전달받아
	// 사칙연산 수행 결과를 리턴하는 메서드 calc() 작성
	
	public static int calc(int n1, int n2, char op) {
		int result = 0;
		if(op== '+') {
			result = n1 + n2;
		} else if (op == '-') {
			result = n1 - n2;
		} else if (op == '*') {
			result = n1 * n2;
		} else if (op == '/') {
			result = n1 / n2;
		}
			
		return result;
		}
	
	} // Ex1 클래스 끝
```

2. 구구단을 짝수 단만 출력하도록 프로그램을 만들어보기 (continue 사용)
```java
			/*
		 * 구구단을 짝수 단만 출력하도록 프로그램을 만들어보기 (continue 사용)
		 * 
		 */

//		for (int dan = 2; dan < 10; dan++) {
//			
//			System.out.println("< " + dan + "단 >");
//			
//			for (int i = 1; i < 10; i++) {
//				if(dan % 2 == 1) { // if문 조건 판별 횟수 8 * 9 = 72회 실시됨.
		// continue가 실행된 횟수 = 4 * 9 = 36회 실시됨.
		// 비효율적.
//					continue;
//				}
//				System.out.println(dan + " * " + i + " = " + (dan * i));
//			}
//			System.out.println();
//		}

		for (int dan = 2; dan < 10; dan++) {

			if (dan % 2 == 1) { // if문 조건 판별 횟수 = 8회
								// continue가 실행된 횟수 = 4회

				continue;
			}

			System.out.println("< " + dan + "단 >");

			for (int i = 1; i < 10; i++) {

				System.out.println(dan + " * " + i + " = " + (dan * i));
			}
			System.out.println();
		}
	}
}
```		
