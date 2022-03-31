# [오전수업] JSP 38차
> 개인 프로젝트용 홈페이지 구현작업 실시. repositories에 파일을 업로드해놓음.

---

# [오후수업] JAVA 28차
## 컬렉션 프레임워크(Collection Framework)
- 컴퓨터 시스템에서 데이터를 효율적으로 저장 및 관리하는 방법 - 자료구조론
  - 자바에서 자료구조를 구현하여 제공하는 클래스들의 모음
	- 기타 대부분의 언어들은 자료구조를 개발자가 직접 구현해야하지만 자바는 클래스 내의 메서드를 통해 자료구졸르 활용할 수 있도록 지원
- java.util 패키지에 클래스 및 인터페이스 형태로 제공됨
- 컬렉션 3대 인터페이스 : Set, List, Map
	- 각 인터페이스를 구현한 구현체 클래스들이 제공됨
	- 이 중, Set과 List 계열은 Collection 인터페이스를 공통으로 상속받았으므로 대부분의 메서드가 동일함

### Set 계열
- 저장 순서가 유지되지 않음(인덱스 사용 불가)
- 데이터 중복을 허용하지 않음(중복 데이터는 저장되지 않음)
  - 아주 효율적인 중복제거 수단으로 사용됨
- Set 계열의 구현체 클래스 : HashSet, TreeSet 등

```java
package collection;

import java.util.Arrays;
import java.util.HashSet;
import java.util.Set;
import java.util.TreeSet;

public class Ex1 {

	public static void main(String[] args) {
	
		// HashSet 객체 생성
//		Hashset set = new HashSet(); // 일반적인 HashSet 객체 생성 방법
		// HashSet -> Set 업캐스팅하여 사용하는 방법
		// => 대부분의 메서드를 부모 인터페이스인 Set인터페이스가 보유중이므로
		//	  업캐스팅 상태에서도 대부분의 기능을 사용하는데 문제가 없음
		Set set = new HashSet();
		
		// Set 객체의 메서드
		// boolean isEmpty() : 컬렉션 객체가 비어있는지 판별
		System.out.println("Set 객체가 비어있는가? " + set.isEmpty());
		
		// int size() : 컬렉션 객체 내의 요소(데이터) 갯수 리턴
		System.out.println("Set 객체에 저장된 요소 갯수 : " + set.size());
		
		// String toString() : 컬렉션 객체 내의 모든 요소를 문자열료 리턴(오버라이딩)
		System.out.println("Set 객체의 모든 요소 : " + set.toString());
		System.out.println("Set 객체의 모든 요소 : " + set);	// 생략가능
		
		System.out.println("----------------------------------------");
		
		// boolean add(Object e) : 요소 1개(e)를 컬렉션에 추가 후 추가 성공여부 리턴
		// => 파라미터가 Object 타입이므로 모든 데이터타입 추가 가능(기본형, 객체 모두 가능)
		// => 리턴타입이 boolean 이므로 요소 추가 성공 여부 리턴됨(중복데이터는 추가X)
		set.add(1);
		set.add("TWO");
		set.add(3.14);
		
		System.out.println("Set 객체가 비어있는가? " + set.isEmpty());
		System.out.println("Set 객체에 저장된 요소 갯수 : " + set.size());
		System.out.println("Set 객체의 모든 요소 : " + set);
		// => 저장 순서가 유지되지 않으므로 출력되는 데이터의 순서는 다를 수 있다!
		
		System.out.println("실수 3.14 추가 가능한가? " + set.add(3.14));
		System.out.println("문자 '4' 추가 가능한가? " + set.add(4));
		System.out.println("Set 객체의 모든 요소 : " + set);
		
		System.out.println("--------------------------------------------");
		
		System.out.println("Set 객체의 실수 3.14가 포함되어 있는가? " + set.contains(3.14));
//		System.out.println("Set 객체의 실수 3.14가 포함되어 있는가? " + set.contains("3.14"));
		System.out.println("Set 객체에 정수 3이 포함되어 있는가? " + set.contains(3));
		
		// boolean remove(Object o) : 특정 요소(o)를 컬렉션 객체에서 제거
		System.out.println("Set 객체 내의 실수 3.14 삭제 : " + set.remove(3.14));
		System.out.println("Set 객체 내의 실수 3.14 삭제 : " + set.remove(3.14));
		System.out.println("Set 객체의 모든 요소 : " + set);
		
		Object[] objArr = set.toArray();
		System.out.println(Arrays.toString(objArr));
		System.out.println(objArr[0]);
		
		// addAll()
		Set set2 = new HashSet();
		set2.add(9); set2.add(10); set2.add(1);
		
		System.out.println("Set2 객체에 set 객체 모두 추가 : " + set2.add(set));
		System.out.println("Set2 객체의 모든 요소 : " + set2);
		System.out.println("Set2 객체에 set 객체 모두 추가 : " + set2.add(set));

		// clear() : 컬렉션 내의 모든 요소 초기화(제거)
		set2.clear();
		System.out.println("Set2 객체의 모든 요소 : " + set2);
		
		// HashSet 등 컬렉션 객체 생성 시 파라미터로 다른 컬렉션 객체를 전달하면
		// 해당 컬렉션 객체의 요소를 갖는 새로운 컬랙션 객체가 생성됨
		Set set3 = new HashSet(set);
		System.out.println("Set3 객체의 모든 요소 : " + set3);
		System.out.println("Set 객체의 모든 요소 : " + set);
		
		System.out.println("set과 set3 객체는 동일한 객체인가? " + (set == set3));
		// boolean equals(Collection c) : 컬렉션 요소가 동일한지 판별
		System.out.println("set과 set3 객체는 동일한 객체인가? " + set.equals(set3));
		
		System.out.println("===========================================");
		
		// TreeSet 객체를 활용하면 같은 타입 데이터가 저장된 Set 객체 정렬 가능
		// => 주의! 반드시 같은 타입 데이터만 저장해야한다!
		// => 이진 탐색 트리(Binary Search Tree)를 개량한 레드-블랙 트리(Red-Black Tree) 구조 사용
		// => 수치 데이터는 수치의 크기 순으로 오름차순 정렬되며(0 -> 9)
		//	  문자데이터는 문자 코드값의 크기 순으로 오름차순 정렬되므로
		//	  수치 데이터와 문자 데이터의 정렬 결과는 다를 수 있다!
		Set set4 = new HashSet();
		
		set4.add(100);
		set4.add(99);
		set4.add(500);
		set4.add(2);
		set4.add(35);
		set4.add(999);
		System.out.println(set4);
		
		Set<Integer> set5 = new TreeSet(set4);
		System.out.println(set5); // [2, 35, 99, 100, 500, 999] => 항상 결과 같음(정렬)
		
		System.out.println("=======================================");
		
		/*
		 * Set 계열의 모든 요소를 반복문을 통해 출력
		 * => 인덱스 사용이 불가능하므로 일반 for문을 통해 접근 불가능
		 * => 대신, 향상된 for문을 통해 저장된 요소를 차례대로 접근 가능
		 * 
		 */
		for(int i : set5) {
			System.out.println(i);
		}
		
	}
}
```

연습문제 (로또 번호 )
```java
package collection;

import java.util.Random;
import java.util.TreeSet;

public class Test1 {

	public static void main(String[] args) {
		/*
		 * Set 계열 컬렉션 객체를 사용하여 로또 번호 생성 프로그램 작성
		 * 1) 1 ~ 45 사이의 중복되지 않는 숫자 6개를 Set 객체(myLotto)에 저장 후 출력
		 * 	 => 이 때, 저장되는 숫자는 오름차순 정렬 수행
		 * 	 ex) 나의 로또 번호 : [1, 10, 30, 35, 40, 44]
		 * 
		 * 2) 1등 당첨 번호를 저장하는 Set 객체 (thisWeekLotto) 생성
		 * 	 => 저장할 번호 : 7, 3, 21, 12, 40, 33
		 * 	 => 이 때, 저장되는 숫자는 오름차순 정렬 수행
		 * 	 ex) 1등 당첨 번호 : [3, 7, 12, 21, 33, 40]
		 * 
		 * 3) 자신의 로또 번호와 1등 당첨 번호를 비교하여 일치하는 번호 갯수 출력하고
		 * 	  일치하는 번호 갯수에 따른 결과 출력
		 * 	  ex) 일치 갯수 0개(꽝)
		 * 		  일치 갯수 1개(꽝)
		 * 		  일치 갯수 2개(꽝)
		 * 		  일치 갯수 3개(4등)
		 * 		  일치 갯수 4개(3등)
		 * 		  일치 갯수 5개(2등)
		 * 		  일치 갯수 6개(1등)
		 * 			
		 * 
		 */
		Random r = new Random();
		
		int winCount = 0;
		
		while(true) {
			TreeSet myLotto = new TreeSet();
			
			while(myLotto.size() < 6) {
				myLotto.add(r.nextInt(45) + 1);
			}
			
			System.out.println("나의 로또 번호 : " + myLotto);
			
//			2) 1등 당첨 번호를 저장하는 Set 객체 (thisWeekLotto) 생성
//			    => 저장할 번호 : 7, 3, 21, 12, 40, 33
			TreeSet thisWeekLotto = new TreeSet();
			thisWeekLotto.add(7);
			thisWeekLotto.add(3);
			thisWeekLotto.add(21);
			thisWeekLotto.add(12);
			thisWeekLotto.add(40);
			thisWeekLotto.add(33);
			
			System.out.println("1등 당첨 번호 : " + thisWeekLotto);
			
			int count = 0;
			for(Object o : thisWeekLotto) {
				if(myLotto.contains(o)) count++;
			}
			
			String result = "";
			switch (count) {
			case 6: result = "1등"; break;
			case 5: result = "2등"; break;
			case 4: result = "3등"; break;
			case 3: result = "4등"; break;
			default: result = "꽝";
			}
			
			System.out.println("일치하는 번호 갯수 : " + count + "개(" + result + ")");
			
			winCount++;
			
			if(count == 6) {
				break;
			}
			
			System.out.println("-----------------------------------------------------");
		}
		
		System.out.println("1등 당첨까지 걸린 추첨 횟수 : " + winCount + "회");
		
	}

}

```

### 2. List 계열
- 인덱스 번호를 사용하여 저장 순서가 유지됨
- 데이터 중복 허용
- 배열과 유사하나, 배열과는 달리 저장 공간이 자동으로 확장됨
- List 계열의 구현체 클래스 : ArrayList, Vector, LinkedList 등
- 기본적인 메서드 대부분 Set 계열과 동일함(= 부모가 동일하기 때문에)

```
- 기본적인 구조가 동일하며, 메서드가 동일함
- ArrayList 와 Vector 가 다른 점은 Vector의 경우멀티쓰레드 환경에서 안전하게 객체를 사용할 수 있음 (쓰레드 나중에 배움)
	 => ArrayList는 멀티쓰레드 환경을 지원하지 않음

< ArrayList vs LinkedList >
- 기본적인 구조가 완전 다르며, 메서드는 동일함
- ArrayList는 배열 구조로써 인덱스를 활용하므로 데이터 탐색이나 순차적인 추가/삭제에 빠르다
- LinkedList는 다음 데이터의 위치를 현재 데이터가 갖고 있는 형태이며 항상 시작점부터 순차적으로 다음 데이터를 추적해가는 형식의 구조
  => 중간 데이터 추가/삭제는 빠르지만, 데이터 탐색이나 순차적인 작업은 느림
```

```java
package collection;

import java.util.ArrayList;
import java.util.Arrays;
import java.util.Collections;
import java.util.HashSet;
import java.util.List;
import java.util.Set;

public class Ex2 {

	public static void main(String[] args) {
		
		List list = new ArrayList();	// ArrayList -> List 업캐스팅 가능
		
		list.add("ONE");
		list.add(2);
		list.add(3.14);
		
		System.out.println("list 객체가 비어있는가? " + list.isEmpty());
		System.out.println("list 객체에 저장된 요소의 갯수는? " + list.size());
		System.out.println("list 객체에 모든 요소 출력 : " + list); // toString() 생략
		
		// list 객체의 중복 허용 확인
		System.out.println("중복 데이터 3.14 추가 가능한가? " + list.add(3.14));
		
		System.out.println("list 객체가 비어있는가? " + list.isEmpty());
		System.out.println("list 객체에 저장된 요소의 갯수는? " + list.size());
		System.out.println("list 객체에 모든 요소 출력 : " + list);	// toString() 생략
		
		// add(int index, Object e)
		list.add(2, 3); // 기존 2번 인덱스의 데이터를 밀어내고 정수 3을 2번 인덱스에 삽입
		System.out.println("list 객체에 모든 요소 출력 : " + list);
		System.out.println("-----------------------------");
		
		System.out.println("3번 인덱스 요소 : " + list.get(3));
//		System.out.println("5번 인덱스 요소 : " + list.get(5));	 // 오류 발생!
		
		// List 객체의 모든 요소 꺼내기
		// 일반 for문 사용하여 List 객체의 get() 메서드로 인덱스를 통해 접근
		// => 특정 인덱스 범위 반복이 가능하다는 장점이 있음 (배열 접근 방법과 동일)
		// => 0번 인덱스 부터 List크기-1 까지 반복
		for(int i = 0; i < list.size(); i++) {
			System.out.println(list.get(i));
		}
		
		for(Object o : list) {
			System.out.println(o);
		}
		
		System.out.println("----------------------------------");
		
		// Object remove(int index) : index에 해당하는 요소 제거(제거되는 요소 리턴됨)
		// boolean remove(Object o) : o에 해당하는 객체 제거(제거될 경우 true 리턴됨)
		
		// 정수 2를 지정하는 것이 아닌 2번 인덱스 지정으로 취급됨
		// 따라서, 정수 2를 지정하여 삭제해야하는 경우 Object 타입으로 형변환 필요
		System.out.println("정수 2를 저장하여 해당 요소 직접 삭제 : " + list.remove((Object)2));
		System.out.println("list 객체에 모든 요소 출력 : " + list );
		
		// Object set(int index), Object o) : 지정된 인덱스의 데이터를 교체(덮어쓰기)
		// => 덮어쓰기 위해 제거되는 요소가 리턴됨
		System.out.println("3번 인덱스 요소를 문자 '4'로 교체 : " + list.set(3, 4));
		System.out.println("list 객체에 모든 요소 출력 : " + list );
		
		// List sublist(int fromIndex, int toIndex) : 
		List subList = list.subList(1, 3);
		System.out.println("1번 인덱스부터 3번 인덱스보다 작은 부분 리스트 추출 : " + subList);

		System.out.println("----------------------------------------------------------");
		//Object[] toArray() : List 객체의 모든 요소를 Object[] 배열로 리턴
		Object[] arr = list.toArray();
		for(Object o : arr) {
			System.out.println(o);
		}
		System.out.println("---------------------------------------------------");
		
		list.add(3);
		System.out.println("list 객체에 모든 요소 출력 : " + list );

		System.out.println("3이라는 요소의 인덱스 번호 : " + list.indexOf(3));
		System.out.println("3이라는 요소의 인덱스 번호 : " + list.lastIndexOf(3));
		
		// 존재하지 않는 요소 지정할 경우 인덱스 번호 -1 리턴(데이터 없음)
		System.out.println("10이라는 요소의 인덱스 번호 : " + list.indexOf(10));
		
		System.out.println("실수 3.14가 존재 하는가? " + list.contains(3.14));
		System.out.println("실수 5.0이 존재 하는가? " + list.contains(5.0));
		
		System.out.println("========================================");
		
		List list2 = new ArrayList(list);
		System.out.println("list2 객체의 모든 요소 출력 " + list2);
		System.out.println("list와 list2 객체는 동일한 객체인가? " + (list == list2));
		System.out.println("lost와 list2 객체는 요소가 동일한 객체인가? " + list.equals(list2));
		
		System.out.println("========================================");
		
		// Arrays 클래스 asList() 메서드 호출하여 데이터를 연속적으로 전달하면
		// 해당 데이터들을 List 타입 객체로 변환해준다.
		List arrayList = Arrays.asList(1, 2, 3, 4); // 가변인자
		System.out.println(arrayList);
		
		// 주의! 기본 데이터타입 배열 자체를 asList() 메서드 파라미터로 전달하면
		// 정상적인 변환 불가능(asList() 메서드 파라미터로 클래스타입 배열 전달해야한다!)
		// => 오류는 발생하지 않으나 배열 데이터를 정상적으로 전달 불가능
//		int[] iArr = {1, 2, 3, 4, 5, 6};
//		List arrayList2 = Arrays.asList(iArr);
//		System.out.println(arrayList2);
//		
//		for(int i = 0; i < arrayList2.size(); i++) {
//			System.out.println(arrayList2.get(i));
//		}
		
		// -> int 타입 대신 클래스타입인 Integer 사용할 경우 사용 가능
		
		Integer[] iArr = {1, 2, 3, 4, 5, 6};
		List arrayList2 = Arrays.asList(iArr);
		System.out.println(arrayList2);
		
		String[] strArr = {"1", "2", "3"};
		List arrayList3 = Arrays.asList(strArr);
		System.out.println(arrayList3);
		
		System.out.println("===================================");
		
		// ArrayList(list3) 객체 생성하고, 정수(3, 4, 1, 6, 5, 2) 추가
		List list3 = new ArrayList();
		list3.add(3);
		list3.add(4);
		list3.add(1);
		list3.add(6);
		list3.add(5);
		list3.add(2);
		System.out.println("정렬 전 : " + list3);
		
		// Collections 클래스의 static 메서드 sort() 사용 시 List 객체 정렬 가능
		Collections.sort(list3);
		System.out.println("정렬 후 : " + list3);
		
		// Collections 클래스의 static 메서드 shuffle() 사용 시 List 객체 뒤섞기 가능 
		Collections.shuffle(list3);
		System.out.println("셔플 후 : " + list3);
		
		// -----------------------------------------------------------
		
		// 참고! TreeSet 객체에 저장된 요소를 List 객체로 변환하여 shuffle 하거나
		// HashSet 객체에 저장된 요소를 List 객체로 변환하여 sort 가능
		
//		Set set = new HashSet();
//		set.add(1);
//		set.add(20);
//		set.add(3);
//		set.add(450);
//		set.add(55);
//		System.out.println(set);
		
		// SEt 객체 생성 시 Arrays.asList() 메서드를 통해 List 객체를 생성한 후
		// 해당 객체를 Set 객체 생성자 파라미터로 전달해도 된다!
		// (Set <-> List 상호 변환 가능)
		Set set = new HashSet(Arrays.asList(1, 20, 3, 450, 55));
		System.out.println("set 객체 : " + set);
		
		List list4 = new ArrayList(set);
		System.out.println("List 객체 : " + list4);
		Collections.sort(list4);
		System.out.println("List 객체 정렬 후 : " + list4);
		

		

	}

}
```

연습문제 (트럼프카드)

```java
package collection;

import java.util.ArrayList;
import java.util.Arrays;
import java.util.Collections;
import java.util.List;

public class Test2 {

	public static void main(String[] args) {
		/*
		 * 트럼프 카드 구현을 위해 List 객체 사용
		 * - ArrayList 객체(cards)를 생성하여
		 * 	 기호 ▲, ◆, ♥, ♣ 와 숫자 2~9, 영문자 A, J, Q, K 를 조합하여 카드 생성
		 */
		String[] marks = {"♠", "◆", "♥", "♣"};
		String[] numbers = {"A", "2", "3", "4", "5", "6", "7", "8", "9", "J", "Q", "K"};
				
		List cards = new ArrayList();
		
		/*
		 * 반복문을 사용하여 숫자와 마크를 중첩으로 반복(총 52개의 트럼프 카드 생성)
		 * => 마크 + 숫자를 결합한 문자열을 ArrayList 객체 (cards)에 추가
		 * 	  ex) "♠" 마크와 숫자 "2" 를 결합하여 "♠2" -> cards에 추가
		 * 		  "♥" 마크와 숫자 "Q" 를 결합하여 "♥Q" -> cards에 추가
		 * 
		 * => 최종적으로 생성된 트럼프 카드 목록(cards)을 출력
		 * 
		 */
		for(String mark : marks) {
			for(String number : numbers) {
				cards.add(mark + number);
			}
		}
		
		System.out.println(cards);
		
		
		System.out.println("=======================================");
		
		// 생성된 트럼프 카드
		Collections.shuffle(cards);
		System.out.println(cards);
		
		System.out.println("=======================================");
		
		// 3명의 플레이어가 카드 5장을 가진다고 가정할 때 String[] 배열 3개 생성
		String[] player1 = new String[5];
		String[] player2 = new String[5];
		String[] player3 = new String[5];
		
		// get() 메서드를 사용하여 카드를 전달하는 경우 = 카드가 제거되지 않고 전달됨
//		player1[0] = cards.get(0).toString();
//		player2[0] = cards.get(1).toString();
//		player3[0] = cards.get(2).toString();
//		
//		System.out.println(player1[0]);
//		System.out.println(player2[0]);
//		System.out.println(player3[0]);
//		System.out.println(cards);

		// remove 사용
//		player1[0] = cards.remove(0).toString();
//		player2[0] = cards.remove(0).toString();
//		player3[0] = cards.remove(0).toString();
//		player1[1] = cards.remove(0).toString();
//		player2[1] = cards.remove(0).toString();
//		player3[1] = cards.remove(0).toString();
//		player1[2] = cards.remove(0).toString();
//		player2[2] = cards.remove(0).toString();
//		player3[2] = cards.remove(0).toString();
//		player1[3] = cards.remove(0).toString();
//		player2[3] = cards.remove(0).toString();
//		player3[3] = cards.remove(0).toString();
//		player1[4] = cards.remove(0).toString();
//		player2[4] = cards.remove(0).toString();
//		player3[4] = cards.remove(0).toString();
		
		for(int i = 0; i < player1.length; i++) {
			player1[i] = cards.remove(0).toString();
			player2[i] = cards.remove(0).toString();
			player3[i] = cards.remove(0).toString();
		}
		
		System.out.println("플레이어 1의 카드 : " + Arrays.toString(player1));
		System.out.println("플레이어 2의 카드 : " + Arrays.toString(player2));
		System.out.println("플레이어 3의 카드 : " + Arrays.toString(player3));
		System.out.println("전달하고 남은 카드들 : " + cards);

		
	}

}
```

