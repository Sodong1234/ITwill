# [오전수업]

# 데이터베이스란?
- 대량의 데이터를 보관하는 소프트웨어 → 프로그램 / 웹 app에서 사용
- 데이터의 백업, 관리, 연산, 보안의 기능
- 저장하는 데이터의 종류(정형, 비정형), 규모에 따라 사용 데이터베이스를 결정
  - 정형 (글자, 문자, 숫자 등 눈에 보이는 것) / 비정형 (영상, 이미지, 음향, 음성정보 등 완벽히 정형화 되어 있지 않은 것)
- 정형(Text)의 데이터를 다루는 DB(관계형 데이터베이스(RDBMS)) → SQL 언어를 통해서 사용
  - 주로 사용하는 RDBMS의 종류 : Oracle(빠름, 대규모) / MySQL(중규모)


<center><img src="https://user-images.githubusercontent.com/95197594/150445471-388f7525-f54b-47b1-a1c5-84c91ff57b9c.JPG" width="60%" height="60%"></center>


- 주로 사용하는 서버용 운영체제(OS) - Wiodnws(비쌈) / Linus(오픈소스, 무료/상용)
  - Linus 오픈소스는 GPL이라는 라이센스가 존재, 개발자가 오픈된 소스로 무엇인가를 개발하였을 경우 그것을 오픈시켜 공개해야함. 

---

# MySQL 설치
## 윈도우즈에 설치하기
1. https://downloads.mysql.com/archives/installer/
  -        현재 최신 버전에서는 한국어 윈도우에서 GUI 원격 접속 시 문제가 있으므로 구버전(8.0.22)으로 설치한다.
2. execute로 실행 필요 파일 받기, 일부 설치가 불가능한 요소가 있는데 현재 파이썬이나 비주얼 스튜디오가 없는 상태이므로 그대로 설치 진행
3. MySQL의 약속된 포트번호는 3306.(기본설정이니 건드릴 필요 없음)
4. Account and Roles 탭에서 Root(관리자) 계정의 비밀번호는 편의상 1234로 사용.
5. Connect To server 탭에서 패스워드는 1234 입력.

## DB 접속하기
### TUI(CUI)
- 터미널 환경에서 데이터베이스에 접속하는 방법
- 명령어로만 작업을 수행한다.
- cmd -> mysql -u root -p 입력 -> 비밀번호 입력

      ■ 환경변수 설정
      - 터미널의 어느 위치에서건 프로그램에 접근할 수 있도록 해주는 설정값.
      - 시스템 변수 -> Path -> 새로만들기 -> C:\Program Files\MySQL\MySQL Server 8.0\bin 추가 -> 확인

- 접속 성공 화면
  <center><img src="https://user-images.githubusercontent.com/95197594/150458829-6484c4fc-ac5f-43b4-93c9-71077dafef8b.png" width="60%" height="60%"></center>
  
  
- 사용법 예시  
  <center><img src="https://user-images.githubusercontent.com/95197594/150459691-54152f7f-e570-4e23-bc8c-0f5bf6b8d133.JPG" width="60%" height="60%"></center>


### GUI
- Workbench로 실행.

- 사용법 예시
<center><img src="https://user-images.githubusercontent.com/95197594/150460384-9771f3a2-dc2b-47aa-812e-36528d339380.png" width="60%" height="60%"></center>





## 리눅스에 설치하기
- CentOS 7 → VM 환경으로 실습

### Virtual box 설치
- https://www.virtualbox.org/wiki/Downloads -> VirtualBox 6.1.32 platform packages ->  Windows hosts 클릭하여 설치

### Cent OS 설치
- http://mirror.kakao.com/centos/7.9.2009/isos/x86_64/ -> CentOS-7-x86_64-Minimal-2009.iso 설치


---

# [오후수업]
- 서버(Server) - 정보를 제공
- 클라이언트(Client) - 정보를 요청

## 이클립스 서버 설정
```
  1. Windows -> Preferences -> Server -> Runtime Environment -> Add -> Apache -> Apache Tomcat v8.5 -> Next -> Browse -> Tomcat 8.5 폴더 지정 -> Finish
  2. 아랫쪽 Servers -> new server -> Tomcat v8.5 Server 지정 -> Finish
  3. 설정한 서버 더블클릭 -> Ports -> Tomcat admin port를 8005로 지정
  4. Tomcat v8.5 마우스 오른쪽 - Start로 서버 실행.
  5. 왼쪽 빈 공간에 마우스 오른쪽 클릭 -> New -> Dynamic Web Project -> Target runtime을 Apache Tomcat v8.5로 설정 -> Finish
  6. 방금 만든 Project 마우스 오른쪽 -> New -> JSP File 생성
  7. <h1>Hello, World!</h1> 작성 및 Run.
```

-> <center><img src="https://user-images.githubusercontent.com/95197594/150476387-3b0ad9cb-d684-4b1c-9401-e0bf9ea63d84.JPG" width="60%" height="60%"></center>







