# [오전수업] DB 2차
## VirtualBox 기본 설정 및 구동법
### 기본설정
- 환경설정 -> 입력 -> 가상 머신 -> 호스트 키 조합 Shift+Ctrl로 설정(편의상) -> 
  - 호스트키 : 가상운영체제를 구동 할 때 원래 사용하던 운영체제와 입력이 겹치지 않게 스위치 전환 해주는 키 
### 구동
- 새로 만들기 -> 이름을 Centos7_mysqp로 설정(편의상) -> 메모리크기 기본(다음) -> 하드디스크 기본(다음) -> 만들기 -> 하드디스크 파일종류 기본(다음)-> 파일 크기 50GB(편의상) -> 만들기
- 설정 -> 시스템 -> 포인팅 장치를 USB 태블릿으로 설정 & 하드웨어 시각 체크 해제
       -> 저장소 -> 컨트롤러SATA에 호스트 I/O 캐시 사용 체크 -> 컨트롤러 IDE 비어있음 클릭 -> 오른쪽 디스크 모양 누르기 -> 디스크 파일 선택 -> 저번 시간에 받은 CentOS-7-x86_64-Minimal-2009           선택 -> 열기 -> 확인
### 가상운영체제 설치 및 설정     
- 시작버튼 클릭 -> Install CentOS 7 설치 선택 -> 언어는 영어로 설정하고 다음 -> System -> INSTALLATION DESTINATION 클릭 -> 설정 변경 없이 Done 클릭 -> NETWORK & HOST NAME 클릭 -> 아래쪽 Host name을 MySQL로(편의상) -> Apply -> 옆의 Configure... 클릭 -> General 항목 클릭 -> 첫 번째 항목 체크 -> save -> done -> DATE & TIME 클릭 -> 지역 서울로 클릭 -> done -> Begin installlation 클릭 -> ROOT PASSWORD 클릭 -> 비밀번호 1234로 설정(편의상) -> done -> 설치가 종료되면 reboot 클릭
- 윗쪽의 머신 -> 설정 -> 네트워크 -> NAT를 어댑터에브리지 -> 확인

### 로그인
- 로그인명 root / 비밀번호 1234(입력해도 아무것도 안 보이는게 정상)
- 로그인에 성공하면 # ip addr show 입력 -> 본인의 아이피 확인 (2번 항목에서 inet 옆에 있는 아이피가 본인의 아이피)
- ![캡처](https://user-images.githubusercontent.com/95197594/151470802-c48dc366-c2cf-455d-b119-77e837d55aae.PNG)
- (윈도우와 원격 연결) 윈도우즈로 돌아와서 cmd 실행 -> ssh root@아이피 입력 -> 메세지 뜨면 yes -> 비밀번호 1234 입력 -> 로그인 완료 

### 설치파일 다운로드
https://www.mysql.com/ -> 다운로드 -> MySQL Community (GPL) Downloads -> MySQL Yum Repository -> Red Hat Enterprise Linux 7 / Oracle Linux 7 (Architecture) 클릭 -> No thanks, just start my download 주소 복사(https://dev.mysql.com/get/mysql80-community-release-el7-5.noarch.rpm) -> cmd 창으로 돌아와서 yum update -y 입력
  - yum : 플레이스토어같은 역할
  - yum update -y는 최신버전 업데이트 명령어

### mySQL repository 추가
- cmd에서 yum install -y 마우스우클릭(https://dev.mysql.com/get/mysql80-community-release-el7-5.noarch.rpm 링크 붙여넣기) 

### mySQL community server 설치
- cmd에서 yum search mysql-community-server 입력 -> yum install -y mysql-community-server 입력

### mySQL server 접속
- systemctl start mysqld로 실행 -> systemctl status mysqld로 실행됐는지 확인 -> grep 'temporary password' /var/log/mysqld.log 로 비밀번호 확인 및 복사
![제목 없음](https://user-images.githubusercontent.com/95197594/151478069-0f92cddd-10f5-48d2-9fde-f7e28200d630.png)
  - 위의 하이라이트 된 값은 MySQL 서비스의 관리자 계정인 'root'계정의 랜덤 생성된 패스워드 값으로 첫 접속 시에 입력해줘야한다.
- mysqp -uroot -p 입력 -> 패스워드 입력 (마우스 오른쪽 클릭) -> 로그인 완료 -> exit

### password 재설정
- echo 'validate_password.policy=LOW' | sudo tee -a /etc/my.cnf 입력
  - 패스워드 복잡성 수준을 낮음으로 설정. 패스워드 길이의 조건만 만족하면 된다.
- echo 'validate_password.length=1' | sudo tee -a /etc/my.cnf 입력
  - 패스워드의 길이를 1글자만 넘기면 되게끔 설정
- echo 'default_password_lifetime=0' | sudo tee -a /etc/my.cnf 입력
  - 패스워드의 유효기간을 무기한으로 변경
- systemctl restart mysqld 입력
  - MySQL의 서비스를 재시작해서 위의 설정값을 적용한다.
- mysql -uroot -p 입력 -> 기존 랜덤 패스워드값 입력 -> 로그인 성공시 ALTER user 'root'@'localhost' IDENTIFIED BY '1234'; 입력
  - 'root' 계정의 패스워드를 1234로 갱신 
- CREATE USER 'root'@'%' IDENTIFIED BY '1234'; 입력
  - 외부에서 접속 가능한 'root' 계정 생성하고 패스워드는 '1234'로 지정
- GRANT ALL PRIVILEGES ON *.* TO 'root'@'%'; 입력
  - 생성한 'root'에 모든 권한을 부여하는 명령어 실행
 - flush privileges; 입력
 - SELECT host, user FROM mysql.user; 입력 후 뜨는 창에서 % root가 맨 위에 위치해 있으면 설정 완료 -> exit 입력
 ![제목 없음](https://user-images.githubusercontent.com/95197594/151482951-94e39456-f445-46db-ad74-0fbc867acd74.png)
 
 ### 방화벽 설정 (원격 설정을 하기 위함)
 - firewall-cmd --permanent --add-port=3306/tcp 입력
  - 시스템에 MySQL의 기본 포트번호 3306의 값을 방화벽에서 영구 예외처리하는 명령어
 - firewall-cmd --reload 입력
  - 방화벽 설정 다시 읽어오기

### Workbench 접속
- 실행 -> MySQL Connection 옆의 + 버튼 클릭 -> Hostname에 본인의 아이피값 입력 Connection name에 Cento_mysql 입력 -> Test Connection 클릭 -> 패스워드에 1234 입력 & 밑에 자동저장 체크 -> 메인 화면으로 돌아와서 설정한 서버 더블클릭 -> 정상 실행
  - Hostname은 서버의 주소 

---

# [오후수업] JSP 7차
## 메서드
- 객체의 동작을 나타내는 최소 단위
- 메서드를 통해 수행할 작업을 기술하는 것을 '메서드 정의' 라고 함
- 메서드는 선언부(Header)와 구현부(Body)로 구성됨
  - 이 때, 메서드 구현부는 중괄호{} 로 둘러쌓여 있는 부분 
- 메서드는 반드시 호출되어야만 사용 가능함. 이 때, 호출하는 메서드를 Caller(일 시키는 것) 메서드라고 하며, 호출 당하는 메서드를 Worker(일 하는 것) 메서드라고 함
- main() 메서드도 메서드이며, JVM 에 의해 자동으로 호출되는 메서드 = 자바 프로그램의 시작점
- 메서드 호출 시 메서드에 전달하는 값(데이터)를 전달인자(Argument) 라고 하며, 이 값을 메서드에게 전달받기 위해서 선언하는 변수를 매개변수(Parameter) 라고 함
  - 메서드 호출 시 전달할 값이 없을 수도 있으며, 이 때는 메서드 호출 지점에서 소괄호 내부에 아무것도 명시하지 않음 (매개변수를 명시하지 않음)
	- 만약, 매개변수가 존재할 경우 선언부 매개변수 타입과 갯수에 맞게 데이터를 전달해야한다!
- 메서드 수행이 끝날 때 메서드를 호출한 곳으로 전달할(= 되돌려 줄) 값을 리턴값이라고 함
  - 메서드 수행 후 리턴할 것이 없을수도 있는데 이 때는 메서드 선언부 리턴타입 부분에 반드시 void 라는 특수한 타입을 명시해야한다!
  	- 한 번에 리턴 가능한 데이터는 한 개이다! (= 복수개 리턴 불가) 
 
### < 메서드 정의 기본 문법 >
```
[제한자] 리턴타입 메서드명([매개변수선언...]) < 선언부(header){
    // 메서드가 호출되었을 때 실행할 코드들...
    [return [];]
 } < 구현부(body)
```

### < 메서드 정의 방법(형태)에 따른 분류 >
```
1. 매개변수가 없고, 리턴값도 없는 메서드
2. 매개변수는 없고, 리턴값만 있는 메서드
3. 매개변수만 있고, 리턴값은 없는 메서드
4. 매개변수도 있고, 리턴값도 있는 메서드
```

### 정리 **<순서주의>**
```java
package method;

public class Ex {

	// 이 위치에도 메서드가 올 수 있다! (클래스 안쪽, 다른 메서드 바깥쪽에 위치) 

	public static void main(String[] args) {
		
		
		// ----------------- Caller 메서드에서 Worker 메서드를 호출하는 곳 ----------------
		// 1. 매개변수가 없고, 리턴값도 없는 메서드
		//   - 미리 정의된 sister_1 이라는 메서드 이름을 명시하고
		//	 - 매개변수가 없으므로 메서드 호출 시 소괄호() 내부에 아무 데이터도 전달하지 않음
		System.out.println("동생아! 불 좀 꺼라!");
		sister_1(); // sister_1() 메서드 호출 => 코드 흐름이 메서드쪽으로 변경됨
		// sister_1() 메서드 실행이 끝난 후 복귀된 흐름에 의해 다시 차례대로 다음 코드를 실행
		System.out.println("동생이 불을 껐다");
		
		System.out.println("======================================");
		
		// 2. 매개변수는 없고, 리턴값만 있는 메서드
		// => 미리 정의된 sister_2 라는 메서드 이름을 명시하고
		// => 매개변수가 없으므로 메서드 호출 시 소괄호() 내부에 아무 데이터도 전달하지 않음
		System.out.println("동생아! 물 좀 떠온나!");
		
		// sister_2() 메서드를 호출한 뒤 리턴값을 변수에 저장한 뒤 사용할 경우
		String result = sister_2(); // sister_2() 메서드를 호출한 뒤 리턴값을 변수에 저장
		// 결과적으로 String result = "물"; 코드와 동일한 결과값을 갖게 된다!
		System.out.println("동생이 가져다 준 것 : " + result); // 리턴값인 "물"이 출력됨
		
		
		//sister_2() 메서드를 호출한 뒤 리턴값을 변수에 저장하지 않고
		// 출력문 등에서 바로 사용할 경우
		System.out.println("동생이 가져다 준 것 : " + sister_2());
		// System.out.println("동생이 가져다 준 것 : " + "물"); 과 동일한 결과값을 갖게됨
		// => 단, 출력문 실행되기 전 문자열 결합을 위해 sister_2() 메서드로 이동하여
		//    리턴되는 데이터(문자열 "물")를 전달받아 "동생이 가져다 준 것 : " 문자열과 결합 후 출력함
		
		System.out.println("======================================");
		
		// 3. 매개변수만 있고, 리턴값은 없는 메서드 정의
		System.out.println("동생아! 200원 줄테니 과자 사먹어라!");
		// => 미리 정의된 sister_3 이라는 메서드 이름을 명시하du 메서드 호출
		// => 매개변수가 있으므로 메서드 호출 시 소괄호() 내부에 매개변수와 일치하는 데이터타입의 데이터 전달 필수
		//    (리터럴을 직접 전달하거나, 변수 값 전달도 가능)
		sister_3(200); // sister_3() 메서드 호출 시 전달인자로 정수형 리터럴 200을 전달
		
		// 변수에 전달할 데이터를 저장한 뒤 메서드 호출 시 전달하는 경우
		int money = 200;
		sister_3(money);
		
		System.out.println("======================================");
		
		// 4. 매개변수도 있고, 리턴값도 있는 메서드 호출
		// => 매개변수가 있으므로 메서드 호출 시 소괄호() 내부에 매개변수와 일치하는 데이터타입의 데이터 전달 필수
		//    (리터럴을 직접 전달하거나, 변수 값 전달도 가능)
		// => 리턴타입이 String 타입이므로 리턴값을 전달받아 사용 가능
		System.out.println("동생아! 1000원을 줄테니 나도 새우깡 좀 사다도!");
		String snack = sister_4(1000); // 메서드 호출 시 1000 전달하고, 리턴값(String 타입)을 snack 변수에 저장
		System.out.println("동생이 사다준 과자 : "  + snack);
		
		System.out.println("======================================");
		
		<<추가>>매개변수가 2개 이상인 메서드
		// => 전달할 데이터가 복수개일 경우 해당 데이터의 타입과 순서에 맞게 메서드 매개변수를 지정해야함
		// 	  (참고. 리턴값은 2개 이상 지정 불가)
		// ex) 정수 2개를 전달할 경우 매개변수는 정수형 변수 2개 선언
		// ex) 정수 1개와 문자열 1개 전달할 경우 매개변수는 정수형 변수, 문자열 변수 순으로 선언해야함
		System.out.println("동생아! 새우깡과 500원 줄테니 쿠크다스로 바꿔온나!");
		String snack = sister_5("새우깡", 500);
		System.out.println("동생이 바꿔다 준 것 : " + snack);
		
	} // manin() 메서드 끝

	
	
	// ---------------- Worker 메서드를 정의하는 곳 ----------------
	// 1. 매개변수가 없고, 리턴값도 없는 메서드
	//   - 리턴값이 없으므로 리턴타입 부분에 void 라는 특수한 타입을 명시 (생략불가)
	//   - 매개변수가 없으므로 매개변수 부분(소괄호() 내부)은 아무것도 기술하지 않음
	public static void sister_1() {
		System.out.println("동생 : 오빠가 불을 끄라고 시켜서 불을 껐다!");
	} // 메서드 실행이 끝나면 현재 메서드를 호출한 곳으로 흐름이 되돌아감(복귀)
	  // => main() 메서드 내의 stsiter_1() 코드 부분으로 이동
	
	// ----------------------------------
	
	// 2. 매개변수는 없고, 리턴값만 있는 메서드
	// => 리턴값이 있으므로 리턴할 데이터에 해당하는 데이터타입을 리턴타입 부분에 기술
	// 	  만약, 문자열을 리턴할 경우 STring, 정수를 리턴할 경우 int 타입을 기술
	//	  리턴타입이 void가 아닌 상태에서 아무런 데이터도 리턴하지 않으면 오류 발생
	//	  (This method must return a result of type XXX) => XXX 부분은 리턴할 데이터타입
	// => 매개변수가 없으므로 메서드 호출 시 소괄호() 내부에 아무 데이터도 전달하지 않음
	public static String sister_2() {
		System.out.println("동생 : 오빠가 물 떠오라고 시켰다!");
		
		// 리턴타입이 void가 아닌 경우 반드시 return 문을 사용하여 리턴할 데이터를 명시해야한다!
		// => 이 때, 리턴할 데이터는 리터럴 또는 변수 모두 지정 가능
		// 1) String 타입 리터럴을 직접 리턴하는 경우
//		return "물"; // 문자열 "물"을 가지고 호출한 곳으로 돌아감
		
		// 2) String 타입 변수에 리터럴을 저장한 후 변수 값을 리턴하는 경우 
		String result = "물";
		return result;
		
		// return 문 뒤(아래쪽)에 다른 문장을 기술 할 수 없다!
		// => return 문을 만나면 현재 메서드를 종료하고 호출한 곳으로 되돌아가므로 다음 코드는 실행 불가능
		System.out.println("return 문 뒤의 실행문장"); // Unreachable code (도달 불가능한 코드)
		
		// 주의! 리턴타입과 맞지 않는 데이터 리턴 시 오류 발생!
//		return 10; // Type mismatch: cannot convert from int to String

	// ----------------------------------
	
	// 3. 매개변수만 있고, 리턴값은 없는 메서드 정의
	//    => sister_3 메서드 정의
	//    => 리턴값이 없으므로 리턴타입 부분에 void 라는 특수한 타입을 명시(생략 불가)
	//    => 매개변수가 있으므로 전달받는 데이터의 데이터타입과 일치하는 변수 선언 필수
	//       ex) 정수형 데이터를 전달받을 경우 정수형 변수 선언 필수	
	public static void sister_3(int money) { 
	    	// 정수형 데이터 1개를 전달받아 int형 변수 money에 저장
	    	// => 전달받은 전달인자가 저장된 매개변수는 메서드 내에서 자유롭게 사용 가능함
	    	System.out.println("동생 : 오빠가 과자 사먹으라고 " + money + "원을 줬다!");
	    	money -= 200;
	    	System.out.println("동생 : 새우깡 사먹고 " + money + "원이 남았다!");
	}
	
	// ----------------------------------
	
	// 4. 매개변수도 있고, 리턴값도 있는 메서드 정의
	// => sister_4() 메서드 정의
	// => 리턴값이 있으므로 리턴할 데이터에 해당하는 데이터타입을 리턴타입 부분에 기술
	// => 매개변수가 있으므로 전달받는 데이터의 데이터타입과 일치하는 변수 선언 필수
	//    ex) 정수형 데이터를 전달받을 경우 정수형 변수 선언 필수

	public static String sister_4(int money) {
		System.out.println("동생 : 오빠가 과자 사오라고 " + money + "원을 줬다!");
			
		money -= 200;
		System.out.println("동생 : 새우깡 사고 " + money + "원이 남았다!");
			
		// 리턴할 데이터가 있으므로 return 문 뒤에 리턴할 데이터(String 타입)를 명시
		return "새우깡";
		// => 단, 리턴값으로 지정 가능한 데이터는 동시에 1개만 지정 가능하므로
		//    현재 "새우깡"(String 타입)만 리턴 가능하고, 잔돈 800원은 리턴 불가능
		//    (같은 타입의 데이터도 하나만 리턴 가능함)
	}
	
	// ----------------------------------
	
	// <<추가>>매개변수가 2개 이상인 메서드
	// => 전달하는 데이터의 타입과 순서에 맞게 매개변수를 선언해야함
	public static String sister_5(String snack, int money) { // 문자열, 정수 순으로 선언 필수!
		System.out.println("동생 : 오빠가 과자 바꿔오라고 준 것 : " + snack + "과 " + money + "원을 줬다!");
		money -= 500;
		snack = "쿠크다스";
		System.out.println("동생 : " + snack + "(으)로 바꿨다!");
		
		return snack;
		
		
} // Ex 클래스 끝
```

### 문제출제
```java
package method;

public class test {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		// ----------------- Caller 메서드에서 Worker 메서드를 호출하는 곳 ----------------
		// 1. 매개변수도 없고, 리턴값도 없는 메서드 정의
		// 1) "Hello, World!" 문자열을 출력하는 hello() 메서드 정의
		System.out.println("hello() 메서드 호출 시작!");
		hello();
		System.out.println("hello() 메서드 호출 끝!");
		
		
		System.out.println("==================================");
		
		
		// 1-2) "Hello World!" 문자열을 10번 반복 출력하는 hello2() 메서드 정의
		hello2();
		
		
		System.out.println("==================================");
		
		
		// 2. 매개변수는 없고, 리턴값만 있는 메서드
		// 2-1) "Hello, World!" 문자열을 리턴하는 returnHello() 메서드 정의
//				=> 매개변수가 없으므로 소괄호 내부에 아무것도 명시하지 않으며,
//				=> 리턴값이 있으므로 리턴타입인 String 타입으로 변수에 저장하거나 출력문 등에 직접 사용
				
		String result = returnHello();
		System.out.println("리턴값 : " + result);
		
		
		// 2-2) 정수 1 ~ 10 까지의 합(55)을 계산하여 리턴하는 sum() 메서드 호출
		int result1 = sum();
		System.out.println("1 ~ 10 까지의 합 : " + result1);
		
		System.out.println("==================================");
		
		// 3. 매개변수만 있고, 리턴값은 없는 메서드 호출
		// 3-1) 정수 1개를 전달받아 해당 정수값만큼 "Hello, World" 문자열을 반복 출력하는 hello() 메서드 출력
		
		hello(5);
		
		// 3-2) 문자열 1개를 전달받아 해당 문자열을 10번 출력하는 pirnt() 메서드 호출 
		
		print("JAVA");
		
		// 3-3) 문자 1개를 전달받아 해당 문자의 아스키코드값과 해당 문자보다 정수 3만큼 큰 아스키코드의 문자 출력하는 encrypt() 메서드 호출
		// ex) 문자 데이터 "A" 전달 시 출력 예시 : 문자 데이터는 A(65) 이고, 암호화 데이터는 D(68) 
		
		encrypt('A'); // 
		
		// 암호화를 활용하는 예시)
		// h = k	e = h		l = o		o = r		의 값으로 변환되므로
		// hello를 암호화하면 khoor 가 된다!
		
		System.out.println("==================================");
		
		// 4. 매개변수도 있고, 리턴값도 있는 메서드 
		// 정수 1개(num)를 전달받아 1 ~ num 까지의 합을 계산 후 리턴하는 sum() 메서드 정의
		// ex) 10 전달할 경우 1 ~ 10 까지의 합 55 를 계산한 후 리턴
		
		int result = sum(10);
		System.out.println("합계 : " + result);
		
		// 4-2) 정수 1개(num2)를 전달받아 "홀수" 또는 "짝수" 를 판별하여 리턴하는 checkNum() 메서드 호출
		// ex) 10 전달할 경우 "짝수" 리턴, 3 전달할 경우 "홀수" 리턴 (0은 짝수에 포함시킬 것)
		
		System.out.println("정수 10 : " + checkNum(10));
		System.out.println("정수 5 : " + checkNum(5));
		
	} // main() 메서드 끝

	
	// ---------------- Worker 메서드를 정의하는 곳 ----------------
	// 1. 매개변수도 없고, 리턴값도 없는 메서드 정의
	// 1) "Hello, World!" 문자열을 출력하는 hello() 메서드 정의
	//	  => 리턴값이 없으므로 리턴타입 부분에 void 명시
	//	  => 매개변수가 없으므로 메서드 선언부 소괄호() 내부에 아무것도 명시하지 않음
	public static void hello() {
		System.out.println("Hello World!");
	}
	
	// 1-2) "Hello World!" 문자열을 10번 반복 출력하는 hello2() 메서드 정의
	public static void hello2() {
		// for문을 사용하여 제어변수 i값이 1부터 10까지 1씩 증가하면서 10번 반복
		for(int i = 1; i<=10; i++) {
			System.out.println(i + "Hello World!");
		}
		
	}
	
	// 2. 매개변수는 없고, 리턴값만 있는 메서드
	// 2-1) "Hello, World!" 문자열을 리턴하는 returnHello() 메서드 정의
	//		=> 매개변수가 없으므로 매개변수 부분(소괄호() 내부)은 아무것도 기술하지 않음
	//		=> 리턴할 데이터가 문자열이므로 리턴타입에 String 타입 기술
	public static String returnHello() {
		return "Hello World!!";
		
	}
	
	// 2-2) 정수 1 ~ 10 까지의 합 (55)을 계산하여 리턴하는 sum() 메서드 정의
	public static int sum() {
		int sum = 0; // 누적 변수
		for(int i = 1; i <= 10; i++) {
			sum += i;
		}
		return sum;
	}
	
	// 3. 매개변수만 있고, 리턴값은 없는 메서드 호출
	// 3-1) 정수 1개를 전달받아 해당 정수값만큼 "Hello, World" 문자열을 반복 출력하는 hello() 메서드 출력
	public static void hello(int count) {
	for (int i = 1; i <= count; i++)
		System.out.println(i + "Hello, World");
	}
	
	// 3-2) 문자열 1개를 전달받아 해당 문자열을 10번 출력하는 print() 메서드 호출
	public static void print(String str) {
		
	for (int i = 1; i <= 10; i++) {
		System.out.println(str);
	}
	
	// 3-3) 문자 1개를 전달받아 해당 문자의 아스키코드값과 해당 문자보다 정수 3만큼 큰 아스키코드의 문자 출력하는 encrypt() 메서드 호출
	// ex) 문자 데이터 "A" 전달 시 출력 예시 : 문자 데이터는 A(65) 이고, 암호화 데이터는 D(68) 
	public static void encrypt(char ch) {
	System.out.println("문자 데이터는 " + ch  + "이고, 정수로 변환하면 " + (int)ch + "이다.");
		
	// 문자 ch의 값을 3만큼 증가시키기
//	ch = (char)(ch + 3); // char + int = int 이므로 char 타입 변수에 저장 시 형변환 필수!
	ch += 3; // 형변환 불필요
		
	System.out.println("암호화 데이터는 " + ch + "이고, 정수로 변환하면 " + (int)ch + "이다.");
	}
	
	// 4. 매개변수도 있고, 리턴값도 있는 메서드 정의
	// 정수 1개(num)를 전달받아 1 ~ num 까지의 합을 계산 후 리턴하는 sum() 메서드 정의
	// ex) 10 전달할 경우 1 ~ 10 까지의 합 55 를 계산한 후 리턴
	public static int sum(int num) {
	int total = 0;
	for(int i = 1; i <= num; i++) {
		total += i;
	}
		return total;
	}
	
	// 4-2) 정수 1개(num2)를 전달받아 "홀수" 또는 "짝수" 를 판별하여 리턴하는 checkNum() 메서드 호출
	// ex) 10 전달할 경우 "짝수" 리턴, 3 전달할 경우 "홀수" 리턴 (0은 짝수에 포함시킬 것)
	public static String checkNum(int num2) {
	String result = " "; // 결과를 저장할 변수

//		if (num2 % 2 == 0) { // 짝수일 경우
//			result = "짝수";
//		} else { // 짝수가 아닐 경우 (= 홀수일 경우)
//			result = "홀수";
//		}
//			return result; // 결과값 리턴

		// ----------------------------------------------------------
		// if 문 내에서 직접 return 문을 지정하는 경우
		if (num2 % 2 == 0) {
			return "짝수";
		} else {
			return "홀수";
		}

	}
	
} // Test 클래스 끝
```

### return문 사용시 주의사항
```java
		// return 문 사용 시 주의사항
		// - return 문이 실행되면 현재 메서드 수행을 종료하고 호출한 곳으로 흐름이 되돌아감
		//	 따라서, return문 아래쪽(뒤)의 문장은 실행되지 못한다!
		// - 리턴타입이 void 인 경우에도 return 문은 사용 가능하나, return 문 뒤에 데이터 지정 불가능
		//	 즉, 현재 메서드를 종료할 목적으로 return 문을 사용 가능
		// - 리턴문을 조건문과 결합하여 사용할 경우 모든 경우의 수에 return 문이 적용되어야 한다!
		
		// ex) 홀수, 짝수를 판별하여 리턴하는 메서드 checkNum() 호출 시
		System.out.println("판별 결과 : " + checkNum(10));

	}

	public static String checkNum(int num) {
		// 주의사항! 리턴타입을 명시한 메서드는 어떤 경우라도 반드시 return 문이 실행되어야 함!
		// => if 문 사용 시 else if 문 등을 사용하는 경우 else 문을 제외하고 사용할 경우 모든 경우의 수를 판별 할 수 없게 되므로
		// return 문이 실행되지 않는 조건이 생길 수 있다!
		
		
		
		// <문제점>
		// if 문과 else if문 조합 시 나머지 경우인 else 문이 없으므로 return 문이 실행되지 못 하는 경우가 발생
		// => 단, 모든 조건이 만족하는 것처럼 보여도 문법적으로 else 문을 필요로 하게 됨
		if(num % 2 == 1) { // 홀수 
			return "홀수"; // if문 결과가 true 일 때 return 문 실행
		} else if(num % 2 == 0) {
			return "짝수"; // else if문 결과가 true 일 때 return 문 실행
		}
		// => 두 조건이 모두 false 일 때 return 문이 실행되지 않으므로 오류 발생!
		
		
		
		//< 해결책 >
		// 1. else 문을 통해 나머지 경우에 특정 값을 리턴하도록 지정
		if(num % 2 == 1) { // 홀수 
			return "홀수"; // if문 결과가 true 일 때 return 문 실행
		} else { // 짝수
			return "짝수"; // if문 결과가 false 일 때 return 문 실행
		}
		
		// 2. else if 문 사용 시에도 마찬가지로 else 문을 통해 나머지 경우 return 값 지정
		if(num % 2 == 1) { // 홀수 
			return "홀수"; // if문 결과가 true 일 때 return 문 실행
		} else if(num % 2 == 0) {
			return "짝수"; // else if문 결과가 true 일 때 return 문 실행
		} else { // if문과 else if문 결과가 false 일 때 return 문 실행 
			// 단, 홀수와 짝수 판별의 경우 나머지가 1 또는 0 밖에 없으므로 else 문이 실행될 일은 없음
			return "그 외";
		}
		
		// 3. else 문 사용 여부와 관계없이 조건문 내에서는 return 할 값을 생성만 하고
		// 	  조건문이 끝난 후 return 문을 사용하여 생성된 값을 return 하는 경우
		String result = ""; // 리턴값을 저장할 변수 선언하고 기본값으로 초기화
		// => 조건문 내에서 result 변수에 리턴값을 저장하는 작업만 수행
		if(num % 2 == 1) { // 홀수 
			result = "홀수";
		} else if(num % 2 == 0) { // 짝수
			result = "짝수";
		} 
		
		// 저장된 결과값을 사용하여 return 문을 기술
		// => if문 또는 else if 문 등과 관계없이 항상 실행되는 문장(리턴값만 조건문에서 생성한 것 뿐이다!)
		return result;
```		

### 리턴타입이 void 인 메서드에서의 return 문 사용
```java
public class Ex {

	public static void main(String[] args) {
	
	int num = 100;
		voidMethod(num);
		System.out.println("voidMethond() 메서드 종료!");
		
	} // main 메서드 끝
	
	
	// for문을 사용하여 1 ~ num 까지의 합 계산(정수형 변수 total 에 누적)
	// => 단, 합계가 1000 을 초과할 경우 해당 시점에서의 i값을 출력 후 현재 메서드 종료
	int total = 0;
		for(int i = 1; i <= num; i++) {
			total += i;
			System.out.println("total = " + total);
			
			//합계가 1000 을 초과하는지 판별
			if(total > 1000) {
				System.out.println("total 변수 합이 1000을 초과할 때의 i값 = " + i);
				// 합계가 1000 보다 클 경우 현재 메서드를 종료하려면 return 문 사용 가능
				// => 단, 현재 voidMethod() 메서드의 리턴타입이 void 이므로 값 지정은 불가능
//				return; // 현재 메서드 종료 후 호출한 곳으로 돌아감

				// void 타입 메서드에서 return 문에 값 지정 시 오류 발생!
				return 1; // oid methods cannot return a value
			}
		}
		System.out.println(" 1 ~ " + num + " 까지의 합 = " + total); // 실행 안 됨

} // Ex 클래스 끝
```

### 연습문제
```java
package method;

public class test5 {

	public static void main(String[] args) {
		// ----------------Caller 메서드에서 Worker 메서드를 호출하는 곳----------------
		// print() 메서드 호출
		// => 파라미터 : 문자열 1개, 정수 1개 	리턴값 : 없음
		// ex) "Hello, World!" 와 5 전달 시 "Hello, World!" 문자열을 5번 출력
		// ex) "JAVA" 와 10 전달 시 "JAVA" 문자열을 10번 출력
		print("Hello, World!", 5);
		print("JAVA", 10);

	} // main() 메서드 끝
	
	
		// ----------------Worker 메서드를 정의하는 곳----------------
		// print() 메서드 정의
		// => 파라미터 : 문자열(data), 정수(count)	리턴값 : 없음
		// => 해당 문자열(data)을 count 번 반복 출력
	public static void print(String data, int count) {
		for(int i = 1; i <= count; i++)
			System.out.println(data);
	}
	
	

} // Test5 클래스 끝
```
### 메서드 내에서 또 다른 메서드 호출
```java
package method;

public class Ex7 {

	public static void main(String[] args) {
		// 하나의 메서드 내에서 또 다른 메서드 호출이 가능하다!
		// => 메서드 호출 스택에 따라 호출되었다가 종료되면 호출 스택의 역순으로 복귀
		sister(); // sister() 메서드로 흐름 이동
		System.out.println("sister() 메서드 호출 끝!");
	}
	
	public static void sister() {
		System.out.println("sister() 메서드 호출됨!");
		
		// sister() 메서드 내에서 또 다른 메서드 cousin() 호출
		cousin(); // cousin() 메서드로 흐름 이동
		
		System.out.println("sister() 메서드 종료됨!");
	} // main() 메서드 내의 sister() 메서드 호출 코드로 복귀

	public static void cousin() {
		System.out.println("cousin() 메서드 호출됨!");
		System.out.println("cousin() 메서드 종료됨!");
	} // sister() 메서드 내의 cousin() 메서드 호출 코드로 복귀
	
	
	
	=> sister() 메서드 호출됨!
	cousin() 메서드 호출됨!
	cousin() 메서드 종료됨!
	sister() 메서드 종료됨!
	sister() 메서드 호출 끝!

	
}
```



