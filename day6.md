# [오전수업]
## 직업기초수업 실시 (직업흥미&적성검사, 자소서첨삭, 취업관련 정보 등을 수업)

---
# [오후수업]
## 전 날 실시한 switch-case 문의 보충수업 실시

ex) switch-case 문을 활용하여 등급 정수값(level)에 따른 등급명칭을 출력하기
```
		// level 이 1일 경우 : "일반 회원" 출력
		// level 이 2일 경우 : "우수 회원" 출력
		// level 이 3일 경우 : "VIP 회원" 출력
		
		
		int level = 3;
		
		switch (level) {
			case 1 : 
				System.out.println("일반 회원");
				break;
			case 2 :
				System.out.println("우수 회원");
				break;
			case 3 :
				System.out.println("VIP 회원");
		}
		----------------------------------------------------------------------------------------------
		//grade 가 "일반" 일 경우 "5% 할인쿠폰 증정" 출력
		//grade 가 "우수" 일 경우 "10% 할인쿠폰 증정" 출력
		//grade 가 "VIP" 일 경우 "20% 할인쿠폰 증정" 출력
		
		
		String grade = "VIP";
		
		switch (grade) {
			case "일반" : System.out.println("할인율은 5% 입니다."); break;
			case "우수" : System.out.println("할인율은 10% 입니다."); break;
			case "VIP" : System.out.println("할인율은 20% 입니다.");
		}
		-----------------------------------------------------------------------------------------------
		//grade 가 "일반" 일 경우 "5% 할인쿠폰 증정" 출력
		//grade 가 "우수" 일 경우 "10% 할인쿠폰 증정", "5% 할인쿠폰 증정" 출력
		//grade 가 "VIP" 일 경우 "20% 할인쿠폰 증정", "10% 할인쿠폰 증정", "5% 할인쿠폰 증정" 출력
		
		 * break 문의 생략을 활용한 코드 중복 제거
		 * - 적절한 위치에서의 break 문 생략은 코드 중복을 제거하는 효율적인 수단이 된다.
		 
		 방법1) 
		 switch (grade) {
			case "일반" : System.out.println("5% 할인쿠폰 증정"); break;
				    
			case "우수" : System.out.println("5% 할인쿠폰 증정"); 
			 	     System.out.println("10% 할인쿠폰 증정"); break;
				     
			case "VIP" : System.out.println("5% 할인쿠폰 증정"); 
				     System.out.println("10% 할인쿠폰 증정");
				     System.out.println("20% 할인쿠폰 증정"); break;
			}

		 방법2)
		 switch (grade) {
			case "VIP" : System.out.println("20% 할인쿠폰 증정");
			// => VIP 의 경우 우수와 일반의 혜택도 모두 받기 때문에, break 문 없이 맨 위에서부터 실행하면 됨
			case "우수" : System.out.println("10% 할인쿠폰 증정"); 
			// => 우수의 경우 VIP 혜택을 제외하고, 우수와 일반 혜택을 받으므로, 두 번째 case 문에서 실행하면 됨
			case "일반" : System.out.println("5% 할인쿠폰 증정"); 
			// => 일반의 경우 자신의 혜택만 받기 때문에 가장 마지막 case 문으로 실행
		  }
				     
		 
		 
```
ex) 입력받은 월(month)에 따른 그 달의 일수 출력하기
- 1, 3, 5, 7, 8, 10, 12월 : "xx월은 31일까지 입니다"
- 4, 6, 9, 11월 : "xx월은 30일까지 입니다"
- 2월 : "xx월은 28일까지 입니다"

```
방법1)
		int month = 1; // 월을 입력받았다고 가정
		
		//switch (month) {
//			case 1 : System.out.println("1월은 31일까지 입니다"); break;
//			case 2 : System.out.println("2월은 28일까지 입니다"); break;
//			case 3 : System.out.println("3월은 31일까지 입니다"); break;
//			case 4 : System.out.println("4월은 30일까지 입니다"); break;
//			case 5 : System.out.println("5월은 31일까지 입니다"); break;
//			case 6 : System.out.println("6월은 30일까지 입니다"); break;
//			case 7: System.out.println("7월은 31일까지 입니다"); break;
//			case 8 : System.out.println("8월은 31일까지 입니다"); break;
//			case 9 : System.out.println("9월은 30일까지 입니다"); break;
//			case 10 : System.out.println("10월은 31일까지 입니다"); break;
//			case 11 : System.out.println("11월은 30일까지 입니다");break;
//			case 12 : System.out.println("12월은 31일까지 입니다"); break;	


	----------------------------------------------------------------------------------
방법2)
		switch (month) {
			case 1 : 
			case 3 : 
			case 5 : 
			case 7: 
			case 8 : 
			case 10 : 
			case 12 : System.out.println(month + "월은 31일까지 입니다"); break;
			
			case 4 : 
			case 6 : 
			case 9 : 
			case 11 : System.out.println(month + "월은 30일까지 입니다");break;
			
			case 2 : System.out.println(month + "월은 28일까지 입니다"); break;
		}
```
## switch-case 문의 취약점
- switch-case문은 리터럴(정수, 문자열)만 사용 가능하기 때문에 범위로는 사용이 어려움
  - switch-case는 연산자 사용 불가( && 등등) 

ex)
```
    // if문을 사용하여 점수(score)가 90 이상이고 100 이하일 경우 "A학점" 출력하고,
		// 아니면, 점수(score)가 80 이상이고 89 이하일 경우 "B학점" 출력
		// 아니면, 점수(score)가 70 이상이고 79 이하일 경우 "C학점" 출력
		// 아니면, 점수(score)가 60 이상이고 69 이하일 경우 "D학점" 출력
		// 아니면, 점수(score)가 0 이상이고 59 이하일 경우 "F학점" 출력
		// 그 외의 모든 점수는 "점수 입력 오류!" 출력
		
		if (score>=90 && score<=100) {
			System.out.println("A학점");
		} else if (score>=80 && score<=89) {
			System.out.println("B학점");
		} else if (score>=70 && score<=79) {
			System.out.println("C학점");
		} else if (score>=60 && score<=69) {
			System.out.println("D학점");
		} else if (score>=0 && score<=59) {
			System.out.println("F학점");
		} else {
			System.out.println("점수 입력 오류!");
		}
    
    -----------------------------------------------------------------------------
    
// switch-case문인 경우
		switch (score) {
			case 100 : 
			case 99 :
			case 98 :
			case 97 :
			// ... 생략 ...
			case 90 : System.out.println("A학점"); break;
			case 89 :
			case 88 : 
			// ... 생략 ...
			case 80 : System.out.println("B학점"); break;
			// ... 생략 ...
		}
    
    // but 위와 같은 코드는 점수(score)를 10을 나눈 몫을 계산하면 각 점수대를 판별 할 수 있으며 해당 점수대에 맞는 학점을 판별 가능
    
		switch(score / 10) {
			case 10 :
			case 9 : System.out.println("A학점"); break;
			case 8 : System.out.println("B학점"); break;
			case 7 : System.out.println("C학점"); break;
			case 6 : System.out.println("D학점"); break;
			case 5 :
			case 4 :
			case 3 :
			case 2 :
			case 1 : 
			case 0 : System.out.println("F학점"); break;
      default : System.out.println("점수입력오류!");
    }  
    
    // but 위와 같은 코드는 case10의 경우 100~109 점까지 모두 해당되므로 올바르지 않은 점수도 A학점이 출력됨.
    // 이와 같은 문제는 if문을 통해 먼저 올바른 범위(0 ~ 100)의 점수인지 판별한 후 올바른 점수일 때만 switch 문을 실행하도록 하면 오류를 제거 할 수 있다!
    
    
    
		
		if (score>=0 && score<=100) {
			switch(score / 10) {
			  case 10 :
			  case 9 : System.out.println("A학점"); break;
			  case 8 : System.out.println("B학점"); break;
			  case 7 : System.out.println("C학점"); break;
			  case 6 : System.out.println("D학점"); break;
			  case 5 :
			  case 4 :
			  case 3 :
			  case 2 :
			  case 1 : 
			  case 0 : System.out.println("F학점"); break;
			
		}
		
	} else {
		System.out.println("점수입력오류!");
	}
	
  
  // if문의 도움으로 0 ~ 100 사이만 처리되므로 A ~ D 학점을 제외한 나머지는 모두 F학점이다.
  
  
  
  	
		if (score>=0 && score<=100) {
			switch(score / 10) {
			  case 10 :
			  case 9 : System.out.println("A학점"); break;
			  case 8 : System.out.println("B학점"); break;
			  case 7 : System.out.println("C학점"); break;
			  case 6 : System.out.println("D학점"); break;
			  default : System.out.println("F학점"); break;
			
		}
		
	} else {
		System.out.println("점수입력오류!");
	}
```

연습문제1) 나이(age)에 따른 10대, 20대, 30대, 40대, 그 외를 판별하기
- 정수형 변수 age에 임의의 나이 저장
- if문을 사용하여 나이대 판별
  - 2-1) 10대 : 나이(age)가 10 이상이고 20 미만(또는 19 이하) => "10대입니다" 출력
  - 2-2) 20대 : 나이(age)가 20 이상이고 30 미만(또는 29 이하) => "20대입니다" 출력
  - 2-3) 30대 : 나이(age)가 30 이상이고 40 미만(또는 39 이하) => "30대입니다" 출력
  - 2-4) 40대 : 나이(age)가 40 이상이고 50 미만(또는 49 이하) => "40대입니다" 출력
  - 2-5) 그 외 나머지 : "기타" 출력

```
		int age = 49;
		if (age>=10 && age<=49) {
			switch (age / 10) {
				case 1 : System.out.println("10대입니다"); break;
				case 2 : System.out.println("20대입니다"); break;
				case 3 : System.out.println("30대입니다"); break;
				case 4 : System.out.println("40대입니다"); break;
			}
		} else {
			System.out.println("기타");
		}
```

연습문제2) 나이(age)에 따른 놀이동산 할인 조건 판별하기
- 정수형 변수 age에 임의의 나이 저장
- if문을 사용하여 나이대 판별
  - 2-1) 5세 미만이거나 65세 이상일 경우 : "무료입장 대상입니다!" 출력
  - 2-2) 5세 이상 19세 이하일 경우 : "50% 할인 대상입니다!" 출력
  - 2-3) 그 외 나머지 : "할인 대상이 아닙니다!" 출력

```
		age = 65;
		
		if (age<5 || age>=65) {
			System.out.println("무료입장 대상입니다!");
		} else if (age>=5 && age<=19) {
			System.out.println("50% 할인 대상입니다!");
		} else {
			System.out.println("할인 대상이 아닙니다!");
		}
```

연습문제3) 앞의 과정에서 
- int pee = 50000; 값 부여받고 
- System.out.println("최종 입장료는 " + pee + "원 입니다!"); 출력하기

```
		age = 65;
	  int pee = 50000;
    
		if (age<5 || age>=65) {
			System.out.println("무료입장 대상입니다!");
      pee = 0;
		} else if (age>=5 && age<=19) {
			System.out.println("50% 할인 대상입니다!");
      pee = (int)(pee * 0.7); // int * double = double 이므로 int 타입으로 강제 형변환 필수!
		} else {
			System.out.println("할인 대상이 아닙니다!");
		}

      System.out.println("최종 입장료는 " + pee + "원 입니다!");

```
