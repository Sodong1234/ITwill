# [오전수업] JAVA 24차


## String 클래스
```java
package lang;

public class Ex1 {

	public static void main(String[] args) {
		
		// 리터럴을 사용하여 문자열을 생성하는 방법 (가장 많이 사용)
		// => 문자열을 상수 풀(Constant Pool)
		//	  중복되는 문자열이 있을 경우 해당 문자열의 주소값을 리턴
		String s1 = "홍길동";		// 처음 상수 풀에 저장되므로 "홍길동" 문자열이 생성됨
		String s2 = "홍길동";		// 기존 상수 풀에 저장된 문자열이 존재하므로
								// s1이 가리키는 주소와 동일한 주소값을 리턴하여 s2에 저장
		// => 따라서, 두 문자열을 저장하는 변수 s1, s2는 가리키는 주소값이 같다!
		
		if(s1 == s2) {
			System.out.println("s1과 s2는 주소값이 같다!");
		} else {
			System.out.println("s1과 s2는 주소값이 다르다!");
		}
		
		// Object 클래스로부터 상속받아 오버라이딩 된
		// String 클래스의 equals() 메서드 사용하여 비교하는 방법
		if(s1.equals(s2)) {
			System.out.println("s1과 s2는 문자열이 같다!");
		} else {
			System.out.println("s1과 s2는 문자열이 다르다!");
		}
		
		
		// 객체 생성을 통해 문자열을 생성하는 방법
		// => 인스턴스 생성을 위해 new 연산자 사용 시 무조건 Heap 공간에 새로운 메모리 할당
		//	  따라서, 동일한 문자열이라도 서로 다른 공간에 저장됨
		String s3 = new String("홍길동");
		String s4 = new String("홍길동");
		
		// 동등비교연산을 통해 비교하는 방법 = 인스턴스 주소값 비교
		// => new 연산자로 인해 서로 다른 공간에 문자열이 생성되므로 주소값이 다르다!
		if(s3 == s4) {
			System.out.println("s3 와 s4는 주소값이 같다!");
		} else {
			System.out.println("s3 와 s4는 주소값이 다르다!");
		}
		
		// Object 클래스로부터 상속받아 오버라이딩 된 
		// String 클래스의 equals() 메서드 사용하여 비교하는 방법
		// => 주소는 달라도 저장된 문자열은 동일하므로 문자열이 같다!
		if(s3.equals(s4)) {
			System.out.println("s3 와 s4는 주소값이 같다!");
		} else {
			System.out.println("s3 와 s4는 주소값이 다르다!");
		}
	}

}
```

### String 클래스의 메서드
- 문자열 수정, 검색, 치환 등 다양한 작업을 수행하는 메서드를 제공
- String 객체는 불변 객체이므로 원본 문자열에 대한 수정이 불가능하며, 수정, 치환 등 변경 작업 수행 시 원본 데이터가 아닌 변경된 문자열에 대한 새로운 문자열을 생성하여 관리
  - => 따라서, 문자열 수정이 빈번할 경우 메모리 낭비가 심하므로 StringBuilder 또는 StringBuffer 객체를 활용 

```java
package lang;

import java.util.Arrays;

public class Ex2 {

	public static void main(String[] args) {
		
		String s1 = "Java Programming";
		String s2 = "			아이티윌		부산 교육센터		";
		String s3 = "자바/JSP/안드로이드";
		
		// length() : 문자열 길이 리턴
		System.out.println("s1.length() : " + s1.length());
		
		// equals() : 문자열 비교 (대소문자 구별하여 비교)
		System.out.println("s1.equals(JAVA PROGRAMMING) : " + s1.equals("JAVA PROGRAMMING"));
		// equalsIgnoreCase() : 문자열 비교 (대소문자 구별하지 않고 비교)
		System.out.println("s1.equalsIgnoreCase(JAVA PROGRAMMING) : " + s1.equalsIgnoreCase("JAVA PROGRAMMING"));
		
		// charAt() : 특정 인덱스에 위치한 문자 1개 리턴
		System.out.println("s1.charAt(5) : " + s1.charAt(5));
		
		// substring() : 특정 범위 문자열(부분 문자열)을 추출
//		1) substring(int beginIndex) : 시작 인덱스로부터 모든 문자열을 추출
		System.out.println("s1.substring(5) : " + s1.substring(5));
		// => 5번 인덱스 위치가 대문자 P이므로 P부터 끝까찌 모든 문자열(Programming) 추출됨
		
		// 2) substring(beginIndex, int endIndex) : 시작인덱스부터 끝인덱스-1까지 추출
		System.out.println("s1.substring(5, 12) : " + s1.substring(5, 12));
		
		// concat() : 문자열 결합(결합연산자(+) 보다 연산 속도가 빠르다)
		System.out.println("s1.concat(!) : " + s1.concat("!"));
		
		// indexOf() : 문자열의 앞쪽(첫인덱스)부터 찾고자 하는 문자 또는 문자열의 인덱스 리턴
		// => 탐색할 문자 또는 문자열이 없을 경우 -1 리턴
		System.out.println(s1.indexOf('a')); 		// 앞에서부터 문자 'a' 를 찾아 인덱스(1) 리턴
		System.out.println(s1.indexOf('x'));		// 'x'는 존재하지 않으므로 -1 리턴됨
		System.out.println(s1.indexOf("a"));		// 앞에서부터 문자열 "a"를 찾아 인덱스(1) 리턴
		System.out.println(s1.indexOf("Program"));	// 문자열의 시작위치 5 리턴
		
		// lastIndexOf() : 문자열 뒷쪽(끝인덱스) 부터 찾고자 하는 문자 또는 문자열의 인덱스 리턴
		System.out.println(s1.lastIndexOf("a")); 	// 10 리턴됨
		// => "Java Programming" 문자열에서 뒷쪽부터 "a" 를 탐색할 경우 10번 인덱스의 a를
		//	  먼저 만나므로 해당 인덱스(10)를 리턴
		// => indexOf()를 사용하면 Java의 두 번째 글자 "a" 탐색되므로 인덱스 1 리턴됨
		
		// replace() : 특정 문자 또는 문자열에 대한 치환 기능을 제공
		// => 동일한 문자 또는 문자열이 여러개 있을 경우 모두 치환
		System.out.println(s1.replace('a', '@'));	// char 타입 또는
		System.out.println(s1.replace("a", "@"));	// String 타입 모두 사용 가능
		System.out.println(s1.replace("Java", "Android"));
		
		// toUppercase(), toLowerCase() : 알파벳(영문자) 대소문자 변환 기능
		System.out.println(s1.toUpperCase());
		System.out.println(s1.toLowerCase());
		
		// trim() : 문자열 앞뒤 공백을 제거 (주의! 문자열 사이의 공백은 제거하지 않음
		// => 문자열 입력 시 의도치 않은 불필요한 공백이 삽입되는 것을 방지할 때 사용
		System.out.println("교육기관은 " + s2.trim() +"입니다.");
		
		// ----------------------------------------------------------------
		// split() : 문자열을 특정 기준으로 분리하여 분리된 문자열을 "배열"로 리턴
		// String s3 = "자바/JSP/안드로이드";
		String[] subject = s3.split("/");
		// => 분리된 각각의 문자열은 String[] 타입으로 리턴되어 배열로 관리됨
		for(int i = 0; i < subject.length; i++) {
			System.out.println(subject[i]);
		}
		
		// 위의 for문을 향상된 for문으로 작성
		for(String str : s3.split("/")) {
			System.out.println(str);
		}
		
		// Arrays 클래스의 toString() 메서드를 활용하여 데이터를 꺼내는 경우
		System.out.println(Arrays.toString(subject));
		
		// split() 메서드 사용하여 문자열 분리할 때 주의사항
		// => 문자열의 구분자로 마침표(.)를 사용할 경우
		//	  "." 기호 대신 "\\." 형태로 사용해야함
		// => 만약, "." 기호를 구분자로 지정하면 정규표현식에서는
		//	  모든 문자를 기준으로 삼기 때문에 모든 문자가 기준이 되어 모든 문자가 제거됨
		String s4 = "안녕하세요. 자바 프로그래밍 수업입니다.";
		String[] strArr = s4.split(".");
		System.out.println(Arrays.toString(strArr));
		
		String[] strArr2 = s4.split("\\.");
		System.out.println(Arrays.toString(strArr2));
		
		// ----------------------------------------------------------------
		// contains() 메서드 : 특정 문자열 포함 여부 확인(boolean 타입 리턴)
		System.out.println("문자열 자바가 포함되어 있는가? " + s1.contains("자바"));
		System.out.println("문자열 자바가 포함되어 있는가? " + s1.contains("Java"));

		// ----------------------------------------------------------------
		// String.format() 메서드 : 특정 문자열을 형식 지정 문자와 결합하여 형식을 부여
		// => System.out.printf() 메서드 역할과 동일하나 출력을 위한 것이 아니라
		//	  문자열을 생성할 때 형식을 지정하기 위함
		String name = "홍길동";
		int age = 20;
		double height = 180.5;
		// => 위의 세가지 데이터를 다음과 같이 문자열로 결합하여 변수 total에 저장
		System.out.println("이름 : " + name + ", 나이 : " + age + ", 키 : " + height);
		
		// println() 메서드 대신 printf() 메서드 사용할 경우
		System.out.printf("이름 : %s, 나이 : %d, 키 : %.2f\n", name, age, height);
		
		String formatStr = String.format("이름 : %s, 나이 : %d, 키 : %.2f\n", name, age, height);
		System.out.println("생성된 회원 정보는 " + formatStr);
		
		// String 클래스는 불변(final char[]) 객체이므로 원본 문자열은 변경되지 않는다!
		// => 항상 변경 결과를 새로운 문자열로 생성하게 되기 때문
		System.out.println(s1);
		System.out.println(s2);
		System.out.println(s3);

		
		
	}

}
```

- 연습문제
```java
package lang;

import java.util.Arrays;
import java.util.Scanner;

public class Test2 {

	public static void main(String[] args) {
		/*
		 * 주민등록번호(jumin)를 문자열로 입력받아 성별(남 또는 여) 판별
		 * 입력 형식 : "XXXXXX-XXXXXXX"
		 * 판별 조건
		 * 1) 뒷자리 첫번째 숫자가 1 또는 3 : "남성" 출력
		 * 2) 뒷자리 첫번째 숫자가 2 또는 4 : "여성" 출력
		 * 3) 뒷자리 첫번째 숫자가 5 또는 6 : "외국인" 출력
		 * 
		 */
//		Scanner sc = new Scanner(System.in);
//		String jumin = sc.nextLine();
		
		String jumin = "921020-1234567";
		
//		String[] strArr = jumin.split("-");
//		System.out.println(Arrays.toString(strArr));
//		String str = strArr[1];
//		System.out.println(strArr[1]);
//		char ch = str.charAt(0);
//		System.out.println(ch);
		
		// 위 문장을 한 줄로 표현
		char ch = jumin.split("-")[1].charAt(0);
		
		String result = "";
		
		if(ch == '1' || ch == '3') {
			result = "남성";
		} else if (ch == '2' || ch == '4') {
			result = "여성";
		} else if (ch == '5' || ch == '6') {
			result = "외국인";
		}

		System.out.println(result);
		
		System.out.println("--------------------------------------");
		
		// switch - case
		switch (ch) {
		case '1' : 
		case '3' : result = "남성"; break;
		case '2' : 
		case '4' : result = "여성"; break;
		case '5' : 
		case '6' : result = "외국인"; break;
		}
		
		System.out.println(result);
		
		System.out.println("--------------------------------------");
		
		// 주소 부분만 출력 (부산광역시 부산진구 동천로:109)
		String data1 = "Adress:부산광역시 부산진구 동천로:109";
//		String data1 = "addr:부산광역시 부산진구 동천로:109";
//		System.out.println("Address:", ""));
//		System.out.println(data1.substring(data1.indexOf(":"+1));	// 0
//		String[] str = data1.split(":");
//		System.out.println(str[1] + str[2]);
		
		// 이름 부분만 출력 (홍길동)
		String data2 = "이름:홍길동, 나이:20";
		System.out.println(data2.substring(data2.indexOf(":") + 1 , data2.indexOf(",")).trim());
		System.out.println(data2.split(",")[0].split(":")[1].trim());
		
		// 주소 부분만 출력 (서울특별시 용산구 24번길, 210동 702호)
		String data3 = "Adress:서울특별시 용산구 24번길, 210동 702호, Tel:0518030909";
		System.out.println(data3.substring(data3.indexOf(":") + 1, data3.lastIndexOf(",")));
		
		// 필요한 데이터만 추출 (부산광역시 부산진구 동천로109) (7층) (051-803-0909)
		String data4 = "Address:부산광역시 부산진구 동천로109, Floor:7층, Tel:051-803-0909";
		String[] strArr = data4.split(",");
		for(String s : strArr) {
			System.out.println(s.split(":")[1].replace("-", "").trim());
		}
		
		// 필요한 데이터만 추출(부산광역시 부산진구 동천로 24번길, 109호) (7층) (0518030909)
		String data5 = "Address:부산광역시 부산진구 동천로 24번길, 109호, Floor:7층, Tel:051-803-0909";
		int index = data5.lastIndexOf(",");
		System.out.println(data5.substring(index + 1).split(":")[1].replace("-", ""));
		data5 = data5.substring(0, index);
		
		index = data5.lastIndexOf(",");
		System.out.println(data5.substring(index + 1).split(":")[1]);
		data5 = data5.substring(0, index);
		
		System.out.println(data5.split(":")[1]);
	}

}

```

# [오후수업] JSP 33차
