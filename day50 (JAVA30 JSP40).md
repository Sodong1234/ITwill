# [오전수업] JAVA 30차
## 제네릭(Generic, 일반화)
- 객체에서 사용할 데이터타입을 클래스 정의 시 미리 명시하지 않고 실제 객체 생성 시 데이터 타입을 지정하여 사용하도록 하는 기법
  - 객체를 생성할 때마다 각각 다른 데이터타입으로 사용 가능함
  - 데이터타입을 지정 시 반드시 참조데이터타입만 지정 가능함 (기본형 사용 불가)
    - ex) int(X) -> Integer
- 데이터를 저장하는 시점에서 미리 저장 데이터에 대한 타입을 판별 가능하므로 데이터 저장시점에서 안정성을 향상시키고, 데이터를 꺼내는 시점에서 지정된 데이터타입만 저장된다는 보장이 생기므로 별도의 판별이나 형변환 없이 해당 타입 그대로 사용 가능하게 된다.
- 주로, Collection API 들은 대부분 제네릭 타입으로 클래스가 설계되어 있음
- 만약, 제네릭 타입 지정을 생략하는 경우 모든 데이터타입은 Object로 고정됨
```
< 제네릭 타입 지정을 통한 객체 생성 방법 >
클래스명<제네릭타입명> 변수명 = new 클래스명<제네릭타입명>();
```
```java
package Generic;

import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

public class Ex1 {

	public static void main(String[] args) {
		/*
		 * 제네릭(Generic, 일반화)
		 * - 객체에서 사용할 데이터타입을 클래스 정의 시 미리 명시하지 않고
		 * 	 실제 객체 생성 시 데이터 타입을 지정하여 사용하도록 하는 기법
		 * 	 => 객체를 생성할 때마다 각각 다른 데이터타입으로 사용 가능함
		 *   => 데이터타입을 지정 시 반드시 참조데이터타입만 지정 가능함 (기본형 사용 불가)
		 *   	ex) int(X) -> Integer
		 * - 데이터를 저장하는 시점에서 미리 저장 데이터에 대한 타입을 판별 가능하므로
		 *   데이터 저장시점에서 안정성을 향상시키고,
		 *   데이터를 꺼내는 시점에서 지정된 데이터타입만 저장된다는 보장이 생기므로
		 *   별도의 판별이나 형변환 없이 해당 타입 그대로 사용 가능하게 된다.
		 * - 주로, Collection API 들은 대부분 제네릭 타입으로 클래스가 설계되어 있음
		 * - 만약, 제네릭 타입 지정을 생략하는 경우 모든 데이터타입은 Object로 고정됨
		 * 
		 * < 제네릭 타입 지정을 통한 객체 생성 방법 >
		 * 클래스명<제네릭타입명> 변수명 = new 클래스명<제네릭타입명>();
		 * 
		 */
		
List list = new ArrayList();
		
		// Object 타입 파라미터를 갖는 add() 메서드는 어떤 데이터타입도 모두 추가 가능하므로
		// 데이터를 추가하는 시점에서는 매우 편리함
		list.add(1);
		list.add("TWO");
		list.add(3.14);
		
		
		for(int i=0; i<list.size(); i++) {
//			int data = list.get(i);	// 컴파일 오류 발생! (Object 타입 리턴 시 int 타입에 저장 불가)
//			int data = (int) list.get(i);	// 런타입 오류 발생!
			// => 첫번째 데이터는 정수이므로 int형으로 변환 가능하지만,
			//    두번째 데이터는 문자열이므로 int형으로 변환이 불가능함
			
			// 모든 데이터타입을 구분없이 저장하기 위해서는 Object 타입 변수 필요함
			Object data = list.get(i);
			
			// 어떤 문제도 발생하지 않도록 각 데이터타입에 맞는 변수에 저장하려면
			// 데이터를 꺼낼 때 타입 판별을 통해 알맞은 타입으로 저장해야한다!
			// => instanceof 연산자를 사용하여 각 타입 판별 후 변수에 저장
			//    각 타입으로 저장할 때 Object -> 각 타입으로 강제형변환 필수!
			if(data instanceof Integer) {	// 정수판별
				int iData = (int)data;
				System.out.println(iData);
			} else if(data instanceof String) {	// 문자열 판별
				String strData = (String)data;
				System.out.println(strData);
			} else if(data instanceof Double) {	// 실수 판별
				double dData = (double)data;
				System.out.println(dData);
			}
		}
		
		System.out.println("============================================");
		
		// 제네릭 타입을 지정하여 Collection Framework 객체들을 사용할 수 있다!
		// => 객체 생성 시점에서 저장될(사용될) 데이터타입을 지정하면
		//    해당 데이터타입만 사용 가능하도록 변경됨
		// => 클래스명 뒤에 <> 기호를 명시하고, 기호 사이에 사용할 데이터타입을 지정
		//    단, 데이터타입은 반드시 참조형 데이터타입만 지정 가능! (기본형 사용 불가)
		//    ex) int(X) -> Integer, char(X) -> Character
		
		// 제네릭 타입으로 정수를 사용하기 위해 Integer 타입을 지정 (list2)
		List<Integer> list2 = new ArrayList<Integer>();
		
		list2.add(1);
//		list2.add("TWO");	// 컴파일 에러 발생! Integer 타입(정수)만 추가 가능
//		list2.add(3.14);	// 컴파일 에러 발생! Integer 타입(정수)만 추가 가능
		list2.add(2);
		list2.add(3);
		
//		for(int i=0; i<list.size(); i++) {
//			// 반복문 등을 통해 데이터(요소)를 꺼낼 때
//			// 제네릭 타입으로 리턴되므로 별도의 형변환이나 데이터타입 검사가 불필요함
//			// => instanceof 불필요, 형변환 연산자 불필요
//			// => 특정 데이터타입만 저장된다는 보장이 생기므로 간편하게 사용 가능!
//			int data = list2.get(i);
//			System.out.println(data);
//		}
		
		for(int data : list2) {
			System.out.println(data);
		}
		
		System.out.println("--------------------------------");
		
		// ArrayList 객체 (list3) 생성 => 제네릭 타입을 String 으로 지정
		List<String> list3 = new ArrayList<String>();
		// => list3 객체의 대부분의 Object 타입 파라미터 또는 리턴타입 String 으로 변경됨

		// 데이터 몇건 추가
		list3.add("홍길동");
//		list3.add(1);	// 컴파일 에러 발생!
		
		// 향상된 for문으로 출력
		for(String data : list3) {
			System.out.println(data);
		}
		
		System.out.println("---------------------------------");
		
		// Map 계열 객체 생성 시 제네릭 타입 지정은 Key, Value 순으로 지정
		Map<Integer, String> map = new HashMap<Integer, String>();
		map.put(1, "홍길동");
//		map.put(2, 10);	// 컴파일 에러 발생! int, String 순으로 전달해야함!
		
		
		
		
		
	}

}
```

- 연습문제
```java
package Generic;

import java.util.ArrayList;
import java.util.List;

public class Test1 {

	public static void main(String[] args) {
		// Person 클래스 인스턴스 2개 생성
		Person p1 = new Person("홍길동", 20);
		Person p2 = new Person("이순신", 44);
		
		System.out.println(p1.toString());
		System.out.println(p2);
		
		// =========================================
		// Person 객체 여러개를 하나의 객체에 저장하여 관리할 경우
		// 1. Object[] 배열(Person[] 배열)을 통해 관리
		// => 배열은 크기가 불변이므로 추가적인 객체 저장 불가
		Object[] objArr = {p1, p2};	// Person -> Object 업새스팅 되어 관리됨
		
		for(int i = 0; i < objArr.length; i++) {
			Object o = objArr[i];
//			System.out.println(o.getName()); // 컴파일 에러 발생!
			
			// Object[] 배열 내의 Person 객체를 꺼내서 Person 타입 변수에 저장
			// Object -> Person 다운캐스팅 필수!
			Person p = (Person)objArr[i];
			
			System.out.println(p.getName() + ", " + p.getAge());
		}
		
		// 2. Collection(ArrayList)을 활용하여 Person 객체 여러개를 관리할 경우
		// => 배열의 단점인 크기 불변을 해결하게 되므로 여러 객체를 자유롭게 추가 가능
		//	  1) 제네릭을 사용하지 않을 경우
		List list = new ArrayList();
		
		list.add(p1);
		list.add(p2);
		list.add(new Person("강감찬", 35)); // 새로 추가되는 객체도 자동으로 확장되어 저장
		
		list.add("전지현"); // Person 객체가 아닌 데이터도 추가가 가능
		// => Person 객체 형태로 꺼내서 사용하는 시점에서 문제가 발생할 수 있다!
		
		for(Object o : list) {
			
			if(o instanceof Person) {
				Person p = (Person)o;
				System.out.println(p.getName() + ", " + p.getAge());
			} else {
				System.out.println("타입 불일치!");
			}
		}
	
		
		System.out.println("-----------------------------------------");
		
		// 2) 제네릭 타입을 사용할 경우
		//	   => 저장할 객체 타입이 Person 타입이므로 제네릭 타입 <Person> 지정
		//	   - 객체 저장 시 Person 타입 객체만 저장 가능하도록 자동으로 판별 수행
		//		 즉, 잘못된 객체(데이터)가 저장될 우려가 없다!
		//	   - Object 타입으로 업캐스팅 되지 않고 Person 타입 자체로 저장되기 때문에
		//	     객체(데이터)를 꺼내는 시점에도 Person 타입 그대로 꺼낼 수 있다!
		//		 => 별도의 다운캐스팅 등 형변환이 불필요!
		
		List<Person> list2 = new ArrayList<Person>();
		
		list2.add(p1);
		list2.add(p2);
		list2.add(new Person("강감찬", 35));
//		list2.add("전지현");	// 컴파일에러 발생!
		
		// 향상된 for문 사용 시 Person 타입 변수에 저장하는 작업 별도로 필요 없음
		for(Person p : list2) {
			System.out.println(p);
		}
		
		System.out.println("==============================================");
		
		// MemberBean 객체 3개 생성
		// 생성자에 아이디, 패스워드, 이름, 이메일, 주소, 전화번호, 휴대폰번호 전달
		// => 생성되는 3개의 객체를 ArrayList 객체에 추가(저장)
		//	  (ArrayList 객체 생성 시 제네릭 타입 지정)
		// 반복문을 통해 ArrayList 객체 내의 MemberBean 객체를 꺼내서 출력
		
		MemberBean member1 = new MemberBean("hong", "hong", "홍길동", "hong@hong.com", "부산", "0511111111", "01012345678");
		MemberBean member2 = new MemberBean("leess", "leess", "이순신", "leess@leess.com", "부산", "0512222222", "01022222222");
		MemberBean member3 = new MemberBean("kang", "kang", "강감찬", "kang@kang.com", "부산", "0513333333", "01033333333");
		
		List<MemberBean> memberList = new ArrayList<MemberBean>();
		memberList.add(member1);
		memberList.add(member2);
		memberList.add(member3);
		
		for(MemberBean bean : memberList) {
			System.out.println(bean);
		}

	}

}

/*
 * Person 클래스 정의
 * - 멤버변수(private) : 이름(name, 문자열), 나이(age, 정수)
 * - 생성자 : 이름과 나이를 전달받아 초기화하는 생성자
 * - toString() 메서드 오버라이딩 (이름과 나이를 결합하여 리턴)
 * - Getter/Setter 정의
 * 
 */

class Person {
	private String name;
	private int age;
	
	
	public Person(String name, int age) {
		this.name = name;
		this.age = age;
	}
	
	
	
	@Override
	public String toString() {
		return "Person [name=" + name + ", age=" + age + "]";
	}
	
	
	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}

	public int getAge() {
		return age;
	}

	public void setAge(int age) {
		this.age = age;
	}

	
}

// -----------------------------------------------------------

/*
 * MemberBean 클래스 정의
 * - 멤버변수(private) : 아이디(id, 문자열), 패스워드(pass, 문자열)
 * 					   이름(name, 문자열), 이메일(email, 문자열)
 * 					   주소(address, 문자열), 폰(phone, 문자열), 모바일(mobile, 문자열)
 * - 생성자 : 모든 멤버변수를 전달받아 초기화하는 생성자
 * - toString() 메서드 오버라이딩 (모든 멤버변수를 결합하여 리턴)
 * - 모든 멤버변수 Getter/Setter 정의
 */

class MemberBean {
	
	private String name;
	private String email;
	private String address;
	private String phone;
	private String mobile;
	private String pass;
	
	
	private String id;
	public MemberBean(String id, String name, String email, String address, String phone, String mobile, String pass) {
		this.id = id;
		this.name = name;
		this.email = email;
		this.address = address;
		this.phone = phone;
		this.mobile = mobile;
		this.pass = pass;
	}
		
	
	@Override
	public String toString() {
		return "MemberBean [id=" + id + ", pass=" + pass + ", name=" + name + ", email=" + email + ", address="
				+ address + ", phone=" + phone + ", mobile=" + mobile + "]";
	}
	
	
	public String getId() {
		return id;
	}
	public void setId(String id) {
		this.id = id;
	}
	public String getPass() {
		return pass;
	}
	public void setPass(String pass) {
		this.pass = pass;
	}
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public String getEmail() {
		return email;
	}
	public void setEmail(String email) {
		this.email = email;
	}
	public String getAddress() {
		return address;
	}
	public void setAddress(String address) {
		this.address = address;
	}
	public String getPhone() {
		return phone;
	}
	public void setPhone(String phone) {
		this.phone = phone;
	}
	public String getMobile() {
		return mobile;
	}
	public void setMobile(String mobile) {
		this.mobile = mobile;
	}
	
	
}
```


```java
package Generic;

import java.util.ArrayList;

public class Ex2 {

	public static void main(String[] args) {
		
		NormalIntegerClass nic = new NormalIntegerClass();
		nic.data = 10;	//	정수 저장 가능
//		nic.data = 3.14;	//	실수 저장 불가
//		nic.data = "홍길동";	//	문자열 저장 불가
		// => 여러 데이터타입 데이터를 모두 저장하려면 최소한 Object 타입 변수 선언 해야함
		
		// ----------------------------------------------------------------------
		NormalObjectClass noc = new NormalObjectClass();
		noc.data = 10;
		noc.data = 3.14;
		noc.data = "홍길동";
		// Object 타입 변수이므로 모든 데이터타입 저장이 가능
		// => 단, 객체 내의 데이터를 꺼내서 사용할 때 타입 판별이 필수
		if(noc.data instanceof Integer) {
			System.out.println((int)noc.data + 10);
		}
		
		// ===============================================
		
		// 제네릭을 사용한 클래스의 인스턴스 생성
		// => 제네릭 타입 지정 시 반드시 클래스 타입(참조형)으로 명시!
		// 1. 제네릭 타입 T를 Integer 타입으로 지정(대체)
		GenericClass<Integer> gc = new GenericClass<Integer>();
		// => GenericClass 클래스 내의 모든 T 타입이 Integer 타입으로 변경됨
		gc.setData(1);
//		gc.setData("홍길동"); // 컴파일에러 발생! Integer 타입만 전달 가능!
		
		System.out.println(gc.getData()); // 리턴타입이 Integer 타입으로 바뀌어 있음
		int num = gc.getData();
		System.out.println(num);
		
		// 2. 제네릭 타입 T를 Doublc 타입으로 지정(대체)
		GenericClass<Double> gc2 = new GenericClass<Double>();
		gc2.setData(3.14);
		double dNum = gc2.getData();
		System.out.println(dNum);
		
		// 3. 제네릭 타입 T를 String 타입으로 지정(대체)
		GenericClass<String> gc3 = new GenericClass<String>();
		gc3.setData("홍길동");
		String strName = gc3.getData();
		System.out.println(strName);
		
		// 4. 제네릭 타입 T를 Person 타입으로 지정
		GenericClass<Person> gc4 = new GenericClass<Person>();
		gc4.setData(new Person("홍길동", 20));
		Person p = gc4.getData();
		System.out.println(p.getName() + ", " + p.getAge());
		
		System.out.println("--------------------------------");
		
		GenericClass gc5 = new GenericClass();
		gc5.setData("전지현");
		GenericClass<Object> gc6 = new GenericClass<Object>();
		gc6.setData(1);
		GenericClass<String> gc7 = new GenericClass<String>();
		gc7.setData("강감찬");
		
		
		
		
		
		
		
		
	}

}
/*
 * 제네릭을 사용한 클래스 정의
 * - 클래스 정의 시점에서 클래스명 뒤에 <> 기호를 쓰고, 기호 사이에 '가상의 데이터타입' 명시
 * 	 => 보통 1글자 영문 대문자 사용(주로 E(Element), T(Type) 등을 사용)
 * 	 => 가상의 데이터타입이므로 실제 데이터타입으로 사용은 불가능하나
 * 		제네릭 타입에서 '임시'로 설정하여 관리함
 * - 지정된 가상의 자료형은 클래스 내부에서 실제 데이터타입 명시하는 부분에 대체가 가능함
 */

class GenericClass<T> {
	
	/*
	 * 제네릭 타입 T 지정 시 클래스 내의 데이터타입 부분을 T로 지정하여
	 * 임시 데이터타입으로 클래스 정의 가능 (실제 사용 가능한 데이터타입은 아니다!)
	 * => 차후, 객체 생성 시점에서 제네릭 타입에 대한 실제 데이터타입을 명시할 경우
	 * 	  현재 클래스 내의 제네릭
	 */
	private T data;

	public T getData() {
		return data;
	}

	public void setData(T data) {
		this.data = data;
	}
}







// 제네릭을 적용하지 않는 일반 클래스 정의
// 1) 사용할 데이터타입을 특정 타입으로 관리하는 일반 클래스
class NormalIntegerClass {
	int data;
	
	public int getData() {
		return data;
	}
	
	public void setData(int data) {
		this.data = data;
	}
	
}

// 2) 사용할 데이터타입을 Object 타입으로 관리하는 일반 클래스
class NormalObjectClass {
	Object data;	//	data 변수에는 모든 데이터타입 데이터저장 가능

	public Object getData() {
		return data;
	}

	public void setData(Object data) {
		this.data = data;
	}
	
	
}
```

### 제네릭 타입 사용 시 주의사항
1. static 멤버 내에서 제네릭 타입 파라미터 사용 불가!
  - 제네릭 타입은 인스턴스 생성 시점에서 데이터타입으로 변환되는데 static 멤버는 인스턴스 생성 시점보다 먼저(클래스 로딩시점) 로딩되므로 데이터타입이 저장되지 않은 상태이기 때문에 사용이 불가능함!
2. new 연산자 사용 시 제네릭 타입 파라미터 사용 불가
3. instanceof 연산자 사용 시 제네릭 타입 파라미터 사용 불가
```java
package Generic;

public class Ex3 {

	public static void main(String[] args) {
  
	}

}

class GenericClass2<T> {
	private T data;
//	private static T staticData;	// static 멤버변수에 제네릭 타입 파라미터 사용 불가!
//	T instance = new T();	// 인스턴 생성(new)시 제네릭 타입 파라미터 생성자 호출 불가!
	// => 컴파일 시점에서 생성자 타입이 확인 불가능하므로 사용할 수 없다!

	public T getData() {
		return data;
	}

	public void setData(T data) {
		this.data = data;
	}
	
//	public static void staticMethod(T data) {} // static 메서드에 제네릭 타입 파라미터 사용 불가!
	// => 인스턴스 생성 시점보다 먼저 메모리에 로딩되므로 타입 변경 불가능
	
	public void compare() {
		Object o = new Object();
//		if(o instanceof T) {	// instanceof 연산자에 제네릭타입 파라미터 사용불가!
//			// => 컴파일 시점에서 T의 데이터타입 확인이 불가능하므로
//			//    true, false를 미리 판별할 수 없으며, 형변환 등의 수행이 불가능
//		}
		
	}
	
	
	
}
```

```java
package Generic;

public class Ex4 {

	public static void main(String[] args) {

		SubClass<Integer, String, Object> sc = new SubClass();
		sc.var1 = 1;		// Integer
//		sc.var1 = "홍길동";
		
		sc.var2 = "홍길동";	// String
//		sc.var2 = 10;
		
		sc.var3 = 3.14;		// Object
		sc.var3 = "전지현";
		
		// -------------------------------------------------------
		
		GenericClass3<Integer> gc;
//		GenericClass3<String> gc2;	// Number 계열이 아니므로 지정 불가!
		GenericClass3<Double> gc3;
//		GenericClass3<Object> gc4;	// Number 의 상위타입이므로 지정 불가!
		
	}

}

// --------------------------------------
// 제네릭 타입의 상속과 구현
class Class1<P>{}
interface Interface1<Q>{}

// 부모 타입에 제네릭 타입이 지정되어 있을 경우
// 서브클래스에서 상속 받을 때 부모의 타입 파라미터를 서브클래스 타입파라미터로 명시 해야한다!
class SubClass<P, Q, R> extends Class1<P> implements Interface1<Q> {
	// => Class1<P>, Interface<Q> 을 상속받으려면 최소한 SubClass 뒤에 P와 Q를 명시 필수!
	//    또한, 서브클래스 자신만의 제네릭 타입도 추가할 수 있다!
	P var1;	// 슈퍼클래스 Class1의 타입 P
	Q var2;	// 슈퍼클래스 Interface1의 타입 Q
	R var3;	// 자신의 타입 R
}

/*
 * 제네릭 타입에 대한 사용 가능한 파라미터 타입 제한
 * - 제네릭 타입 파라미터 선언 시 Object 타입과 그 자식 타입들 모두 사용 가능
 * - 필요에 따라 파라미터 타입에 올 수 있는 데이터타입을 제한할 수 있음
 * 
 * < 기본 문법 >
 * - 파라미터에 대한 서브클래스 타입으로 제한하는 경우
 *   class 클래스명<타입파라미터 extends 클래스타입> {}
 *   => 타입파라미터(ex. E 또는 T등)는 extends 뒤의 클래스 타입이거나 하위타입만 지정 가능
 * 
 * */
class GenericClass3 <E extends Number> {}
```

---

# [오후수업] JSP 40차

## 정규표현식(Regular Expression, Regex 또는 Regexp)
- 문자열을 처리하기 위한 패턴 기반의 문자열(또는 식)
- 정규표현식 패턴을 통해 처리할 문자열을 지정하고 특정 클래스 등을 사용하여 정규표현식에 해당하는지 여부를 체크
	- ex) 패스워드 검사, 회원가입란의 전화번호나 이메일 양식 검사 등
		- (= 유효성 검사 = Validation check 라고 함)
- 프로그램 개발 뿐만 아니라 네트워크, 웹 등에서 공용으로 사용 가능한 표준 표현 방법
	- (프로그램 언어도 자바 뿐만 아니라, 자바스크립트 등 다양한 언어에서 활용 가능)

< 정규표현식에 사용되는 패턴 문자 - 메타 문자(Meta character) >
- x 또는 y 라는 임의의 문자를 기준으로 해당 문자를 기준으로 앞 뒤에 기호를 붙여 패턴 정의

```
< 일반적인 기호 >
1. ^x : x로 시작하는 문자열
  ex) "x", "xa", "xxx", "xyz" 등... => 패턴과 일치하는(부합하는) 문자열(사용 가능)
      "y", "ya", "yyy", "yzx" 등... => 패턴과 일치하지 않는 문자열(사용 불가능)
  ex2) x가 숫자하는 의미일 때 : "숫자admin"(O), "숫자123"(O), "admin숫자"(X)

2. x$ : x로 끝나는 문자열
  ex) "x", "ax", "xxx", "zyx" 등... => 사용 가능
      "y", "xa", "xxy", "xyz" 등... => 사용 불가능
--------------------------------------------------------------
x로 시작해서 x로 끝나는 문자열(= x만 존재해야하는 경우)
=> ^x$ : "x"(O), "xy"(x), "ax"(X), "x1"(X)
--------------------------------------------------------------
3. .x : x앞에 1개의 문자가 포함되는 문자열
  ex) "ax", "bx", "abxy" => 사용 가능(어떤 위치든 x문자 앞에 한 글자가 존재)
      "xa", "xb", "xyz"  => 사용 불가능

4. x+ : x가 한 번 이상 반복되는 문자열
  ex) "x", "xx", "xxxx" 등...

5. x* : x가 0번 이상 반복되는 문자열(없을 수도 있음)
  ex) "", "a", "b", "x", "xxx" 등... => 사용 가능(단독으로 사용 시 아무 문자열이나 모두 해당)
      => 따라서, 주로 다른 패턴들과 결합하여 사용

6. x? : x가 나올 수도 있고, 나오지 않을 수도 있는 문자열
  ex) "", "a", "b", "x", "xxx" 등... => 사용 가능(단독으로 사용 시 아무 문자열이나 모두 해당)
      => 따라서, 주로 다른 패턴들과 결합하여 사용

7. x|y : x 또는 y 가 포함되는 문자열
  ex) "x", "y", "xy", "xyz" 등... => 사용 가능
```	
