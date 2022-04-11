# [오전수업] DB 17차
> 16차 수업 복습 실시

```
last_name컬럼의 오른쪽첫자리를 추출한 문자열이 'n'과 같은 행 출력
SELECT employee_id, CONCAT(first_name, last_name) name,
LENGTH(last_name), INSTR(last_name, 'a') "Contains 'a'?"
FROM employees
WHERE SUBSTR(last_name, -1, 1) = 'n';

EMPLOYEE_ID|NAME            |LENGTH(LAST_NAME)|Contains 'a'?|
-----------+----------------+-----------------+-------------+
        102|LexDe Haan      |                7|            5|
        105|DavidAustin     |                6|            0|
        110|JohnChen        |                4|            0|
        112|Jose ManuelUrman|                5|            4|
        123|ShantaVollman   |                7|            6|
        130|MozheAtkinson   |                8|            0|
        132|TJOlson         |                5|            0|
        133|JasonMallin     |                6|            2|
        151|DavidBernstein  |                9|            0|
        153|ChristopherOlsen|                5|            0|
…
```

## 숫자함수
- 숫자데이터를 입력받아 연산하는 함수
  - ROUND	: 입력 된 숫자에 대해서 반올림 연산하는 함수
  - TRUNC	: 입력 된 숫자에 대해서 버림 연산하는 함수
  - MOD	: 나눈 나머지 값을 구하는 함수

```
SELECT ROUND(45.926, 2), TRUNC(45.926, 2), MOD(1600, 300)
FROM dual;

ROUND(45.926,2)|TRUNC(45.926,2)|MOD(1600,300)|
---------------+---------------+-------------+
          45.93|          45.92|          100|
```

### ROUND|TRUNC
- ROUND|TRUNC(숫자, 연산자리값)
- 자리값은 소수점을 0으로 기준해서 양수는 소수점 아래로 음수면 소수점 위로 연산
```
정수부분의 반올림 시 자리값에 유의! (정수에서는 0을 생략할 수 없다)

SELECT ROUND(45.923, 2), ROUND(45.923, 0), ROUND(45.923, -1)
FROM dual;

ROUND(45.923,2)|ROUND(45.923,0)|ROUND(45.923,-1)|
---------------+---------------+----------------+
          45.92|             46|              50|



------------------------------------------------------------------------------

SELECT TRUNC(45.923,2), TRUNC(45.923), TRUNC(45.923, -1)
FROM dual;

TRUNC(45.923,2)|TRUNC(45.923)|TRUNC(45.923,-1)|
---------------+-------------+----------------+
          45.92|           45|              40|
```

### MOD
- 나머지 연산 함수
- MOD(나눠질수, 나누는 수)
```
SELECT last_name, salary, MOD(salary, 5000)
FROM employees
WHERE job_id = 'SA_REP';

LAST_NAME |SALARY|MOD(SALARY,5000)|
----------+------+----------------+
Tucker    | 10000|               0|
Bernstein |  9500|            4500|
Hall      |  9000|            4000|
Olsen     |  8000|            3000|
Cambrault |  7500|            2500|
Tuvault   |  7000|            2000|
King      | 10000|               0|
Sully     |  9500|            4500|
McEwen    |  9000|            4000|
Smith     |  8000|            3000|
Doran     |  7500|            2500|
Sewall    |  7000|            2000|
…


--------------------------------------------------------------------------------------------------


SELECT last_name, salary, TRUNC(salary/5000) "몫", MOD(salary, 5000) "나머지"
FROM employees
WHERE job_id = 'SA_REP';

LAST_NAME |SALARY|몫|나머지 |
----------+------+-+----+
Tucker    | 10000|2|   0|
Bernstein |  9500|1|4500|
Hall      |  9000|1|4000|
Olsen     |  8000|1|3000|
Cambrault |  7500|1|2500|
Tuvault   |  7000|1|2000|
King      | 10000|2|   0|
Sully     |  9500|1|4500|
McEwen    |  9000|1|4000|
Smith     |  8000|1|3000|
Doran     |  7500|1|2500|
Sewall    |  7000|1|2000|
…
```

- 연습문제 풀이
```

SELECT employee_id, last_name, salary,
ROUND(1.155*salary, 0) "New Salary", 
ROUND(1.155*salary - salary, 0) "Increase"
FROM employees;

EMPLOYEE_ID|LAST_NAME  |SALARY|New Salary|Increase|
-----------+-----------+------+----------+--------+
        100|King       | 24000|     27720|    3720|
        101|Kochhar    | 17000|     19635|    2635|
        102|De Haan    | 17000|     19635|    2635|
        103|Hunold     |  9000|     10395|    1395|
        104|Ernst      |  6000|      6930|     930|
        105|Austin     |  4800|      5544|     744|
        106|Pataballa  |  4800|      5544|     744|
        107|Lorentz    |  4200|      4851|     651|
        108|Greenberg  | 12008|     13869|    1861|
        109|Faviet     |  9000|     10395|    1395|
        110|Chen       |  8200|      9471|    1271|
        111|Sciarra    |  7700|      8894|    1194|
        112|Urman      |  7800|      9009|    1209|
        113|Popp       |  6900|      7970|    1070|
…


-----------------------------------------------------------------


SELECT last_name, LPAD(salary, 15, '$') salary
FROM employees;

LAST_NAME  |SALARY         |
-----------+---------------+
King       |$$$$$$$$$$24000|
Kochhar    |$$$$$$$$$$17000|
De Haan    |$$$$$$$$$$17000|
Hunold     |$$$$$$$$$$$9000|
Ernst      |$$$$$$$$$$$6000|
Austin     |$$$$$$$$$$$4800|
Pataballa  |$$$$$$$$$$$4800|
Lorentz    |$$$$$$$$$$$4200|
Greenberg  |$$$$$$$$$$12008|
Faviet     |$$$$$$$$$$$9000|
```

## 날짜함수
- 날짜데이터를 입력받아 연산하는 함수

### SYSDATE
- 현재 접속 중인 DB서버의 시간값을 날짜데이터로 돌려주는 함수
```

SELECT SYSDATE
FROM dual;

SYSDATE                |
-----------------------+
2022-04-04 12:17:26.000|
```

### 날짜의 산술연산
- 날짜 + 숫자
  - 날짜의 산술연산에서 숫자 1의 크기는 하루의 크기와 같다
  - 더한 숫자의 크기만큼 이후의 날짜가 출력된다.
```
SELECT SYSDATE, SYSDATE + 1
FROM dual;

SYSDATE                |SYSDATE+1              |
-----------------------+-----------------------+
2022-04-04 12:24:04.000|2022-04-05 12:24:04.000|
```
- 날짜 - 숫자
  - 뺀 숫자의 크기만큼 이전의 날짜가 출력된다.
```  
SELECT SYSDATE, SYSDATE - 1
FROM dual;

SYSDATE                |SYSDATE-1              |
-----------------------+-----------------------+
2022-04-04 12:25:38.000|2022-04-03 12:25:38.000|
```
- 날짜 - 날짜
  - 날짜 - 날짜의 연산에서는 두 날짜 간 차이나는 일 수 만큼이 숫자로 출력된다.
```  
SELECT SYSDATE - (SYSDATE - 7)
FROM dual;

SYSDATE-(SYSDATE-7)|
-------------------+
                  7|

--------------------------------------------------------------------------


이전 날짜에서 이후 날짜를 빼는 경우 음수의 결과가 출력된다.
SELECT (SYSDATE - 7) - SYSDATE 
FROM dual;

(SYSDATE-7)-SYSDATE|
-------------------+
                 -7|

```

- 날짜 + 숫자/24
  - 날짜에서 숫자 1의 크기는 하루
  - 1시간의 크기는 1/24

```
SELECT SYSDATE, SYSDATE + 1/24
FROM dual;

SYSDATE                |SYSDATE+1/24           |
-----------------------+-----------------------+
2022-04-04 12:34:50.000|2022-04-04 13:34:50.000|


-------------------------------------------------------------------


5시간 이후 연산
SELECT SYSDATE, SYSDATE + 5/24
FROM dual;

SYSDATE                |SYSDATE+5/24           |
-----------------------+-----------------------+
2022-04-04 12:36:45.000|2022-04-04 17:36:45.000|


-------------------------------------------------------------------


1시간을 60으로 나누면 1분이 된다.
1분 이후 시간 연산
SELECT SYSDATE, SYSDATE + 1/24/60
FROM dual;

SYSDATE                |SYSDATE+1/24/60        |
-----------------------+-----------------------+
2022-04-04 12:37:46.000|2022-04-04 12:38:46.000|


-------------------------------------------------------------------


1분은 60초
1초 이후 시간 연산
SELECT SYSDATE, SYSDATE + 1/24/60/60
FROM dual;

SYSDATE                |SYSDATE+1/24/60/60     |
-----------------------+-----------------------+
2022-04-04 12:39:31.000|2022-04-04 12:39:32.000|
```
```
SELECT last_name, (SYSDATE-hire_date)/7 AS WEEKS
FROM employees
WHERE department_id = 90;

LAST_NAME|WEEKS                                    |
---------+-----------------------------------------+
King     | 980.933422619047619047619047619047619048|
Kochhar  |  862.79056547619047619047619047619047619|
De Haan  |1107.361994047619047619047619047619047619|


---------------------------------------------------------------------


SELECT last_name, (SYSDATE-hire_date)/365 AS YEARS
FROM employees
WHERE department_id = 90;

LAST_NAME|YEARS                                    |
---------+-----------------------------------------+
King     |18.81241853754439370877727042110603754439|
Kochhar  |16.54666511288685946220192795535261288686|
De Haan  |21.23707607179096905124302384576357179097|
```


# [오후수업] JAVA 29차

## 반복자(Iterator)
- Set 과 List 객체를 차례대로 접근하기 위한 객체
- Set 또는 List 객체의 iterator() 메서드를 호출하여 Ierator 타입 객체를 리턴받고 hasNext() 메서드로 다음 요소 존재 여부를 판별한 뒤 next() 메서드로 요소가 존재할 경우 해당 요소를 꺼내올 수 있다.
- Set 과 List 객체에 대한 통일된 접근 방법을 제공함
  - 단, Iterator 대신 향상된 for문 사용도 가능 

```java
package collection;

import java.util.ArrayList;
import java.util.Arrays;
import java.util.Collection;
import java.util.HashSet;
import java.util.Iterator;
import java.util.List;
import java.util.Set;

public class Ex1 {

	public static void main(String[] args) {
		List list = new ArrayList(Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8));
		Set set = new HashSet(list);
		
		// Iterator 활용
		// 1. Set 또는 List 객체의 iterator() 메서드를 호출하여 Iterator 객체 리턴받기
		Iterator iterator = set.iterator();
		
		// 2. while() 문의 조건식으로 Iterator 객체의 hasNext() 메서드 결과를 전달
		// => 다음 요소가 존재할 경우 true 리턴되므로, 다음 요소가 존재할 동안 반복
		while(iterator.hasNext()) {
			Object o = iterator.next();
			System.out.println(o);
			
//			System.out.println(iterator.next());
		}
		
		System.out.println("===============================");
		
		// => Set 또는 List 객체 접근 방법이 완전히 동일하며
		// 	  iterator() 메서드 호출 주체만 달라지므로 통일된 접근 방법이 제공됨
		
		printElements(list);
		printElements(set);
	}

	public static void printElements(Collection c) {
		Iterator iterator = c.iterator();
		while(iterator.hasNext()) {
			System.out.println(iterator.next());
		}
	}
}
```

## 3. Map 계열
- 데이터 키(Key)와 값(Value) 한 쌍의 형태로 관리하는 자료구조 (해쉬테이블 구조)
- 키는 중복이 불가능하며, 값은 중복이 가능함
	- 키 관리 객체 : Set(중복 제거에 효과적)
- 구현체 클래스 : ashMap, Properties 등

```java
package collection;

import java.util.ArrayList;
import java.util.HashMap;
import java.util.HashSet;
import java.util.Iterator;
import java.util.List;
import java.util.Map;
import java.util.Map.Entry;
import java.util.Set;

public class Ex2 {

	public static void main(String[] args) {

		Map map = new HashMap();
		
		map.put(1, "Java");
		map.put(2, "JSP");
		map.put(3, "Android");
		
		System.out.println("map 객체의 모든 키와 값 출력 : " + map); // toString() 생략

		// 중복되는 키를 사용하여 다른 값을 저장하는 경우
//		map.put(3, "Oracle");
		// => 기존에 3번 키가 존재하므로 "Android" 값을 제거하고, "Oracle" 값 저장(덮어쓰기)
		//	  이 때, 제거되는 값 "Android" 가 Object 타입으로 리턴됨
		System.out.println("3번 키에 Oracle 문자열 저장 : " + map.put(3, "Oracle"));
		System.out.println("map 객체의 모든 키와 값 출력 : " + map); // toString() 생략
		
		// 새로운 키에 중복된 값을 저장하는 경우
		map.put(4, "JSP");
		System.out.println("map 객체의 모든 키와 값 출력 : " + map); // toString() 생략
		
		// Object get(Object key) : key에 해당하는 value 리턴
		// => key 가 존재하지 않으면 null 값 리턴됨
		System.out.println("정수 2에 해당하는 키의 데이터 : " + map.get(2));	// JSP
		System.out.println("정수 6에 해당하는 키의 데이터 : " + map.get(6));	// null
		
		// Set keySet() : Map 객체 내의 모든 키 리턴
		// => Set 타입 변수에 저장하거나 Set 타입 객체 형태로 직접 사용 가능
		Set keySet = map.keySet();
		System.out.println("map 객체 내의 모든 키 : " + keySet);
		
		Set setData = new HashSet(map.values());
		List listData = new ArrayList(map.values());
		System.out.println("map 객체 내의 모든 값 : " + setData);
		System.out.println("map 객체 내의 모든 값 : " + listData);
		
		// Set entrySet() : 키와 값을 한 쌍으로 갖는 Map.Entry 객체의 모임
		// => 키와 값의 한 쌍이 다른 키와 값의 한 쌍과 중복될 수 없으므로 Set 타입으로 관리됨
		Set entrySet = map.entrySet();
		System.out.println(entrySet);
		
		System.out.println("map 객체가 비어있는가? " + map.isEmpty());
		System.out.println("map 객체에 저장된 요소 갯수 : " + map.size());
		
		
		// 요소를 제거하는 remove() 메서드는 2가지로 제공됨
		// Object remove(Object key) : key 에 해당하는 요소(key,value) 제거
		System.out.println("4번 key에 해당하는 요소 제거 : " + map.remove(4));
		System.out.println("map 객체의 모든 키와 값 출력 : " + map); // 키(4)와 값(JSP) 제거됨
		// 만약, 존재하지 않는 키를 저장할 경우 null 값 리턴됨
		System.out.println("4번 key에 해당하는 요소 제거 : " + map.remove(4));

		// boolean remove(Object key, Object value) : key와 value 모두 일치하는 요소 제거
		System.out.println("3번 key의 Oracle 값에 해당하는 요소 제거 : " + map.remove(3, "Oracle"));
		System.out.println("2번 key의 JAVA 값에 해당하는 요소 제거 : " + map.remove(2, "JAVA"));
		
		System.out.println("================================================");
		// HashMap 객체 (map2) 생성 후 다음 요소를 추가
		// 		 KEY		 VALUE
		// "010-1234-5678"	"홍길동"
		// "010-2222-3333"	"이순신"
		// "010-4444-5555"	"강감찬"
		
		Map map2 = new HashMap();
		map2.put("010-1234-5678", "홍길동");
		map2.put("010-2222-3333", "이순신");
		map2.put("010-4444-5555", "강감찬");
		
		System.out.println(map2);

		System.out.println("================================================");

		// Map 객체 반복하는 방법
		// 1. Iterator(반복자) 사용시
		// => Map 개겣의 keySet() 메서드를 호출하여 모든 키를 꺼낸 다음
		//	  해당 Key가 저장된 Set 객체의 iterator() 메서드를 호출하여
		//	  Iterator 객체 리턴받아 사용
		Set keySet2 = map2.keySet();
		Iterator iterator = keySet2.iterator();
		while(iterator.hasNext()) { // 다음 요소(키)가 존재할 동안 반복
			// 1) 키 꺼내기 위해서는 next() 메서드로 반복자의 키 하나씩 접근
			// => 이 때, next() 메서드 리턴타입이 Object 이므로 상황에 따라 형변호나 필요
			String phone = iterator.next().toString();
			
			// 2) 값 꺼내기 위해서는 키를 사용하여 get() 메서드를 통해 값에 접근
			// => 이 때, get() 메서드 리턴타입이 Object 이므로 상황에 따라 형변환 필요
			String name = map2.get(phone).toString();
			System.out.println("이름 : " + name + ", 전화번호 : " + phone);
		}
		
		System.out.println("=======================================");
		
		// key를 사용하여 반복하지 않고, Map.Entry 객체를 통한 반복
		Set entrySet2 = map2.entrySet();
		Iterator entryIterator = entrySet2.iterator();
		
		while(entryIterator.hasNext()) {
			Map.Entry entry = (Entry) entryIterator.next();
			
			String phone = entry.getKey().toString();
			String name = entry.getValue().toString();
			
			System.out.println("이름 : " + name + ", 전화번호 : " + phone);
		}
		
		System.out.println("===========================================");
			
		// 2. 향상된 for문 사용하여 접근 (map2)
		Set keySet3 = map2.keySet();
		for(Object phone : keySet3) {
			String name = map2.get(phone).toString();
			System.out.println("이름 : " + name + ", 전화번호 : " + phone);
		}
		
		// 2) entrySet() 방법
		for(Object o : map2.entrySet()) {
			Map.Entry entry = (Entry)o;
			System.out.println("이름 : " + entry.getValue() + ", 전화번호 : " + entry.getKey());
		}
					
		// =============================================================
		
		// boolean containsKey(Object key) : 해당 key의 존재 여부 리턴
		System.out.println("010-1234-5678 전화번호(키)가 존재 하는가? " + map2.containsKey("010-1234-5678"));
		System.out.println("010-1234-9999 전화번호(키)가 존재 하는가? " + map2.containsKey("010-1234-9999")); 

		
		// boolean containsValue(Object value) : 해당 value의 존재 여부 리턴
		System.out.println("홍길동 이름(값)이 존재하는가? " + map2.containsKey("홍길동"));
		System.out.println("김태희 이름(값)이 존재하는가? " + map2.containsValue("김태희"));
		
		// void clear() : 모든 키와 값을 초기화
		map2.clear();
		System.out.println(map2);
		
	}

}
```

## Stack vs Queue
### 1. Stack(스택)
- 하나의 상자에 데이터를 아래쪽(Bottom) 에서 부터 차례대로 쌓는 구조
- 데이터의 삽입/삭제가 항상 한 쪽(Top)에서만 이루어짐
- 가장 마지막에 추가된 데이터가 가장 먼저 삭제됨 (= LIFO Last In First Out)
	- 가장 먼저 추가된 데이터가 가장 나중에 삭제됨 ( FILO First In Last Out)
- 주로 웹브라우저의 뒤쪽/앞으로, 응용프로그램의 Redo/Undo 기능에 활용
 	- (2개의 스택을 Backward, Forward 로 구분하여 사용)

```java
package collection;

import java.util.LinkedList;
import java.util.Queue;
import java.util.Stack;

public class Ex3 {

	public static void main(String[] args) {
		Stack stack = new Stack();
		stack.push("1 - www.itwillbs.co.kr");
		stack.push("2 - www.naver.com");
		stack.push("3 - www.youtube.com");
		System.out.println("모든 요소 출력 : " + stack);
		System.out.println("www.nate.com 추가 : " + stack.push("4 - www.nate.com"));

		// Object peek() : 스택의 맨 위의 요소(마지막에 추가된 요소)를 리턴(요소 제거 X)
		System.out.println("맨 위(Top)의 요소 출력(peek) : " + stack.peek());
		System.out.println("맨 위(Top)의 요소 출력(peek) : " + stack.peek());
		
		// Object pop() : 스택의 맨 위의 요소(마지막에 추가된 요소)를 리턴(요소 제거 O)
		System.out.println("맨 위(Top)의 요소 출력(pop) : " + stack.pop());
		System.out.println("맨 위(Top)의 요소 출력(pop) : " + stack.pop());
		
		System.out.println("모든 요소 출력 : " + stack);
		
	}

}
```

### 2. Queue(큐)
- 한 쪽 면에 추가, 반대편에서 삭제가 일어나는 구조
- 가장 먼저 추가된 데이터가 가장 먼저 삭제되는 구조(= FIFO, First In First Out)
	- 가장 나중에 추가된 데이터가 가장 마지막에 삭제되는 구조(= LILO, Last In Last Out)
- Queue는 인터페이스로 제공되며, 실제 구현 클래스로 LinkedList 클래스 사용
	- LinkedList 클래스는 List와 Queue 인터페이스 모두를 구현한 클래스
- 주로, 응용프로그램의 최근 문서 항목이나 번호표 시스템(선착순 대기열)에 활용
	- 1개의 큐에 순차적으로 추가한 뒤 차례대로 제거하면서 사용

```java
package collection;

import java.util.LinkedList;
import java.util.Queue;
import java.util.Stack;

public class Ex3 {

	public static void main(String[] args) {
		Queue q = new LinkedList();
		
		// boolean offer(object o) : 요소 o를 추가(추가 결과를 boolean 타입으로 리턴)
		q.offer("1 - Ex.java");
		q.offer("2 - main.jsp");
		q.offer("3 - a.txt");
		System.out.println("모든 Queue 객체 출력 : " + q); // toString() 생략됨
		
		System.out.println("Ex2.java 추가 결과 : " + q.offer("4 - Ex2.java"));
		System.out.println("모든 Queue 객체 출력 : " + q);	// toString() 생략됨
		
		// Object peek() : 큐의 맨 끝의 요소(먼저 추가된 요소)를 리턴(요소 제거 X)
		System.out.println("맨 처음 추가된 요소(peek) : " + q.peek());
		System.out.println("맨 처음 추가된 요소(peek) : " + q.peek());
		
		// Object poll() : 큐의 맨 끝의 요소(먼저 추가된 요소)를 리턴(요소 제거 O)
		System.out.println("맨 처음 추가된 요소(poll) : " + q.poll());
		System.out.println("맨 처음 추가된 요소(poll) : " + q.poll());
		
		System.out.println("모든 Queue 객체 출력 : " + q); // toString() 생략됨


	}

}
```

- 연습문제
```java
package collection;

import java.util.LinkedList;
import java.util.Queue;
import java.util.Stack;

public class Test3 {

	public static void main(String[] args) {
		/*
		 * Stack 객체 1개 생성(stack)
		 * => 웹사이트 주소 4~5개 추가
		 * => 반복문 사용하여 Top 에서부터 하나씩 객체를 꺼내서 출력 후 제거(pop())
		 * 
		 * */
		Stack stack = new Stack();
		
		stack.push("1 - www.itwillbs.co.kr");
		stack.push("2 - www.naver.com");
		stack.push("3 - www.youtube.com");
		stack.push("4 - www.google.com");
		stack.push("5 - www.nate.com");
		
		System.out.println("제거 전 : " + stack);
		
//		int length = stack.size();
//		for(int i=0; i<length; i++) {
//			System.out.println(stack.pop()); 
//		}
		
//		for(int i = stack.size(); i > 0; i--) {
//			System.out.println(stack.pop());
//		}
		
		// 향상된 for문은 원본데이터 그대로 유지될때 써야함
//		for(Object o : stack) {
//			System.out.println(o);
//		}
		
//		while(stack.size() > 0) {
//			System.out.println(stack.pop());
//		}
		
		while(!stack.isEmpty()) {
			System.out.println(stack.pop());
		}
		
		System.out.println("제거 후 : " + stack);
		
		
		
		System.out.println("==========================================");
		
		/*
		 * Queue 객체 1개 생성
		 * => 파일 4~5개 추가
		 * => 반복문을 사용하여 하나씩 객체를 꺼내서 출력 후 제거(poll())
		 * 
		 * */
		
		Queue q = new LinkedList();
		
		q.offer("1 - Ex.java");
		q.offer("2 - main.jsp");
		q.offer("3 - a.txt");
		q.offer("4 - windows.jpg");
		q.offer("5 - talk.wav");
		
		System.out.println("제거 전 : " + q);
		
		while(!q.isEmpty()) {
			System.out.println(q.poll());
		}
		
		System.out.println("제거 후 : " + q);
	}

}
```

