# [오전수업]
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

# [오후수업]
## 메서드
- 객체의 동작을 나타내는 최소 단위
- 메서드를 통해 수행할 작업을 기술하는 것을 '메서드 정의' 라고 함
- 메서드는 선언부(Header)와 구현부(Body)로 구성됨
  - 이 때, 메서드 구현부는 중괄호{} 로 둘러쌓여 있는 부분 
- 메서드는 반드시 호출되어야만 사용 가능함. 이 때, 호출하는 메서드를 Caller(일 시키는 것) 메서드라고 하며, 호출 당하는 메서드를 Worker(일 하는 것) 메서드라고 함
- main() 메서드도 메서드이며, JVM 에 의해 자동으로 호출되는 메서드 = 자바 프로그램의 시작점
- 메서드 호출 시 메서드에 전달하는 값(데이터)를 전달인자(Argument) 라고 하며, 이 값을 메서드에게 전달받기 위해서 선언하는 변수를 매개변수(Parameter) 라고 함
  - 메서드 호출 시 전달할 값이 없을 수도 있으며, 이 때는 메서드 호출 지점에서 소괄호 내부에 아무것도 명시하지 않음
	- 만약, 매개변수가 존재할 경우 선언부 매개변수 타입과 갯수에 맞게 데이터를 전달해야한다!
- 메서드 수행이 끝날 때 메서드를 호출한 곳으로 전달할(= 되돌려 줄) 값을 리턴값이라고 함
  - 메서드 수행 후 리턴할 것이 없을수도 있는데 이 때는 메서드 선언부 리턴타입 부분에 반드시 void 라는 특수한 타입을 명시해야한다!
 
< 메서드 정의 기본 문법 >
```
[제한자] 리턴타입 메서드명([매개변수선언...]) {
    // 메서드가 호출되었을 때 실행할 코드들...
 }
```

< 메서드 정의 방법(형태)에 따른 분류 >
```
1. 매개변수가 없고, 리턴값도 없는 메서드
2. 매개변수는 없고, 리턴값만 있는 메서드
3. 매개변수만 있고, 리턴값은 없는 메서드
4. 매개변수도 있고, 리턴값도 있는 메서드
```

정리 **<순서주의>**
```java
package method;

public class Ex {

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
		
		// 주의! 리턴타입과 맞지 않는 데이터 리턴 시 오류 발생!
//		return 10; // Type mismatch: cannot convert from int to String
				
	}
	
} // Ex 클래스 끝
```

문제출제
```java
package method;

public class test {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		// ----------------- Caller 메서드에서 Worker 메서드를 호출하는 곳 ----------------
		// 1. 매개변수도 없고, 리턴값도 없는 메서드 정의
		// 1) "Hello, World!" 문자열을 출력하는 hello() 메서드 정의
		hello();
		System.out.println("==================================");
		// 1-2) "Hello World!" 문자열을 10번 반복 출력하는 hello2() 메서드 정의
		hello2();
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
	
	
} // Test 클래스 끝
```

