# [오전수업] JAVA 57차
## Dialog

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity"
    android:orientation="vertical"
    android:gravity="center">

    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:id="@+id/btnDialog"
        android:text="다이얼로그 표시"
        android:textSize="20sp"
        android:onClick="showDialog"/>

    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:id="@+id/btnDialog2"
        android:text="다이얼로그 표시2"
        android:textSize="20sp"
        android:onClick="showDialog2"/>


</LinearLayout>
```
```java
package com.example.and0614_dialog;

import androidx.appcompat.app.AppCompatActivity;

import android.app.AlertDialog;
import android.content.DialogInterface;
import android.os.Bundle;
import android.view.View;
import android.widget.Toast;

public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        /*
         *  대화상자(Dialog)
         *  - 사용자에게 알림을 전달하거나, 사용자로부터 상호작용을 수행하는 별도의 창
         *  - AlertDialog.Builder 클래스 사용하여 다이얼로그 생성하고,
         *  setXXX() 메서드를 사용하여 다이얼로그 설정하고,
         *  show() 메서드를 사용하여 다이얼로그를 표시
         *  */
    }

    public void showDialog(View v) {
        // 버튼 클릭 시 다이얼로그 표시
        // 1. AlertDialog.Builder 클래스를 활용하여 다이얼로그 객체 생성
        // => 파라미터로 Context 객체 전달 (this 또는 MainActivity.this 또는 getAplicationContext())
        AlertDialog.Builder dialog = new AlertDialog.Builder(this);

        // 2. AlertDialog.Builder 객체의 setXXX() 메서드를 호출하여 다이얼로그 설정 작업 수행
        // 2-1. 다이얼로그에 표시할 제목 설정
        dialog.setTitle("다이얼로그 제목");
        // 2-2. 다이얼로그에 표시할 내용 설정
        dialog.setMessage("다이얼로그 본문");
        // 2-3. 아이콘 설정
        dialog.setIcon(R.mipmap.ic_launcher_round);

        // 2-4. 다이얼로그에 버튼 부착 (기본 버튼 3개 제공됨)
        // => 메서드에 따른 버튼 역할의 차이는 없으며, 관례적인 의미에 맞게 사용
//        dialog.setPositiveButton("확인", null);   // 긍정의미
//        dialog.setNegativeButton("취소", null);   // 부정의미
//        dialog.setNeutralButton("중립버튼", null); // 중립

        // 다이럴로그 버튼에 이벤트 연결
        // => 일반 Button 객체의 OnClickListener 와 동일한 방법으로 이벤트 처리 가능
        //     단, DialogInterface.OnclickListener 사용하는 점이 다름
        dialog.setPositiveButton("확인", new DialogInterface.OnClickListener() {
            @Override
            public void onClick(DialogInterface dialogInterface, int i) {
                Toast.makeText(MainActivity.this, "확인 버튼 누름!", Toast.LENGTH_SHORT).show();
            }
        });

        // 3. AlertDialog.Builder 객체의 show() 메서드를 호출하여 다이얼로그 표시
        dialog.show();
    }

    // showDialog2() 메서드 정의
    // 제목 : 영화 정보
    // 내용 : 영화 줄거리...............
    // 아이콘 : 영화 포스터 이미지
    // 버튼 : 상세보기        (취소) 확인

    public void showDialog2(View v) {
        AlertDialog.Builder dialog = new AlertDialog.Builder(this);

        dialog.setTitle("마더");
        dialog.setMessage("줄거리 모름");
        dialog.setIcon(R.drawable.mov19);

        // 4단계로 구현된 리스너 연결 시
        dialog.setNeutralButton("상세보기", dialogBtnListener);
        dialog.setNegativeButton("취소", dialogBtnListener);
        dialog.setPositiveButton("확인", dialogBtnListener);

        dialog.show();
    }

    // 다이얼로그 내의 버튼에 리스너 연결 4단계 구현 시
    DialogInterface.OnClickListener dialogBtnListener = new DialogInterface.OnClickListener() {
        @Override
        public void onClick(DialogInterface dialogInterface, int i) {
            Toast.makeText(MainActivity.this, i + "", Toast.LENGTH_SHORT).show();
            // 파라미터로 전달받은 i 값을 사용하여 확인 버튼과 상세보기 버튼 구별
            // PositiveButton : -1
            // NegativeButton : -2
            // NeutralButton  : -3
            
            switch (1) {
                case -1:
                    Toast.makeText(MainActivity.this, "확인 클릭됨!", Toast.LENGTH_SHORT).show();
                    break;
                case -2:
                    Toast.makeText(MainActivity.this, "취소 클릭됨!", Toast.LENGTH_SHORT).show();
                    break;
                case -3:
                    Toast.makeText(MainActivity.this, "상세보기 클릭됨!", Toast.LENGTH_SHORT).show();
                    break;
            }
        }
    };

}
```

## Dialog 2

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity"
    android:orientation="vertical">

    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:id="@+id/btnListDialog"
        android:text="목록 다이얼로그 표시"
        android:textSize="20sp"
        android:onClick="showListDialog"/>

    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:id="@+id/btnRadioDialog"
        android:text="라디오버튼 목록 다이얼로그 표시"
        android:textSize="20sp"
        android:onClick="showRadioListDialog"/>

    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:id="@+id/btnCheckListDialog"
        android:text="체크박스 목록 다이얼로그 표시"
        android:textSize="20sp"
        android:onClick="showCheckListDialog"/>

</LinearLayout>
```
```java
package com.example.and0614_dialog2;

import androidx.appcompat.app.AppCompatActivity;

import android.app.AlertDialog;
import android.content.DialogInterface;
import android.os.Bundle;
import android.view.View;
import android.widget.Toast;

public class MainActivity extends AppCompatActivity {

    int selectedItem = -1;  // 선택 항목에 대한 인덱스를 저장할 변수 선언

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
    }
//  showListDialog
    public void showListDialog(View v) {
        AlertDialog.Builder dialog = new AlertDialog.Builder(this);
                
        dialog.setTitle("좋아하는 과목은?");
        dialog.setIcon(R.mipmap.ic_launcher_round);
     
        // 목록 대화상자 출력을 위해 목록으로 사용할 데이터를 배열로 생성
        String[] listItems = {"Java", "JSP", "Android", "SPRING"};

        // AlertDialog.Builder 객체의 setItems() 메서드를 호출하여 목록 생성
        // => 파라미터 : 목록으로 사용할 String 배열, 리스너 객체
        //    => setXXXButton의 리스너와 동일한 객체 구현하여 이벤트 처리
        dialog.setItems(listItems, new DialogInterface.OnClickListener() {
            @Override
            public void onClick(DialogInterface dialogInterface, int i) {
                // 파라미터로 전달되는 i 값을 사용하여 목록 중 선택된 항목을 판별
//                Toast.makeText(MainActivity.this, i + "", Toast.LENGTH_SHORT).show();
                // => i 값은 배열 인덱스와 동일한 순서로 전달되므로 배열 항목의 인덱스로 활용
                Toast.makeText(MainActivity.this, listItems[i], Toast.LENGTH_SHORT).show();
                selectedItem = i;
            }
        });
        
        dialog.setPositiveButton("확인", null);
        dialog.show();
    }
//  showRadioListDialog
    public void showRadioListDialog(View v) {
        AlertDialog.Builder dialog = new AlertDialog.Builder(this);

        dialog.setTitle("좋아하는 과목은?");
        dialog.setIcon(R.mipmap.ic_launcher_round);

        String[] listItems = {"Java", "JSP", "Android", "SPRING"};
        
        // AlertDialog.Builder 객체의 setSingleChoiceItems() 메서드를 호출하여
        // 라디오버튼 형태의 단일 선택 목록 생성
        // => 파라미터 : 목록으로 사용할 String 배열, 초기 선택 항목 인덱스, 리스너 객체
        //    => setXXXButton 의 리스너와 동일한 객체 구현하여 이벤트 처리
        dialog.setSingleChoiceItems(listItems, 0, new DialogInterface.OnClickListener() {
            @Override
            public void onClick(DialogInterface dialogInterface, int i) {
//                Toast.makeText(MainActivity.this, listItems[i], Toast.LENGTH_SHORT).show();
                selectedItem = i;
            }
        });

        // 확인 버튼 클릭 시 선택된 라디오버튼의 항목 출력
        // => 라디오버튼 선택 시 이벤트 처리에서 저장된 selectedItem 변수값 활용
        dialog.setPositiveButton("확인", (dialogInterface, i) -> {
            Toast.makeText(MainActivity.this, listItems[selectedItem] + " 선택됨", Toast.LENGTH_SHORT).show();
        });
        
        dialog.show();
    }
//  showCheckListDialog
    public void showCheckListDialog(View v) {
        AlertDialog.Builder dialog = new AlertDialog.Builder(this);

        dialog.setTitle("좋아하는 과목은?");
        dialog.setIcon(R.mipmap.ic_launcher_round);

        String[] listItems = {"Java", "JSP", "Android", "SPRING"};
        boolean[] checkItems = {true, true, false, true};

        // AlertDialog.Builder 객체의 setMultiChoiceItems() 메서드 호출하여
        // 체크박스 형태의 다중 선택 목록 생성
        // => 파라미터 : 목록으로 사용할 String 배열, 초기 선택 항목 boolean 배열, 리스너 객체
        //    => OnMultiChoiceClickListener 구현하여 이벤트 처리
        dialog.setMultiChoiceItems(listItems, checkItems, new DialogInterface.OnMultiChoiceClickListener() {
            @Override
            public void onClick(DialogInterface dialogInterface, int i, boolean isChecked) {
                // 특정 항목(i)의 체크 상태(isChecked)를 boolean 타입 배열에 저장
                checkItems[i] = isChecked;
            }
        });

        // 확인 버튼 클릭 시 선택된 라디오 버튼의 항목 출력
        dialog.setPositiveButton("확인", new DialogInterface.OnClickListener() {
            @Override
            public void onClick(DialogInterface dialogInterface, int index) {
                // checkItems 배열의 체크 여부를 확인하여 체크상태(true)인 항목의 내용(listItems)을
                // 문자열로 결합하여 출력
                String items = "";

                // 반복문을 사용하여 체크 상태 여부를 배열로부터 확인 후
                // 아이템 항목의 인덱스를 통해 문자열 결합
                for(int i = 0; i < checkItems.length; i++) {
                    if(checkItems[i]) {
                        items += listItems[i] + " ";
                    }
                }

                Toast.makeText(MainActivity.this, items + " 체크됨", Toast.LENGTH_SHORT).show();
            }
        });
        dialog.show();
    }


}

```
---

# [오후수업] JSP 82차

> - Maven Repository에서 JSON.simply 1.1.1 / Gson 2.9.0 버전 라이브러리를 pom.xml에 추가
> - pom.xml springframework-version을 3.1.1.RELEASE에서 5.3.20으로 변경
> - vo 폴더에 UserInfoRequestVO.java & UserInfoResponseVO.java & AccountVO.java 파일 생성
> - views 폴더에 account 폴더 생성, 그 안에 user_info.jsp 파일 생성

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/maven-v4_0_0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<groupId>com.itwillbs</groupId>
	<artifactId>fintech</artifactId>
	<name>FinTech</name>
	<packaging>war</packaging>
	<version>1.0.0-BUILD-SNAPSHOT</version>
	<properties>
		<java-version>1.8</java-version>
		<org.springframework-version>5.3.20</org.springframework-version>
		<org.aspectj-version>1.6.10</org.aspectj-version>
		<org.slf4j-version>1.6.6</org.slf4j-version>
	</properties>
	<dependencies>
		<!-- Spring -->
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-context</artifactId>
			<version>${org.springframework-version}</version>
			<exclusions>
				<!-- Exclude Commons Logging in favor of SLF4j -->
				<exclusion>
					<groupId>commons-logging</groupId>
					<artifactId>commons-logging</artifactId>
				</exclusion>
			</exclusions>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-webmvc</artifactId>
			<version>${org.springframework-version}</version>
		</dependency>

		<!-- AspectJ -->
		<dependency>
			<groupId>org.aspectj</groupId>
			<artifactId>aspectjrt</artifactId>
			<version>${org.aspectj-version}</version>
		</dependency>

		<!-- Logging -->
		<dependency>
			<groupId>org.slf4j</groupId>
			<artifactId>slf4j-api</artifactId>
			<version>${org.slf4j-version}</version>
		</dependency>
		<dependency>
			<groupId>org.slf4j</groupId>
			<artifactId>jcl-over-slf4j</artifactId>
			<version>${org.slf4j-version}</version>
			<scope>runtime</scope>
		</dependency>
		<dependency>
			<groupId>org.slf4j</groupId>
			<artifactId>slf4j-log4j12</artifactId>
			<version>${org.slf4j-version}</version>
			<scope>runtime</scope>
		</dependency>
		<dependency>
			<groupId>log4j</groupId>
			<artifactId>log4j</artifactId>
			<version>1.2.15</version>
			<exclusions>
				<exclusion>
					<groupId>javax.mail</groupId>
					<artifactId>mail</artifactId>
				</exclusion>
				<exclusion>
					<groupId>javax.jms</groupId>
					<artifactId>jms</artifactId>
				</exclusion>
				<exclusion>
					<groupId>com.sun.jdmk</groupId>
					<artifactId>jmxtools</artifactId>
				</exclusion>
				<exclusion>
					<groupId>com.sun.jmx</groupId>
					<artifactId>jmxri</artifactId>
				</exclusion>
			</exclusions>
			<scope>runtime</scope>
		</dependency>

		<!-- @Inject -->
		<dependency>
			<groupId>javax.inject</groupId>
			<artifactId>javax.inject</artifactId>
			<version>1</version>
		</dependency>

		<!-- Servlet -->
		<dependency>
			<groupId>javax.servlet</groupId>
			<artifactId>servlet-api</artifactId>
			<version>2.5</version>
			<scope>provided</scope>
		</dependency>
		<dependency>
			<groupId>javax.servlet.jsp</groupId>
			<artifactId>jsp-api</artifactId>
			<version>2.1</version>
			<scope>provided</scope>
		</dependency>
		<dependency>
			<groupId>javax.servlet</groupId>
			<artifactId>jstl</artifactId>
			<version>1.2</version>
		</dependency>



		<!-- Test -->
		<dependency>
			<groupId>junit</groupId>
			<artifactId>junit</artifactId>
			<version>4.7</version>
			<scope>test</scope>
		</dependency>

		<!-- 추가 라이브러리 등록 -->
		<!-- https://mvnrepository.com/artifact/mysql/mysql-connector-java -->
		<dependency>
			<groupId>mysql</groupId>
			<artifactId>mysql-connector-java</artifactId>
			<version>8.0.29</version>
		</dependency>

		<!-- RestTemplate 객체 사용하여 JSON 응답 처리할 때 필요한 라이브러리(jsoin-simple, gson) -->
		<!-- https://mvnrepository.com/artifact/com.googlecode.json-simple/json-simple -->
		<dependency>
			<groupId>com.googlecode.json-simple</groupId>
			<artifactId>json-simple</artifactId>
			<version>1.1.1</version>
		</dependency>

		<!-- https://mvnrepository.com/artifact/com.google.code.gson/gson -->
		<dependency>
			<groupId>com.google.code.gson</groupId>
			<artifactId>gson</artifactId>
			<version>2.9.0</version>
		</dependency>


	</dependencies>
	<build>
		<plugins>
			<plugin>
				<artifactId>maven-eclipse-plugin</artifactId>
				<version>2.9</version>
				<configuration>
					<additionalProjectnatures>
						<projectnature>org.springframework.ide.eclipse.core.springnature</projectnature>
					</additionalProjectnatures>
					<additionalBuildcommands>
						<buildcommand>org.springframework.ide.eclipse.core.springbuilder</buildcommand>
					</additionalBuildcommands>
					<downloadSources>true</downloadSources>
					<downloadJavadocs>true</downloadJavadocs>
				</configuration>
			</plugin>
			<plugin>
				<groupId>org.apache.maven.plugins</groupId>
				<artifactId>maven-compiler-plugin</artifactId>
				<version>2.5.1</version>
				<configuration>
					<source>1.6</source>
					<target>1.6</target>
					<compilerArgument>-Xlint:all</compilerArgument>
					<showWarnings>true</showWarnings>
					<showDeprecation>true</showDeprecation>
				</configuration>
			</plugin>
			<plugin>
				<groupId>org.codehaus.mojo</groupId>
				<artifactId>exec-maven-plugin</artifactId>
				<version>1.2.1</version>
				<configuration>
					<mainClass>org.test.int1.Main</mainClass>
				</configuration>
			</plugin>
		</plugins>
	</build>
</project>

```

- vo
```java
----------------------------------------------UserInfoRequestVO.java----------------------------------------------
package com.itwillbs.fintech.vo;

public class UserInfoRequestVO {
	private String access_token;
	private String user_seq_no;
	public String getAccess_token() {
		return access_token;
	}
	public void setAccess_token(String access_token) {
		this.access_token = access_token;
	}
	public String getUser_seq_no() {
		return user_seq_no;
	}
	public void setUser_seq_no(String user_seq_no) {
		this.user_seq_no = user_seq_no;
	}
	
}

----------------------------------------------UserInfoResponseVO.java----------------------------------------------
package com.itwillbs.fintech.vo;

import java.util.List;

// 2.2.1 사용자 정보조회 API 응답 데이터
public class UserInfoResponseVO {
	private String api_tran_id;
	private String api_tran_dtm;
	private String rsp_code;
	private String rsp_message;
	private String user_seq_no; // 고객마다 다른 고정값
	private String user_ci; // 고객마다 다른 고정값
	private String user_name;
	private String user_info;
	private String user_gender;
	private String user_cell_no;
	private String user_email;
	private String res_cnt;
	private List<AccountVO> res_list;
	
	
	public String getApi_tran_id() {
		return api_tran_id;
	}
	public void setApi_tran_id(String api_tran_id) {
		this.api_tran_id = api_tran_id;
	}
	public String getApi_tran_dtm() {
		return api_tran_dtm;
	}
	public void setApi_tran_dtm(String api_tran_dtm) {
		this.api_tran_dtm = api_tran_dtm;
	}
	public String getRsp_code() {
		return rsp_code;
	}
	public void setRsp_code(String rsp_code) {
		this.rsp_code = rsp_code;
	}
	public String getRsp_message() {
		return rsp_message;
	}
	public void setRsp_message(String rsp_message) {
		this.rsp_message = rsp_message;
	}
	public String getUser_seq_no() {
		return user_seq_no;
	}
	public void setUser_seq_no(String user_seq_no) {
		this.user_seq_no = user_seq_no;
	}
	public String getUser_ci() {
		return user_ci;
	}
	public void setUser_ci(String user_ci) {
		this.user_ci = user_ci;
	}
	public String getUser_name() {
		return user_name;
	}
	public void setUser_name(String user_name) {
		this.user_name = user_name;
	}
	public String getUser_info() {
		return user_info;
	}
	public void setUser_info(String user_info) {
		this.user_info = user_info;
	}
	public String getUser_gender() {
		return user_gender;
	}
	public void setUser_gender(String user_gender) {
		this.user_gender = user_gender;
	}
	public String getUser_cell_no() {
		return user_cell_no;
	}
	public void setUser_cell_no(String user_cell_no) {
		this.user_cell_no = user_cell_no;
	}
	public String getUser_email() {
		return user_email;
	}
	public void setUser_email(String user_email) {
		this.user_email = user_email;
	}
	public String getRes_cnt() {
		return res_cnt;
	}
	public void setRes_cnt(String res_cnt) {
		this.res_cnt = res_cnt;
	}
	public List<AccountVO> getRes_list() {
		return res_list;
	}
	public void setRes_list(List<AccountVO> res_list) {
		this.res_list = res_list;
	}
	
	
}

----------------------------------------------AccountVO.java----------------------------------------------
package com.itwillbs.fintech.vo;

// 2. 사용자/계좌 관리에서 사용되는 계좌 정보(배열로 전달된 데이터)
public class AccountVO {
	private String fintech_use_num;
    private String account_alias;
    private String bank_code_std;
    private String bank_code_sub;
    private String bank_name;
    private String savings_bank_name;
    private String account_num;
    private String account_num_masked;
    private String account_seq;
    private String account_holder_name;
    private String account_holder_type;
    private String account_type;
    private String inquiry_agree_yn;
    private String inquiry_agree_dtime;
    private String transfer_agree_yn;
    private String transfer_agree_dtime;
    private String account_state;
    
	public String getFintech_use_num() {
		return fintech_use_num;
	}
	public void setFintech_use_num(String fintech_use_num) {
		this.fintech_use_num = fintech_use_num;
	}
	public String getAccount_alias() {
		return account_alias;
	}
	public void setAccount_alias(String account_alias) {
		this.account_alias = account_alias;
	}
	public String getBank_code_std() {
		return bank_code_std;
	}
	public void setBank_code_std(String bank_code_std) {
		this.bank_code_std = bank_code_std;
	}
	public String getBank_code_sub() {
		return bank_code_sub;
	}
	public void setBank_code_sub(String bank_code_sub) {
		this.bank_code_sub = bank_code_sub;
	}
	public String getBank_name() {
		return bank_name;
	}
	public void setBank_name(String bank_name) {
		this.bank_name = bank_name;
	}
	public String getSavings_bank_name() {
		return savings_bank_name;
	}
	public void setSavings_bank_name(String savings_bank_name) {
		this.savings_bank_name = savings_bank_name;
	}
	public String getAccount_num() {
		return account_num;
	}
	public void setAccount_num(String account_num) {
		this.account_num = account_num;
	}
	public String getAccount_num_masked() {
		return account_num_masked;
	}
	public void setAccount_num_masked(String account_num_masked) {
		this.account_num_masked = account_num_masked;
	}
	public String getAccount_seq() {
		return account_seq;
	}
	public void setAccount_seq(String account_seq) {
		this.account_seq = account_seq;
	}
	public String getAccount_holder_name() {
		return account_holder_name;
	}
	public void setAccount_holder_name(String account_holder_name) {
		this.account_holder_name = account_holder_name;
	}
	public String getAccount_holder_type() {
		return account_holder_type;
	}
	public void setAccount_holder_type(String account_holder_type) {
		this.account_holder_type = account_holder_type;
	}
	public String getAccount_type() {
		return account_type;
	}
	public void setAccount_type(String account_type) {
		this.account_type = account_type;
	}
	public String getInquiry_agree_yn() {
		return inquiry_agree_yn;
	}
	public void setInquiry_agree_yn(String inquiry_agree_yn) {
		this.inquiry_agree_yn = inquiry_agree_yn;
	}
	public String getInquiry_agree_dtime() {
		return inquiry_agree_dtime;
	}
	public void setInquiry_agree_dtime(String inquiry_agree_dtime) {
		this.inquiry_agree_dtime = inquiry_agree_dtime;
	}
	public String getTransfer_agree_yn() {
		return transfer_agree_yn;
	}
	public void setTransfer_agree_yn(String transfer_agree_yn) {
		this.transfer_agree_yn = transfer_agree_yn;
	}
	public String getTransfer_agree_dtime() {
		return transfer_agree_dtime;
	}
	public void setTransfer_agree_dtime(String transfer_agree_dtime) {
		this.transfer_agree_dtime = transfer_agree_dtime;
	}
	public String getAccount_state() {
		return account_state;
	}
	public void setAccount_state(String account_state) {
		this.account_state = account_state;
	}
    
    
}

----------------------------------------------AccountSearchRequestVO.java----------------------------------------------
package com.itwillbs.fintech.vo;

public class AccountSearchRequestVO {
    private String access_token;
    private String user_seq_no;
    private String include_cancel_yn;
    private String sort_order;
    private String model;
	public String getAccess_token() {
		return access_token;
	}
	public void setAccess_token(String access_token) {
		this.access_token = access_token;
	}
	public String getUser_seq_no() {
		return user_seq_no;
	}
	public void setUser_seq_no(String user_seq_no) {
		this.user_seq_no = user_seq_no;
	}
	public String getInclude_cancel_yn() {
		return include_cancel_yn;
	}
	public void setInclude_cancel_yn(String include_cancel_yn) {
		this.include_cancel_yn = include_cancel_yn;
	}
	public String getSort_order() {
		return sort_order;
	}
	public void setSort_order(String sort_order) {
		this.sort_order = sort_order;
	}
	public String getModel() {
		return model;
	}
	public void setModel(String model) {
		this.model = model;
	}
    
    
}


----------------------------------------------AccountSearchResponseVO.java----------------------------------------------

package com.itwillbs.fintech.vo;

import java.util.List;

public class AccountSearchResponseVO {
	private String api_tran_id;
	private String rsp_code;
	private String rsp_message;
	private String api_tran_dtm;
	private String user_name;
	private String res_cnt;
	private List<AccountVO> res_list;
	public String getApi_tran_id() {
		return api_tran_id;
	}
	public void setApi_tran_id(String api_tran_id) {
		this.api_tran_id = api_tran_id;
	}
	public String getRsp_code() {
		return rsp_code;
	}
	public void setRsp_code(String rsp_code) {
		this.rsp_code = rsp_code;
	}
	public String getRsp_message() {
		return rsp_message;
	}
	public void setRsp_message(String rsp_message) {
		this.rsp_message = rsp_message;
	}
	public String getApi_tran_dtm() {
		return api_tran_dtm;
	}
	public void setApi_tran_dtm(String api_tran_dtm) {
		this.api_tran_dtm = api_tran_dtm;
	}
	public String getUser_name() {
		return user_name;
	}
	public void setUser_name(String user_name) {
		this.user_name = user_name;
	}
	public String getRes_cnt() {
		return res_cnt;
	}
	public void setRes_cnt(String res_cnt) {
		this.res_cnt = res_cnt;
	}
	public List<AccountVO> getRes_list() {
		return res_list;
	}
	public void setRes_list(List<AccountVO> res_list) {
		this.res_list = res_list;
	}
	
	
}
```

- Controller
```java
----------------------------------------------OpenBankingController.java----------------------------------------------
package com.itwillbs.fintech.controller;

import javax.servlet.http.HttpServletRequest;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.ModelAttribute;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;

import com.itwillbs.fintech.service.OpenBankingService;
import com.itwillbs.fintech.vo.RequestTokenVO;
import com.itwillbs.fintech.vo.ResponseTokenVO;
import com.itwillbs.fintech.vo.UserInfoRequestVO;
import com.itwillbs.fintech.vo.UserInfoResponseVO;
import com.itwillbs.fintech.vo.AccountSearchRequestVO;
import com.itwillbs.fintech.vo.AccountSearchResponseVO;


@Controller
public class OpenBankingController {
	private String clientId = "xxxxx";
	private String clientSecret = "xxxx";

	// OpenBankingService 객체 자동 주입
	@Autowired 
	private OpenBankingService openBankingService;
	
	// @RequestMapping(value = "/callback", method = RequestMethod.GET) 대신
	// @GetMapping(value = "/callback") 사용 가능

	@RequestMapping(value = "/callback", method = RequestMethod.GET)
	public String getToken(@ModelAttribute RequestTokenVO requestToken, Model model) {
		// OAuth 인증 완료 후 전송되는 인증코드(code)를 자동으로 RequestTokenVO 객체에 저장
		System.out.println("인증코드 : " + requestToken.getCode());
		
		// 응답데이터로 전달받은 인증코드를 사용하여 엑세스토큰 발급 받기
		// OpenBankingService 객체의 requestToken() 메서드를 호출하여 엑세스토큰 발급 요청
		// => 파라미터 : RequestTokenVO 객체, 리턴타입 : ResponseTokenVO 객체(responseToken)
		
		ResponseTokenVO responseToken = openBankingService.requestToken(requestToken);
		
		responseToken.setAccess_token("xxxxxxxxxxx");
		responseToken.setUser_seq_no("xxxxxxx");
		
		// bank_main.jsp 페이지로 포워딩
		// => 이 때, 전달받은 토큰 정보(ResponseTokenVO 객체)를 함께 전달
		model.addAttribute("responseToken", responseToken);
		

		return "bank_main";
	}
	
	// 사용자 정보 조회
	@RequestMapping(value = "/userInfo", method = RequestMethod.GET)
	public String getUserInfo(@ModelAttribute UserInfoRequestVO userInfoRequestVO, Model model) {
		// SErvice 객체의 findUser() 메서드를 호출하여 사용자 정보 조회
		// => 파라미터 : UserInfoRequestVO, 리턴타입 : UserInfoResponseVO
		UserInfoResponseVO userInfo = openBankingService.findUser(userInfoRequestVO);
		
		// Model 객체에 UserInfoResponseVO 객체와 엑세스토큰 저장
		model.addAttribute("userInfo", userInfo);
		model.addAttribute("access_token", userInfoRequestVO.getAccess_token());

		
		return "account/user_info";
	}
	
	// 등록계좌 조회
	@RequestMapping(value = "/accountList", method = RequestMethod.GET)
	public String getAccountList(@ModelAttribute AccountSearchRequestVO accountSearchRequestVO, Model model) {
		// Service 객체의 findAccount() 메서드를 호출하여 사용자 정보 조회
		// => 파라미터 : AccountSearchRequestVO, 리턴타입 : AccountSearchResponseVO
		AccountSearchResponseVO accountList = openBankingService.findAccount(accountSearchRequestVO);
		
		// Model 객체에 UserInfoResponseVO 객체와 엑세스토큰 저장
		model.addAttribute("accountList", accountList);
		model.addAttribute("access_token", accountSearchRequestVO.getAccess_token());

		
		return "account/list";
	}

}

```

- Service
```java
----------------------------------------------OpenBankingService.java----------------------------------------------
package com.itwillbs.fintech.service;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import com.itwillbs.fintech.vo.AccountSearchRequestVO;
import com.itwillbs.fintech.vo.AccountSearchResponseVO;
import com.itwillbs.fintech.vo.RequestTokenVO;
import com.itwillbs.fintech.vo.ResponseTokenVO;
import com.itwillbs.fintech.vo.UserInfoRequestVO;
import com.itwillbs.fintech.vo.UserInfoResponseVO;

@Service
public class OpenBankingService { // OpenBankingController - OpenBankingApiClient 객체 중간자 역할
	// OpenBankingApiClient 객체 자동 주입
	@Autowired 
	private OpenBankingApiClient openBankingApiClient;

	// 엑세스토큰 발급 요청을 위한 requestToken() 메서드 호출
	public ResponseTokenVO requestToken(RequestTokenVO requestToken) {
		return openBankingApiClient.requestToken(requestToken);
	}

	// 사용자 정보 조회
	public UserInfoResponseVO findUser(UserInfoRequestVO userInfoRequestVO) {
		return openBankingApiClient.findUser(userInfoRequestVO);
	}

	// 등록계좌 조회
	public AccountSearchResponseVO findAccount(AccountSearchRequestVO accountSearchRequestVO) {
		return openBankingApiClient.findAccount(accountSearchRequestVO);
	}
}

----------------------------------------------OpenBankingSApiClient.java----------------------------------------------
package com.itwillbs.fintech.service;

import org.springframework.http.HttpEntity;
import org.springframework.http.HttpHeaders;
import org.springframework.http.HttpMethod;
import org.springframework.stereotype.Service;
import org.springframework.util.LinkedMultiValueMap;
import org.springframework.util.MultiValueMap;
import org.springframework.web.client.RestTemplate;
import org.springframework.web.util.UriComponents;
import org.springframework.web.util.UriComponentsBuilder;

import com.itwillbs.fintech.vo.AccountSearchRequestVO;
import com.itwillbs.fintech.vo.AccountSearchResponseVO;
import com.itwillbs.fintech.vo.RequestTokenVO;
import com.itwillbs.fintech.vo.ResponseTokenVO;
import com.itwillbs.fintech.vo.UserInfoRequestVO;
import com.itwillbs.fintech.vo.UserInfoResponseVO;

@Service
public class OpenBankingApiClient {
	private String clientId = "0ded34b8-72a5-42d0-bb58-7af4dd683f41";
	private String clientSecret = "483c45ab-ea4f-4a3d-9902-80c90b522c47";
	private String redirectUri = "http://localhost:8080/fintech/callback_token";
	private String baseUrl = "https://testapi.openbanking.or.kr/v2.0";
	
	// REST 방식의 API 요청에 사용할 클래스 타입 변수 선언
	private RestTemplate restTemplate; // REST 방식 요청 및 응답에 사용되는 클래스
	private HttpHeaders httpHeaders; // 헤더 정보를 관리할 클래스
	
	// 헤더에 엑세스 토큰을 추가하는 setHeaderAccessToken() 메서드 정의
	// => 파라미터 : 엑세스토큰, 리턴타입 : HttpHeaders
	public HttpHeaders setHeaderAccessToken(String access_token) {
		// HttpHeaders 객체의 add() 메서드를 호출하여 "항목", "값" 형태로 파라미터 전달
		httpHeaders.add("Authorization", "Bearer " + access_token);
		return httpHeaders;
	}
	
	// 2.1.2. 토큰 발급 API 요청 - Access Token 가져오기
	// 엑세스토큰 발급 요청 작업을 수행할 requestToken() 메서드 정의
	public ResponseTokenVO requestToken(RequestTokenVO requestToken) {
		// REST 방식 요청에 필요한 객체 생성
		restTemplate = new RestTemplate();
		httpHeaders = new HttpHeaders();
		
		/* 요청 메세지 URL
		 * HTTP URL : https://openapi.openbanking.or.kr/oauth/2.0/token
		 * HTTP Method : POST
		 * Content-Type : application/x-www-form-urlencoded; charset=UTF-8
		 */
		// 1. HTTP Header 오브젝트(정보) 생성
		httpHeaders.add("Content-Type", "application/x-www-form-urlencoded;charset=UTF-8");
		
		// 2. HTTP Body 오브젝트(정보) 생성
		// 2-1. Body 에 추가할 데이터를 관리하는 RequestTokenVO 객체에 필요한 데이터 저장
		// => grant_type 값은 "authorization_code" 값 고정
		requestToken.setRequestToken(clientId, clientSecret, redirectUri, "authorization_code");
		
		// 헤더의 content-type 이 application/x-www-form-urlencoded 이므로 객체 저장이 불가능하며
		// 대신 객체 형태를 파라미터 형태로 변환하여 관리할 MultiValueMap 객체 사용
		MultiValueMap<String, String> parameters = new LinkedMultiValueMap<String, String>();
		// MultiValueMap 객체의 add() 메서드를 호출하여 키, 값 형태로 파라미터 데이터 저장
		parameters.add("code", requestToken.getCode());
		parameters.add("client_id", requestToken.getClient_id());
		parameters.add("client_secret", requestToken.getClient_secret());
		parameters.add("redirect_uri", requestToken.getRedirect_uri());
		parameters.add("grant_type", requestToken.getGrant_type());

		// HttpHeader 와 HttpBody 오브젝트를 하나의 객체로 관리하기 위한 HttpEntity 객체 생성
		// => 제네릭타입은 파라미터 데이터를 관리하는 MultiValueMap<String, String> 타입 지정
		HttpEntity<MultiValueMap<String, String>> param = new HttpEntity<MultiValueMap<String,String>>(parameters, httpHeaders);
		
		// 엑세스토큰 요청에 사용될 요청 URL 저장
		String requestUrl = "https://testapi.openbanking.or.kr/oauth/2.0/token";
		
		// RestTemplate 객체의 exchange() 메서드를 호출하여 REST 방식 요청 작업 수행하고
		// 리턴되는 결과값(응답데이터)의 바디 영역 데이터를 리턴 
		// => exchange(요청 URL, HttpMethod.요청메서드, HttpEntity 객체, 응답받을 객체의 클래스타입).getBody();
		// => 이 때, 리턴되는 데이터 타입은 응답받을 객체의 클래스타입으로 
		// 	  Body 데이터가 자동으로 저장되어 리턴됨 
		return restTemplate.exchange(requestUrl, HttpMethod.POST, param, ResponseTokenVO.class).getBody();
	}

	// 사용자 정보 조회
	public UserInfoResponseVO findUser(UserInfoRequestVO userInfoRequestVO) {
		// REST 방식 요청에 필요한 객체 생성
		restTemplate = new RestTemplate();
		httpHeaders = new HttpHeaders();
		
		// 2.2.1 사용자정보조회 API URL 주소 생성
		String url = baseUrl + "/user/me";
		
		// HttpHeaders 와 HttpBody 오브젝트를 하나의 객체로 관리하기 위한 HttpEntity 객체 생성
		// => 파라미터로 HttpHeaders 객체 전달을 위해 
		// 	  헤더 생성 작업을 수행하는 사용자 정의 메서드 setHeaderAccessToken() 호출 
		//	  (파라미터로 엑세스 토큰 전달 => UserInforequestVO 객체에 저장되어 있음)
		HttpEntity<String> openBankingUserInfoRequest = new HttpEntity<String>(setHeaderAccessToken(userInfoRequestVO.getAccess_token()));
		
		// UriComponentsBuilder 클래스의 fromHttpUrl() 메서드를 호출하여 URL 파라미터 정보 생성
		// 1단계. UriComponentsBuilder.fromHttpUrl() 메서드를 호출하여 요청 URL 주소 전달
		// 2단계. 1단계에서 생성된 객체의 queryParam() 메서드를 호출하여 전달할 파라미터를
		//	     키, 값 형식으로 전달
		// 3단계. 2단계에서 생성된 객체의 build() 메서드를 호출하여 UriComponents 객체 리턴(생성)
		// 위의 세 과정을 빌더 패턴(Builder Pattern)을 활용하여 하나의 문장으로 압축 가능
		// (자기 자신을 리턴하는 메서드 호출 후 연쇄적으로 메서드를 이이어나가는 것)
		
		UriComponents uriBuilder = UriComponentsBuilder.fromHttpUrl(url)
				.queryParam("user_seq_no", userInfoRequestVO.getUser_seq_no())
				.build();
		
		// exchange() 메서드 파라미터 : UriBuilder 문자열로 변환, 요청방식, HttpEntity 객체,
		// 						   응답데이터를 파싱하기 위한 클래스(.class 필수)
		// => 메서드 뒤에 .getBody() 메서드를 호출하여 body 데이터에 대한 파싱된 결과를 리턴받기
		return restTemplate.exchange(uriBuilder.toString(), HttpMethod.GET, openBankingUserInfoRequest, UserInfoResponseVO.class).getBody();
	}

	// 등록계좌 조회
	public AccountSearchResponseVO findAccount(AccountSearchRequestVO accountSearchRequestVO) {
		restTemplate = new RestTemplate();
		httpHeaders = new HttpHeaders();
		
		// 2.2.1 사용자정보조회 API URL 주소 생성
		String url = baseUrl + "/account/list";
		
		httpHeaders.add("Authorization", "Bearer " + accountSearchRequestVO.getAccess_token());
		
		HttpEntity<String> openBankingAccountListRequest = new HttpEntity<String>(httpHeaders);

		UriComponents uriBuilder = UriComponentsBuilder.fromHttpUrl(url)
				.queryParam("user_seq_no", accountSearchRequestVO.getUser_seq_no())
				.queryParam("include_cancel_yn", accountSearchRequestVO.getInclude_cancel_yn())
				.queryParam("sort_order", accountSearchRequestVO.getSort_order())
				.build();
		
		return restTemplate.exchange(uriBuilder.toString(), HttpMethod.GET, openBankingAccountListRequest, AccountSearchResponseVO.class).getBody();
	}
	
}

```




```jsp
----------------------------------------------bank_main.jsp----------------------------------------------
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<h3>인증완료</h3>
	<h3>엑세스 토큰 : ${responseToken.access_token }</h3>
	<h3>사용자 번호 : ${responseToken.user_seq_no }</h3>
	
	
	<form method="get" action="userInfo">
			<%-- 필요 파라미터는 입력데이터 없이 hidden 속성으로 --%>
			<input type="hidden" name="access_token" value="${responseToken.access_token }">
			<input type="hidden" name="user_seq_no" value="${responseToken.user_seq_no }">
			<input type="submit" value="사용자정보조회">
	</form>
	<hr>
	<!-- 2.3.3 등록계좌조회 -->
	<form method="get" action="accountList">
			<%-- 필요 파라미터는 입력데이터 없이 hidden 속성으로 --%>
			<input type="hidden" name="access_token" value="${responseToken.access_token }">
			<input type="hidden" name="user_seq_no" value="${responseToken.user_seq_no }">
			<input type="hidden" name="include_cancel_yn" value="Y">
			<input type="hidden" name="sort_order" value="D">
			<input type="submit" value="등록계좌조회">
	</form>
		
		
</body>
</html>

----------------------------------------------user_info.jsp----------------------------------------------
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<h1>사용자 정보조회 결과</h1>
	<h3>고객번호 : ${userInfo.user_seq_no }</h3>
	<h3>고객CI값 : ${userInfo.user_ci }</h3>
</body>
</html>

----------------------------------------------list.jsp----------------------------------------------
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<table>
		<tr>
			<th>계좌번호</th><th>은행명</th><th>예금주명</th>
		</tr>
		<%-- accountList 객체에 저장되어 있는 계좌 목록(res_list) 가져와서 반복하여 복수개 계좌 접근 --%>
		<c:forEach var="account" items="${accountList.res_list }">
			<tr>
				<td>${account.account_num_masked }</td>
				<td>${account.bank_name }</td>
				<td>${account.account_holder_name }</td>
			</tr>
		</c:forEach>
	</table>
</body>
</html>
```
