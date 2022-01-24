# [오전수업]
## 수업 들어가기에 앞서서 저번주 문제 복습 겸 강사님께서 9문제 출제.

```
1. 식별자 작성 규칙

필수규칙 : 
1. 첫 글자에 숫자 사용 불가 
2. 특수문자는 $와 _만 사용 가능 
3. 대소문자 구별 
4. 예약어 사용 불가 
5. 공백 사용 불가 

권장규칙:
1. 의미 있는 단어 조합 사용 
2. 두 단어 이상 조합 시, 두 번째 단어부터 이니셜은 대문자 
3. 길이 제한 없음. 
* 한글 사용 가능. 단, 보통 사용 안 함
-----------------------------------------------------------------------------
2. 14(10) -> ?(2)
1110 
-----------------------------------------------------------------------------
3. 10101000(2) -> ?(10)
168 
-----------------------------------------------------------------------------
4. 2^10 1024 
----------------------------------------------------------------------------
5. 16진수 한 자리는 어떻게 표기?
1~9, A~E X ***0~9, A~F***
-----------------------------------------------------------------------------
6. 이클립스와 같은 소프트웨어를 총칭?
? X IDE(Interate Development Environment, 통합 개발 환경)
------------------------------------------------------------------------------ 
7. 자바로 프로그램을 개발하기 위하여 반드시 설치해야 하는 프로그램
JDK(Java Development Kit, 자바 개발 도구)
1) JVM(Java Virtual Machine, 자바 가상 머신)
2) JRE(Java Runtime Environment, 자바 실행 환경)
->JVM 과 자바 실행에 필요한 도구
3) JDK(Java Development Kit, 자바 개발 도구)
-> JRE(JVM 포함) 와 자바 개발에 필요한 도구(컴파일러 등)를 포함
-----------------------------------------------------------------------------
8. 자바에서 사용하는 데이터타입
1. 정수형
-> 소수점이 없는 데이터, ex)short, long, byte, int
2. 실수형
-> 소수점이 있는 데이터, ex)float, double
3. 논리형
-> 참과 거짓 두 가지 값만을 가지는 데이터, ex) boolean(true, false)
4. 문자형
-> 한 가지 문자만을 가지는 데이터, ex) char
5. 기타(참조 데이터타입)
->string, 배열
-----------------------------------------------------------------------------
9. 작은따옴표와 큰따옴표의 용도
작은따옴표는 문자 데이터를 표현하며, 문자 1개를 반드시 포함해야함.
(공백도 가능하지만, 아무런 문자도 포함되지 않으면 안된다)
큰따옴표는 문자열 데이터를 표현하며, 0개 이상의 문자를 포함한
(아무런 문자열을 포함하지 않는 "" 형태의 데이터를 null string이라고 한다)

**5, 6번 문제 틀렸음**

```


---



# 자바 기본 데이터타입
## 1. 정수형 ((-2^(n-1) 부터 2^(n-1)-1 까지의 범위 표현 가능함. 이 때 n은 필요한 bit 수)
- byte : 1Byte 크기(= 8bit)를 갖는 데이터타입.
  - 1bit = 2진수 1자리이며, 8bit 는 2진수 8자리이므로 2^8 = 256 으로 총 256 가지의 경우의 수 표현 가능함
  - 양수와 음수 표현을 하므로, 256 / 2 = 128 가지의 경우의 수를 나타낼 수 있으며 -128 ~ +127 까지의 범위를 표현 가능함 (0은 양수로 취급)

```
//		byte 타입 변수 사용
		byte bNum = 10; // byte 타입 변수에 정수 10 저장(표현 가능 범위 내에 포함되므로 사용 가능)
		System.out.println(bNum); // 저장된 정수 10 출력
		
		bNum = 127;
		System.out.println(bNum);
		
		bNum = -128; 
		bNum = 128; // 오류! byte 타입 양수 최대값인 127 을 초과하므로 저장 불가
		bNum = -129; // 오류! byte 타입 음수 최대값인 -128을 초과하므로 저장 불가
```

- short : 2Byte 크기(= 16 bit) 를 갖는 데이터타입.
  - 16bit = 2^16 = 65536 / 2 = 32768
  - -32768 ~ +32767 까지의 범위를 표현 가능함.

``` 
//		short 타입 변수 사용
		short sNum = 10;
		System.out.println(sNum);
		
		sNum = 32767;
		System.out.println(sNum);
		
//		sNum = 32768; // 오류! 2byte 타입 양수 최대값인 32767 을 초과하므로 저장 불가
		sNum = -32768;
		System.out.println(sNum);
```

- int(중요!) : 4Byte 크기(= 32bit) 를 갖는 데이터타입
  - 2^32 = 약 42억 / 2 = 약 21억 
  - -21억 ~ +21억 까지의 범위를 표현 가능함.

```
//		int 타입 변수 사용
		int iNum = 2147483647;
		iNum = -2147483648;
//		iNum = 2147483648; // 오류! int형 양수 최대값 초과 -> 숫자 자체가 int 형의 범위를 벗어났음
//		(The literal 2147483648 of type int is out of range 에러)
//		즉, 해당 정수를 변수에 저장 할 수 없어서 생기는 문제가 아닌 숫자 자체의 표현에 대한 문제 발생!
//		int형 정수가 표현 가능한 범위는 2147483647 이며, 이를 벗어나는 범위는 int 값이 아니게 됨.
//		따라서, int형 범위를 벗어나는 정수는 반드시 long 타입으로 표기해야한다!
//		즉, 정수 뒤에 접미사 L을 붙여서 long 타입임을 표시해야함
		
//		iNum = 2147483648L; // 오류! long 타입 데이터는 int 타입에 저장이 불가능하므로 오류 발생!
//		(Type mismatch: cannot convert from long to int 에러)
```

- long : 8Byte 크기(= 64bit) 를 갖는 데이터타입
  - 약 -922경 ~ +922경까지 표현 가능 
  - 주의! long 타입으로 표현하는 정수는 숫자 뒤에 접미사 L 필수.


```
//		long 타입 변수 사용
		long lNum = 2147483648L; // 접미사 L 필수!
		System.out.println(lNum);    
```


**정수 데이터의 진법 표현 방법**
- 일반적인 정수는 10진수로 취급되며, 다른 진법의 데이터 표현 시 접두사 필요
  1) 2진수 : 0 또는 1 로만 구성된 데이터를 사용하며, 접두사 0b 붙임(binary)  
  2) 8진수 : 0 ~ 7 사이 범위 데이터를 사용하며, 접두사 0(octet의 의미 o는 생략) 
  3) 16진수 : 0 ~15 사이 범위 데이터(10 ~ 15까지 알파벳 A ~ F로 대체됨)를 사용하며, 접미사 0x(Hexadeciaml의 x) 사용

```
		int num = 0b1110; // 2진수 1110
		System.out.println(num); // 10진수 14로 변환되어 출력됨
			
		num = 0123; // 8진수 123
		System.out.println(num); // 10진수 83으로 변환되어 출력됨
		
		num = 0xA; // 16진수 A
		System.out.println(num); // 10진수 10으로 변환되어 출력됨
		
		num = 0b1234; // 오류! 0 또는 1만 사용 가능함
		num = 0789; // 오류! 0 ~ 7 사이의 범위 값만 사용 가능함
		num = 0xAG; // 오류! 0 ~ 9, A ~ F 사이의 값만 사용 가능함
```

## 2. 실수형(실수 타입은 표현 방식의 차이로 인해 정수 타입보다 훨씬 큰 숫자도 표현 가능함.)
     *실수를 나타낼때는 근사치 표현 방식에 의한 값으로 표현(정확한 값이 아닐 수 있음)

- float : 소수점 45 자리와 정수부분 38 자리까지 표현 가능. 단, 자바에서 float 타입은 소수점 7자리 정도까지 표현(근사치)
  - float 타입 실수 표현 시 실수 데이터 뒤에 접미사 F 또는 f 사용

```
//			float 타입 변수 사용
//			float fNum = 3.14; // 오류! 3.14는 double 타입이므로 float 타입에 저장 불가.
			float fNum = 3.14f;
			System.out.println(fNum); // 출력 시 접미사 f가 빠진 3.14만 표현됨.
```     
 
- double(기본) : 8Byte 크기를 갖는 데이터타입(long과 크기는 같으나 표현 범위가 전혀 다름).
  - 대략 300 자리 정도의 숫자까지 표현 가능하며, double 타입은 소수점 15자리 정도까지 표현(근사치)
  - double 타입 실수는 일반적인 실수로 표현하거나 접미사 D 또는 d 사용.  

``` 
//			double 타입 변수 사용
			double dNum = 3.14; // 실수 뒤에 접미사 생략 시 double 타입으로 취급됨.
			System.out.println(dNum);
			
			dNum = 3.123456789123456789;
			System.out.println(dNum); // 출력하면 3.123456789123457가 표현되는데, 근사치에 의해서 자동으로 반올림 된 것.
```      

## 3. 논리형 : boolean 타입을 사용하며, 특수한 값인true 또는 false 두 가지 값만 사용 가능.

```
//		논리형 타입 변수 사용
		boolean bool = true;
		System.out.println(bool);
		
		bool = false;
		System.out.println(bool);
		
		bool = FALSE; // 오류! ture와 false 값 외에는 사용 불가.
		bool = 1; // 오류! ture와 false 값 외에는 사용 불가.
```

## 4. 문자형
- 1개의 문자 데이터를 표현하는 데이터타입. 반드시 작은따옴표(' ') 사이에 1개의 문자가 포함되어야 함.
  - (두 개 이상의 문자 사용 불가, 아무것도 포함되지 않으면 안 됨)
- 기본적으로 표준 체계인 2Byte 유니코드(Unicode) 체계를 사용하여 전 세계의 모든 문자 표현
  - 문자 데이터를 2Byte 크기의 정수 형태로 관리  
    - (2Byte = 16bit = 2^16 = 65536 가지의 문자 데이터 표현 가능) 
    - 유니코드 내에 아스키 코드 및 한글, 한자 등의 다양한 국가의 문자가 모두 포함됨
  - 문자 데이터를 정수 형태로 변환하여 처리함
  - 자바에서 char 타입은 작은따옴표('') 를 사용한 표기법 외에도, 정수코드 값(0 ~ 65535)을 사용하거나 유니코드 표현법('\uXXXX')도 사용 가능
      - 유니코드 표현법의 XXXX 은 16진수 4자리(0000 ~ FFFF) 사용


### 컴퓨터의 문자 표현 방식
- ASCII(아스키) 코드
  - 7bit 코드(2^7 = 128 가지 문자 표현)
  - 2진수 0000000(0) ~ 1111111(127) 값 사용하여 표현

```		
		System.out.println('A');
		System.out.println('0');
		System.out.println('가');
		
//		System.out.println(''); // 오류! 반드시 1개 문자 포함 필수!
//		System.out.println('AB'); // 오류! 반드시 1개 문자만 사용 필수!
		
		char ch1 = 'A'; // 문자 'A' 를 저장하면 내부적으로 정수 65 로 변환되어 관리되며
		System.out.println(ch1); // 출력 시 대문자 'A' 출력됨
		
		char ch2 = 65; // 정수 65 를 저장하면 'A' 를 의미하는 코드로 취급되어
		System.out.println(ch2); // 출력 시 대문자 'A' 출력됨
		
		char ch3 = '\u0041'; // 유니코드 문자 '\u0041` 를 저장하면 내부적으로 정수 65로 변환되며
		System.out.println(ch3); // 출력 시 대문자 'A' 출력됨
		
		ch3 = '\uAC00'; // 한글 '가' 에 해당하는 유니코드
		System.out.println(ch3); // 출력 시 한글 '가' 출력됨

//		\ 기호나 ' 기호 등을 char 타입으로 표현해야하는 경우 
//		반드시 기호 앞에 \ 기호를 하나 붙여서 이스케이프 문자 형태로 해당 기호를 표현해야한다!
		System.out.println('\\'); // \ 기호를 문자로 취급
		System.out.println('\''); // \ 기호를 문자로 취급
    System.out.println('"');

```

### 문자열
- 기본 데이터타입이 아닌 참조 데이터타입에 해당하는 String 타입 사용
  - 주의! String 타입의 첫 글자는 대문자 S 사용 (키워드는 아니지만 약속된 이름)
  - 참조 데이터타입은 기본 데이터타입과 달리 모든 타입이 4Byte 크기를 갖는다.
- 0개 이상의 문자를 큰따옴표("") 로 둘러싸서 표현
  - "" 사이에 아무런 문자도 포함하지 않는 문자열을 널스트링(Null String) 이라고 함
- \ 기호를 활용한 이스케이프 문자도 표현 가능

```
	System.out.println("Hello, World!");
		
//		String 타입 변수도 기본 데이터타입 변수와 선언 방법 동일함
		String s1 = "String과 Char는 다르다!"; // 문자열을 String 타입 변수 s1에 저장
		System.out.println(s1);		
		
		String s2 = "A"; // char 타입 'A' 와 데이터 구조가 다르다!
		System.out.println(s2);
		
		String s3 = ""; // 널스트링(아무것도 포함하지 않는 "") 값 사용 가능
		System.out.println(s3); // 출력값에는 아무것도 보이지 않음
		
		String s4 = "\\"; // \ 기호를 나타내려면 \\ 형태로 표현
		System.out.println(s4);
		
		System.out.println("'");
		System.out.println("\""); // 큰따옴표를 표현하려면 \" 사용
		
		
		
//		Java Programming in "ITWILL" 출력해보기
		
		System.out.println("Java Programming\n in \"ITWILL\"");
```


![캡처](https://user-images.githubusercontent.com/95197594/150718700-dd282242-dcbf-4029-8294-4622f921b09c.JPG)

---

# [오후수업]

# 형변환
- 데이터 타입이 서로 다른 데이터를 연산하기위해 형변환이 발생
- 자동(묵시적, 암시적)형변환, 강제(명시적)형변환 이 있음.

      byte a = 10;
      int b = a;

## 자동 형변환
- 표현 범위가 좁은 데이터 타입에서 넓은 데이터 타입으로 변환
- byte -> short & char -> int -> long -> float -> double (정수(long, 8byte) -> 실수(float, 4byte))

		 int a = 3;
		 float b = 1.0F;
		 double c = a + b;
		
- a와 b를 더하기 위해서 형변환이 일어나야하는데 표현범위가 좁은 int가 float로 변환되어 계산된다. (이때 결과는 float)
- 연산 결과가 담겨질 변수 c의 타입은 double이고 double이 float보다 더 큰 범위를 표현할 수 있으므로 자동형변환이 발생

```
		int a1 = 128;
		byte a2 = 127;
		
		System.out.println("변환 전 a1(int) : " + a1 + ", a2(byte) : " + a2);
		
		// 묵시적 형변환 : 작은 데이터타입 -> 큰 데이터타입으로 변환(자동)
		a1 = a2;
		a1 = (int)a2; // 형변환 연산자를 사용해도 되지만, 자동으로 수행하기 때문에 생략 가능!
		System.out.println("변환 후 a1(int) : " + a1 + ", a2(byte) : " + a2);
```

## 강제 형변환
- 자동형변환이 적용되지 않는 경우! 수동으로(강제로) 형변환

		float a = 100.0; -> float a = (float)100.0;
		int b = 100.0F; -> int b = (int)100.0F;
		
```
		int b1 = 130;
		byte b2 = 127;
		System.out.println("변환 전 b1(int) : " + b1 + ", b2(byte) : " + b2);
		b2 = (byte)b1; // 명시적 형변환 (= 강제 형변환)
		System.out.println("변환 후 b1(int) : " + b1 + ", b2(byte) : " + b2);
//	변환에 성공하더라도 오버플로우(Overflow)에 의해 원본 데이터가 아닌 다른 데이터로 변형 될 수 있다
		
		System.out.println("============================================");
		
//	강제 형변환 후에도 오버플로우가 발생하지 않는 경우 -> 작은 데이터타입에 저장 가능한 데이터 범위인 경우 오버플로우 발생X
		int c1 = 10;
		byte c2 = 0;
		
		System.out.println("변환 전 c1(int) : " + c1 + ", c2(byte) : " + c2);
		c2 = (byte)c1; // 명시적 형변환 (= 강제 형변환)
		System.out.println("변환 후 c1(int) : " + c1 + ", c2(byte) : " + c2);
```

## 요약
- 자동 형변환은 좁은 → 큰 데이터 타입으로 변환되는 것으로 자연스럽게 변환할 수 있기 때문에 자동으로 형변환 가능!
- 강제 형변환은 큰 → 좁은 데이터 타입으로 변환되는 것으로 데이터 오버플로우(data overflow)가 발생할 수 있기 때문에 강제로 형변환 할것을 명시해야한다.

---

# 연산자
## 단항 연산자
1. 선행연산자 (전위연산자)
- 피연산자의 앞쪽(좌측)에 붙여서 값을 1 증가 또는 감소
- 먼저 피연산자의 값을 증가 또는 감소시킨 후에 다른 연산에 참여 
	- ex) ++i, --i

```
		int x1 = 5;
		int y1 = ++x1;
		
		int x2 = 5;
		int y2 = x2++;
		
		
		System.out.println("x1 : " + x1 + ", y1 : " + y1);
		System.out.println("x2 : " + x2 + ", y2 : " + y2);
		
		System.out.println("=================================");
		
		int i = 3;
		i++;					
		System.out.println(i);			//4
		++i;					
		System.out.println(i);			//5
		System.out.println(++i);		//6
		System.out.println(i++);		//6
		System.out.println(i);			//7
```


## 산술연산자 (+, -, *, /, %)
- 일반적인 사칙연산과 동일
- % 연산자 : 나머지 연산자(퍼센트 연산자) 라고 하며, 나눈 나머지 값을 반환

```
		System.out.println(10 + 2);
		System.out.println(10 - 2);
		System.out.println(10 * 2);
		System.out.println(10 / 3); // float나 double 연산자였으면 3.3333.. 으로 나왔겠지만 int형이기 때문에 몫인 3만 출력
		System.out.println(10 % 3); // 10을 3으로 나눈 나머지 1 출력
```

**산술 연산 시 자동 형변환**
- 산술 연산전에 데이터 타입이 다르면 산술 연산을 수행하기 전 피연산자 끼리의 데이터타입을 "일치" 시킨 후에 연산 수행
	- 규칙1. int 타입보다 작은 타입끼리의 연산은 모두 int 타입으로 변환 후 연산 수행 -> 따라서, 결과값은 무조건 int 타입이 됨
		- ex) byte + byte = (int)byte + (int)byte = 결과가 int 타입
		- ex) char + int = (int)char + int = 결과가 int 타입

	- 규칙2. int 타입보다 큰 타입과의 연산은 큰 타입으로 변환 후 연산 수행
		- ex) int + long = (long)int + long = 결과가 long 타입
		- ex) int + double = (double)int + double = 결과가 double 타입 
```
		byte b1 = 10, 
		     b2 = 20, 
		     b3;
				 
//		b3 = b1 + b2;			// 오류! byte + byte = int 이므로 byte 타입인 b3에 저장 불가!
		b3 = (byte)(b1 + b2);		// "연산 결과"에 형변환 연산자를 적용해서 byte 타입으로 변환
		System.out.println(b3);
		
		char ch = 'A';
		System.err.println(ch + 2);

		char ch2 = (char)(ch + 2);
		System.out.println(ch2);
		
		int i = 100;
		long l = 200;
//  		int i2 = i + 1; // 오류! int + long => long + long = long
		int i2 = (int)(i + l);
		System.out.println(i2);

		float f = 3.14F;
		System.out.println(l + f);
		long l2 = (long)(l + f); //  long + float => float + float = float
		System.out.println("==============================================");
		
		System.out.println((double)10 / 3);
		System.out.println(10 / 3.0); // == 10 / (double)3
		
		System.out.println((double)(10 / 3));
//		10 / 3 				int / int = int
//		(double)(10 / 3)	int / int = (double)int = (double)3
//		(double)10 / 3		double 10.0 / int = 10.3 / 3.0 = 3.3333
//		10 / (double)3		int / double 3.0 = double / double = 3.3333
		
		byte b1 = 10,
		     b2 = 20,
		     b3;
		
		b3 = 10 + 20;   // 상수(리터럴) 끼리의 연산은 형변환이 발생하지 않는다
		b3 = 100 + 28;  // 오류! 리터럴끼리의 연산이더라도 범위를 초과하면 오류 발생!
```

## 대입 연산자
- 우변의 데이터를 좌변의 변수에 대입(저장, 할당)
	- 연산의 방향이 좌<-우로 진행됨
		- ex) x = y = 3; y에 3을 대입하고, x에 y를 대입

**확장 대입연산자(=복합 대입연산자) (+=, -=, *=, /=, %=)**
- 대입연산자와 산술연산자가 조합된 연산자
- 좌변과 우변의 데이터끼리 산술연산을 먼저 수행한 후, 결과값을 좌변에 대입
	- ex) a = a + 1; -> a += 1;

```
		int a = 10, b = 0;
		b += a;
		System.out.println(b);
		// b + a의 결과를 다시 변수 b에 저장 -> b에 a값을(a만큼) "누적" 하는 것과 동일
		b += a;
		System.out.println(b);
		
		b -= a;
		System.out.println(b);
		
		b *= a;
		System.out.println(b);
		
		b /= a;
		System.out.println(b);
		
		b %= a;
		System.out.println(b);

		System.out.println("==================================");
		
		
		// 중요 포인트! 확장대입연산자는 "자동형변환이 일어나지 않는다"
		byte b1 = 10;
//		b1 = b1 + 10;
		b1 += 10;
		System.out.println(b1);
		
		
		char c = 'A';
//		c = c + 2;
		c += 2;
		System.out.println(c);
```

## 비교 연산자
- 두 피연산자 간의 대소관계 등을 비교하여 true 또는 false 값 반환 

```
		int a = 10, b = 5;
		boolean bool = a > b;
		System.out.println(bool);
		System.out.println("a > b 인가? " + bool);
		System.out.println("a > b 인가? " + (a > b));
		System.out.println("a < b 인가? " + (a < b));
		System.out.println("a >= b 인가? " + (a >= b));
		System.out.println("a <= b 인가? " + (a <= b));
		System.out.println("a == b 인가? " + (a == b));
		System.out.println("a != b 인가? " + (a != b));
		
		System.out.println("===============================");
		// char 타입을 비교연산자에 사용시 정수(유니코드) 값을 비교
		System.out.println('A' > 'B');
//		System.out.println("A" > "B"); // 문자열형(String) 끼리 대소관계 비교불가!
		System.out.println('A' == 65);
		
		System.out.println(3 == 3.0);
		System.out.println("===============================");
		
		System.out.println(0.1 == 0.1F);
		System.out.println((float)0.1 == 0.1F);
```

## 논리연산자(&&, ||, !, ^) (& Ampersend), (| Vertical bar), (^ Caret)
- 두 피연산자 간의 논리적인 판별을 수행하는 연산자
- 피연산자는 모두 boolean 타입 데이터만 사용 가능하며, 결과값도 boolean 타입으로 리턴

```
		 * < AND 연산자 (& 또는 &&) - 논리곱 >
		 *  - 두 피연산자 모두 true일 경우에만 결과가 true
		 *  - 하나라도 false일 경우 결과값이 false
		System.out.println("false && false = " + (a && a));
		System.out.println("false && true = " + (a && b));
		System.out.println("true && false = " + (b && a));
		System.out.println("true && true = " + (b && b));				
		
		 * < OR 연산자 (| 또는 ||) - 논리합 >
		 *  - 두 피연산자 중 하나라도 true일 경우 결과값이 true 이고, 모두 false일 경우에만 false
		System.out.println("false || false = " + (a || a));			
		System.out.println("false || true = " + (a || b));
		System.out.println("true || false = " + (b || a));
		System.out.println("true || true = " + (b || b));

		 * < NOT 연산자 (!) - 논리부정 >
		 *  - 단항 연산자로, 현재 값을 반대로 반전
		 *  - !T = F, !F = T
		System.out.println("!false = " + (!a));
		System.out.println("!true = " + (!b));
		
		 * < XOR 연산자 (^) - 배타적 논리합 >
		 *  - 두 피연산자가 서로 다를 때 true, 같으면 false
		 *  - F XOR F = F
		 *  - T XOR T = F
		 *  - T XOR F = T
		 *  - F XOR T = T
		System.out.println("false ^ false = " + (a ^ a));
		System.out.println("false ^ true = " + (a ^ b));
		System.out.println("true ^ false = " + (b ^ a));
		System.out.println("true ^ true = " + (b ^ b));
```

## 삼항 연산자 (조건 연산자)
- 연산에 참여하는 항이 3개인 연산자
- if ~ else 문과 동일한 기능을 수행
- 2가지 경우의 수 (true 또는 false) 에 대한 결과를 수행

		 < 기본 문법 >
		 연산식 ? 값1 : 값2
		 밥뭇나 ? 응 : 아니
		 -> 연산식에는 결과값이 boolean 타입 (true 또는 false)인 식만 올 수 있다.
		 -> 연산식 판별 결과가 true일 경우 값1, false일 경우 값2

```
		int a = 10;
		String result = a%2 == 0 ? "짝수" : "홀수";
		System.out.println(result);
		
		// 문제 1.
		a = 20;
		int b = 50;
		
		int max = a>b? a : b;
		int min = a>b? b : a;
		
		System.out.println("최대값 : " + max);
		System.out.println("최소값 : " + min);
		
		//문제 2. 반올림한 값을 출력 **카카오 입사문제 1번**
		double d = 95.7;
		
		if(d*10%10 >= 5) {
			d += 1;
		} 
		
		int result2 = (int)d;
		
		System.out.println(result2);
		
		-----------------------------------------------------
		
		double d = 95.7;
		
		int result2 = d*10%10 >= 5 ? (int)d+1 : (int)d;
		
		System.out.println(result2);
		
		
```
