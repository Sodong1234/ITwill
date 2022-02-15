# [오전수업] 자바 3차
**어제 수업 복습 겸 문제풀이 실시**

1. 초를 입력받아 "XX분 / XX초" 형태로 출력 (int a = 190;)

*나의 해답 : 

```java
  		int a = 190;
  		int b = a/60;
				
  		System.out.println("b" + "분" + "/" + a + "초");
```

*정답 : 

```java
		int a = 190;
		System.out.println((a / 60) + "분" + "/" + (a % 60) + "초");
```    
```java
		int a = 190;
		int min = a / 60;
		int sec = a % 60;
		System.out.println(min + "분 / " + sec + "초");
``` 

2. 두 정수 x, y를 입력받아 x가 y보다 크거나 같으면(이상) 1, 작으면(미만) 0 을 출력  (int x = 10, y = 20;

*나의 해답 & 정답: 
```java
		int x = 10;
		int y = 20;
		
		
		String result = (x>=y) ? "1" : "0";
		System.out.println(result);
    
    String 문을 사용할 시 "1" : "0" , int 문을 사용할 시 1 : 0
```
---
## 조건문
- 특정 조건에 따라 문장의 실행여부를 결정하는 문

### if문
- 조건식에 따라 특정 문장 실행여부를 결정하는 기본적인 조건문
- 조건식 판별 결과가 true 이면 블록{} 문 내의 문장글을 실행하고 조건식 판별 결과가 false이면 블록문을 생략함

		  < 기본 문법 >
		  
		  문장1;
		  if (조건식) {
		  		문장2;
		  }
		  문장3;
		  
		   조건식 true : 문장1 > 문장2 > 문장3
		   조건식 false : 문장1 > 문장3
       
ex)

		int num = 10;
		
		if(num>=5) {
			System.out.println("num이 5보다 크다!");
		}
		System.out.println("num: " + num);
    
문제  1. 정수형 변수 num에 대한 절대값 계산하여 출력 (num이 -10일 때 -> "변수 num = 10)
```java
		int num = -10;
		
		if (num < 0) {
			num = -num;
		} 
		System.out.println("변수 num = " + num);
```

문제 2. 문자 ch가 대문자일때, 소문자로 변환하여 출력 (출력예시) "ch = a")
- ch가 대문자일 때 + 32 -> 소문자

```java
		char ch = 'A';
				
		if ('A'<= ch && ch<='Z') {
//		ch = (char)(ch+32);
		ch += 32;
		}
		System.out.println("ch = " + ch);
    
    
    참고 : 'A' <= ch <='Z' 쓰면 오류 나니 따로다로 엮어주기
```

## if ~ else문
- if문과 동일하지만 ()안의 결과가 false 일 경우 else의 {} 블록 안의 실행문이 실행된다.

		  < 기본 문법 >
		  
		  문장1;
		  
		  if(조건식){
		  
		  		문장2;
		  
		  } else {
		  
		  		문장3;
		  
		  }
		  
		  문장4;
		  
		  조건식 true  : 문장1 -> 문장2 -> 문장4
		  조건식 false : 문장1 -> 문장3 -> 문장4
		  (주의! 문장2와 문장3은 절대 동시에 실행 될 수 없다!)
      
ex) 
```java
		 정수형 변수 num이 음수인지 양수인지 출력 (단, 0을 양수로 취급) (int num = 3)
		 출력예시) "양수", "음수"
		 

		int num = 3;
		if (num >= 0) {
			System.out.println("양수");
		} else {
			System.out.println("음수");
		}
		
    
//		삼항연산자 사용
		String result = (num >= 0) ? "양수" : "음수";
		System.out.println(result);
```

문제1. 정수형 변수 num에 대해서 7의 배수인지 판별하여 7의 배수이면 "multiple" , 7의 배수가 아니면 "not multiple" (int num = 21)
```java
		int num = 21;
		if (num%7 == 0) {
			System.out.println("multiple");
		} else {
			System.out.println("not multiple");
		}
    
//		삼항연산자 사용
		String result = num%7 == 0 ? "multiple" : "not multiple"
		System.out.println(result);  
```
문제2. 문자 ch가 대문자 -> 소문자로 변환 (+32) / 소문자 -> 대문자로 변환 (-32) , "ch = X" 로 출력 (char ch = 'A')
```
*중괄호 없이 풀이*

		char ch = 'A';
		if ('A'<=ch && ch<='Z') ch += 32;
		else 		        ch -= 32;
		 
		System.out.println("ch = " + ch);
		
//		삼항연산자 사용
		ch = 'A'<=ch && ch<='Z' ? (char)(ch+32) : (char)(ch-32);
		System.out.println("ch = " + ch);
```    
## if ~ else if ~ else 문
- 조건이 여러 개일 경우 사용된다

		  < 기본 문법 >
		  
		  if(조건식1) {
		  
		  } else if(조건식2) {
		  
		  } else if(조건식3) {
		  
		  } else { 
		  
		  }
		  
ex)
```java
정수형 num이 양수, 음수, 0을 판별

		int num = 0;
		
		if(num > 0) {
			System.out.println("양수");
		} else if(num == 0) {
			System.out.println("0");
		} else {
			System.out.println("음수");
		}

	}
```

문제1. 문자 ch가 대문자 -> 소문자 변경 후 출력 / 소문자 -> 대문자 변경 후 출력 / "ch는 영문자가 아닙니다!" 출력. (출력예시) "ch = X"
```java
		char ch = 'A';
		if ('A'<= ch && ch <= 'Z') {
			ch+=32;
			System.out.println("ch = " + ch);
		} else if ('a'<= ch && ch <= 'z') {
			ch-=32;
			System.out.println("ch = " + ch);
		} else {
			System.out.println("ch는 영문자가 아닙니다!");
		}
    
    --------------------------------------------------------------------
    
		if('A' <= ch && ch <= 'Z')	result = "ch = " + (char)(ch+32);
		else if('a' <= ch && ch <= 'z')	result = "ch = " + (char)(ch-32);
		else			        result = "ch는 영문자가 아닙니다!";
```

문제2. 학생 점수(score)에 대한 학점(grade) 판별
- A학점 : 90 ~ 100점
- B학점 : 80 ~ 89점
- C학점 : 70 ~ 79점
- D학점 : 60 ~ 69점
- F학점 : 0 ~ 59점
- 출력예시 : "학점 : X"  (단, 0 ~ 100 사이의 수라고 가정)

```java
		int score = 77;
		String grade = "학점: ";
		
		
		
		if (90 <= score && score <= 100) 	 grade += "A";
		else if (80 <= score)			 grade += "B";
		else if (70 <= score)			 grade += "C";
		else if (60 <= score)			 grade += "D";
		else 					 grade += "X";
```
---

# [오후수업] JSP 5차
**수업 시작에 앞서 복습문제 출제**
```
1. 식별자 작성 규칙
- 첫 글자는 숫자 X
- 대소문자 구별
- 예약어 사용 불가
- 특수문자는 $와 _만 사용 가능
- 공백 사용 불가

권장 
의미 있는 단어 조합
두 단어 이상 조합시 두 번째 단어부터 이니셜은 대문자
길이 제한 없음
-----------------------------------------------------------------
2. 자바의 기본 데이터타입
정수형 - byte, short, int, long
실수형 - float, double
논리형 - boolean (true / false)
문자형 - char
기타(참조 데이터타입) - String, 배열
------------------------------------------------------------------------------------
3. byte 타입으로 표현 할 수 있는 정수의 범위
-128 ~ 127
------------------------------------------------------------------------------------
4. 2^16 = 65536
------------------------------------------------------------------------------------
5. char 타입에서 A, a, 0의 정수값
A = 65
a = 97
0 = ?				// 답 48
-------------------------------------------------------------------
6. 연산자 중 증감연산자가 있었음. 증감연산자를 사용하여 변수의 값을 1만큼 증가시키는 방법
++a, a++, ?			// 답 a=a+1, a+=1, a++ or ++a 
------------------------------------------------------------------------------------
7. 두 조건이 true일때만 결과값이 true가 되는 연산자의 이름과 연산자
and, &(Ampersand)
------------------------------------------------------------------------------------
8. 두 피연산자 중 하나라도 true이면 결과값이 true, 둘 다 false일때만 결과가 false인 연산자
or, |(Vertical bar)
---------------------------------------------------------------------
9. 자바 개발을 위해서 반드시 설치해야 되는 프로그램의 이름과 
자바 개발에 필요한 코드 작성이나 관리나 컴파일 실행 이 모든 것들을 하나로 묶어주는
프로그램의 이름
JDK, IDE
------------------------------------------------------------------------------------
5,6번 틀림
```
**후에 오전수업에서 배운 if문, if-else문, if~else if문 복습 실시
```java
		// if문
		
		int money = 1000;
		System.out.println("지갑에 돈이 5천원 이상 있는가?"); // 문장 1
		
		// 정수형 변수 money 의 값이 5천 이상인가?
		if (money >= 5000) { // 조건식
			// 조건식 판별 결과가 true 일 때 실행한 문장들...
			System.out.println("택시 타자!"); // 문장 2
		}
		
		System.out.println("아이티윌 부산교육센터까지 가기!"); // 문장 3
		
		
		
		
		
		// if-else문
		
		int money = 5000;
		System.out.println("돈이 5000원 이상 있는가?"); // 문장1
		
		// 돈이 5천원 이상이면 "택시 타기!", 아니면 "걷기!" 출력
		if (money>=5000) {
			// 조건식 판별 결과가 true 일 때 실행한 문장들...
			System.out.println("택시 타기!"); // 문장2
		} else {
			// 조건식 판별 결과가 false 일 때 실행한 문장들...
			System.out.println("걷기!"); // 문장3
		}
		
		System.out.println("아이티윌 가기!"); // 문장4
		
		
		
		
		
		// if-else if문
		int num = 10;
		
		if (num > 10) {
			System.out.println("10보다 크다");
		} else if (num < 10) {
			System.out.println("10보다 작다");
		} else {
			System.out.println("10과 같다");
		}
		
```

연습문제1. 정수형 변수 num이 10보다 큰지 판별하는 문장 
- 만약, 10보다 클 경우 "num 이 10보다 크다!" 출력 / 아니면, "num 이 10보다 크지 않다!" 출력 (int num = 5;)
```java
		int num = 5;
		if (num>10) {
			System.out.println("num 이 10보다 크다!"); 
		} else {
			System.out.println("num 이 10보다 크지 않다!");
		}
```

연습문제2. 정수 num에 대해 양수와 음수를 판별 (단, 0은 양수로 가정)
```java
		if (num >= 0) {
			System.out.println("양수");
		} else {
			System.out.println("음수");
		}
```
연습문제3. 입력받은 아이디와 조회된 아이디가 같고, 입력받은 패스워드와 조회된 패스워드가 같으면 "로그인 성공" 출력하고, 아니면 "로그인 실패" 출력
```java
		// 사용자로부터 입력받은 아이디, 패스워드
		String id = "admin";
		String pass = "1234";
		// 데이터베이스로부터 조회된 아이디, 패스워드
		String dbId = "admin";
		String dbPass = "1234";
		
		
		
		if (id == dbId && pass == dbPass) {
			System.out.println("로그인 성공");
		} else {
			System.out.println("로그인 실패");
		}
```

연습문제4. 정수형 변수 money 에 금액을 저장하고
- 5000 이상일 경우 : "택시 타기!" 출력
- 2000 이상일 경우 : "버스 타기!" 출력
- 그 외 : "걸어가기!" 출력
```java
		int money = 2000;
		
		if (money>=5000) {
			System.out.println("택시 타기!");
		} else if (money>=2000) {
			System.out.println("버스 타기!");
		} else {
			System.out.println("걸어가기!");
		}
		
		
		
		// 주의! if 문은 위에서부터 차례대로 조건식을 판별하므로
		// 위의 조건식이 아래쪽 조건식보다 범위가 클 경우 위의 조건식이 문장을 모두 실행하게 되므로
		// 아래쪽 조건식의 문장은 실행되지 못 할 수 있다!
		int money = 2000;

		if (money >= 2000) { // money >= 5000 범위도 포함되는 조건식이므로
				     // 2천 이상이면 모두 "버스 타기!" 가 출력되므로
			System.out.println("택시 타기!");
		} else if (money >= 5000) { // 이 조건식은 실행되지 않는다!
			System.out.println("버스 타기!");
		} else {
			System.out.println("걸어가기!");
		}
```

**tip) 이클립스에서 코드 작성 후 Ctrl + Shift + F 누르면 이클립스 추천 자동정렬!**

연습문제5. 양수, 음수, 0 판별 & 홀수, 짝수, 0 판별
```java
		// 양수, 음수, 0 판별 -> 순서 무관
		if (num > 0) {
			System.out.println("양수");
		} else if (num < 0) {
			System.out.println("음수");
		} else {
			System.out.println("0");
		}
		
		
		
		
		
		// 홀수, 짝수, 0 판별 -> 주의! 짝수와 0은 순서 중요
		// 1) 짝수 판별 조건식이 0 판별 조건식보다 위(앞)에 있을 경우
		//    0은 짝수 판별 조건식에도 포함되므로 0 이 출력되지 않는다!
		if (num % 2 == 1) {
			System.out.println("홀수");
		} else if (num % 2 == 0) {
			System.out.println("짝수");
		} else {
			System.out.println("0");
			
			
			
			
		if (num % 2 == 1) {
			System.out.println("홀수");
		} else if (num == 0) {
			System.out.println("0");
		} else {
			System.out.println("짝수");
```
연습문제6. 입력받은 아이디와 조회된 아이디가 같고, 입력받은 패스워드와 조회된 패스워드가 같으면 "로그인 성공" 출력하고, 
- 아니면 입력받은 아이디는 같고 입력받은 패스워드가 다르면 "패스워드 틀림" 출력하고
- 아니면 "아이디 없음" 출력
```java

		// 사용자로부터 입력받은 아이디, 패스워드
		String id = "admin";
		String pass = "1234";
		// 데이터베이스로부터 조회된 아이디, 패스워드
		String dbId = "admin";
		String dbPass = "1234";



		if (id == dbId && pass == dbPass) {
			System.out.println("로그인 성공");
		} else if (id == dbId && pass != dbPass) {
			System.out.println("패스워드 틀림");
		} else {
			System.out.println("아이디 없음");
		}
```

---

# Switch-case문
- if문과 마찬가지로 특정 조건에 대한 결과에 따라 다른 문장을 실행하는 조건문
- if문은 실행 시점에서 조건식을 판별하여 결과에 따라 실행할 문장이 결정되지만 switch-case 문은 컴파일(번역) 시점에서 이미 실행할 문장이 결정되므로 if문에 비해 switch-case 문의 실행 속도가 빠르다!
- if 문에 비해 제약 사항이 많으므로 문장 작성 시 유연성이 떨어진다!
	- 모든 if 문을 switch-case 문으로 변환할 수 없지만, 모든 switch-case 문은 if 문으로 변환 가능하다! 

```java
		    < 기본 문법 >
		    switch(식) {
		    	case 값1 : // 식의 판별 결과가 값1 과 같을 때 실행할 문장들... 
		    			[break];
		    	case 값2 : // 식의 판별 결과가 값2 와 같을 때 실행할 문장들...
		    			[break[;
		    	case 값n : // 식의 판별 결과가 값n 과 같을 때 실행할 문장들...
		    			[break];
		    	[default : // 식의 판별 결과가 일치하는 값이 없을 때 실행할 문장들...]
		    }
		    
		    < 문법 구조 >
		    - switch 문에 판별에 필요한 식을 기술하고, 일치하는 값을 case 문으로 판별
		    - switch 문의 식에 올 수 있는 문장은 결과값이 정수(long 제외) or 문자열 or Enum 상수만 올 수 있다!
		    - case 문에서 조건식 결과값과 비교할 값을 기술하고, 조건식과 비교하여 일치하는 값의 문장을 실행
		      - case 문의 값 위치에 올 수 있는 것은 리터럴(정수 or 문자열 or Enum 상수) 뿐이다!
		      - case 문의 값은 중복될 수 없으며, case 문끼리의 순서는 무관함
```

```java
		int num = 10;
		
//		switch (num > 5) {} // if문처럼 결과값이 boolean 타입인 식은 올 수 없다!
		
		// 정수or문자열의 변수 또는 리터럴을 직접 사용하거나
		// 식의 결과값이 정수 or 문자열인 식을 사용
		switch (num) { // 식이 아니라도 변수 타입이 정수거나 문자열이면 사용 가능함
			// case 문을 사용하여 비교할 값일 직접 기술
			case 10 : System.out.println("num = 10 이다!");
			// 주의! num 과 일치하는 값이 10일 경우 case 10 뒤의 문장을 실행하는데
			// 이 때, case 10 뒤의 문장 실행 후 break 문이 없으므로
			// 다음 case 문부터 아래쪽 모든 case 문과 default 문까지 조건과 관계 없이 차례대로 실행함
			// => 단, 도중에 break 문을 만나거나 switch 문이 끝 중괄호를 만나면 종료됨
			case 9 : System.out.println("num = 9 이다!");
			case 8 : System.out.println("num = 8 이다!");
			case 7 : System.out.println("num = 7 이다!");
			case 6 : System.out.println("num = 6 이다!");
		}
		------------------------------------------------------------------------------------------------------------------------------
		num = 4;
		
		switch (num) {
			case 10 : 
				System.out.println("num = 10 이다!");
				System.out.println("break 문을 만남");
				break; // switch 문을 종료하고 빠져나감
			case 9 : 
				System.out.println("num = 9 이다!");
				System.out.println("break 문을 만남");
				break; // switch 문을 종료하고 빠져나감
			case 8 : 
				System.out.println("num = 8 이다!");
				System.out.println("break 문을 만남");
				break; // switch 문을 종료하고 빠져나감
			case 7 : 
				System.out.println("num = 7 이다!");
				System.out.println("break 문을 만남");
				break; // switch 문을 종료하고 빠져나감
			case 6 : 
				System.out.println("num = 6 이다!");
				break; // switch 문을 종료하고 빠져나감
			default :
				System.out.println("일치하는 case 문이 없을 경우");
				System.out.println("default 문을 찾아 실행함");
		}
```
