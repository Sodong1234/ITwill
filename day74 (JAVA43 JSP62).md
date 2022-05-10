# [오전수업] JAVA 43차
## 간단한 계산기 제작

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity"
    android:orientation="vertical"
    android:padding="10dp">

    <EditText
        android:id="@+id/edit1"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:textSize="30sp"
        android:hint="숫자1"
        android:inputType="number"/>

    <EditText
        android:id="@+id/edit2"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:textSize="30sp"
        android:hint="숫자2"
        android:inputType="number"/>

    <Button
        android:id="@+id/btnAdd"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="더하기"
        android:textSize="30sp"/>

    <Button
        android:id="@+id/btnSub"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="빼기"
        android:textSize="30sp"/>

    <Button
        android:id="@+id/btnMul"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="곱하기"
        android:textSize="30sp"/>

    <Button
        android:id="@+id/btnDiv"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="나누기"
        android:textSize="30sp"/>

    <EditText
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="계산결과 : "
        android:textSize="30sp"
        android:id="@+id/tvResult"
        android:textColor="#FF0000"/>

</LinearLayout>
```
```java
package com.example.and0510_basicwidget_simplecalculator;

import androidx.appcompat.app.AppCompatActivity;

import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.EditText;
import android.widget.TextView;
import android.widget.Toast;

public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        // 위젯 ID 가져와서 객체 연결하기
        EditText edit1 = findViewById(R.id.edit1);
        EditText edit2 = findViewById(R.id.edit2);

        Button btnAdd = findViewById(R.id.btnAdd);
        Button btnSub = findViewById(R.id.btnSub);
        Button btnMul = findViewById(R.id.btnMul);
        Button btnDiv = findViewById(R.id.btnDiv);

        TextView tvResult = findViewById(R.id.tvResult);

        // 4개의 버튼에 리스너(OnClickListener) 연결하여 이벤트 처리
       View.OnClickListener btnListener = new View.OnClickListener() {
           @Override
           public void onClick(View view) {
               // EditText(edit1, edit2)에 입력된 숫자 2개 가져와서 정수로 변환
               // 연산하여 결과를 TextView(tvResult)에 출력
               // 단, 숫자가 입력되지 않았을 경우 오류 메세지 출력 및 커서 요청
               if (edit1.length() == 0) {
                   Toast.makeText(MainActivity.this, "숫자1 입력 필수!", Toast.LENGTH_SHORT).show();
                   edit1.requestFocus();
                   return;
               } else if (edit2.length() == 0) {
                   Toast.makeText(MainActivity.this, "숫자2 입력 필수!", Toast.LENGTH_SHORT).show();
                   edit2.requestFocus();
                   return;
               }

               int num1 = Integer.parseInt(edit1.getText().toString());
               int num2 = Integer.parseInt(edit2.getText().toString());

               // if문으로 버튼 판별
//               if(view.getId() == R.id.btnAdd) {
//                   Toast.makeText(MainActivity.this, "더하기버튼 클릭!", Toast.LENGTH_SHORT).show();
//               } else if(view.getId() == R.id.btnSub) {
//                   Toast.makeText(MainActivity.this, "빼기버튼 클릭!", Toast.LENGTH_SHORT).show();
//               }

               // 연산 결과를 저장할 변수
               int result = 0;
               // switch - case 문으로 버튼 판별
               switch (view.getId()) {
                   case R.id.btnAdd:
                       result = num1 + num2;
                       break;
                   case R.id.btnSub:
                       result = num1 - num2;
                       break;
                   case R.id.btnMul:
                       result = num1 * num2;
                       break;
                   case R.id.btnDiv:

                       // 나눗셈의 경우 두번째 피연산자가 0이면 ArithmeticException 발생
                       if (num2 == 0) {
                           Toast.makeText(MainActivity.this, "나눗셈에 0 사용 금지", Toast.LENGTH_SHORT).show();;
                           edit2.requestFocus();
                           tvResult.setText("계산 결과 : 오류 발생!");
                           return;
                       }

                       result = num1 / num2;
                       break;
               }

               tvResult.setText("계산결과 : " + result);

           }
       };

      btnAdd.setOnClickListener(btnListener);
      btnSub.setOnClickListener(btnListener);
      btnMul.setOnClickListener(btnListener);
      btnDiv.setOnClickListener(btnListener);

    }




}
```

---

## 체크박스

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity"
    android:orientation="vertical">

    <CheckBox
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:id="@+id/cbAndroid"
        android:text="안드로이드폰"
        android:textSize="30sp"/>

    <CheckBox
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:id="@+id/cbIPhone"
        android:text="아이폰"
        android:textSize="30sp"/>

    <CheckBox
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:id="@+id/cbWindows"
        android:text="윈도우폰"
        android:textSize="30sp"/>

    <CheckBox
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:id="@+id/cbAll"
        android:text="전체 선택"
        android:textSize="30sp"/>

    <Button
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="확인"
        android:id="@+id/btnMix"/>
</LinearLayout>
```
```java
package com.example.and0510_basicwidget_checkbox;

import androidx.appcompat.app.AppCompatActivity;

import android.annotation.SuppressLint;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.CheckBox;
import android.widget.CompoundButton;
import android.widget.Toast;

public class MainActivity extends AppCompatActivity {

    CheckBox cbAndroid, cbIPhone, cbWindows, cbAll;
    Button btnMix;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

       cbAndroid = findViewById(R.id.cbAndroid);
       cbIPhone = findViewById(R.id.cbIPhone);
       cbWindows = findViewById(R.id.cbWindows);
       cbAll = findViewById(R.id.cbAll);

       btnMix = findViewById(R.id.btnMix);

//       // 체크박스 1개를 대상으로 체크 상태 변화를 감지하는 리스너 연결
//        cbAndroid.setOnCheckedChangeListener(new CompoundButton.OnCheckedChangeListener() {
//            @Override
//            public void onCheckedChanged(CompoundButton compoundButton, boolean isChecked) {
//                if(isChecked) {
//                    Toast.makeText(MainActivity.this, "안드로이드폰 체크됨!", Toast.LENGTH_SHORT).show();
//                } else {
//                    Toast.makeText(MainActivity.this, "안드로이드폰 체크해제됨!", Toast.LENGTH_SHORT).show();
//                }
//
//            }
//        });

        CompoundButton.OnCheckedChangeListener cbListener= new CompoundButton.OnCheckedChangeListener() {
            @Override
            public void onCheckedChanged(CompoundButton compoundButton, boolean isChecked) {
//                if(isChecked) {
//                    Toast.makeText(MainActivity.this, compoundButton.getText() + "체크됨", Toast.LENGTH_SHORT).show();
//                } else {
//                Toast.makeText(MainActivity.this, compoundButton.getText() + "체크 해제됨", Toast.LENGTH_SHORT).show();
//            }

                // 만약, 현재 리스너를 호출한 체크박스가 '전체 선택' 일 경우
                // 해당 체크박스 상태가 체크 상태이면 세 가지 체크박스를 모두 체크하고,
                //        ""         체크 해제 상태이면      ""          체크해제

                if(compoundButton.getId() == R.id.cbAll) {

                    CheckBox[] checkBoxes = {cbAndroid, cbIPhone, cbWindows};
                    for(CheckBox ch : checkBoxes) {
                        ch.setChecked(isChecked);
                    }

                    cbAndroid.setChecked(isChecked);
                    cbIPhone.setChecked(isChecked);
                    cbWindows.setChecked(isChecked);

//                    if (isChecked) { // 전체 체크
//                        cbAndroid.setChecked(true);
//                        cbIPhone.setChecked(true);
//                        cbWindows.setChecked(true);
//                    } else { // 전체해제
//                        cbAndroid.setChecked(false);
//                        cbIPhone.setChecked(false);
//                        cbWindows.setChecked(false);
//                    }
                }
            }
        };

        cbAndroid.setOnCheckedChangeListener(cbListener);
        cbIPhone.setOnCheckedChangeListener(cbListener);
        cbWindows.setOnCheckedChangeListener(cbListener);
        cbAll.setOnCheckedChangeListener(cbListener);


        // 1. Button을 하나 생성
        // 2. Button 클릭 시 현재 체크되어 있는 항목의 텍스트를 결합하여 출력
        // ex) 안드로이드폰과 아이폰 체크 시 : "안드로이드폰 / 아이폰" 출력

        btnMix.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {

                String items = ""; // 항목을 묶어서 저장할 변수
//                if(cbAndroid.isChecked()) {
//                    items += "/ " + cbAndroid.getText();
//                }
//                if(cbIPhone.isChecked()) {
//                    items += "/ " + cbIPhone.getText();
//                }
//                if(cbWindows.isChecked()) {
//                    items += "/ " + cbWindows.getText();
//                }

//                items = items.substring(3);
                CheckBox[] cbs = {cbAndroid, cbIPhone, cbWindows};
                for(CheckBox cb : cbs) {
                    if(cb.isChecked()) items += cb.getText() + " / ";
                }
                items = items.substring(0, items.length()-3);

                Toast.makeText(MainActivity.this, "선택된 항목 : " + items, Toast.LENGTH_SHORT).show();
            }
        });



    }


}
```

---

# [오후수업] JSP 62차

## 메일로 인증코드 보내기
> jsp11_board의 파일들을 사용
> 61차에서 수업했던 GenerateAuthenticationCode.java 파일을 jsp11_board 클래스에 복사함
> study_jsp3 DB 수정 

```
1. ALTER TABLE member ADD COLUMN auth_status CHAR(1)
2. UPDATE member SET auth_status='N';
3. CREATE TABLE member_auth_info 
(
   id VARCHAR(16) PRIMARY KEY,
   auth_code VARCHAR(50)
);
```

```java
---------------------------------------------login_pro.jsp---------------------------------------------
<%@page import="jsp11_board.MemberDAO"%>
<%@page import="jsp11_board.MemberDTO"%>
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%
// 아이디, 패스워드 파라미터 가져오기
String id = request.getParameter("id");
String passwd = request.getParameter("passwd");

MemberDAO dao = new MemberDAO();

// MemberDTO 객체에 id, passwd 저장 여부는 선택사항
// 1. MemberDTO 객체 사용 시
// MemberDTO member = new MemberDTO();
// member.setId(id);
// member.setPasswd(passwd);

// boolean isLoginSuccess = dao.login(member);

// 2. MemberDTO 객체 미사용 시
boolean isLoginSuccess = dao.login(id, passwd);

// 로그인 작업 수행 후 결과를 boolean 타입(isLoginSuccess) 으로 리턴받아 결과에 따른 판별 작업 수행
// 만약, 로그인 성공 시(isLoginSuccess 가 true) 세션에 아이디("sId" 속성명)를 저장 후 메인페이지로 이동
// 로그인 실패 시(isLoginSuccess 가 false) 자바스크립트를 통해 "로그인 실패" 출력 후 이전페이지로
if(isLoginSuccess) {
	// 로그인 성공 시 회원의 인증 여부를 조회
	// 미인증 회원일 경우 인증 수행 메세지 출력하고 이전페이지로 돌아가기
	// 아니면, main.jsp 페이지로 이동
	boolean isAuthenticatedMember = dao.getAuthenticationStatus(id);
	
	if(isAuthenticatedMember) { // 인증된 회원일 경우
	session.setAttribute("sId", id);
	response.sendRedirect("../main.jsp");
	} else { // 미인증 회원일 경우
		%>
		<script>
			alert("회원 인증 필수!");
			history.back();
		</script>
		<%
	}
	
	
	
} else {
	%>
	<script>
		alert("로그인 실패!");
		history.back();
	</script>
	<%
}
%>


---------------------------------------------MemberDAO.java---------------------------------------------
package jsp11_board;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;

public class MemberDAO {

	// 회원 가입을 수행하는 insert() 메서드 정의
	// => 파라미터 : MemberDTO 객체(member), 리턴타입 : int(insertCount)
	public int insert(MemberDTO member) {
		int insertCount = 0;

		Connection con = null;
		PreparedStatement pstmt = null;

		try {
			// 1단계 & 2단계
			// JdbcUtil 객체의 getConnection() 메서드를 호출하여 DB 연결 객체 가져오기
			con = JdbcUtil.getConnection();

			// 3단계. SQL 구문 작성 및 전달
			// => MemberDTO 객체에 저장된 아이디, 패스워드 이름, 이메일, 전화번호를 추가하고
			// 가입일(date)의 경우 데이터베이스에서 제공되는 now() 함수 사용하여 자동 생성
			String sql = "INSERT INTO member VALUES (?,?,?,?,?,now(),?)";
			pstmt = con.prepareStatement(sql);
			pstmt.setString(1, member.getId());
			pstmt.setString(2, member.getPasswd());
			pstmt.setString(3, member.getName());
			pstmt.setString(4, member.getEmail());
			pstmt.setString(5, member.getPhone());
			pstmt.setString(6, "N"); // 인증 여부 기본값("N") 설정

			// 4단계. SQL 구문 실행 및 결과 처리
			insertCount = pstmt.executeUpdate();

		} catch (SQLException e) {
			e.printStackTrace();
			System.out.println("SQL 구문 오류 발생! - insert()");
		} finally {
			// DB 자원 반환
			JdbcUtil.close(pstmt);
			JdbcUtil.close(con);
		}

		return insertCount;
	}

	// 로그인 작업 수행 후 결과를 boolean 타입으로 리턴하는 login() 메서드 정의
	public boolean login(String id, String passwd) {
		// 로그인 결과를 저장할 변수 isLoginSuccess 선언
		boolean isLoginSuccess = false;

		Connection con = null;
		PreparedStatement pstmt = null;
		ResultSet rs = null;

		try {
			// 1단계 & 2단계
			// JdbcUtil 객체의 getConnection() 메서드를 호출하여 DB 연결 객체 가져오기
			con = JdbcUtil.getConnection();

			// 3단계. SQL 구문 작성 및 전달
			// => member 테이블의 id 컬럼 조회(단, id 와 passwd 가 일치하는 레코드 조회)
			String sql = "SELECT id FROM member WHERE id=? AND passwd=?";
			pstmt = con.prepareStatement(sql);
			pstmt.setString(1, id);
			pstmt.setString(2, passwd);

			// 4단계. SQL 구문 실행 및 결과 처리
			rs = pstmt.executeQuery();
			// 조회 결과가 존재할 경우(= rs.next() 가 true 일 경우)
			// isLoginSuccess 변수값을 true 로 변경
			if (rs.next()) {
				isLoginSuccess = true;
			}

		} catch (SQLException e) {
			e.printStackTrace();
			System.out.println("SQL 구문 오류 - login()");
		} finally {
			// 자원 반환
			JdbcUtil.close(rs);
			JdbcUtil.close(pstmt);
			JdbcUtil.close(con);
		}

		return isLoginSuccess;
	}

	// 회원의 인증 상태를 조회하여 리턴하는 getAuthenticationStatus() 메서드 정의
	// => 파라미터 : 아이디(id)
	public boolean getAuthenticationStatus(String id) {
		boolean isAuthenticatedMember = false;

		// member 테이블의 auth_status 컬럼 조회 후 "Y" 일 경우
		// isAuthenticatedMember 값을 true 로 변경

		Connection con = null;
		PreparedStatement pstmt = null;
		ResultSet rs = null;

		try {
			// 1단계 & 2단계
			// JdbcUtil 객체의 getConnection() 메서드를 호출하여 DB 연결 객체 가져오기
			con = JdbcUtil.getConnection();

			// 3단계. SQL 구문 작성 및 전달
			// => member 테이블의 id 컬럼 조회(단, id 와 passwd 가 일치하는 레코드 조회)
			String sql = "SELECT auth_status FROM member WHERE id=?";
			pstmt = con.prepareStatement(sql);
			pstmt.setString(1, id);

			// 4단계. SQL 구문 실행 및 결과 처리
			rs = pstmt.executeQuery();
			// 조회 결과가 존재할 경우(= rs.next() 가 true 일 경우)
			// isLoginSuccess 변수값을 true 로 변경
			if (rs.next()) {
				if (rs.getString(1).equals("Y")) {
					isAuthenticatedMember = true;
				}
			}

		} catch (SQLException e) {
			e.printStackTrace();
			System.out.println("SQL 구문 오류 - getAuthenticationStatus()");
		} finally {
			// 자원 반환
			JdbcUtil.close(rs);
			JdbcUtil.close(pstmt);
			JdbcUtil.close(con);
		}

		return isAuthenticatedMember;
	}

	// 인증 코드 등록 작업을 수행하는 insertAuthInfo() 메서드 정의
	// => 파라미터 : 아이디(id), 인증코드(code) 리턴값 없음
	public void insertAuthInfo(String id, String code) {
		// 전달받은 ID 에 대한 기존 인증코드가 존재하는지 조회

		Connection con = null;
		PreparedStatement pstmt = null, pstmt2 = null;
		ResultSet rs = null;

		try {
			con = JdbcUtil.getConnection();

			String sql = "SELECT auth_code FROM member_auth_info WHERE id=?";
			pstmt = con.prepareStatement(sql);
			pstmt.setString(1, id);

			rs = pstmt.executeQuery();
			if (rs.next()) {
				// 1) 인증코드 존재 시 : UPDATE 구문을 사용하여 새 인증코드로 갱신
				sql = "UPDATE member_auth_info SET auth_code=? WHERE id=?";
				pstmt2 = con.prepareStatement(sql);
				pstmt2.setString(1, code);
				pstmt2.setString(2, id);
				pstmt2.executeUpdate();
			} else {
				// 2) 인증코드 미 존재 시 : INSERT 구문을 사용하여 새 인증코드 등록
				sql = "INSERT INTO member_auth_info VALUES(?,?)";
				pstmt2 = con.prepareStatement(sql);
				pstmt2.setString(1, id);
				pstmt2.setString(2, code);
				pstmt2.executeUpdate();
			}
		} catch (SQLException e) {
			e.printStackTrace();
			System.out.println("SQL 구문 오류 - getAuthenticationStatus()");
		} finally {
			// 자원 반환
			JdbcUtil.close(rs);
			JdbcUtil.close(pstmt);
			JdbcUtil.close(con);
		}

	}

	// 인증 작업을 수행하는 selectAuthInfo() 메서드 정의
	public int selectAuthInfo(String id, String code) {
		int result = 0;

		Connection con = null;
		PreparedStatement pstmt = null, pstmt2 = null;
		ResultSet rs = null;

		try {
			con = JdbcUtil.getConnection();

			// 1. 기존 인증코드 조회

			String sql = "SELECT auth_status FROM member_auth_info WHERE id=?";
			pstmt = con.prepareStatement(sql);
			pstmt.setString(1, id);

			rs = pstmt.executeQuery();

			if (rs.next()) {
				// 1) 인증코드 존재 시 : 인증코드 비교
				if (code.equals(rs.getString("auth_code"))) { // 일치
					result = 1;
				}
			} else {
				// 2) 인증코드 미 존재 시 : 리턴값을 -1 로 설정
				result = -1;
			}

		} catch (SQLException e) {
			e.printStackTrace();
			System.out.println("SQL 구문 오류 - getAuthenticationStatus()");
		} finally {
			JdbcUtil.close(rs);
			JdbcUtil.close(pstmt);
			JdbcUtil.close(con);
		}
		return result;
	}
}


---------------------------------------------join_pro.jsp---------------------------------------------
<%@page import="jsp11_board.MemberDAO"%>
<%@page import="jsp11_board.MemberDTO"%>
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%
// POST 방식 한글 처리
request.setCharacterEncoding("UTF-8");

// 폼 파라미터 가져오기
String id = request.getParameter("id");
String passwd = request.getParameter("passwd");
String name = request.getParameter("name");
String email = request.getParameter("email");
String phone = request.getParameter("phone");

// 데이터를 MemberDTO 객체에 저장
// MemberDTO member = new MemberDTO();
// member.setId(id);
// member.setPasswd(passwd);
// member.setName(name);
// member.setEmail(email);
// member.setPhone(phone);

// 만약, 파라미터 생성자를 정의했을 경우(생성자 파라미터에 저장할 데이터 전달)
MemberDTO member = new MemberDTO(id, passwd, name, email, phone);

// MemberDAO 클래스 객체 생성 후 insert() 메서드 호출
// => 파라미터 : MemberDTO 객체(member), 리턴타입 : int(insertCount)
MemberDAO dao = new MemberDAO();
int insertCount = dao.insert(member);

// insertCount 가 0보다 크면 main.jsp 페이지로 이동하고
// 아니면, 자바스크립트를 통해 "회원 가입 실패!" 출력 후 이전페이지로 돌아가기
if(insertCount > 0) {
	// 절대경로로 지정할 경우
	// request.getContextPath() 메서드(프로젝트명까지의 경로 리턴)를 활용하여 이동 경로 설정
// 	response.sendRedirect(request.getContextPath()); // http://localhost:8080/StudyJSP/
// 	response.sendRedirect(request.getContextPath() + "/jsp11_board/main.jsp");

	// 상대경로로 지정할 경우
// 	response.sendRedirect("../main.jsp");
	
	// ---------------------------------------------------------------------------------------------
	// 회원 가입 성공 시 인증 메일 발송을 위한 send_authentication_code.jsp 페이지로 이동
	// => 파라미터로 아이디(id)와 이메일주소(email) 전송
	response.sendRedirect("./send_authentication_code.jsp?id=" + id + "&email=" + email);
	
} else {
	%>
	<script>
		alert("회원 가입 실패!");
		history.back();
	</script>
	<%
}

%>    



---------------------------------------------send_authentication_code.jsp---------------------------------------------
<%@page import="javax.mail.Transport"%>
<%@page import="java.util.Date"%>
<%@page import="javax.mail.internet.MimeMessage.RecipientType"%>
<%@page import="javax.mail.internet.InternetAddress"%>
<%@page import="javax.mail.Address"%>
<%@page import="javax.mail.internet.MimeMessage"%>
<%@page import="javax.mail.Message"%>
<%@page import="javax.mail.Session"%>
<%@page import="jsp14_java_mail.GoogleSMTPAuthenticator"%>
<%@page import="javax.mail.Authenticator"%>
<%@page import="java.util.Properties"%>
<%@page import="jsp11_board.MemberDTO"%>
<%@page import="jsp11_board.MemberDAO"%>
<%@page import="jsp11_board.GenerateAuthenticationCode"%>
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%
// URL 을 통해 전달받은 아이디(id), 이메일 주소(email) 가져와서 변수에 저장
String id = request.getParameter("id");
// String email = request.getParameter("email");
// out.println(email);

// 50자리 인증코드를 생성하여 가입 시 입력한 메일 주소로 인증 코드 발송
GenerateAuthenticationCode genCode = new GenerateAuthenticationCode();
String code = genCode.getAuthenticationCode();

request.setCharacterEncoding("UTF-8");

String sender = "admin@itwillbs.co.kr";
String receiver = request.getParameter("email");
String title = "회원 가입 인증 메일입니다.";
String content = "<h3>인증하려면 아래 링크를 클릭하세요.</h3>" 
				+ "<a href='http://localhost:8080/StudyJSP/jsp11_board/member/member_authentication.jsp?id=" + id + "&code=" + code + "'>인증하기</a>";


try {
	Properties properties = System.getProperties();
	properties.put("mail.smtp.starttls.enable", "true");
	properties.put("mail.smtp.ssl.protocols", "TLSv1.2");
	properties.put("mail.smtp.host", "smtp.gmail.com"); 
	properties.put("mail.smtp.auth", "true"); 
	properties.put("mail.smtp.port", "587"); 

	Authenticator authenticator = new GoogleSMTPAuthenticator(request); 
	
	Session mailSession = Session.getDefaultInstance(properties, authenticator);

	Message mailMessage = new MimeMessage(mailSession);

	Address sender_address = new InternetAddress(sender, "아이티윌"); 
	Address receiver_address = new InternetAddress(receiver);
	mailMessage.setHeader("content-type", "text/html; charset=UTF-8");
	mailMessage.setFrom(sender_address);
	mailMessage.addRecipient(RecipientType.TO, receiver_address);
	mailMessage.setSubject(title);
	mailMessage.setContent(content, "text/html; charset=UTF-8");
	mailMessage.setSentDate(new Date());

	Transport.send(mailMessage);
	out.println("<h3>메일이 정상적으로 전송되었습니다!</h3>");
	
	// ------------------------------------------------------------------------------------------
	// 인증 코드를 member_auth_info 테이블에 추가하기 위해
	// MemberDAO 객체의 insertAuthInfo() 메서드를 호출
	// => 파라미터 : 아이디, 인증코드		리턴타입 : void
	MemberDAO dao = new MemberDAO();
	dao.insertAuthInfo(id, code);
	// ------------------------------------------------------------------------------------------
	response.sendRedirect("join_success.jsp");
} catch(Exception e) {
	e.printStackTrace();
	out.println("<h3>SMTP 서버 설정 또는 서비스 문제 발생!</h3>");
}


%>


---------------------------------------------join_success.jsp---------------------------------------------
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<h1>회원 가입을 축하합니다.</h1>
	<h5>인증 메일을 확인하여 회원 인증을 수행하세요.</h5>
	<input type="button" value="홈으로" onclick="location.href='../main.jsp'">
	<input type="button" value="로그인" onclick="location.href='./login_form.jsp'">	
	
</body>
</html>

---------------------------------------------member_authentication.jsp---------------------------------------------
