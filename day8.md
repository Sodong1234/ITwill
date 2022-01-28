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

## 실행
systemctl start mysqld로 실행 -> systemctl status mysqld로 실행됐는지 확인 -> grep 'temporary password' /var/log/mysqld.log 로 비밀번호 확인  


