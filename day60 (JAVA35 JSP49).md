# [오전수업] JAVA 35차

### StringBuilder & StringBuffer
- StringBuffer  : 문자열이 안전하게 변경되도록 보장(O)
- StringBuilder : 문자열이 안전하게 변경되도록 보장(X)
  - (멀티 쓰레드 프로그램 아니라면 StringBuilder 속도가 좀 더 빠름)

```java

public class Ex1 {

	public static void main(String[] args) {
		
		String javaStr = new String("Java");
		// System.identityHashCode 인스턴스가 처음 생성되었을 때 메모리 주소
		System.out.println("javaStr 문자열 주소 : " + System.identityHashCode(javaStr));
		
		StringBuilder buffer = new StringBuilder(javaStr);
		System.out.println(buffer);
		System.out.println("연산 전 buffer 메모리 주소 : " + System.identityHashCode(buffer));
		
		// 문자열 추가
		buffer.append(" and");
		buffer.append(" android");
		buffer.append(" programming is fun!!");
		System.out.println(buffer);
		System.out.println("연산 후 buffer 메모리 주소 : " + System.identityHashCode(buffer));
		
		javaStr = buffer.toString();
		System.out.println(javaStr);
		System.out.println("새로 만들어진 문자열 주소 : " + System.identityHashCode(javaStr));

		System.out.println("---------------------------------------------------");
		
		String str1 = "abc";
		String str2 = str1 + "de";
		String str3 = "abc";
		
		System.out.println(System.identityHashCode(str1));
		System.out.println(System.identityHashCode(str2));
		System.out.println(System.identityHashCode(str3));
		
		if(str1 == str3) {
			System.out.println("주소가 같다!");
		} else {
			System.out.println("주소가 다르다!");
		}
		
		System.out.println("-----------------------------------");
		
		String str = "abc";
		
		System.out.println("변경 전 주소 : " + System.identityHashCode(str));
		
		str += "de";
		
		System.out.println("변경 후 주소 : " + System.identityHashCode(str));
		
		int a = 10;
		System.out.println(System.identityHashCode(a));
		a = 20;
		System.out.println(System.identityHashCode(a));

	}

}
```


---

# [오후수업] JSP 49차
