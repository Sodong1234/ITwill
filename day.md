# [오전수업]
**어제 수업 복습 겸 문제풀이 실시**

1. 초를 입력받아 "XX분 / XX초" 형태로 출력 (int a = 190;)

*나의 해답 : 

```
  		int a = 190;
  		int b = a/60;
				
  		System.out.println("b" + "분" + "/" + a + "초");
```

*정답 : 

```
		int a = 190;
		System.out.println((a / 60) + "분" + "/" + (a % 60) + "초");
```    
```
		int a = 190;
		int min = a / 60;
		int sec = a % 60;
		System.out.println(min + "분 / " + sec + "초");
``` 

2. 두 정수 x, y를 입력받아 x가 y보다 크거나 같으면(이상) 1, 작으면(미만) 0 을 출력  (int x = 10, y = 20;

*나의 해답 & 정답: 
```
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
```
		int num = -10;
		
		if (num < 0) {
			num = -num;
		} 
		System.out.println("변수 num = " + num);
```

문제 2. 문자 ch가 대문자일때, 소문자로 변환하여 출력 (출렉예시) "ch = a")
- ch가 대문자일 때 + 32 -> 소문자

```
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
```
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
```
		int num = 21;
		if (num%7 == 0) {
			System.out.println("multiple");
		} else {
			System.out.println("not multiple");
		}
    
		// 삼항연산자 사용
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
```
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
```
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

```
		int score = 77;
		String grade = "학점: ";
		
		
		
		if (90 <= score && score <= 100) grade += "A";
		else if (80 <= score)			 grade += "B";
		else if (70 <= score)			 grade += "C";
		else if (60 <= score)			 grade += "D";
		else 							      grade += "F";



    
    
