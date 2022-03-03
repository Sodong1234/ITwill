# [오전수업] JSP 23차

> mysql 작업을 위하여 초기세팅 실시

> - study_jsp 데이터베이스 생성 -> test 테이블 생성 -> idx컬럼(INT타입) 생성 -> idx 컬럼에 임의의 숫자 2개 추가(INSERT INTO test VALUES (1); INSERT INTO test VALUES (2);)
> - https://downloads.mysql.com/archives/c-j/ -> Platform Independent로 변경 -> zip 파일 다운로드 및 압축풀기 (파일에 대한 내용은 아래 기술)
> - 압축 풀 때 나온 jar 파일 복사 -> 이클립스에서 StudyJSP 폴더 -> src -> main -> webapp -> WEB-INF -> lib에 붙여넣기 -> jar 마우스 오른쪽 클릭 -> Build Path 클릭 -> Add to build Path 클릭



### 라이브러리(Library)
- 함수(또는 메서드)들의 집합
- 사용해야하는 함수(또는 메서드)들을 미리 만들어서 모아놓은 곳

### API(Application Programming Interface)
- 라이브러리에 접근하기 위한 규칙들을 정의한 것
- 라이브러리에서 제공하는 함수(또는 메서드)들에 접근하기 위해 API에 정의된 규칙에 따라 입력을 통해 결과를 전달받아 사용할 수 있도록 해주는 것
- 라이브러리 = API (= 클래스들의 모음집)

## JDBC(Java DataBase Connectivity) 
- 자바에서 데이터베이스에 접근하기 위한 드라이버 클래스 등을 묶어서 제공하는 표준화 된 API(라이브러리)
- MySQL, Oracle 등 각 데이터베이스 제조사의 개발자가 프로그래밍 언어를 통해 자사의 데이터베이스에 쉽게 접근 할 수 있도록 해당 베이스 접근에 필요한 기능들을 드라이버 클래스 형태로 제공해줌
- 따라서, 프로그램 개발자는 데이터베이스 내부 구조를 알지 못해도 JDBC를 활용하여 드라이버 클래스를 사용하면 프로그래밍언어-DB 연결을 수행 가능
  - 데이터베이스 접근 방법을 표준 형태로 제공해주므로 데이터베이스가 변경되더라도 드라이버 교체 및 간단한 설정 변경을 통해 기존 코드를 거의 수정하지 않고도 데이터베이스 교체가 가능 

```java
[ MySQL 을 위한 JDBC 드라이버 준비 방법 ]
1. MySQL 연동을 위한 드라이버가 포함된 API(라이브러리) 다운로드
  1) https://www.mysql.com 접속 후 상단 Downloads 클릭
  2) 아래쪽 MySQL community (GPL) Downloads 링크 클릭
    (또는 https://www.mysql.com/downloads/ 접속)
  3) Connector/J 링크 클릭
  4) Archive 메뉴 클릭
  5) Operation System 항목에서 Platform Independent 클릭
  6) ZIP 파일 다운로드 후 압축 풀기
  
2. 압축 푼 폴더 내의 mysql-connector-java-x.x.xx.jar 파일 사용법
  1) 특정 프로젝트에서만 사용할 경우
    - 이클립스 내의 프로젝트명 -> src -> main -> webapp -> WEB-INF -> lib 폴더에 복사
    - 해당 파일 우클릭 - Build Path - Add to Build Path 선택
    - 프로젝트 내의 Referenced Libraries 항목에 mysql-connector-java-x.x.xx.jar 파일 추가 확인
  2) 모든 프로젝트에서 해당 jar 파일을 사용할 경우
    - 이클립스에서 설정된 JRE 폴더 열기 (Window 메뉴 - Preferences - Java - Installed JREs 메뉴에서 확인 및 설정)
    - 해당 JRE 폴더 - lib - ext 폴더 내에 mysql-connector-java-x.x.xx.jar 파일 복사
    - 이클립스 재시작 후 프로젝트 내의 JRE System Libraries 항목에 mysql-connector-java-x.x.xx.jar 파일 추가 확인
```
```java
[ JDBC 를 활용한 MySQL 연동(사용) 방법 - 4단계
1단계. JDBC 드라이버 로드
  - java.lang 패키지의 Class 클래스의 static 메서드 forName() 를 호출하여
    드라이버 클래스가 위치한 패키지명과 클래스명을 문자열로 전달
  - com.mysql.jdbc 패키지 내의 Driver.class 파일을 지정해야함
    => "com.mysql.jdbc.Driver" 형식으로 지정(주의! .class 생략)
    
    < 문법 >
    Class.forName("드라이버클래스")
    ex) Class.forName("com.mysql.jdbc.Driver");
    => 주의! 드라이버 클래스 위치 또는 파일명이 틀렸을 경우(= 파일이 없을 경우) 오류 발생
    java.lang.ClassNotFoundException: com.mysql.jdbc.Drive
    => com.mysql.jdbc.Drive 클래스를 찾지 못해서 발생한 오류라는 의미 

2단계. DB 연결(접속)
- java.sql 패키지의 DriverManager 클래스의 static 메서드인 getConnection() 호출
- 파라미터로 DB 접속 URL, 계정명, 패스워드를 전달(문자열)
  => URL 형식 : "jdbc:mysql://접속할주소:포트번호/DB명"
     ex) "jdbc:mysql://localhost:3306/study_jsp3"
     
---------------------------------------------------------------------------- 
포트번호 :
- 특정 프로토콜을 사용하여 외부와 통신을 할 때 사용하는 번호(= 출입구)
- 반드시 하나의 서비스 당 하나의 포트만 사용 가능하며 현재 사용중인 포트를 다른 서비스가 중복 사용 불가능(= 충돌 발생)
- 0 ~ 65535 번까지의 포트가 제공됨

[ Well-known 포트 ]
HTTP(웹) : 80
HTTPS(웹보안) : 443
FTP(파일전송) : 21, 20
TELNET(원격접속) : 23
SSH(원격접속-보안) : 22
----------------------------------------------------------------------------     
    
  < 문법 >
  DriverManager.getConnection("URL", "계정명", "패스워드");
